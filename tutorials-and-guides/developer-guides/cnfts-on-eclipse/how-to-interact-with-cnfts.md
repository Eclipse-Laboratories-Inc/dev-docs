# How to Interact with cNFTs

### Overview <a href="#overview" id="overview"></a>

This guide details the specific requirements for interacting with compressed NFT (cNFT) assets using JavaScript on Eclipse's devnet and mainnet. For a more comprehensive overview of creating cNFTs, see the Create 1,000,000 NFTs on Eclipse with Bubblegum guide.

#### Required Package <a href="#required-package" id="required-package"></a>

This guide makes use of a specific beta npm package for `@metaplex-foundation/mpl-bubblegum`. Install using:

```
npm -i @metaplex-foundation/mpl-bubblegum@4.3.1-beta.0
```

#### Connecting to Eclipse <a href="#connecting-to-the-svm" id="connecting-to-the-svm"></a>

Note you will need to create your umi instance using the endpoint.

```
import { createUmi } from "@metaplex-foundation/umi-bundle-defaults";

const umi = createUmi('<RPC endpoint for the Eclipse devnet/mainnet>')
  .use(mplBubblegum())
  .use(mplTokenMetadata())
  ...
```

[List of RPCs for Eclipse devnet/mainnet](../../../developers/rpc-and-block-explorers/)&#x20;

#### Creating a Tree <a href="#creating-a-tree" id="creating-a-tree"></a>

{% hint style="warning" %}
#### Tree Cost

We are creating a Merkle Tree that with a real up-front SOL cost that will vary depending on the tree size and the specific network you are using. Until you are ready, please try this example on devnet only, as Merkle Trees can not be closed or refunded.
{% endhint %}

Creating a tree can be done using the same `createTree` function that is used on Eclipse devnet/mainnet. However, we must override the default `logWrapper` and `compressionProgram` values. This could be accomplished as simply as:

```
import {
  createTree,
  MPL_ACCOUNT_COMPRESSION_PROGRAM_ID,
  MPL_NOOP_PROGRAM_ID,
} from '@metaplex-foundation/mpl-bubblegum'
import {
  generateSigner,
  publicKey,
} from '@metaplex-foundation/umi';

// Create a Merkle tree specifying the correct `logWrapper` and
// `compressionProgram` for Eclipse.
const merkleTree = generateSigner(umi);
const createTreeTx = await createTree(umi, {
  merkleTree,
  maxDepth: 3,
  maxBufferSize: 8,
  canopyDepth: 0,
  logWrapper: MPL_NOOP_PROGRAM_ID,
  compressionProgram: MPL_ACCOUNT_COMPRESSION_PROGRAM_ID,
});

await createTreeTx.sendAndConfirm(umi);
```

However, a helper function has been provided to automatically resolve these program IDs, and this is the recommended approach as it will work on Eclipse devnet/mainnet to which Bubblegum has been deployed:

```
import {
  getCompressionPrograms,
  createTree,
} from '@metaplex-foundation/mpl-bubblegum'
import {
  generateSigner,
  publicKey,
} from '@metaplex-foundation/umi';

// Create a Merkle tree using the `getCompressionPrograms` helper function.
const merkleTree = generateSigner(umi);
const createTreeTx = await createTree(umi, {
  merkleTree,
  maxDepth: 3,
  maxBufferSize: 8,
  canopyDepth: 0,
  ...(await getCompressionPrograms(umi)),
});

await createTreeTx.sendAndConfirm(umi);
```

#### Mint and Transfer a cNFT <a href="#mint-and-transfer-a-c-nft" id="mint-and-transfer-a-c-nft"></a>

Similarly to creating the Merkle tree on Eclipse, other SDK functions such as `mintV1` and `transfer` will also require specifying the compression programs. Again we use the `getCompressionPrograms` helper.

```
import {
  fetchMerkleTree,
  getCurrentRoot,
  hashMetadataCreators,
  hashMetadataData,
  transfer,
  getCompressionPrograms,
  createTree,
  MetadataArgsArgs,
  mintV1,
} from '@metaplex-foundation/mpl-bubblegum'
import {
  generateSigner,
  none,
} from '@metaplex-foundation/umi';

// Get leaf index before minting.
const leafIndex = Number(
  (await fetchMerkleTree(umi, merkleTree.publicKey)).tree.activeIndex
);

// Define Metadata.
const metadata: MetadataArgsArgs = {
  name: 'My NFT',
  uri: 'https://example.com/my-nft.json',
  sellerFeeBasisPoints: 500, // 5%
  collection: none(),
  creators: [],
};

// Mint a cNFT.
const originalOwner = generateSigner(umi);
const mintTxn = await mintV1(umi, {
  leafOwner: originalOwner.publicKey,
  merkleTree: merkleTree.publicKey,
  metadata,
  ...(await getCompressionPrograms(umi)),
}).sendAndConfirm(umi);

// Transfer the cNFT to a new owner.
const newOwner = generateSigner(umi);
const merkleTreeAccount = await fetchMerkleTree(umi, merkleTree.publicKey);
const transferTxn = await transfer(umi, {
  leafOwner: originalOwner,
  newLeafOwner: newOwner.publicKey,
  merkleTree: merkleTree.publicKey,
  root: getCurrentRoot(merkleTreeAccount.tree),
  dataHash: hashMetadataData(metadata),
  creatorHash: hashMetadataCreators(metadata.creators),
  nonce: leafIndex,
  index: leafIndex,
  proof: [],
  ...(await getCompressionPrograms(umi)),
}).sendAndConfirm(umi);
```

\
