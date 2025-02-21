# Program JSX & CSS

Here's the complete JSX code for `App.tsx`:

```jsx
import React, { useEffect, useState } from 'react';
import * as web3 from '@solana/web3.js';
import './SolanaFeeEstimator.css';

const SolanaFeeEstimator: React.FC = () => {
  const [fee, setFee] = useState<number | null>(null);
  const [error, setError] = useState<string>('');
  const [senderAddress, setSenderAddress] = useState<string>('');
  const [receiverAddress, setReceiverAddress] = useState<string>('');
  const [rpcUrl, setRpcUrl] = useState<string>('');

  useEffect(() => {
    const estimateTransactionFee = async () => {
      try {
        const customRpcUrl = "https://testnet.dev2.eclipsenetwork.xyz";
        const connection = new web3.Connection(customRpcUrl);
        setRpcUrl(customRpcUrl);

        const transaction = new web3.Transaction();
        const senderPublicKey = new web3.PublicKey('8HE3zo1iwCGnzcdVgrbRXikSYoBmv9MAaJLBAAkcmBBd');
        const recipientPublicKey = new web3.PublicKey('B3xJNQ8LKSStbjzCD3EMPq3xdcav6mmMWBsHAFE9TAkT');
        const amountInLamports = 1_000_000_000; // Example amount for transferring 1 ETH

        setSenderAddress(senderPublicKey.toString());
        setReceiverAddress(recipientPublicKey.toString());

        const transferInstruction = web3.SystemProgram.transfer({
          fromPubkey: senderPublicKey,
          toPubkey: recipientPublicKey,
          lamports: amountInLamports
        });
        transaction.add(transferInstruction);

        const { blockhash } = await connection.getLatestBlockhash();
        transaction.recentBlockhash = blockhash;
        transaction.feePayer = senderPublicKey;

        const message = transaction.compileMessage();
        const estimatedFee = await connection.getFeeForMessage(message, 'recent');

        if (estimatedFee.value === null) {
          throw new Error('Failed to estimate fee');
        }

        setFee(estimatedFee.value);
      } catch (err) {
        setError(err instanceof Error ? err.message : 'An unknown error occurred');
      }
    };

    estimateTransactionFee();
  }, []);

  return (
    <div>
      <h1>Eclipse Testnet ETH Transfer Transaction Fee Estimator</h1>
      <p>Connected RPC URL: {rpcUrl}</p>
      <p>Sender Address: {senderAddress}</p>
      <p>Receiver Address: {receiverAddress}</p>
      {fee !== null ? (
        <p>Estimated Fee: {fee} wei</p>
      ) : error ? (
        <p>Error: {error}</p>
      ) : (
        <p>Estimating fee...</p>
      )}
    </div>
  );
};

export default SolanaFeeEstimator;
```

Here is the complete CSS code for `SolanaFeeEstimator.css`:

```css
/* SolanaFeeEstimator.css */

/* Define color variables for light and dark themes */
:root {
    --background-color-light: #f4f7f6;
    --text-color-light: #333;
    --highlight-color: #010c00; /* Highlight color */
    --box-background-color-light: #ffffff;
  
    /* Dark mode colors */
    --background-color-dark: #121212;
    --text-color-dark: #110101;
    --box-background-color-dark: #1e1e1e;
  }
  
  body {
    font-family: 'Helvetica Neue', Arial, sans-serif;
    background-color: var(--background-color-light);
    color: var(--text-color-light);
    padding: 20px;
    margin: 0;
    line-height: 1.6;
    transition: background-color 0.3s ease, color 0.3s ease;
  }
  
  @media (prefers-color-scheme: dark) {
    body {
      background-color: var(--background-color-dark);
      color: var(--text-color-dark);
    }
  
    div {
      background-color: var(--box-background-color-dark);
    }
  
    h1, p, a {
      color: var(--text-color-dark);
    }
  }
  
  div {
    background-color: var(--box-background-color-light);
    border-radius: 12px;
    box-shadow: 0 6px 10px rgba(0, 0, 0, 0.1);
    padding: 30px;
    max-width: 800px; /* Adjusted for wider layout */
    margin: 50px auto;
    text-align: center;
    transition: transform 0.3s ease-in-out, background-color 0.3s ease;
  }
  
  div:hover {
    transform: translateY(-5px);
    box-shadow: 0 12px 20px rgba(0, 0, 0, 0.2);
  }
  
  h1 {
    color: var(--highlight-color); /* Using highlight color */
    font-size: 28px;
    margin-bottom: 30px;
    font-weight: 600;
  }
  
  p {
    font-size: 18px;
    margin: 15px 0;
  }
  
  p.error {
    color: #e57373; /* Softer red */
  }
  
  p.estimate {
    color: #81c784; /* Softer green */
  }
  
  /* Enhancements for links or additional elements */
  a {
    color: var(--highlight-color);
    text-decoration: none;
    transition: color 0.2s ease-in-out;
  }
  
  a:hover {
    color: darken(var(--highlight-color), 20%);
  }
  
  /* Responsive Design */
  @media (max-width: 768px) {
    div {
      margin: 30px 20px;
      padding: 20px;
    }
  
    h1 {
      font-size: 24px;
    }
  
    p {
      font-size: 16px;
    }
  }
```

This component efficiently encapsulates the functionality required to estimate transaction fees on the Eclipse testnet, providing a user-friendly interface for interacting with the blockchain's fee estimation capabilities.
