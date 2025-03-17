---
description: >-
  For the most part, developing and interacting with dApps on Eclipse is the
  same as Solana. However, there are some minor differences.
---

# Differences Between Eclipse and Solana

### Native Token

The native token on Eclipse is ETH. It uses 9 decimal places, unlike Ethereum. You can get this token by bridging over ETH from mainnet Ethereum or from bridging over SOL or USDC from Solana. See our guide on how to do this [here](../users/readme/2.-bridge-assets-for-gas-and-transactions.md).

Similar to Solana, there are also wrapped versions of the native token for both the SPL Token and SPL Token 2022, both with 9 decimals as well. You can read more about the reasoning behind this [here](https://www.quicknode.com/guides/solana-development/getting-started/a-complete-guide-to-wrapped-sol#what-is-wrapped-sol).

The addresses for these wrapped native tokens are the same as Solana:

SPL Token: `So11111111111111111111111111111111111111112`

SPL Token 2022: `9pan9bMn5HatX4EJdBwg9VgCa7Uz5HL8N1m5D3NdXejP`

### Priority Fees

Priority fees are paid using our native token ETH but [use the same logic as Solana](https://solana.com/developers/guides/advanced/how-to-use-priority-fees). When calculating them with libraries like `@solana/web3.js`, it will still say micro-lamports since the libraries are made for Solana. However, it still uses your ETH balance and calculates it using the same precision micro-lamports have relative to SOL, i.e. 10^-15 ETH or 1 Kwei (1 micro-lamport == 10^-6 lamports == 10^-15 SOL).

We reccomend using `getRecentPriorityFees` to ensure you pay the lowest possible fee instead of using any default value due to the difference in USD pricing of 1 micro lamport vs 1 kwei.

### Wallets

You can find a complete list of Eclipse compatible wallets [here](https://docs.eclipse.xyz/developers/wallet).

### dApps

Building dApps and deploying programs to Eclipse is the same process as with Solana. Just ensure any hardcoded program addresses exist on Eclipse. Please check [this guide](../tutorials-and-guides/developer-guides/modifying-a-solana-dapp-to-support-eclipse-chomping-glass/) out for an example.
