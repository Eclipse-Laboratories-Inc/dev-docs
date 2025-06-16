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

# Frequently Asked Questions

We've compiled a list of the most commonly asked questions about integrating with Eclipse. If you can't find what you're looking for here, please don't hesitate to reach out via [Discord](https://discord.gg/PVcbxdqj6r).

<details>

<summary>What is the timeline for Eclipse Mainnet?</summary>

Eclipse Mainnet is **live**! Learn about how to get started [here](getting-started.md).

</details>

<details>

<summary>Does Eclipse Mainnet have its own token?</summary>

No. ETH is the native token for Eclipse Mainnet. ETH is used to pay for gas. See [here](differences-between-eclipse-and-solana.md#native-token) for more details.

</details>

<details>

<summary>How expensive will transaction fees be on Eclipse Mainnet?</summary>

It's impossible for any blockchain to guarantee low fees, but median transaction fees on Eclipse Mainnet is in line with or even lower than the cheapest blockchains such as Solana.

</details>

<details>

<summary>Why not just use Solana?</summary>

We think Solana is great! At the same time, the Solana blockchain optimizes for [different goals](https://www.youtube.com/live/YshSwky6nG8?si=sKyYIYX0592dO3nN\&t=790) than the Eclipse L2. In short, Solana is [all about performance](https://x.com/aeyakovenko/status/1713948517277044849?s=20), whereas Eclipse aims to preserve as much of that performance as possible while maximizing verifiability.

The Eclipse ecosystem denominates in ETH, the native currency for the chain which comes via the canonical bridge.

</details>

<details>

<summary>What's a virtual machine (VM)?</summary>

A virtual machine is a piece of software that can run programs. Specifically, the virtual machine executes smart contracts for a blockchain.

</details>

<details>

<summary>What is a rollup? Is Eclipse an optimistic or zero-knowledge rollup?</summary>

For comparison, a Layer 1 blockchain is a blockchain that does not depend on any other chain for security. Layer 1 blockchains require that the majority of voting power is honest. A [_rollup_](https://www.blockchain-gt.io/newsletters/rollups-eclipse) is a type of scaling solution that executes transactions outside of any Layer 1 and later posts the data to a Layer 1 retroactively.\
\
For an _optimistic rollup_, a "sequencer" orders transactions and the resulting state root is posted to a Layer 1 along with a bounty. A "verifier" can re-execute the transactions, and if it disagrees on the result, the verifier can challenge the state root via "settlement." If the verifier is correct, the bounty is awarded to the verifier.\
\
For a _zero-knowledge rollup_, sequencers order transactions, and the resulting state root is posted along with a "validity proof" (evidence) that the transactions were executed correctly. This validity proof must be posted to the settlement layer for a result to be accepted. The validity proof is typically expensive to generate.\
\
Eclipse Mainnet is deploying as an optimistic rollup, but we are working on a zero-knowledge rollup in parallel.

</details>

<details>

<summary>What is a data availability layer?</summary>

This question requires some additional context. A full node in a blockchain network downloads all blocks (transactions) and executes them. A light node doesn't do that, but a _data availability layer_ enables the light node to efficiently verify that blocks are available to all full nodes on the network.\
\
Data availability is important because storing huge amounts of data limits how decentralized and scalable a blockchain can get. It would not be possible to build a decentralized Solana VM rollup without this critical feature. Most chains today don't provide data availability because they aren't designed for rollups. Celestia, Avail, and EigenLayer are all data availability layers, and danksharding will bring data availability sampling to Ethereum in the future.

</details>

<details>

<summary>What is a settlement layer?</summary>

Full nodes re-execute every transaction and determine the current state of the blockchain. What happens when these full nodes disagree? In general, the blockchain will fork.\
\
For a blockchain where the majority of nodes are honest, the "fork choice rule" will dictate that the correct chain is whatever the honest nodes decide. For a rollup that does not make the assumption that the majority of nodes are honest, the majority of actors might be lying. As a blockchain user (light node), how do we determine which is the correct fork?\
\
A _settlement layer_ is a hub to verify proofs and resolve fraud disputes to determine the "correct" chain. The settlement layer also lets you move tokens between the execution chains (a bridge).

</details>

<details>

<summary>Why does Eclipse use the Solana VM?</summary>

The Solana (Sealevel) virtual machine is a highly parallelized runtime that is constantly improving. For EVM blockchains such as Ethereum or Optimism, at any given point there is only a single program running. (This is called "single-threaded.") For the Solana VM, if you have multiple cores, you can run several programs at the exact same time, substantially increasing throughput. Moreover, the execution layer continues to improve:

* Seahorse Lang lets you write Solana VM programs in Python.
* [Soon the Solana VM will support Move bytecode](https://docs.solana.com/proposals/embedding-move)
* [The Solana VM has a best-in-class fee market coming](https://twitter.com/aeyakovenko/status/1537270721570824192?s=20\&t=uOV108no_nYGnkTPnizUMA)

</details>

<details>

<summary>What is the main difference between devnet and testnet for a project deploying on Eclipse?</summary>

The difference is that the testnet includes the initial version of the validating bridge, and it also posts blobs Celestia. This means that you can view these blobs in a [block explorer](https://mocha-4.celenium.io/namespace/0000000000000000000000000000000000000000000065636c74330a?tab=Blobs) and test out the [ETH bridge](https://app.eclipse.xyz/?target=deposit). This is a more realistic experience compared to devnet.

</details>

<details>

<summary>Why did Eclipse choose Celestia DA over Solana’s DA?</summary>

Celestia is purpose-built for data availability, meaning that many convenience features already exist. If we were to use Solana, there is various surrounding infrastructure that we would have to build ourselves (for example, relayers from Solana to Eth L1) or data availability sampling (DAS). Because Celestia already has DAS, users can directly verify the blocks are not being withheld.

</details>

<details>

<summary>Security is a paramount concern. How does Eclipse ensure security and trust of its L2 infrastructure?</summary>

At a high level, we have multiple audits on our code, and our bridge delays withdrawals, meaning that we can identify exploits early and respond as a community. At a more technical level, we provide safety guarantees by posting commitments to the Ethereum canonical bridge, and these commitments can be disputed via "fraud proofs." We provide liveness via a mechanism called "forced inclusion”.

</details>

<details>

<summary>What factors influenced Eclipse’s decision to utilize SVM for tackling scalability and performance issues within the Ethereum ecosystem?</summary>

The Solana Virtual Machine (SVM) is the most battletested parallelized virtual machine on the market. This means that it is able to handle unmatched levels of throughput. Moreover, an innovation called "local fee markets'' means that apps with lots of activity don't spike fees for other apps on the network. This is in contrast to EVM chains where a big NFT drop can congest the entire network.

</details>

<details>

<summary>Have we undergone any third-party audits?</summary>

Yes, we have completed audits with Zellic & OtterSec and Halborn is completing a second audit.

</details>
