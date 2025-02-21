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

# Adding Eclipse Wallet to dApp

{% hint style="info" %}
You will need a wallet that is compatible with Eclipse because the SVM RPC pulls a recent block hash from whichever RPC you're connected to.
{% endhint %}

## Adding MetaMask Snap To Your dApp

To support MetaMask, we recommend adding [Drift's MetaMask Snap](https://www.drift.trade/updates/connect-with-metamask) to your dApp by following their [instructions](https://www.npmjs.com/package/@drift-labs/snap-wallet-adapter). First, install the npm package: `npm install --save @drift-labs/snap-wallet-adapter`

Then in index.tsx:

```jsx
// Import the Drift wallet adapter
import { SnapWalletAdapter } from '@drift-labs/snap-wallet-adapter';
[...]
function Root() {
  return (
    <ConnectionProvider
      endpoint={!isDevelopment ? RPC_URL : `${RPC_URL}${RPC_TOKEN}`}
    >
      // Add the SnapWalletAdapter to the list of wallets
      <WalletProvider wallets={[new SnapWalletAdapter()]} autoConnect>
```

## Adding Salmon Wallet To Your dApp

The [Solana wallet provider](https://github.com/solana-labs/wallet-adapter) automatically identifies the Salmon wallet if it is installed for users, so your SVM dApp requires no additional changes to support Salmon.

## Custom Wallet Support for Solana Wallet Adapter

For a better user experience, customize the Solana wallet adapter to only show Eclipse-compatible wallets.

### Prerequisites[​](https://icarus131.github.io/devcookbook/docs/SolanaWalletAdapter#prerequisites) <a href="#prerequisites" id="prerequisites"></a>

As Solana wallet adapter is a JS/TS specific library, we need to initialize a project with a preferred framework. Let's create an example application using next:

```bash
npx create-next-app custom-wallet-adapter --ts
cd custom-wallet-adapter
```

Next, install the wallet adapter library:

```bash
npm install @solana/wallet-adapter-base @solana/web3.js @solana/wallet-adapter-react @solana/wallet-adapter-wallets
```

### Initializing Wallets[​](https://icarus131.github.io/devcookbook/docs/SolanaWalletAdapter#initializing-wallets) <a href="#initializing-wallets" id="initializing-wallets"></a>

Start by editing your `_app.tsx` file located inside the `src` folder or the `app` folder depending on how you have set up the next.js app. Include the required libraries in the `_app.tsx` file. Here you can use the adapter for Salmon wallet and MetaMask Snap:

```javascript
import "@/styles/globals.css";
import type { AppProps } from "next/app";
import head from "next/head";
import {
  ConnectionProvider,
  WalletProvider,
} from "@solana/wallet-adapter-react";
import { useMemo } from "react";
import { SalmonWalletAdapter } from "@solana/wallet-adapter-wallets";
import { SnapWalletAdapter } from "@drift-labs/snap-wallet-adapter";
import { clusterApiUrl } from "@solana/web3.js";
```

Now, initialize the allowed wallets in your `_app.tsx` file. You can set the network and add the Eclipse RPC:

```javascript
export default function App({ Component, pageProps }: AppProps) {
  const driftSnapWalletAdapter = new SnapWalletAdapter();

  const wallets = useMemo(
    () => [new SalmonWalletAdapter(), new SnapWalletAdapter()],
    [],
  );

  const endpoint = useMemo(() => clusterApiUrl(<network>), []);

  return (
    <div>
      <div className="hero min-h-screen bg-base-200">
        <div className="hero-content text-center">
            <ConnectionProvider endpoint={endpoint}>
              <WalletProvider wallets={wallets} autoConnect>
                <Component {...pageProps} />
              </WalletProvider>
            </ConnectionProvider>
        </div>
      </div>
    </div>
  );
}


```

You have used the `@solana/wallet-adapter-react` to set up the connection with Eclipse compatible wallets.&#x20;

### Filtering Wallets[​](https://icarus131.github.io/devcookbook/docs/SolanaWalletAdapter#filtering-wallets) <a href="#filtering-wallets" id="filtering-wallets"></a>

When a user has multiple wallets installed in their browser, each of the wallets will have a readyState value set to "Installed". This causes wallets incompatible with Eclipse to be autodetected. This can be fixed by whitelisting only the supported wallets. To do this, we must first create a new component. You can name this `wallets.tsx`.

Here is an array of supported wallets, which makes adding support for wallets more modular. Once you modify the `_app.tsx`, just add the Eclipse compatible wallet to the array.

```javascript
const EclipseWallets = () => {
  const { select, wallets, publicKey, disconnect } = useWallet();

  const supportedWalletNames = ["Salmon", "Connect by Drift"];

  const supportedWallets = wallets.filter(
    (wallet) =>
      supportedWalletNames.includes(wallet.adapter.name) &&
      (wallet.readyState === "Installed" ||
        wallet.adapter.name === "Connect by Drift"),
```

The last step is to map the supported wallets and render them as buttons. Since MetaMask Snap does not return a readyState value, you must add an exception for it so that it displays regardless of the readyState.

```javascript
 supportedWallets.map((wallet) => (
          <div key={wallet.adapter.name}>
            <button
              className="btn btn-outline btn-accent"
              key={wallet.adapter.name}
              onClick={() => select(wallet.adapter.name)}
            >
              <img
                src={wallet.adapter.icon}
                alt={wallet.adapter.name}
                width="24px"
                height="24px"
              ></img>
              {wallet.adapter.name === "Connect by Drift"
                ? "MetaMask"
                : wallet.adapter.name}
            </button>
            <div className="divider"></div>
          </div>
        ))
```

This will allow us to 1) restrict the user to only use Eclipse compatible wallets and 2) restrict the dApp to only detect supported wallets that are installed. Finally, you can export the component and add it to your `index.tsx` file.

Here is an example web application styled using DaisyUI:

![Example Application](https://icarus131.github.io/devcookbook/assets/images/walletadapter-83ffe06ea59029a230f8b28b6da75b94.png)

