# Testnet & Devnet Wallets

{% hint style="info" %}
You will need a wallet that is compatible with Eclipse Testnet and Devnet.
{% endhint %}

## MetaMask Snaps&#x20;

### Installation

If you already have MetaMask installed, then installing the MetaMask Snap via a dApp that supports the Snap is trivial. The UI for the wallet adapter will guide you.

## Salmon Wallet

### Installation

In this walkthrough, we'll download Eclipse's fork of the Salmon wallet. This wallet is only to be used for devnet purposes until it is fully audited. These instructions work on Chrome or any Chromium-based browser like Brave or Opera.

1. Download **build-extension\_v0.4.2-alpha.zip** from the [GitHub repo](https://github.com/Eclipse-Laboratories-Inc/eclipse-wallet/releases/tag/v0.4.2-alpha)
2. Unzip the file
3. Type `chrome://extensions` in your browser search bar
4. Toggle on **Developer mode** in the top right corner of your browser
5. Drag and drop the unzipped/extracted folder (**build-extension\_v0.4.2**) to your browser
6. Pin the extension for easy access
7. Configure the wallet by selecting **Eclipse Devnet** from the dropdown within Salmon Wallet

**Download the zip file**: The first step is to download the zip file which contains the Chrome extension for the wallet and unzip it. ​We've uploaded the zip file (build-extension\_v0.4.2.zip) to this [GitHub repo.](https://github.com/Eclipse-Laboratories-Inc/eclipse-wallet/releases/tag/v0.4.2-alpha)

**Enable developer mode:** Type `chrome://extensions` in the search bar. Enable developer mode in the top right corner of the broswer.

<figure><img src="../../../.gitbook/assets/Group 20 (1).png" alt=""><figcaption><p>Type chrome://extensions into the address bar and enable Developer Mode in the top right corner.</p></figcaption></figure>

**Install the extension**: Drag and drop the unzipped file to the browser. This will install the extension. Keep in mind that you can't move or delete the unzipped file once you've installed it, or it will break the extension.

<figure><img src="../../../.gitbook/assets/Group 21.png" alt=""><figcaption><p>Drag the unzipped folder onto the chrome://extensions page.</p></figcaption></figure>

You can pin the wallet for easy access:

<figure><img src="../../../.gitbook/assets/Screenshot 2023-11-27 at 10.22.51 PM.png" alt="" width="325"><figcaption><p>Click the Extensions icon to open your list of extensions, then click the pin button next to Salmon Wallet.</p></figcaption></figure>

**Configure the wallet:** Change the network to Eclipse Devnet.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-11-27 at 10.24.44 PM.png" alt="" width="373"><figcaption><p>Select Eclipse Devnet in the dropdown within Salmon wallet.</p></figcaption></figure>

Your developer wallet for Eclipse is setup. You can request testnet tokens via the [faucet](../../developer-tooling/faucet.md).

### For additional information:

{% content-ref url="adding-eclipse-wallet-to-dapp.md" %}
[adding-eclipse-wallet-to-dapp.md](adding-eclipse-wallet-to-dapp.md)
{% endcontent-ref %}

{% content-ref url="custom-wallets.md" %}
[custom-wallets.md](custom-wallets.md)
{% endcontent-ref %}
