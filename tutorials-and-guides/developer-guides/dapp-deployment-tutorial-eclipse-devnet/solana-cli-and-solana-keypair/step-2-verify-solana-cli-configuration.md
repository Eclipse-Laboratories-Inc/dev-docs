# Step 2: Verify Solana CLI Configuration

After configuring the Solana CLI to connect to the Eclipse blockchain's staging environment, it's crucial to verify that the settings have been applied correctly. This ensures that your development activities are indeed targeting the correct network, allowing for a smooth development process. This step will guide you through verifying the current configuration of your Solana CLI.

**Why Verification Matters**

* **Accuracy**: Verification ensures that the CLI is configured to communicate with the intended RPC URL, preventing any unintended transactions or deployments on the wrong network.
* **Troubleshooting**: Identifying configuration issues early can save time by avoiding troubleshooting errors caused by incorrect network settings.

**Verification Instructions**

1. **Open Your Terminal**:
   * Access your terminal window where you have the Solana CLI available. Windows users should ensure they are using WSL for this process.
2. **Run the Configuration Get Command**:
   *   To display your current Solana CLI configuration, execute the following command:

       ```bash
       solana config get
       ```
   * This command retrieves and displays the current settings of your Solana CLI, including the RPC URL, default wallet path, and other relevant configurations.
3.  **Examine the Output**:

    *   Focus on the "RPC URL" field in the output. It should match the URL you previously set:

        ```arduino
        RPC URL: https://staging-rpc.dev2.eclipsenetwork.xyz
        ```
    * Confirming that the "RPC URL" matches the intended staging environment URL ensures that your CLI is correctly set up to interact with the Eclipse blockchain's devnet.

    <figure><img src="../../../../.gitbook/assets/image (37).png" alt=""><figcaption><p>Example Output</p></figcaption></figure>
