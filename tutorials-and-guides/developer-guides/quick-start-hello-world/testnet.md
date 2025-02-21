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

# Testnet

This guide walks you through deploying a simple smart contract to Eclipse Testnet.

## Prerequisites

You'll have to do a few things before you can deploy your smart contract to Eclipse Testnet.

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

**iii.** Now let's install the Solana CLI. This allows you to interact with Solana clusters (Eclipse Testnet in this case).

```bash
sudo sh -c "$(curl -sSfL https://release.anza.xyz/v1.18.22/install)"
```

**iv.** Instead of setting your Solana CLI to a local cluster, set it to Eclipse Testnet with the following command:

```bash
solana config set --url https://testnet.dev2.eclipsenetwork.xyz
```

**v.** If this is your first time using the Solana CLI, you will need to generate a new keypair:

```bash
solana-keygen new
```

This will generate a new key pair and save it to your local machine. You will need this key pair to sign transactions to deploy your smart contract to Eclipse Testnet.

### **Getting Sepolia ETH Tokens**

You will need an Ethereum wallet such as Metamask to claim and bridge Sepolia ETH. As a user, you can use Sepolia ETH to explore the Eclipse testnet and dApps deployed on the network.&#x20;

{% hint style="info" %}
Sepolia ETH is not meant to be traded, and is only used to test applications.&#x20;
{% endhint %}

Sepolia ETH can be claimed from a number of faucets: Alchemy, QuickNode, Chainlink and Infura.&#x20;

Here are instructions on how to claim Sepolia ETH on [**Chainlink Sepolia ETH faucet**](https://faucets.chain.link/sepolia).&#x20;

1. Select 'Ethereum Sepolia' and click 'Continue'
2. Visit the Alchemy Sepolia faucet and log in with your Alchemy account.
3. Enter your wallet address in the provided box and click "Get tokens".

### Deposit Sepolia ETH to Eclipse Testnet

Once you've acquired Sepolia ETH, this script allows you to deposit Sepolia ETH to the Eclipse test network.

### Wallet Setup

#### Getting Your Ethereum Private Key

Once you have [Sepolia ETH](https://faucets.chain.link/sepolia), copy your Ethereum wallet address and your private private key for later use by navigating to "Account details" and clicking "Show private key".

#### Getting Your Eclipse Public Address

Copy the previously generated "Solana" address using the Solana CLI:&#x20;

`solana-keygen new --no-outfile` or `solana-keygen new --outfile my-wallet.json`.

The public key in the output should resemble something like this following: `6g8wB6cJbodeYaEb5aD9QYqhdxiS8igfcHpz36oHY7p8`

### Create a Deposit

Obtain and set up the deposit.js script: [https://github.com/Eclipse-Laboratories-Inc/testnet-deposit](https://github.com/Eclipse-Laboratories-Inc/testnet-deposit)

You'll need to install the dependencies with `yarn`.

Finally, execute the script:

1. `cd eclipse-deposit`
2. `npm install`
3. Change name of `"example-private-key.txt"` to `"private-key.txt"` and paste your Metamask wallet's private key in the file.
4. Run the CLI tool with the necessary options:

```sh
node bin/cli.js -k <path_to_private_key> -d <solana_destination_address> -a <amount_in_ether> --mainnet|--sepolia 
```

For example:

**Sepolia Testnet Deposit:**

```sh
node bin/cli.js -k private-key.txt -d 6g8wB6cJbodeYaEb5aD9QYqhdxiS8igfcHpz36oHY7p8 -a 0.004 --sepolia
```

* The `-k, --key-file` option specifies the path to the Ethereum private key file.
* The `-d, --destination` option specifies the Solana destination address on the rollup (base58 encoded).
* The `-a, --amount` option specifies the amount of Ether to deposit.
* Use `--mainnet` or `--sepolia` to select the network. The tool will use different contract addresses depending on the network.
* The `-r, --rpc-url` option is optional and allows overriding the default JSON RPC URL.

A successful command example:

{% code title="Output" overflow="wrap" %}
```
Transaction hash: 0x15041e2f67e821f8a76671aa282f3b7994d81b2c0b3434d67880ef88c9884186
```
{% endcode %}

You can check the transaction hash on [Sepolia testnet](https://sepolia.etherscan.io/).

You can verify your testnet account balance using the [Eclipse Explorer](https://explorer.dev.eclipsenetwork.xyz/?cluster=testnet).

## Deploying the Smart Contract[​](https://documentation-nine-smoky.vercel.app/Build/SVM/intro#deploying-the-smart-contract) <a href="#deploying-the-smart-contract" id="deploying-the-smart-contract"></a>

Now that we've set up our environment, we can deploy our smart contract to Eclipse Testnet. Let's make sure that everything is installed properly by running a local Solana cluster.

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

Next,

1. install "[Even Better TOML](https://marketplace.visualstudio.com/items?itemName=tamasfe.even-better-toml)" extension
2. Locate and open the 'Cargo.toml' file at `src/program-rust/Cargo.toml. Might see a few outdated dependencies, marked with a red ❌ thanks to the extension.`
3. Update the dependencies according to the latest versions shown right next to the current ones.

We build the smart contract:

```bash
npm run build:program-rust
```

Finally, we can deploy the smart contract to Eclipse Testnet:

```sh
solana program deploy dist/program/helloworld.so
```

Expected output

```
Program Id: Butt9GJQQPXX6ih65e1Z11R4QyQ8YfAEHit7VmkrLs8v
```

{% hint style="warning" %}
It might prompt with

```
// It might prompt with

Error: Account 9BEnr8WzpJ9kaEJQyG65iE6357hcXG8EpDEGgbLi2AZ1 has insufficient funds for spend (0.00331058 SOL) + fee (0.0000026 SOL)
Nothing to worry about, just ask Anmol to send you some Eclipse testnet funds or do it yourself (hint: the command is somewhere up there 👀 )!
```

Nothing to worry about, just ask Anmol to send you some Eclipse testnet funds or do it yourself (hint: the command is somewhere up there 👀 )!
{% endhint %}

We can run the JavaScript client and confirm whether the smart contract was deployed successfully:

```bash
npm run start
```

The output should be something like this:

```
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
