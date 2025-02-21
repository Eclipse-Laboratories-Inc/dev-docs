# Step 1: Set Solana CLI to Use Eclipse Devnet

When developing for the Eclipse blockchain, connecting to the appropriate network environment is crucial. For this tutorial, we'll be using the Eclipse devnet, specifically targeting its staging environment. This environment offers a testing ground that closely mirrors the mainnet, providing a realistic backdrop for development, testing, and deployment of your projects without the need for real cryptocurrency. Here's how to configure your Solana CLI to connect to this environment.

**Prerequisites**

* Solana CLI installed on your machine. Refer back to earlier sections if you need guidance on installing the Solana CLI.
* Windows users should utilize WSL (Windows Subsystem for Linux) for full compatibility.

**Configuration Instructions**

1. **Open Your Terminal**:
   * Launch your terminal application. Windows users are advised to use WSL by opening the Ubuntu application or any other Linux distribution you've installed through WSL.
2. **Execute the Configuration Command**:
   *   To configure the Solana CLI to communicate with the Eclipse blockchain's staging environment, enter and run the following command in your terminal:

       ```bash
       solana config set --url https://staging-rpc.dev2.eclipsenetwork.xyz
       ```
   * This command sets the Solana CLI's RPC URL to the Eclipse blockchain's devnet staging environment. It ensures all commands issued through the CLI are directed to this specific network.

**Understanding the Command**

* **RPC URL Configuration**: The `--url` parameter specifies the RPC endpoint that the Solana CLI will interact with. By setting it to `https://staging-rpc.dev2.eclipsenetwork.xyz`, you're telling the CLI to route all network requests to the Eclipse blockchain's devnet staging environment.
* **Target Network**: This staging environment is designed for developers to test their applications in a setting that simulates the actual conditions of the Eclipse blockchain's mainnet without the risk associated with real transactions.

**Verification and Next Steps**

* It's good practice to verify that your CLI is correctly configured after updating its settings. You can do this by running `solana config get` in your terminal. This command displays the current configuration of your Solana CLI, including the newly set RPC URL.
* With your CLI now pointing to the Eclipse blockchain's staging environment, you're set to begin the next phase of development, such as creating a wallet, deploying smart contracts, and interacting with the network.
