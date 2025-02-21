# Custom Wallets

{% hint style="info" %}
This section aims to help us enable custom wallet support for Solana Wallet Adapter.&#x20;
{% endhint %}

### What is the Solana wallet adapter[​](https://icarus131.github.io/devcookbook/docs/SolanaWalletAdapter#what-is-the-solana-wallet-adapter) <a href="#what-is-the-solana-wallet-adapter" id="what-is-the-solana-wallet-adapter"></a>

It is a collection of components for solana applications built with typescript or javascript to help interact with wallets on the client side.

### Eclipse Wallet[​](https://icarus131.github.io/devcookbook/docs/SolanaWalletAdapter#eclipse-wallet) <a href="#eclipse-wallet" id="eclipse-wallet"></a>

Eclipse maintains a fork of the Salmon wallet. This is the wallet that is recommended to be used on the devnet. Follow [this guide](https://docs.eclipse.builders/building-on-eclipse/developer-wallet-setup) to setup the Eclipse wallet. You can also use [Drift's MetaMask Snap](https://docs.eclipse.builders/building-on-eclipse/developer-wallet-setup#metamask-snaps).

### Writing support for a custom (Eclipse) wallet[​](https://icarus131.github.io/devcookbook/docs/SolanaWalletAdapter#writing-support-for-a-custom-eclipse-wallet) <a href="#writing-support-for-a-custom-eclipse-wallet" id="writing-support-for-a-custom-eclipse-wallet"></a>

#### Prerequisites[​](https://icarus131.github.io/devcookbook/docs/SolanaWalletAdapter#prerequisites) <a href="#prerequisites" id="prerequisites"></a>

As Solana wallet adapter is a JS/TS specific library, we need to initialize a project with a preferred framework. Let's create an example application using next:

```bash
npx create-next-app custom-wallet-adapter --ts
cd custom-wallet-adapter
```

After that, install the wallet adapter library.

```bash
npm install @solana/wallet-adapter-base @solana/web3.js @solana/wallet-adapter-react @solana/wallet-adapter-wallets
```

#### Initializing Wallets[​](https://icarus131.github.io/devcookbook/docs/SolanaWalletAdapter#initializing-wallets) <a href="#initializing-wallets" id="initializing-wallets"></a>

Edit `_app.tsx` file located inside the `src` folder or the `app` folder depending on how you have setup the next app. Here we are using the adapters for the supported wallets:

```tsx
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

Now, initialize the allowed wallets in our `_app.tsx` file. Here we can set the network and also add the Eclipse RPC:

```tsx
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

We have successfully used the `@solana/wallet-adapter-react` to setup the connection and the wallet provider.

### Filtering Wallets[​](https://icarus131.github.io/devcookbook/docs/SolanaWalletAdapter#filtering-wallets) <a href="#filtering-wallets" id="filtering-wallets"></a>

When a user has multiple wallets installed on their browser, each of the wallets will have a readyState value set to "Installed". This causes certain unsupported wallets to be autodetected. This can be fixed by white-listing only the supported wallets.

To do this, we must first create a new component. You can name this `Wallets.tsx.`Here we have an array of supported wallets. This makes it adding support for wallets much more modular. To add a supported wallet, once we modify the `_app.tsx` we just have to add the wallet to the array.

```tsx
const EclipseWallets = () => {
  const { select, wallets, publicKey, disconnect } = useWallet();

  const supportedWalletNames = ["Salmon", "Connect by Drift"];

  const supportedWallets = wallets.filter(
    (wallet) =>
      supportedWalletNames.includes(wallet.adapter.name) &&
      (wallet.readyState === "Installed" ||
        wallet.adapter.name === "Connect by Drift"),
```

Now all that's left to do is to map the supported wallets and render them as buttons. As MetaMask Snaps do not return a readyState value, we must have an exception for it so that it displays regardless of the readyState.

```tsx
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

This will allow us to restrict the user to only be able to use Eclipse compatible wallets and also have the application detect only the supported wallets that are installed. Finally we can export the component and add it to our `index.tsx` file. Here is an example web application styled using DaisyUI:

<div data-full-width="true"><img src="https://icarus131.github.io/devcookbook/assets/images/walletadapter-83ffe06ea59029a230f8b28b6da75b94.png" alt="Example Application"></div>
