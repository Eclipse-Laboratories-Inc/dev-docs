---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Developer Tooling

{% hint style="info" %}
Submit your deployment address to this [form](https://forms.gle/yJfFABQDPmpvgzAf7). We will provide engineering support and prioritize specific infrastructure needs.
{% endhint %}

Eclipse Mainnet works with all the standard Solana tooling. This guide provides a comprehensive list of tools and resources available for developers working with Eclipse Mainnet.

## Programming Languages

* [Rust](https://www.rust-lang.org/): Solana smart contracts, known as "programs," are primarily written in Rust due to its focus on performance, safety, and concurrency.
* [Python](https://seahorse-lang.org): Seahorse lets you write Solana programs in Python. Developers gain Python's ease-of-use, while still having the same safety guarantees of every Rust program on the SVM chain.

## Integrated Development Environments (IDEs) and Editors

* [Visual Studio Code](https://code.visualstudio.com/): A popular open-source code editor with extensions for Rust development, such as the [Rust Analyzer](https://marketplace.visualstudio.com/items?itemName=matklad.rust-analyzer) or the [official Rust extension](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust).

## Frameworks and Libraries

* [Anchor](https://github.com/project-serum/anchor): A popular Rust framework for Solana that simplifies the development and testing of smart contracts by providing intuitive API and language constructs.
* [Solana Program Library (SPL)](https://spl.solana.com/): A collection of on-chain programs (smart contracts) and off-chain client libraries that serve as building blocks for dApp development on Solana.

## Testnets and Devnets

* [Eclipse Testnet](../rpc-and-block-explorers/): This is a network which should not support real economic value.
* [Eclipse Devnet](../rpc-and-block-explorers/): This network simulates the developer experience on Eclipse.
* [Solana Testnet](https://docs.solana.com/clusters): This is the official Solana L1 devnet and testnet.

## Testing and Debugging

* [Solana's built-in Rust testing](https://docs.solana.com/developing/on-chain-programs/testing): Solana supports Rust's built-in testing framework, enabling developers to write unit tests for their smart contracts.
* [Solana Explorer](https://explorer.solana.com/): A block explorer for the a network, which allows developers to monitor transactions, accounts, and program execution on the testnets and mainnet.

## Wallets and dApp Interaction

* [Eclipse Wallet](https://github.com/Eclipse-Laboratories-Inc/eclipse-wallet): This is a fork of the open-source Salmon wallet which is compatible with Eclipse.
* [Backpack](https://github.com/coral-xyz/backpack): Next-generation wallet for Solana and Ethereum chains, currently in beta.
* [MetaMask Snaps](https://www.drift.trade/updates/connect-with-metamask): Drift has developed a MetaMask Snap compatible with Eclipse.
* [Solflare](https://solflare.com/): A widely-used browser extension for SVM that allows users to interact with dApps, manage NFTs, and swap tokens.

## RPC Providers

* [Helius](https://www.helius.dev/): Solana RPCs, APIs, webhooks and infrastructure to build and ship crypto apps, fast.
* [Triton One](https://triton.one/): The fastest, most reliable RPC solution for Solana.
* [BlockPI](https://blockpi.io/chain/solana): The most cost-effective and high-performance multi-chain RPC provider.
