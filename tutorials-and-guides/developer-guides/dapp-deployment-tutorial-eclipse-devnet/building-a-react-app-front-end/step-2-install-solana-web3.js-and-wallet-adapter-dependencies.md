# Step 2: Install Solana Web3.js and Wallet Adapter Dependencies

Integrating your React app with the Eclipse Devnet and providing wallet connectivity are essential steps in enabling users to interact with your NFT minter. This requires installing several packages related to Solana's web3.js library and wallet adapters. Here's how to proceed with the installation of these dependencies.

**Install Core Dependencies**

1. **Open Your Project's Terminal**:
   * Ensure you're in the root directory of your `nftminterfrontend` React project in your terminal or command line interface.
2. **Execute the Installation Command**:
   *   Run the following command to install the Solana web3.js library and wallet adapter packages:

       ```bash
       npm install @solana/web3.js @solana/wallet-adapter-react @solana/wallet-adapter-wallets @solana/wallet-adapter-react-ui
       ```
   * This command installs the core packages required for interacting with the Solana blockchain and integrating wallet functionality into your React app.

**Install Additional Dependencies**

Some functionalities and wallet integrations require additional packages. Ensure a comprehensive setup by installing these additional dependencies:

1. **Install Extended Dependencies**:
   *   Run the following npm command to include all necessary libraries:

       ```bash
       npm install @solana/web3.js @solana/wallet-adapter-react @solana/wallet-adapter-react-ui @solana/wallet-adapter-wallets @solana/wallet-adapter-base @project-serum/sol-wallet-adapter @emotion/react @emotion/styled
       ```
   * These packages provide the foundational components for Solana blockchain communication, various wallet integrations, and UI components styled with Emotion (a library for writing CSS styles with JavaScript).

**Verify Installation**

* After installing the dependencies, it's good practice to verify that they are correctly added to your project. Check your `package.json` file to see the added libraries under the `dependencies` section.
