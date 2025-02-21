# Step 7: Verify Program Deployment on the Eclipse Devnet Explorer

After deploying your smart contract to the Solana blockchain, it's essential to verify that the deployment was successful and the program is now live. This can be done using the Eclipse blockchain's devnet explorer, a powerful tool for inspecting transactions, accounts, and programs on the blockchain.

**Verifying Your Deployment**

1. **Obtain Your Program ID**:
   * After deploying your program, the terminal outputs a Program ID. Copy this ID; you'll need it for verification.
2. **Visit the Eclipse Devnet Explorer**:
   * Open a web browser and navigate to the Eclipse Devnet Explorer at [https://explorer.dev.eclipsenetwork.xyz/](https://explorer.dev.eclipsenetwork.xyz/).
3. **Search for Your Program ID**:
   * In the Explorer's search bar, paste your Program ID and press Enter or click the search icon.
   *   The Explorer will display information related to your Program ID, including the program's deployment status, recent transactions involving the program, and more.\


       <figure><img src="../../../../.gitbook/assets/image (39).png" alt=""><figcaption><p>Eclipse Devnet Explorer</p></figcaption></figure>

**What to Look for**

* **Account Information**: Ensure that the account details page confirms the account type as a Program and that it is associated with your deployment.
* **Transaction History**: Check for the initial deployment transaction, which should be listed among the recent activities for your Program ID.

**Troubleshooting**

* **No Results Found**: If the Explorer does not show your program or reports it as not found, double-check the Program ID for any typos. If it still doesn't appear, there may have been an issue with the deployment process. Consider redeploying your program and ensuring the terminal confirms successful deployment.
* **Incorrect Information**: If the displayed information does not match your expectations (e.g., wrong account type or no deployment transaction), verify that you're looking at the correct network (devnet) and that the Program ID matches exactly with what was output during deployment.
