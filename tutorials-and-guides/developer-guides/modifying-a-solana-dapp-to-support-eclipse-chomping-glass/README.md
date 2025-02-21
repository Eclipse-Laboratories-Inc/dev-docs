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

# Modifying a Solana dApp to Support Eclipse: "Chomping Glass"

In this tutorial, we're going to take an existing Solana dApp and deploy it multichain to Eclipse Devnet. Specifically, we're going to use Jarry Xiao's [Chomping Glass](https://github.com/jarry-xiao/chomping-glass) game as an example.

You can also take a look at the [forked GitHub repo](https://github.com/Eclipse-Laboratories-Inc/chomping-glass) or the [deployed frontend](https://chomping-glass.vercel.app/).

At a high level, developers should take the following steps:

1. Acquire Devnet tokens or Sepolia Testnet ETH
2. Deploy smart contract with the Eclipse Devnet RPC or Eclipse Testnet RPC
3. Set up developer wallet
4. Modify frontend
5. Test and deploy

## Deploying the Smart Contracts

This part is easy, since smart contracts on Eclipse typically require no changes compared to the Solana deployment. We're going to use the Eclipse Devnet RPC for this example: `https://staging-rpc.dev2.eclipsenetwork.xyz`

The first step is to clone the Chomping Glass repo:

```
git clone git@github.com:jarry-xiao/chomping-glass.git
```

We assume that you've followed the [Quick Start](../quick-start-hello-world/) guide and configured your Solana CLI properly to point to the Eclipse Devnet RPC. Give yourself some devnet tokens: `solana airdrop 1`

Some programs might hardcode dependencies such as oracles or bridges. You should [review your code](./#v2-deploying-a-multichain-frontend) to see where you might need to make changes and reach out to the Eclipse team if you need assistance.

Assuming there are no changes to be made, you can deploy the program as per usual:

```
cargo build-bpf --manifest-path=./Cargo.toml --bpf-out-dir=dist/program
solana program deploy dist/program/chomping_glass.so
```

You'll want to copy the program ID:

```
neelsomani@Neels-MacBook-Pro chomping-glass % solana program deploy dist/program/chomping_glass.so
Program Id: Gg9RXnAuiQDYadKP4tExAFCkhXSc3kBywCGqqPVx2duH
```

## Deploying A New Frontend

For this section of the tutorial, we'll explore the simplest way to get the app working, which just involves creating a new frontend that interacts with Eclipse Devnet, separate from the frontend used for the Solana deployment.

### Updating Hardcoded References

We start by looking through the React app to find any references to hardcoded addresses or keys.

In [App.tsx](https://github.com/jarry-xiao/chomping-glass/blob/main/chomping-glass/src/App.tsx):

```jsx
const PROGRAM_ID = new PublicKey(
  "ChompZg47TcVy5fk2LxPEpW6SytFYBES5SHoqgrm8A4D"
);
const FEE = new PublicKey("EGJnqcxVbhJFJ6Xnchtaw8jmPSvoLXfN2gWsY9Etz5SZ");
```

The `PROGRAM_ID` needs to be changed to the program ID from the deployment above. You might want to consider changing that address that the game is sending fees to!

Here's a spot where a block explorer link is hardcoded, which assumes that the only options are localhost or Solana mainnet:

```jsx
if (connection.rpcEndpoint.includes("localhost")) {
  console.log(
    `https://explorer.solana.com/tx/${signature}?cluster=custom&customUrl=http%3A%2F%2Flocalhost%3A8899`
  );
} else {
  console.log(`https://solscan.io/tx/${signature}`);
}
```

We modify this to provide a third option:

```jsx
else if (connection.rpcEndpoint.includes("eclipsenetwork")) {
    console.log(
        `https://solscan.io/tx/${signature}?cluster=custom&customUrl=https%3A%2F%2Fstaging-rpc.dev2.eclipsenetwork.xyz`
    );
}
```

We could have changed this reference too, but it's not really necessary for the purpose of this walkthrough:

```jsx
  notify(
    `${signature}`,
    `https://solscan.io/tx/${signature}`,
    "View on Solscan"
  );
```

In [index.tsx](https://github.com/jarry-xiao/chomping-glass/blob/main/chomping-glass/src/index.tsx), we see where the RPC connection referenced earlier is provided:

```jsx
const isDevelopment = window.location.hostname === "localhost";
const RPC_TOKEN = process.env.REACT_APP_RPC_TOKEN || "";
const RPC_URL = process.env.REACT_APP_RPC_URL || "";

function Root() {
  return (
    <ConnectionProvider
      endpoint={!isDevelopment ? RPC_URL : `${RPC_URL}${RPC_TOKEN}`}
```

Seems like we don't need to touch this part, and instead we can just update that environment variable. You're likely familiar with how to set an environment variable given your frontend deployment. For create-react-app, you can add a .env file to the root of the project.

Suggested .env file:

```
REACT_APP_RPC_URL=https://staging-rpc.dev2.eclipsenetwork.xyz
```

Finally, we run the frontend and test it to make sure transactions are indeed going through and can be observed in the block explorer.

We run the app with `npm start`. We need to add a custom config-overrides.js file to the root of the React project specified in this [StackOverflow answer](https://stackoverflow.com/a/70488628/657200). We add one additional override not mentioned in the StackOverflow post:

```
zlib: require.resolve('browserify-zlib')
```

And it works:

<figure><img src="../../../.gitbook/assets/Screen Shot 2023-10-18 at 1.45.16 AM.png" alt="" width="375"><figcaption></figcaption></figure>

### Adding Wallet Support

To use the dApp, you can set up an Eclipse [developer wallet](../../../developers/wallet/) and [modify the Solana wallet adapter](../../../developers/wallet/testnet-and-devnet-wallets/adding-eclipse-wallet-to-dapp.md) to only show Eclipse compatible wallets. Note that the Solscan link that pops up on the page won't be correct since we didn't update that part, but you can check the developer console for a working link.

## Deploying A Multichain Frontend

The above example was particularly simple. For a real deployment, you likely need to make the following changes as well:

* Review where your smart contracts invoke other smart contracts or dependencies which might only exist on the Solana L1, and replace those with Eclipse's alternatives.
* Update offchain infrastructure to support this additional RPC, possibly with a flag or a second deployment of your offchain infrastructure.
* If you have any tokens deployed to Solana mainnet, you'll want to consider how you'll incorporate those into your Eclipse Mainnet deployment: maintaining the old mint authority on Solana and bridging tokens, deploying a second mint authority, or removing your application's dependency on tokens.
* Modify your frontend to support both Solana and Eclipse simultaneously, rather than only supporting the Eclipse network such as the example above. This might be achieved via a toggle which changes which RPC the web app sends transactions to, and a prompt to tell the user to switch networks.
