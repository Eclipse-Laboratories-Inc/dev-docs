---
description: >-
  Eclipse Token Dashboard demonstrates essential functionality like connecting
  wallets, checking balances, and sending tokens - all with a clean,
  approachable codebase.
---

# Reading from the blockchain

## App Preview

Here's what the application looks like when running:

#### Balance View

<figure><img src="https://github.com/Eclipse-Laboratories-Inc/Beginner-Example-App/raw/main/screenshots/balance-view.png" alt=""><figcaption></figcaption></figure>

#### Transfer View

<figure><img src="https://github.com/Eclipse-Laboratories-Inc/Beginner-Example-App/raw/main/screenshots/transfer-view.png" alt=""><figcaption></figcaption></figure>

#### Gas Fee Comparison

<figure><img src="https://github.com/Eclipse-Laboratories-Inc/Beginner-Example-App/raw/main/screenshots/gas-fee-comparison.png" alt=""><figcaption></figcaption></figure>

## Learning Goals

This example project covers:

* Connecting crypto wallets to your dApp
* Displaying token balances
* Transferring tokens between addresses
* Comparing gas fees across networks
* Handling transaction receipts and errors

No previous blockchain experience needed - just basic React knowledge!

## Tech Stack

Built with beginner-friendly tools:

* Next.js
* Solana wallet adapters (compatible with Eclipse)
* shadcn/ui components
* Tailwind CSS

## Getting Started

### Prerequisites

* Node.js 18+
* pnpm (or npm/yarn if you prefer)

### Setup Steps

1.  Clone this repo

    ```bash
    git clone https://github.com/Eclipse-Laboratories-Inc/Beginner-Example-App.git
    cd Beginner-Example-App
    ```
2.  Install dependencies

    ```bash
    pnpm install
    ```
3.  Set up your environment\
    Create a `.env.local` file:

    ```
    # Testnet is easier to start with
    NEXT_PUBLIC_SOLANA_RPC_URL=https://testnet.dev2.eclipsenetwork.xyz

    # For mainnet later (just uncomment)
    # NEXT_PUBLIC_SOLANA_RPC_URL=https://mainnet.eclipsenetwork.xyz
    ```
4.  Start the dev server

    ```bash
    pnpm dev
    ```
5. Open [http://localhost:3000](http://localhost:3000) in your browser

### Important! Wallet Setup

Before testing the app:

* Make sure your wallet is set to the Eclipse testnet
* For testnet, use this RPC URL: https://testnet.dev2.eclipsenetwork.xyz
* If you switch to mainnet later, update your wallet settings accordingly

### Code Highlights

Some key aspects of the implementation:

#### Wallet Connection

```tsx
// Simple wallet connection with just one line
const { publicKey, sendTransaction, connected, disconnect } = useWallet()
```

#### Sending Tokens

```tsx
// Basic transaction creation and sending
const transferTokens = async () => {
  const transaction = new Transaction().add(
    SystemProgram.transfer({
      fromPubkey: publicKey,
      toPubkey: recipientPubkey,
      lamports,
    })
  );
  
  const signature = await sendTransaction(transaction, connection);
}
```

## Source Code

[Link to Github](https://github.com/Eclipse-Laboratories-Inc/Beginner-Example-App)

### License

This project is MIT licensed - feel free to use it as a starting point for your own applications!
