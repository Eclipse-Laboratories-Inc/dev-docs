# Step 6: Deploy Your Project to the Eclipse Devnet

Deploying your compiled smart contract to the Eclipse Devnet is a pivotal step, marking the transition from development to live operation. This step involves uploading your program to the blockchain, which assigns it a unique Program ID for interaction.

**Prerequisites**

* Ensure the Solana CLI is correctly configured for the intended network (e.g., devnet) and your wallet is funded with enough SOL to cover deployment costs.
* Confirm your project has been compiled successfully, producing the `.so` file in the `target/deploy/` directory.

**Deployment Process**

1. **Access Terminal in Visual Studio Code**:
   * Open the terminal within VS Code, ensuring you're in the root directory of your project.
2. **Execute Deployment Command**:
   *   Run the following command to deploy your smart contract:

       ```bash
       solana program deploy target/deploy/nft_minter.so
       ```
   *   This command uploads the `nft_minter.so` file to the Eclipse Devnet, and the CLI outputs the Program ID upon successful deployment. Note this Program ID for future transactions and interactions with your program.\


       <figure><img src="https://lh7-us.googleusercontent.com/V68rUuJVdJNKmlHXST3ZgOapT8IopZ7vYByx3xvHy0-7GGqNnSvnnbww4SohSQR_tNYyZbtnggCjVx23C7uhVskozd4CYx-KTAz2OhPLuNWnsBocOiOJiHUX7kf5UttLkfkHbIXNjVSiM868fV9D1uM" alt=""><figcaption><p>Example Output</p></figcaption></figure>

**Troubleshooting**

* **Funding Issues**: If you encounter errors related to insufficient funds, consider using the `solana airdrop` command to acquire ETH for the deployment fee.
* **Network Configuration**: Confirm you're connected to the appropriate Eclipse network. Use `solana config get` to verify your current settings.
