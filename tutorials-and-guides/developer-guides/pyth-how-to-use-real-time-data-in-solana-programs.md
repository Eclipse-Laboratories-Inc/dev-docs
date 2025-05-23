# Pyth: How to Use Real-Time Data in Solana Programs

This guide explains how to use real-time Pyth data in SVM applications.

### Install Pyth SDKs <a href="#install-pyth-sdks" id="install-pyth-sdks"></a>

Pyth provides two SDKs for SVM applications to cover the on- and off-chain portions of the integration:

#### Rust SDK <a href="#rust-sdk" id="rust-sdk"></a>

The [pyth-solana-receiver-sdk crate(opens in a new tab)](https://github.com/pyth-network/pyth-crosschain/tree/main/target_chains/solana/pyth_solana_receiver_sdk) can be used to consume Pyth prices inside Solana programs written in Rust. Add this crate to the dependencies section of your `Cargo.toml` file:

```rust
[dependencies]pyth-solana-receiver-sdk ="0.1.0"
```

#### Typescript SDK <a href="#typescript-sdk" id="typescript-sdk"></a>

Pyth provides two Typescript packages, [@pythnetwork/price-service-client(opens in a new tab)](https://github.com/pyth-network/pyth-crosschain/tree/main/price_service/client/js) and [@pythnetwork/pyth-solana-receiver(opens in a new tab)](https://github.com/pyth-network/pyth-crosschain/tree/main/target_chains/solana/sdk/js/pyth_solana_receiver), for fetching Pyth prices and submitting them to the blockchain respectively. Add these packages to your off-chain dependencies:

{% code overflow="wrap" %}
```typescript
npm install --save @pythnetwork/price-service-client @pythnetwork/pyth-solana-receiver
```
{% endcode %}

### Write Contract Code <a href="#write-contract-code" id="write-contract-code"></a>

Add code to your Solana program to read the Pyth price. Pyth prices are posted to price update accounts that can be passed to any instruction that needs price data. If you are using Anchor, you can simply add an `Account<'info, PriceUpdateV2>` field to your `Context` struct:

{% code overflow="wrap" %}
```rust
use pyth_solana_receiver_sdk::price_update::{PriceUpdateV2};
 
#[derive(Accounts)]
#[instruction()]
pub struct Sample<'info> {
    #[account(mut)]
    pub payer: Signer<'info>,
    // Add this account to any instruction Context that needs price data.
    pub price_update: Account<'info, PriceUpdateV2>,
}
```
{% endcode %}

Warning: users must ensure that the account passed to their instruction is owned by the Pyth pull oracle program. Using Anchor with the `Account<'info, PriceUpdateV2>` type will automatically perform this check. However, if you are not using Anchor, it is your responsibility to perform this check.

Next, update the instruction logic to read the price from the price update account:

{% code overflow="wrap" %}
```rust
pub fn sample(ctx: Context<Sample>) -> Result<()> {
    let price_update = &mut ctx.accounts.price_update;
    // get_price_no_older_than will fail if the price update is more than 30 seconds old
    let maximum_age: u64 = 30;
    // get_price_no_older_than will fail if the price update is for a different price feed.
    // This string is the id of the BTC/USD feed. See https://pyth.network/developers/price-feed-ids for all available ids.
    let feed_id: [u8; 32] = get_feed_id_from_hex("0xe62df6c8b4a85fe1a67db44dc12de5db330f7ac66b72dc658afedf0f4a415b43")?;
    let price = price_update.get_price_no_older_than(&Clock::get()?, maximum_age, &feed_id)?;
    // Sample output:
    // The price is (7160106530699 ± 5129162301) * 10^-8
    msg!("The price is ({} ± {}) * 10^{}", price.price, price.conf, price.exponent);
 
    Ok(())
}
```
{% endcode %}

Warning: it is your responsibility to validate that the provided price update is for the appropriate price feed and timestamp. `PriceUpdateV2` guarantees that the account contains a verified price for _some_ price feed at _some_ point in time. There are various methods on this struct (such as `get_price_no_older_than`) that you can use to implement the necessary checks. Note: if you choose the price feed account integration (see below), you can use an account address check to validate the price feed id.

### Write Frontend Code <a href="#write-frontend-code" id="write-frontend-code"></a>

There are two different paths to the frontend integration of Pyth prices on Solana. Developers can choose to use two different types of accounts:

* **Price feed accounts** hold a sequence of prices for a specific price feed id that always moves forward in time. These accounts have a fixed address that your program can depend on. The Pyth Data Association maintains a set of price feed accounts that are continuously updated. Such accounts are a good fit for applications that always want to consume the most recent price.
* **Price update accounts** are ephemeral accounts that anyone can create, overwrite, and close. These accounts are a good fit for applications that want to consume prices for a specific timestamp.

Both price feed accounts and price update accounts work identically from the perspective of the on-chain program. However, the frontend integration differs slightly between the two. Both options are explained in the sections below, and developers should pick the one that is best suited for their use case.

#### Price Feed Accounts <a href="#price-feed-accounts" id="price-feed-accounts"></a>

If you are using price feed accounts, your frontend code simply needs to pass the relevant price feed account address to the transaction. Price feed accounts are program-derived addresses and thus the account ID for any price feed can be derived automatically. The `PythSolanaReceiver` class provides a method for deriving this information:

{% code overflow="wrap" %}
```rust
import { PythSolanaReceiver } from "@pythnetwork/pyth-solana-receiver";
 
// You will need a Connection from @solana/web3.js and an AnchorWallet to create
// the receiver.
const connection: Connection;
const wallet: AnchorWallet;
const pythSolanaReceiver = new PythSolanaReceiver({ connection, wallet });
 
// There are up to 2^16 different accounts for any given price feed id.
// The 0 value below is the shard id that indicates which of these accounts you would like to use.
// However, you may choose to use a different shard to prevent Solana congestion on another app from affecting your app.
const solUsdPriceFeedAccount = pythSolanaReceiver
  .getPriceFeedAccountAddress(0, SOL_PRICE_FEED_ID)
  .toBase58();
```
{% endcode %}

The price feed account integration assumes that an off-chain process is continuously updating each price feed. The Pyth Data Association sponsors price updates for a subset of commonly-used price feeds on shard 0. Please see [Sponsored Feeds](https://docs.pyth.network/price-feeds/sponsored-feeds) for a list of sponsored feeds and their account addresses. However, updating a price feed is a permissionless operation, and anyone can run this process. Please see [Using Scheduler](https://docs.pyth.network/price-feeds/schedule-price-updates/using-scheduler) for more information. Running the scheduler can help with reliability and to update feed/shard pairs that are not part of the default schedule.

#### Price Update Accounts <a href="#price-update-accounts" id="price-update-accounts"></a>

If you are using price update accounts, your frontend code needs to perform two different tasks:

1. Fetch price updates from Hermes
2. Post the price updates to Solana and invoke your application logic

**Fetch price updates**

Use `PriceServiceConnection` from `@pythnetwork/price-service-client` to fetch Pyth price updates from Hermes:

{% code overflow="wrap" %}
```rust
import { PriceServiceConnection } from "@pythnetwork/price-service-client";
 
// The URL below is a public Hermes instance operated by the Pyth Data Association.
// Hermes is also available from several third party providers listed here:
// https://docs.pyth.network/price-feeds/api-instances-and-providers/hermes
const priceServiceConnection = new PriceServiceConnection(
  "https://hermes.pyth.network/",
  { priceFeedRequestConfig: { binary: true } }
);
 
// Hermes provides other methods for retrieving price updates. See
// https://hermes.pyth.network/docs for more information.
const priceUpdateData: string[] = await priceServiceConnection.getLatestVaas([
  "0xe62df6c8b4a85fe1a67db44dc12de5db330f7ac66b72dc658afedf0f4a415b43",
]);
 
// Price updates are strings of base64-encoded binary data. Example:
// ["UE5BVQEAAAADuAEAAAADDQJoIZzihVp30/71MXDexqiJDjGpEUEcHj/D06pRiYTPPlRPZKKiVFaZdqiGZ7vvGBj1kK69fKRncCKHXlGyS0LcAQPREzVlm7AcY+X0/hCupv8M5KMhEgOCxq5LxCCKQjL/xwm4jTCJrwkHn/PBaoirnlbHMjxAlwJaFF4RGeE1KRDeAASQIFjfbbobiq2OcSRnK3Ny1NY2Zd012ahIhoWpqxbv4UqDZXLh+bPm4Q14SfO/llRTOmqG6O4v+nfjZa7WNjgxAAZFANqAEooKvijK+476YBITKono+vJ4bSQHUiXOaiGs/zcI6WYGRM0xc6ov2mfCaTuH7mx3ElaLyPGvRX4D9DyHAAf+dPg+NEepwMJI1qPLRcTy0xoz2Yq0k2MuL87gCBsYVlS31atGsQcrXfHr4xcTKKyEUh1tIaOfRbPdX1cvs4D6AAj2/vuKpAgYWhd2gtqHsQLV1vMgq8SKH8wiOmhaMZ06GAQSM1pYLpHZRhYaUQbrUJAeqEeX+qMMqQFMPSSUnEKNAAmCE98NUYbHuEoxJGMDybTGCyDEXCmaNM0q6GT0qrbSmT6NF50yz9CE30WWHNOZzFtK2rCyBYFH2aAp6lQ1JKfmAQpW/wUaOhSdwGiEPWvpY3FWL077i0c4auXQjSQNaDD0cBnmvJTS5R3KxK5aunuUvVAT1mHTnpKHIzNKyu7ICM2zAQvrIFfWRFjVE0zRCvoAcvMpmpS7atWu8VgvklpZh9Qt9xYSO2Yq/asgNsMSQaowXiU0MfjggS+UJ8yWaOpUg18vAAxAMuUlOjNzFj6oPES850YNu2k7PM7AGL8Gb/8+HshkfjG0GsNR8H8/vB8v/iEcaScxQFXwtLT0OSgjWMa0ByknAA7PScKUEP8N7iJKYv6lmEs26DZnxzdpGVZRGqbbC0mxyjY0HqsT0rv2wNvy3MbAtABDMsLumII00cRCKBsZXGlKARCC0NzsKnduLsgGfqxYL4yuf910DKrRp5j+fKLmF2QiB2yVT90ja0782/u6BZZUGRMoA/AWl1qvswBtnlSkHcWEABIp74UFLiiA+MBBvBzhLBxSTKXldiLJ75+U/eqK/ej6qT+I+6S1pzT/ntXdpD25jmQhjtsYEqs/rmgs5U2p4AVRAGYULPcAAAAAABrhAfrtrFhR4yubI7X5QRqMK6xKrj7U3XuBHdGnLqSqcQAAAAAC8IR2AUFVV1YAAAAAAAf6dUkAACcQMZv+5jfvAe6sflX1cL7xu9WWQ9UBAFUA5i32yLSoX+GmfbRNwS3l2zMPesZrctxliv7fD0pBW0MAAAaBiqrXwAAAAADYu55y////+AAAAABmFCz3AAAAAGYULPYAAAaFH6MbAAAAAADcBIdMCre0t06ngCnw+N4IkFpZVqOz9YuwKL+UFdt13ZBtay0YZnkw7QGoaTDCLlsNK1tk1F/qgMyOcYozjOTj41XriIpEPeG2HPYl+u0CKolGlCsz1IDu4w2lyh6LWVaMkEybGz7ih4H2RqCj6BVu182ZqsZgJx9ghzKImAo4cIlWzRTwpm4daAqHa4JEyimFDpFt6UeqvS5TNu2F8W+X+edeiph20EulTI7sx38jwhq5Yc0Mf2ElvFgToGQ806Vs2HynuLwh9OIuTTZh"]
console.log(priceUpdateData);
```
{% endcode %}

**Post price updates**

Finally, post the price update to the Pyth program on Solana. This step will create the price update account that your application reads from. Applications typically combine posting the price update and invoking their application into a sequence of transactions. The `PythSolanaReceiver` class in `@pythnetwork/pyth-solana-receiver` provides a convenient transaction builder to help with this process:

{% code overflow="wrap" %}
```rust
import { PythSolanaReceiver } from "@pythnetwork/pyth-solana-receiver";
 
// You will need a Connection from @solana/web3.js and an AnchorWallet to create
// the receiver.
const connection: Connection;
const wallet: AnchorWallet;
const pythSolanaReceiver = new PythSolanaReceiver({ connection, wallet });
 
// Set closeUpdateAccounts: true if you want to delete the price update account at
// the end of the transaction to reclaim rent.
const transactionBuilder = pythSolanaReceiver.newTransactionBuilder({
  closeUpdateAccounts: false,
});
await transactionBuilder.addPostPriceUpdates(priceUpdateData);
 
// Use this function to add your application-specific instructions to the builder
await transactionBuilder.addPriceConsumerInstructions(
  async (
    getPriceUpdateAccount: (priceFeedId: string) => PublicKey
  ): Promise<InstructionWithEphemeralSigners[]> => {
    // Generate instructions here that use the price updates posted above.
    // getPriceUpdateAccount(<price feed id>) will give you the account for each price update.
    return [];
  }
);
 
// Send the instructions in the builder in 1 or more transactions.
// The builder will pack the instructions into transactions automatically.
await pythSolanaReceiver.provider.sendAll(
  await transactionBuilder.buildVersionedTransactions({
    computeUnitPriceMicroLamports: 50000,
  }),
  { skipPreflight: true }
);
```
{% endcode %}

The [SDK documentation(opens in a new tab)](https://github.com/pyth-network/pyth-crosschain/tree/main/target_chains/solana/sdk/js/pyth_solana_receiver) contains more information about interacting with the Pyth solana receiver contract, including working examples.
