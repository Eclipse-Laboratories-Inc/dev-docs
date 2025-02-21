# Eclipse Canonical Bridge

Our enshrined canonical bridge is live for Eclipse Mainnet & Testnet. Withdrawals from the Eclipse Bridge are currently disabled. \
\
You can bridge using the official bridge site at: [https://bridge.eclipse.xyz/](https://bridge.eclipse.xyz/)&#x20;

{% embed url="https://app.eclipse.xyz/bridge" %}

Our canonical bridge deployed directly to Ethereum is the primary way to bring assets to Eclipse Mainnet. You are able to bridge ETH and eventually other assets via our bridge. ETH is the native token for Eclipse Mainnet. We do not have any other native token for Eclipse Mainnet.&#x20;

{% embed url="https://youtu.be/3mbvnr9TGgE" %}
Eclipse Bridge Walkthrough
{% endembed %}



***

## Eclipse Deposit CLI

This CLI tool allows end users to deposit Ether from Ethereum Mainnet or the Sepolia test network into the Eclipse rollup, which utilizes the Solana Virtual Machine (SVM).

This Eclipse Deposit CLI allows end users to deposit Ether from:

* Ethereum Mainnet into Eclipse Mainnet&#x20;
* Sepolia Testnet to the Eclipse Testnet

***

## Prerequisites

### 1. Yarn

Yarn is required for installation. For Mac users, Yarn can be installed via Homebrew using `brew install yarn`. Alternatively, if npm is available, use `npm install -g yarn`.

### 2. Ethereum Wallet

An Ethereum wallet such as [Backpack](https://backpack.app/) or Metamask is needed.

For Metamask:

1. Choose the account you wish to use and copy its address.
2. Visit the [Sepolia faucet](https://sepoliafaucet.com/) to airdrop tokens to yourself, if using Sepolia.
3. Navigate to 'account details' in MetaMask and select 'reveal private key'. Store this key in a secure file.

### 3. Solana CLI

The Solana CLI tools are necessary for generating a deposit address on the rollup.

To generate a wallet for deposits:

1. Install the Solana CLI tools.
2. To generate a wallet:
   * Execute `solana-keygen new --no-outfile` or `solana-keygen new --outfile my-wallet.json`.
3. Copy the public key from the output, which should resemble `6g8wB6cJbodeYaEb5aD9QYqhdxiS8igfcHpz36oHY7p8`.

***

## Installation (via npm)

* COMING SOON

## Installation (via GitHub)

1.  Clone this repository:

    ```
    git clone https://github.com/Eclipse-Laboratories-Inc/eclipse-deposit.git
    cd eclipse-deposit
    ```
2.  Install the necessary dependencies:

    ```
    yarn install
    ```

***

## Create a Deposit

1.  Run the CLI tool with the necessary options:

    ```
    node bin/cli.js -k <path_to_private_key> -d <solana_destination_address> -a <amount_in_ether> --mainnet|--sepolia 
    ```

    \
    For example:\


    **Mainnet Deposit:**

    ```
    node bin/cli.js -k private-key.txt -d 6g8wB6cJbodeYaEb5aD9QYqhdxiS8igfcHpz36oHY7p8 -a 0.002 --mainnet
    ```

    \
    **Sepolia Testnet Deposit:**

    ```
    node bin/cli.js -k private-key.txt -d 6g8wB6cJbodeYaEb5aD9QYqhdxiS8igfcHpz36oHY7p8 -a 0.002 --sepolia
    ```



    * The `-k, --key-file` option specifies the path to the Ethereum private key file.
    * The `-d, --destination` option specifies the Solana destination address on the rollup (base58 encoded).
    * The `-a, --amount` option specifies the amount of Ether to deposit.
    * Use `--mainnet` or `--sepolia` to select the network. The tool will use different contract addresses depending on the network.
    * The `-r, --rpc-url` option is optional and allows overriding the default JSON RPC URL.

{% hint style="warning" %}
Deposits will finalize and be processed in about 2-3 minutes.
{% endhint %}

***

## Security Note

Keep your Ethereum private key secure. Do not share it publicly or expose it in untrusted environments.
