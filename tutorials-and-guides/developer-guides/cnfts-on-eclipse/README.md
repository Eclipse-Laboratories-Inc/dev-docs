# cNFTs on Eclipse

### Overview <a href="#summary" id="summary"></a>

Compressed NFTs (cNFTs) on Eclipse utilize **State Compression** to drastically reduce the cost and storage requirements of minting and managing NFTs. This process involves hashing NFT metadata and storing the hash on-chain in an account backed by a **concurrent Merkle tree**. While the hash doesn’t reveal the NFT data directly, it can verify the accuracy of external data. Supporting RPC providers index this metadata off-chain during minting, allowing developers to access it using a **Read API**.

To simplify the process, abstraction layers (similar to Metaplex Bubblegum) can sit atop State Compression, enabling easier creation, minting, and management of cNFT collections.

### Lesson  <a href="#lesson" id="lesson"></a>

Compressed NFTs (cNFTs) are exactly what their name suggests: NFTs whose structure takes up less account storage than traditional NFTs. Compressed NFTs leverage a concept called **State Compression** to store data in a way that drastically reduces costs.

Eclipse's transaction costs are so cheap that most users never think about how expensive minting NFTs can be at scale. The cost to set up and mint 1 million traditional NFTs using the Token Metadata Program is approximately 24,000 SOL. By comparison, cNFTs can be structured to where the same setup and mint costs 10 SOL or less. That means anyone using NFTs at scale could cut costs by more than 1000x by using cNFTs over traditional NFTs.

However, cNFTs can be tricky to work with. Eventually, the tooling required to work with them will be sufficiently abstracted from the underlying technology that the developer experience between traditional NFTs and cNFTs will be negligible. But for now, you'll still need to understand the low level puzzle pieces, so let's dig in!\
\
[Source](https://solana.com/developers/courses/state-compression/compressed-nfts#summary)
