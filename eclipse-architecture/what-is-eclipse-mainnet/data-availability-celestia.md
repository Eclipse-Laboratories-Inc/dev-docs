# Data Availability - Celestia

For background, [data availabilty](https://celestia.org/glossary/data-availability/) can be defined as the following:

Data availability answers the question, has this data been published? Specifically, a node will verify data availability when it receives a new block that is getting added to the chain. The node will attempt to download all the transaction data for the new block to verify availability. If the node can download all the transaction data, then it successfully verified data availability, proving that the block data was actually published to the network.

### Eclipse Mainnet Will Use Celestia for Data Availability

Eclipse Mainnet's target throughput and fees are unfortunately not supported by Ethereum's current bandwidth. This will remain so even after [EIP-4844](https://eips.ethereum.org/EIPS/eip-4844) (a.k.a. "Proto-danksharding"), which provides an average of \~0.375 MB [blobspace](https://domothy.com/blobspace/) per block (with a limit of \~0.75 MB per block).&#x20;

* For ERC-20 transfers with basic compression ([\~154 bytes per transaction](https://twitter.com/vitalikbuterin/status/1554983955182809088)), this translates to \~213 TPS across all rollups combined.&#x20;
* For swaps with compression [(\~400 bytes per transaction)](https://x.com/gluk64/status/1693716324042621241?s=20), this translates to \~82 TPS across all rollups combined.

In comparison, Celestia will launch with 2 MB blocks later this year. Blobspace is expected to increase to 8 MB shortly after launch, once enough [data availability sampling (DAS)](https://celestia.org/glossary/data-availability-sampling/) light nodes come online and the network proves stable. DAS light nodes serve two critical functions:

* Enable users to verify for themselves that Eclipse block data has been made available&#x20;
* Contribute to securely scaling the entire network, as DA layers can safely increase their throughput as more DAS light nodes come online

Celestia is expected to be the first DA layer to launch with DAS in production. This contrasts with traditional Data Availability Committees (DACs), which reintroduce committee [honesty assumptions without user verification](https://x.com/donnoh_eth/status/1699911540051173858?s=20) (akin to existing monolithic blockchains).

There's an [inherent security assumption](https://x.com/sreeramkannan/status/1517420373163323392?s=20) for users who bridge their funds from Ethereum Mainnet to any chain that uses offchain DA. In particular, it is technically possible for Celestia validators to withhold transaction data but claim to the Ethereum bridge that the data is available. In practice, Celestia's proof-of-stake consensus means data withholding on Celestia itself is slashable, making this risk unrealistic in our view.

Overall, Celestia's DAS light node support from day one, crypto-economic security properties, and highly scalable DA throughput make it the clear choice for Eclipse Mainnet today.

Note that [some view onchain Ethereum DA as a requirement to be a true "L2" here](https://x.com/dankrad/status/1689634128101310464?s=20) for the reasons described above. We're going by the more common L2 terminology cited earlier, and we want to be clear on the security considerations.&#x20;

We also intend to monitor Ethereum's progress on DA scaling after EIP-4844. [Exciting new research continues to come out](https://x.com/dannyryan/status/1698733448175727072?s=20), potentially offering high throughput DA sooner than previous ideas (which use more advanced distributed hash tables). If Ethereum offers greater scale for Eclipse to the benefit of our users, we would assess the possibility of migrating to Ethereum DA.
