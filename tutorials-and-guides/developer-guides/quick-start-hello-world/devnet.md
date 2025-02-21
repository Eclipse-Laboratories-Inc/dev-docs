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

# Devnet

This guide walks you through deploying a simple smart contract to Eclipse Devnet.

## Prerequisites

You'll have to do a few things before you can deploy your smart contract to Eclipse Devnet.

### Install Dependencies

Install Rust, and its package manager Cargo.

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

You can check if the installation was successful by running the following commands:

```bash
rustc --version
cargo --version
```

**ii.** We'll also have to install Node.js and npm using, which will be used later in this guide.

Visit the official [**Node.js download page**](https://nodejs.org/en/download/) and download the installation binary for your system. Versions recommended are 14.0 and above. If you have trouble with this, you might consider using Homebrew or some other package manager.

Note that npm is bundled with the Node.js installation, so you don't have to install it separately.

**iii.** Now let's install the Solana CLI. This allows you to interact with Solana clusters (Eclipse Devnet in this case).

```bash
sh -c "$(curl -sSfL https://release.solana.com/stable/install)"
```

**iv.** Instead of setting your Solana CLI to a local cluster, set it to Eclipse Devnet with the following command:

```bash
solana config set --url https://staging-rpc.dev2.eclipsenetwork.xyz
```

**v.** If this is your first time using the Solana CLI, you will need to generate a new keypair:

```bash
solana-keygen new
```

This will generate a new key pair and save it to your local machine. You will need this key pair to sign transactions to deploy your smart contract to Eclipse Devnet.

### Acquiring Devnet Tokens[​](https://documentation-nine-smoky.vercel.app/Build/SVM/intro#acquiring-testnet-tokens)

We need to claim devnet tokens to pay for transaction fees to deploy your smart contract to the devnet.

Run the following commands to get 10 devnet tokens in your local wallet:

1. `solana config set --url https://staging-rpc.dev2.eclipsenetwork.xyz`
2. `solana airdrop 10`

{% hint style="danger" %}
These devnet tokens are only valid on the devnet and are meant for testing purposes only. Eclipse will never charge you for devnet tokens. Please be wary of scams. Report any suspicious activity to us on our [Discord server](https://discord.com/invite/5jDfXHJGCk).
{% endhint %}

## Deploying the Smart Contract[​](https://documentation-nine-smoky.vercel.app/Build/SVM/intro#deploying-the-smart-contract) <a href="#deploying-the-smart-contract" id="deploying-the-smart-contract"></a>

Now that we've set up our environment, we can deploy our smart contract to Eclipse Devnet. Let's make sure that everything is installed properly by running a local Solana cluster.

```
solana-test-validator
```

{% hint style="info" %}
In case the validator fails to start, restart your computer and run the following command:

```bash
sudo $(command -v solana-sys-tuner) --user $(whoami) > sys-tuner.log 2>&1 &
```
{% endhint %}

We don't need that local Solana cluster but we're using it to check that everything is installed properly. Next, we'll clone the Solana Hello World repository and install the dependencies.

```bash
git clone https://github.com/solana-labs/example-helloworld
cd example-helloworld
npm install
```

We build the smart contract:

```bash
npm run build:program-rust
```

Finally, we can deploy the smart contract to Eclipse Devnet:

```bash
solana program deploy dist/program/helloworld.so
```

We can run the JavaScript client and confirm whether the smart contract was deployed successfully:

```bash
npm run start
```

The output should be something like this:

```bash
Let's say hello to a Solana account...
Connection to cluster established: http://127.0.0.1:8899 { 'feature-set': 2045430982, 'solana-core': '1.7.8' }
Using account AiT1QgeYaK86Lf9kudqKthQPCWwpG8vFA1bAAioBoF4X containing 0.00141872 SOL to pay for fees
Using program Dro9uk45fxMcKWGb1eWALujbTssh6DW8mb4x8x3Eq5h6
Creating account 8MBmHtJvxpKdYhdw6yPpedp6X6y2U9dCpdYaZJdmwV3A to say hello to
Saying hello to 8MBmHtJvxpKdYhdw6yPpedp6X6y2U9dCpdYaZJdmwV3A
8MBmHtJvxpKdYhdw6yPpedp6X6y2U9dCpdYaZJdmwV3A has been greeted 1 times
Success
```

{% hint style="info" %}
**Not seeing the expected output?**

* Make sure you've run all the commands in the previous steps.
* Inspect the program logs by running `solana logs` to see why the program failed.

An example of what you might find is given below.

```bash
Signature: 4pya5iyvNfAZj9sVWHzByrxdKB84uA5sCxLceBwr9UyuETX2QwnKg56MgBKWSM4breVRzHmpb1EZQXFPPmJnEtsJ
Status: Error processing Instruction 0: Program failed to complete
Log Messages:
  Program G5bbS1ipWzqQhekkiCLn6u7Y1jJdnGK85ceSYLx2kKbA invoke [1]
  Program log: Hello World Rust program entrypoint
  Program G5bbS1ipWzqQhekkiCLn6u7Y1jJdnGK85ceSYLx2kKbA consumed 200000 of 200000 compute units
  Program failed to complete: exceeded maximum number of instructions allowed (200000) at instruction #334
  Program G5bbS1ipWzqQhekkiCLn6u7Y1jJdnGK85ceSYLx2kKbA failed: Program failed to complete
```
{% endhint %}

## Integration Assistance <a href="#guides" id="guides"></a>

Do you need additional assistance integrating something special like a wallet, bridge, or something else? Feel free to reach out via [Discord](https://discord.com/invite/5jDfXHJGCk).
