# Switchboard

[Switchboard On-Demand](https://docs.switchboard.xyz/docs) is designed to be a cost optimized, low latency and high security data solution for blockchain applications.

Switchboard on Eclipse works the same way as it does on Solana, except you'll have to pass in an extra parameter and there are a few gotchas. For full documentation, visit [Switchboard](https://docs.switchboard.xyz/docs/switchboard/readme/links-and-technical-documentation/eclipse).

### Program ID <a href="#program-id" id="program-id"></a>

<table><thead><tr><th width="192">Network</th><th>Program address</th></tr></thead><tbody><tr><td>Eclipse Mainnet</td><td><code>SBondMDrcV3K4kxZR1HNVT7osZxAHVHgYXL5Ze1oMUv</code></td></tr><tr><td>Eclipse Devnet</td><td><code>SBondMDrcV3K4kxZR1HNVT7osZxAHVHgYXL5Ze1oMUv</code></td></tr></tbody></table>

#### Getting Started <a href="#getting-started" id="getting-started"></a>

Start from [Integrating On-Chain (SVM)](https://docs.switchboard.xyz/docs/switchboard/readme/links-and-technical-documentation/integrating-on-chain-svm). You can add the network and the chain settings in the params for `fetchUpdateIx`. This will route the requests to the correct Switchboard oracles and map the data back to the target chain (Eclipse).

Copy

```
const provider = ...

// Initialize the program state account
const idl = (await anchor.Program.fetchIdl(new PublicKey("SBondMDrcV3K4kxZR1HNVT7osZxAHVHgYXL5Ze1oMUv"), provider))!;
const program = new anchor.Program(idl, provider);

// Get the Pull Feed - (pass in the feed pubkey)
const pullFeed = new PullFeed(program, new PublicKey(...));

// Get the update for the pull feed
const [pullIx, responses, _, luts] = await pullFeed.fetchUpdateIx({ 
      crossbarClient: crossbar,
      chain: "eclipse",
      network: "mainnet",
});
```

{% hint style="info" %}
For more info on Switchboard, check out the [official docs](https://docs.switchboard.xyz/docs).
{% endhint %}
