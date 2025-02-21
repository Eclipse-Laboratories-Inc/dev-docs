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

# Quick Start: User Guide - Testnet

How to get started:

1. Get Sepolia ETH from a faucet.
2. Set up your Eclipse wallet.
3. Bridge your Sepolia ETH to Eclipse.

Optional:

4. View your transactions in the Eclipse Block Explorer.

## 1. **Getting Sepolia ETH Tokens from a Faucet**

As a user, you can use Sepolia ETH to explore the Eclipse testnet and dApps deployed on the network. Sepolia ETH is not meant to be traded, and is only used to test applications. Sepolia ETH can be claimed from a number of faucets: Alchemy, QuickNode, and Infura.&#x20;

Here are instructions on how to claim Sepolia ETH on [Alchemy Sepolia ETH Faucet](https://sepoliafaucet.com/).&#x20;

1. Create an Alchemy account to request Sepolia ETH.
2. Visit the Alchemy Sepolia faucet and log in with your Alchemy account.
3. Enter your wallet in the provided box, complete the CAPTCHA verification, and click "Send Me ETH".

{% hint style="warning" %}
To prevent bots and abuse, the Alchemy Sepolia ETH faucet requires a minimum mainnet balance of 0.001 ETH in the wallet address being used.
{% endhint %}

## 2. Setting Up Your Eclipse Wallet

Follow the linked instructions below to set up your Eclipse wallet:

{% content-ref url="../../developers/wallet/" %}
[wallet](../../developers/wallet/)
{% endcontent-ref %}

## 3. Bridging ETH to Eclipse

Follow the linked instructions below to deposit Sepolia ETH to Eclipse Testnet:

{% content-ref url="../../developers/bridges/" %}
[bridges](../../developers/bridges/)
{% endcontent-ref %}

## 4. Viewing Your Transactions in the Eclipse Block Explorer

You can view your transactions with any block explorer compatible with Eclipse. The Modular Cloud team previously built CelestiaScan, and they are supporting Eclipse Mainnet with a dedicated block explorer: [Eclipse Explorer](https://explorer.modular.cloud/eclipse-testnet).

The Modular Cloud block explorer will eventually include visibility into Celestia DA blobs and the Ethereum validating bridge.

There are other block explorers which also support Eclipse: [RPC & Block Explorers](../../developers/rpc-and-block-explorers/)

***

## FAQs

### What is Eclipse Testnet?

Eclipse Testnet is a realistic simulation of the rollup client that will run for Eclipse Mainnet. This includes posting blobs to Celestia, a canonical bridge contract on the Ethereum Sepolia testnet, and all related relayers. The tooling and user experience of the Eclipse Testnet will reflect Eclipse Mainnet Beta, except we have not deployed frontends for certain functionality such as the canonical bridge. This means that developers and power users will have to follow instructions in these docs to interact with the Eclipse Testnet.

### How does Eclipse Testnet differ from Eclipse Devnet?

Eclipse Devnet is a Solana VM environment that mimics the developer experience in deploying SVM programs. It is a single node Solana cluster, but does not include surrounding infrastructure.

### How does Eclipse Testnet differ from Eclipse Mainnet?

The Eclipse Testnet differs from Eclipse Mainnet Beta because Testnet must not be used to support real economic value. Eclipse Testnet is still undergoing multiple audits and a bug bounty program. It should not be used in production under any circumstances. It is intended to give developers a realistic environment to try out code they intend to migrate to Mainnet.

Read more on Eclipse Mainnet below:

{% content-ref url="../../eclipse-architecture/what-is-eclipse-mainnet/" %}
[what-is-eclipse-mainnet](../../eclipse-architecture/what-is-eclipse-mainnet/)
{% endcontent-ref %}
