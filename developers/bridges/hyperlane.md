# Hyperlane

Hyperlane is now live on Eclipse, connecting it to Ethereum and Solana. This enables Eclipse to interoperate with two of the largest ecosystems in crypto, and it allows users to bridge assets from these two chains into a bustling ecosystem. You can bridge using the Hyperlane Nexus Bridge:

{% embed url="https://www.usenexus.org/" %}
Hyperlane Nexus Bridge
{% endembed %}

Key Takeaways

1. Eclipse is now connected to Ethereum and Solana through Hyperlane. The Hyperlane Eclipse bridge will enable USDC, SOL, and WIF to be bridged between Eclipse, Ethereum, and Solana.
2. More assets will be supported through Hyperlane, including Eclipse’s Unified Restaking Token, [tETH](https://twitter.com/EclipseFND/status/1833143806478471448).
3. Hyperlane will enable Eclipse to build an ecosystem that sits at the intersection of Ethereum and Solana assets.&#x20;

<figure><img src="../../.gitbook/assets/GX2l-QkbQAAqhTy.jfif" alt=""><figcaption></figcaption></figure>

Through Hyperlane, users can now bridge the following assets between the following chains at launch.

1. USDC - Ethereum <> Eclipse
2. SOL - Solana <> Eclipse
3. USDC - Solana <> Eclipse
4. WIF - Solana <> Eclipse.&#x20;

More assets will be supported in the future.

***

## Hyperlane Dev Docs

* You can find the official Hyperlane docs here: [https://docs.hyperlane.xyz/](https://docs.hyperlane.xyz/)
* Warp Routes: [https://github.com/hyperlane-xyz/hyperlane-registry/tree/main/deployments/warp\_routes](https://github.com/hyperlane-xyz/hyperlane-registry/tree/main/deployments/warp_routes)

## Deploy an SVM Warp Route <a href="#mailbox-contract" id="mailbox-contract"></a>

You can deploy a Warp Route for an asset of your choice, between two SVM chains with an existing Hyperlane core deployment. Currently, supported SVM chains are Eclipse and Solana, but you can find an up-to-date list [here](https://github.com/hyperlane-xyz/hyperlane-monorepo/tree/main/rust/sealevel/environments/mainnet3) (all chain directory names with a `core` subdirectory).

### Warp Route Types[​](https://docs.hyperlane.xyz/docs/guides/deploy-svm-warp-route#warp-route-types) <a href="#warp-route-types" id="warp-route-types"></a>

The type of token used determines the Warp Route type, so it's important to understand the different Warp Route contracts available:

* [Native](https://docs.hyperlane.xyz/docs/protocol/warp-routes/warp-routes-types#native-token-warp-routes): Handles the transfer of native gas tokens (e.g. SOL on Solana, ETH on Eclipse).
* [Collateral](https://docs.hyperlane.xyz/docs/protocol/warp-routes/warp-routes-types#collateral-backed-erc20-warp-routes): Handles the transfer of existing [Token-2022](https://spl.solana.com/token-2022) or [Token](https://spl.solana.com/token) tokens (the ERC20 equivalent on SVM).
* [Synthetic](https://docs.hyperlane.xyz/docs/protocol/warp-routes/warp-routes-types#synthetic-erc20-warp-routes): Handles synthetic tokens that are minted and burned as transfers occur through the Warp Route, to represent tokens from their origin chain. The tooling in this guide deploys a new Token-2022 token in this case, whose authority is set to the deployer key.

Here are the common Warp Route setups (you can find more details [here](https://docs.hyperlane.xyz/docs/protocol/warp-routes/warp-routes-example-usage)):

* Native to Synthetic: Lock Native tokens on the origin chain to mint Synthetic ones on the destination. When transferring back, the Synthetic is burned. An example of this is a SOL Warp Route between Solana and Eclipse.
* Collateral to Synthetic: Lock Collateral tokens on the origin chain to mint Synthetic ones on the destination. When transferring back, the Synthetic is burned. An example of this is a USDC Warp Route between Solana and Eclipse.
* Other: Native to Native (such as ETH between Optimism and Arbitrum), as well as Collateral to Collateral, are also possible if the token already exists on both origin and destination chains. Rebalancing liquidity is an important consideration in this case.

### Before You Start[​](https://docs.hyperlane.xyz/docs/guides/deploy-svm-warp-route#before-you-start) <a href="#before-you-start" id="before-you-start"></a>

Deploying a Warp Route requires there to be a core Hyperlane deployment that is connected (i.e. actively relayed and secured) to the rest of the Hyperlane ecosystem. The core Hyperlane deployments used in this guide are Solana ([core artifacts](https://github.com/hyperlane-xyz/hyperlane-monorepo/blob/main/rust/sealevel/environments/mainnet3/solanamainnet/core/program-ids.json)) and Eclipse ([core artifacts](https://github.com/hyperlane-xyz/hyperlane-monorepo/blob/main/rust/sealevel/environments/mainnet3/eclipsemainnet/core/program-ids.json)). You may need to refer to these core artifacts throughout the guide.

### Deploy a Sealevel Warp Route[​](https://docs.hyperlane.xyz/docs/guides/deploy-svm-warp-route#deploy-a-sealevel-warp-route) <a href="#deploy-a-sealevel-warp-route" id="deploy-a-sealevel-warp-route"></a>

1.  Install `solana-cli 1.14.20` to build the Warp Route programs. Note that you **must** use this version, otherwise deployment may fail.\


    ```
    sh -c "$(curl -sSfL https://release.solana.com/v1.14.20/install)"
    ```
2.  Build the Warp Route programs on your machine

    * Clone [hyperlane-monorepo](https://github.com/hyperlane-xyz/hyperlane-monorepo)
    * Go to `./hyperlane-monorepo/rust/sealevel/programs/`\


    ```
    # starting in rust/sealevel/programs/
    cd hyperlane-sealevel-token
    cargo build-sbf
    cd ../hyperlane-sealevel-token-collateral
    cargo build-sbf
    cd ../hyperlane-sealevel-token-native
    cargo build-sbf
    ```
3.  To deploy the contracts, install `solana-cli 1.18.18`. Note that you **must** use this version, otherwise deployment may fail.\
    \


    <pre><code><strong>sh -c "$(curl -sSfL https://release.solana.com/v1.18.18/install)"
    </strong></code></pre>
4. In the monorepo, in `rust/sealevel/environments/mainnet3/warp-routes`, create a new directory with the name you want your Warp Route deployment to have. For example, the existing SOL Warp Route between Solana and Eclipse lives in `rust/sealevel/environments/mainnet3/warp-routes/eclipsesol`.\

5. If your warp route creates a synthetic token, you can open a PR to the `hyperlane-registry` with metadata to associate with this token (example PR [here](https://github.com/hyperlane-xyz/hyperlane-registry/pull/142)). The `hyperlane-registry` also gives your Warp Route visibility within the Hyperlane ecosystem.\

6. Configure the parameters of your Warp Route in a JSON file named `token-config.json`, based on the `serde_json` serialization of the [TokenConfig](https://github.com/hyperlane-xyz/hyperlane-monorepo/blob/a5afd20f3ae69ccb3289d845d44b99dbdcde2c62/rust/sealevel/client/src/warp_route.rs#L114) Rust struct. The value to set for the `interchainGasPaymaster`, can be found in the [core deployment artifacts](https://docs.hyperlane.xyz/docs/guides/deploy-svm-warp-route#before-you-start).\

   *   The example below shows a testnet Native to Synthetic Warp Route that transfers SOL from Solana and mints synthetic SOL on Eclipse. You can also check [this configuration](https://github.com/hyperlane-xyz/hyperlane-monorepo/blob/a5afd20f3ae69ccb3289d845d44b99dbdcde2c62/rust/sealevel/environments/mainnet3/warp-routes/eclipsesol/token-config.json) of a production SOL Warp Route.

       ```
       {
       "solanatestnet": {
           "type": "native",
           "decimals": 9,
           "interchainGasPaymaster": "<from core program addresses, choose the overhead igp>"
       },
       "eclipsetestnet": {
           "type": "synthetic",
           "decimals": 9,
           "name": "Solana (testnet)",
           "symbol": "SOL",
           "uri": "<permalink to the metadata.json file you merged into hyperlane-registry>"
           "interchainGasPaymaster": "<from core program addresses, choose the overhead igp>"
       }
       }
       ```
7.  Create a Solana private key file. This key pays for the deployment and will be the owner of the deployed programs. An existing funded key can be used if you'd like.

    ```
    solana-keygen new --outfile ./warp-route-deployer-key.json
    ```
8. Fund the new key on both networks the Warp Route is being deployed to. The public key should be the same across SVM networks, but do double check with the wallets recommended by each chain, by loading the private key into them.
   * The funding should be enough to cover rent for all accounts related to the Warp Route, pay for transaction fees, and fund the [ATA](https://www.alchemy.com/overviews/associated-token-account) payer accounts (more on this below). For reference, the observed rent from one Hyperlane Warp Route account is `2.35 SOL` on Solana and `0.025 ETH` on Eclipse, so it's a good idea to fund the key with at least `5 SOL` / `0.05 ETH`.
   *   To read the public key you just created:

       ```
       solana-keygen pubkey ./warp-route-deployer-key.json
       ```
9.  Deploy the warp route with `warp-route deploy`

    info

    Note that since our goal was to make this tooling accessible to developers as soon as possible, it's not as reliable as we would hope. Please get in touch through a [GitHub issue](https://github.com/hyperlane-xyz/hyperlane-monorepo/issues) or via the `developers` channel on [Discord](https://discord.gg/2BYk6kV7) if you run into issues.

    * Overview of CLI flags:
      * `--warp-route-name` - should match the directory name picked for the Warp Route earlier
      * `--environment` - keep as `mainnet3`
      * `--environments-dir ../environments` - keep as `../environments`
      * `--built-so-dir` - keep as `../../target/deploy`, as it points to the compilation output directory of Warp Route programs
      * `--token-config-file` - point this to the `token-config.json` file created earlier
      * `--chain-config-file` - keep as `../environments/mainnet3/chain-config.json`, as this file has been pre-populated with chain settings for all Hyperlane-supported chains
      * `--ata-payer-funding-amount` - this flag specifies by how much to fund the Warp Route [ATA](https://www.alchemy.com/overviews/associated-token-account) payer accounts on both chains the deployment happens on. It's expressed in the lowest currency denomination, which means that it's interpreted as Lamports on Solana and Gwei on Eclipse (since it uses ETH as its native currency). In the command below, the value `10000000` works out to `0.001` ETH and `0.001` SOL, which is enough for an initial deployment. ATA payers can always be topped up later, so it’s fine to pick a small value. For reference, every Warp Route transfer costs the ATA payer `0.000000001 SOL` (on Solana) and `0.000021 ETH` (on Eclipse) on the destination chain.
    * The script is unlikely to work from the first try due to network congestion and program size, but the script should be idempotent and skip contracts that were already deployed / initialized. Errors like `Error: 11 write transactions failed` or `Error: Custom: Invalid blockhash` can always be retried by re-running the command. If retriable errors persist, consider increasing the compute unit price [here](https://github.com/hyperlane-xyz/hyperlane-monorepo/blob/44e0ff0733baf0da4d2b0304915f5f6cce92ffc7/rust/sealevel/client/src/cmd_utils.rs#L76).
      *   For other error types, you may need to close the buffers and programs of your deployer key and redeploy everything from scratch. To display buffers and programs and close them one by one, follow the commands below. Closing programs also helps recover their rent deposit.

          ```
          solana program show --programs --keypair ./warp-route-deployer-key.json --url <CHAIN_RPC_URL>

          solana program show --buffers --keypair ./warp-route-deployer-key.json --url <CHAIN_RPC_URL>

          # You'll need to add the `--bypass-warning` flag when closing program accounts (as opposed to closing buffers)
          solana program close <YOUR_PROGRAM_ADDRESS> --url <CHAIN_RPC_URL>
          ```
    * To increase the odds of the deployment succeeding faster, you can set a private RPC url in the `--chain-config-file` passed to the script. (e.g. in `solanamainnet.rpcUrls.http`)
    * If deploying a synthetic, the command below will create a new token mint and use the metadata token extension to set the token name, symbol, and metadata json using the fields in the `--token-config-file` file
    *   Run `warp-route deploy`

        ```
        # run from `rust/sealevel/client`
        cargo run -- -k ./warp-route-deployer-key.json warp-route deploy --warp-route-name eclipsesol --environment mainnet3 --environments-dir ../environments --built-so-dir ../../target/deploy --token-config-file ../environments/mainnet3/warp-routes/eclipsesol/token-config.json  --chain-config-file ../environments/mainnet3/chain-config.json --ata-payer-funding-amount 10000000
        ```

### Interacting with the Warp Route[​](https://docs.hyperlane.xyz/docs/guides/deploy-svm-warp-route#interacting-with-the-warp-route) <a href="#interacting-with-the-warp-route" id="interacting-with-the-warp-route"></a>

1.  Let’s query one of the Warp Route programs, getting the program ID from the auto-generated `program-ids.json` in the directory you created above (where `token-config.json` also lives). This command prints the Mint Account, Mint Authority, and ATA payer account.

    ```
    # run from `rust/sealevel/client`
    cargo run -- -k ./warp-route-deployer-key.json -u <CHAIN_RPC_URL> token query --program-id <base58 address from program-ids.json> synthetic
    ```

    *   if deploying a synthetic token, query the Mint Authority account to check out the metadata

        ```
        solana account <MINT_AUTHORITY> --url <CHAIN_RPC_URL>
        ```
2.  Try transferring tokens!

    * You'll need the domain ID of the chain you're sending to, which you can find in the chain's `metadata.yaml` entry from the [hyperlane-registry](https://github.com/hyperlane-xyz/hyperlane-registry/tree/main/chains).

    ```
    # run from `rust/sealevel/client`
    cargo run -- -u <ORIGIN_CHAIN_RPC_URL> -k ./warp-route-deployer-key.json token transfer-remote ./warp-route-deployer-key.json <AMOUNT_IN_LOWEST_DENOM> <DESTINATION_CHAIN_DOMAIN_ID> <RECIPIENT_ADDRESS> <WARP_TOKEN_TYPE_ON_ORIGIN_CHAIN: native|synthetic|collateral> --program-id <origin chain base58 address from program-ids.json>
    ```
3.  Look for the balance of the recipient on the destination chain, by querying the Mint Account address

    ```
    spl-token balance --owner ./warp-route-deployer-key.json -u <DESTINATION_CHAIN_RPC_URL> <MINT_ACCOUNT_ADDRESS>
    ```

    * The final parameter here is the SPL token ID. So if this is a synthetic warp route you want to check the balance of, you need to use the Mint address from a prior query you made a few steps ago.
    * You can also check out the last tx made to the recipient account in the explorer
4.  This guide has made heavy use of the `hyperlane-sealevel-client` CLI from `hyperlane-monorepo`. You may find its various commands useful for configuring the Warp Route, making state queries, sending transfers, and more. Check out the other utilities it provides, in particular those under the `token` subcommand.

    ```
    # run from `rust/sealevel/client`
    cargo run -- --help
    ```

## Mailbox Contract[​](https://icarus131.github.io/devcookbook/docs/Hyperlane#mailbox-contract) <a href="#mailbox-contract" id="mailbox-contract"></a>

Eclipse and [Hyperlane](https://www.hyperlane.xyz/) partnered to bring Hyperlane's Permissionless Interoperability solution to Solana Virtual Machine (SVM) based blockchains. The Eclipse team worked with Hyperlane to deploy their mailbox contracts for the SVM.

The mailbox contract facilitates interchain operations. To develop smart contracts for the bridge, you must interact with this contract. If you would like to test a user interface against the active mailbox deployment.

Assuming you have already set up Rust, Solana CLI, and switched to the Eclipse Devnet using our RPC, let's proceed to writing the smart contract.

### Interacting with the mailbox contract[​](https://icarus131.github.io/devcookbook/docs/Hyperlane#interacting-with-the-mailbox-contract) <a href="#interacting-with-the-mailbox-contract" id="interacting-with-the-mailbox-contract"></a>

For Hyperlane to deliver a message to our smart contract, we need to implement a handle function. This function will be called by the mailbox.

## Writing the Smart Contract[​](https://icarus131.github.io/devcookbook/docs/Hyperlane#writing-the-smart-contract) <a href="#writing-the-smart-contract" id="writing-the-smart-contract"></a>

### **Parsing Instruction Data**[**​**](https://icarus131.github.io/devcookbook/docs/Hyperlane#parsing-instruction-data)

Send the instruction data and set up the send message function:

```rust
fn process_instruction(
    program_id: &Pubkey,
    accounts: &[AccountInfo],
    instruction_data: &[u8],
) -> ProgramResult {
    match instruction_data.get(0) {
        Some(&0) => {
            let accounts_iter = &mut accounts.iter();
            let sender_account = next_account_info(accounts_iter)?;
            let recipient_account = next_account_info(accounts_iter)?;
            let message_data = &instruction_data[1..];
```

Now, call the send message function:

```rust
            mailbox::instruction::send_message(
                program_id,
                sender_account,
                recipient_account,
                message_data,
            )?;
        }
```

Here you can call the receive message function. This goes inside the main match block:

```rust
    Some(&1) => {
      let accounts_iter = &mut accounts.iter();
      let recipient_account = next_account_info(accounts_iter)?;
      mailbox::instruction::receive_message(program_id, recipient_account)?;
    }
    _ => return Err(solana_program::program_error::ProgramError::InvalidInstructionData),
```

### Hyperlane CLI[​](https://icarus131.github.io/devcookbook/docs/Hyperlane#hyperlane-cli) <a href="#hyperlane-cli" id="hyperlane-cli"></a>

You can use the Hyperlane CLI to get a better understanding of how the deployments work and how to interact with them. The CLI can also be used to test sending and receiving messages.
