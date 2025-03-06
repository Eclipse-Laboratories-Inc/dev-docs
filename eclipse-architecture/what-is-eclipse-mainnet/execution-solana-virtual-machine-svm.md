# Execution - Solana Virtual Machine (SVM)

## Virtual Machine (VM) Basics

At the most basic level, a VM in the blockchain context is the software that executes machine instructions to process transactions. When a transaction reaches a blockchain, it is processed by the virtual machine. Every language, whether Solidity, Rust, or Move, is ultimately converted (compiled) to bytecode which is executed by a blockchain’s virtual machine.

Different languages support compilation to different bytecodes. For example, Solidity is typically compiled to Ethereum Virtual Machine (EVM) bytecode, but the [Solang compiler](https://www.quicknode.com/guides/solana-development/solidity/solang-get-started) enables you to compile Solidity to other bytecodes such as WebAssembly (wasm) or Berkeley Packet Filter (BPF).

### Comparing Virtual Machines

There are many virtual machines. How should you decide which is best for you?

* **Performance**: Some virtual machines are designed to enable greater parallelism across transactions, such as the Solana Virtual Machine (SVM) which Eclipse uses. Others such as the EVM typically have been single-threaded.
* **Security**: Some languages such as Rust can more easily protect against many bugs that Solidity does not. For example, Ethereum smart contracts are vulnerable to so-called reentrancy attacks.
* **Community**: Popular blockchains like Ethereum and Solana have fostered thriving developer communities around the EVM and SVM respectively. This means better tooling and developer support compared to newer virtual machines like the Move VM or Fuel VM.
* **Ease-of-use**: Languages such as Solidity are easier to code in, and not all bytecodes support compilation from Solidity.&#x20;

Eclipse Mainnet runs the **Solana Virtual Machine (SVM)**. You can even use existing tooling for the SVM such as the Solana CLI or Seahorse Lang.

## **Optimized Parallel Execution**&#x20;

[The SVM](https://squads.so/blog/solana-svm-sealevel-virtual-machine) and its Sealevel runtime famously enable parallel transaction execution. Transactions which don’t touch overlapping state can be executed in parallel rather than sequentially.

This allows the SVM to directly scale with hardware as processors continue to add more cores at lower cost. [Single-threaded runtimes (such as today’s EVM) fundamentally do not benefit from reducing the cost per core](https://x.com/aeyakovenko/status/1686916794962079744?s=20). For over a decade now, single-threaded performance speedups have been continually diminishing. Nearly all improvements continue to come from increasing the number of cores, so it’s critical to take advantage of [this trend](https://github.com/karlrupp/microprocessor-trend-data) [by parallelizing workloads](https://www.karlrupp.net/2018/02/42-years-of-microprocessor-trend-data/):

<figure><img src="https://lh3.googleusercontent.com/2SOSpeG_ceWMt-DtuSZvnviiyKc5p8a6wQuHHjQgHaCkgvCiJ40wqU2k4UNV2PpCc-t35d9fn5xpLCXB7hefEGJY8ehRoLIhtTxRtxBQSehKytDenh5iS6Cx_D0O4W0uF_ag9_4GbyUI4tm1lGFAmH4" alt=""><figcaption></figcaption></figure>

There are some very early unproven attempts to [parallelize the EVM](https://writings.flashbots.net/speeding-up-evm-part-1), but adding this on while maintaining compatibility brings fundamental tradeoffs including suboptimal performance without addressing other bottlenecks (e.g., state growth). Contracts declaring state dependencies upfront (as in the SVM) allows for optimal parallelization.

## **Local Fee Markets**&#x20;

Most fee markets today are global, meaning that one hot application increases fees for all users of the chain. [One NFT mint shouldn't render the chain useless for everything else](https://x.com/yugalabs/status/1520612362986078208?s=20). Solana's amazing work on [local fee markets](https://twitter.com/solana/status/1615571640372580352?lang=en) [solves](https://x.com/time_composer/status/1649776724777762817?s=20) this cross-app state contention. In its current implementation, the scheduler prioritizes transactions without conflicts, allowing conflict-free transactions to go through with lower fees. Longer term, local fee markets will be implemented at a protocol level. This ensures that fee spikes for a single app don't impact the rest of the chain.&#x20;

<figure><img src="../../.gitbook/assets/Screenshot 2023-09-21 at 8.29.51 PM.png" alt=""><figcaption></figcaption></figure>

Local fee markets are possible thanks to Solana’s uniquely parallelized runtime. Trying to implement local fee markets for state hotspots in the EVM using heuristics (i.e., without declaring state access upfront) would present inefficiencies and likely attack vectors.

There's also [early research underway](https://github.com/solana-foundation/solana-improvement-documents/pull/16) that would allow applications to easily internalize the local value attributable to them, which today generally requires [more creative app-level design](https://blog.uniswap.org/uniswap-v4).

## State Growth Management&#x20;

Before the EVM even bumps up against sequential execution as a bottleneck, state growth is its far more pressing bottleneck.

Because there is no global Merkle tree for state, Solana doesn't incur the overhead of updating a Merkle tree for each state update. Instead, after each epoch (\~2.5 days), the entire state is merklized. This is much [cheaper than real-time merklization](https://x.com/toghrulmaharram/status/1699466187166335176?s=20) (as in the EVM).

More importantly, the EVM has [dynamic account access](https://x.com/aeyakovenko/status/1699465524231692384?s=20) (i.e., transactions can touch any state on-demand). That dynamic state lookup means that state cannot be loaded into memory prior to execution. In the SVM, each transaction specifies all the state that’s needed for execution.

As a result, [state size doesn't impact SVM execution](https://x.com/aeyakovenko/status/1699460999697555512?s=20). The network could safely double the snapshot size every 2 years without running into major issues assuming validators upgrade their storage disks every 2 years.

Furthermore, teams like [Helius](https://www.helius.dev/) are actively improving the accessibility of historical data and reducing state size with [compression](https://docs.helius.dev/compression-and-das-api/what-is-compression-on-solana).

## MetaMask Snaps&#x20;

Onboarding EVM users to non-EVM chains has historically been a major hurdle, but the recently unveiled [Metamask Snaps](https://www.coindesk.com/tech/2023/09/12/ethereum-developer-consensys-unveils-snaps-add-ons-for-metamask-browser/) are set to break down that barrier. EVM users can continue to use [MetaMask](https://snaps.metamask.io/snap/npm/solflare-wallet/solana-snap/) without needing to switch wallets. The UX is comparable to interacting with any EVM chain, thanks to [Drift](https://www.drift.trade/)'s open-source contributions building out a great MetaMask Snap implementation. Eclipse Mainnet users will be able to interact with apps natively in MetaMask or use a Solana-native wallet like [Salmon](https://salmonwallet.io/).

[Here's a sneak peek of the UX from Drift](https://x.com/DriftProtocol/status/1700135022454276414?s=20):

<figure><img src="https://lh4.googleusercontent.com/RJ7_S3i2IBlCuv0zCB8BNd0c22MsQF9VeEm744117kw_f40c65DRL5MkVVQoNLX_IkfU-UOHVagTdIQxpxSEDnaS4eJbIzyR4g-WtvK1rGbCGiUxqOsvrfYGJehLNvQwPcGISuZuDVck7i28Lb5SO4A" alt=""><figcaption></figcaption></figure>

## Firedancer&#x20;

[Firedancer](https://jumpcrypto.com/firedancer/) is the highly anticipated Solana client being developed by Jump to drastically increase the network's throughput, resilience, and efficiency. At launch we will stick as closely as possible to the Solana core client, but we plan to adopt Firedancer once the code is live and stable.&#x20;

## Safety&#x20;

Solana's runtime has a greatly reduced attack surface area which prevents the [infamous reentrancy exploits](https://blog.chain.link/reentrancy-attacks-and-the-dao-hack/) we’ve seen far too often. Specifically, the Solana runtime only allows programs to self-recurse, rather than allowing arbitrary reentrant cross program invocations. Moreover, separating state and code results in stateless code, which is typically easier to test effectively.

## Easier Proving&#x20;

The SVM is register-based and has a much smaller instruction set vs. the EVM, making SVM execution easier to prove in ZK. For optimistic rollups, the register-based design allows for easier checkpointing.\
