# Nifty Asset

Nifty Asset is a fully-featured digital asset standard for the SVM. It is lightweight and efficient, designed to offer a small footprint (compute units consumption) and be highly flexible. The main features of the standard are:

* Single account to represent a digital asset.
* Flexible on-chain representation: store as much or as little data using optional extensions.
* Efficient zero-copy de-/serialization to minimize compute units utilization.
* Full-featured standard, including royalty enforcement, delegates, lock/unlock, inscriptions and groups (collections).
* Rust and JavaScript client SDKs.

Extensions can be combined to create a wide variety of non-fungible assets, from simple assets with links to off-chain data to fully on-chain assets. In addition to extensions, Nifty Asset follows the [⎘Proxy Pattern](https://nifty-oss.org/blog/proxy-pattern) to provide developers a program interface to customize every aspect of the protocol – and it enables that without requiring direct changes to the program. The advantage of that is that developers have full flexibility to extend its behavior in an non-opiniated way. The only requirement is to implement the [⎘program interface](https://crates.io/crates/nifty-asset-interface).

{% hint style="info" %}
The complete Nifty Asset documentation is available at: [https://nifty-oss.org/docs/category/nifty-asset](https://nifty-oss.org/docs/category/nifty-asset)
{% endhint %}

***

## Nifty CLI

### Install from Source[​](https://nifty-oss.org/docs/quickstart/cli#install-from-source) <a href="#install-from-source" id="install-from-source"></a>

Requires [Rust to be installed](https://www.rust-lang.org/learn/get-started):

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Now you can install with `pnpm` from the root directory of the `nifty-asset` repository:

```
pnpm clients:cli:install
```

With Rust's `cargo`:

```
cd clients/cli
cargo install --path .
```

Directly from crates.io:

```
cargo install nifty-cli
```

To see all the available commands and usage suggestions run:

```
nifty --help
```

```
Usage: nifty [OPTIONS] <COMMAND>

Commands:
  burn      Burn an asset
  mint      Create an asset with extensions
  create    Create a basic asset without extensions
  decode    Get an asset account's data and decode it
  approve   Set a delegate on an asset with specific roles
  lock      Lock an asset, preventing any actions to be performed on it
  revoke    Revoke a delegate from an asset
  transfer  Transfer an asset to a new owner
  unlock    Unlock an asset, allowing actions to be performed on it
  help      Print this message or the help of the given subcommand(s)

Options:
  -k, --keypair-path <KEYPAIR_PATH>  Path to the keypair file
  -r, --rpc-url <RPC_URL>            RPC URL for the Solana cluster
  -h, --help                         Print help
  -V, --version                      Print version
```

To see the help for a specific command, run the command with the `--help` option, e.g.:

```
nifty create --help
```

```
Create a basic asset without extensions

Usage: nifty create [OPTIONS] --name <NAME>

Options:
  -k, --keypair-path <KEYPAIR_PATH>
          Path to the keypair file
  -n, --name <NAME>
          The name of the asset
  -a, --asset-keypair-path <ASSET_KEYPAIR_PATH>
          Path to the mint keypair file
  -r, --rpc-url <RPC_URL>
          RPC URL for the SVM cluster
      --immutable
          Create the asset as immutable
  -o, --owner <OWNER>
          Owner of the created asset, defaults to authority pubkey
  -h, --help
          Print help
```

We install the Solana CLI which we use to set and configure both a default keypair and RPC node URL:

```
sh -c "$(curl -sSfL https://release.solana.com/v1.16.25/install)"
```

Now we can set the default keypair and Eclipse RPC node URL:

```
solana config set --url https://mainnetbeta-rpc.eclipse.xyz
solana-keygen new
```

Nifty will use these values by default, but you can also pass them as options to the commands.

***

## JavaScript Client

The Nifty Asset JS Client is a JavaScript client that provides a set of methods for interacting with the Nifty Asset program on the Solana Virtual Machine. This guide will help you get started with the Nifty Asset JS Client and show you how to use it with the Umi framework.

### Requirements[​](https://nifty-oss.org/docs/quickstart/javascript#requirements) <a href="#requirements" id="requirements"></a>

* [Node.js](https://nodejs.org/) version 18 or higher
* [NPM](https://www.npmjs.com/) or other package manager (e.g., Yarn, PNPM)
* [TypeScript](https://www.typescriptlang.org/) version 4.0 or higher installed
* [Solana CLI](https://docs.solanalabs.com/cli) installed (for local development)

### Umi[​](https://nifty-oss.org/docs/quickstart/javascript#umi) <a href="#umi" id="umi"></a>

The Nifty Asset JS Client is built to work with the Umi framework. [Umi](https://github.com/metaplex-foundation/umi) is a lightweight JavaScript framework that is used to build Solana clients. Umi was built and is maintained by the Metaplex Foundation. The Nifty Asset JS Client is an Umi plugin that provides a set of methods for interacting with the Nifty Asset program.

#### Required Dependencies[​](https://nifty-oss.org/docs/quickstart/javascript#required-dependencies) <a href="#required-dependencies" id="required-dependencies"></a>

To get started you will need to install the following libraries:

* `@metaplex-foundation/umi`: The core Umi framework [npmjs.com](https://www.npmjs.com/package/@metaplex-foundation/umi)
* `@metaplex-foundation/umi-bundle-defaults`: Default plug-ins bundle for Umi [npmjs.com](https://www.npmjs.com/package/@metaplex-foundation/umi-bundle-defaults)
* `@nifty-oss/asset`: The Nifty Asset JS Client [npmjs.com](https://www.npmjs.com/package/@nifty-oss/asset)

Add these dependencies to your project by running the following command:

```
npm install \
  @metaplex-foundation/umi \
  @metaplex-foundation/umi-bundle-defaults \
  @nifty-oss/asset
```

#### Umi Configuration[​](https://nifty-oss.org/docs/quickstart/javascript#umi-configuration) <a href="#umi-configuration" id="umi-configuration"></a>

To use the Nifty Asset JS Client, you will need to configure Umi to use the network of your choice (e.g., Eclipse Mainnet, Devnet, Testnet, or Local) and the Nifty Asset plugin. Create an instance of _Umi_ with the `createUmi` method, and pass in the network URL:

```
import { createUmi } from '@metaplex-foundation/umi-bundle-defaults';

const umi = createUmi('https://mainnetbeta-rpc.eclipse.xyz');
```

To configure Umi on other networks, simply replace the URL in the `createUmi` method with the RPC endpoint of your choice.

### RPC Endpoints

Nifty Asset is available on the following Eclipse networks:

| Network         | URL                                                                                         |
| --------------- | ------------------------------------------------------------------------------------------- |
| Eclipse Mainnet | [https://mainnetbeta-rpc.eclipse.xyz](https://mainnetbeta-rpc.eclipse.xyz/)                 |
| Eclipse Testnet | [https://testnet.dev2.eclipsenetwork.xyz](https://testnet.dev2.eclipsenetwork.xyz/)         |
| Eclipse Devnet  | [https://staging-rpc.dev2.eclipsenetwork.xyz](https://staging-rpc.dev2.eclipsenetwork.xyz/) |

Replace the URL in your `createUmi` method with the network of your choice.

***

## Use Nifty Asset JS Client[​](https://nifty-oss.org/docs/quickstart/javascript#use-nifty-asset-js-client) <a href="#use-nifty-asset-js-client" id="use-nifty-asset-js-client"></a>

To use the Nifty Asset JS Client, you will need to import the client and add it to your Umi instance using the `.use()` this this:

```
import { createUmi } from '@metaplex-foundation/umi-bundle-defaults';
import { niftyAsset } from '@nifty-oss/asset';

const umi = createUmi('https://mainnetbeta-rpc.eclipse.xyz').use(niftyAsset());
```

The `.use()` method registers the Nifty Asset plugin with the Umi instance, enabling access to the Nifty Asset methods.

You are all set and ready to start using the Nifty Asset JS Client with Umi. We will use this same structure throughout the documentation to demonstrate how to use the Nifty Asset JS Client with Umi, so take a moment to familiarize yourself with this setup. If you have any questions, please reach out to the Nifty Asset team on [Discord](https://discord.gg/uTMaCT7x9D).
