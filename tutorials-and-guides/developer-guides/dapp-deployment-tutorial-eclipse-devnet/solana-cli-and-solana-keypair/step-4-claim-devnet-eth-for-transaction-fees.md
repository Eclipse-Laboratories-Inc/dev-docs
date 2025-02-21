# Step 4: Claim Devnet ETH for Transaction Fees

To deploy your smart contract to the Eclipse blockchain's devnet, you'll need devnet ETH to cover transaction fees. Despite the use of Ethereum as the transaction currency, the Solana CLI provides a convenient method for claiming devnet tokens, which in this context, are used similarly to ETH for transaction fees on the Eclipse blockchain's devnet. This step will guide you through the process of obtaining these tokens.

**Why You Need Devnet ETH**

* **Transaction Fees**: Deploying smart contracts and executing transactions on the blockchain requires paying fees, which are denoted in ETH within the Eclipse devnet environment.
* **Testing Environment**: Having devnet ETH allows you to test your smart contracts thoroughly, simulating real-world transaction conditions without the need for real ETH.

**Claiming Devnet ETH**

1. **Open Your Terminal**:
   * Ensure you have access to your terminal or command line interface where the Solana CLI is installed. Windows users should use WSL for this process.
2. **Confirm CLI Configuration for Devnet**:
   * Before proceeding, ensure that your Solana CLI is correctly configured to interact with the Eclipse blockchain's devnet. This setup should have been completed in previous steps.
3. **Execute the Airdrop Command**:
   *   To claim devnet ETH, use the same Solana CLI airdrop command, which in this context, facilitates the acquisition of the necessary tokens for development activities:

       ```bash
       solana airdrop 10
       ```
   *   This command requests 10 units of the devnet currency, which, for the purposes of Eclipse blockchain development, will be used as if it were ETH. Adjust the amount as needed based on your testing requirements.\


       <figure><img src="https://lh7-us.googleusercontent.com/sRGKVe5kyWk0mivls04qWA7G6wda8pJYslV8sOesYlBYPR4W41gHPZ7iLd2w-Ensq-VRaaumvDenNF2y8fZ4z9IQ_-sF-whS8Jb8QwCW-3ArldpplnMS41YuTfAO545OBOd296EGtkpAdjwT1gl1JpY" alt=""><figcaption><p>Example Output</p></figcaption></figure>
4. **Verify Receipt of Tokens**:
   *   After requesting the airdrop, confirm that you've received the tokens by checking your balance:

       ```bash
       solana balance
       ```
   * The output will show your updated balance, reflecting the receipt of the devnet tokens.

**Troubleshooting**

* **Issues with Airdrop**: If the airdrop doesn't seem to work, double-check that your configuration is set to the Eclipse blockchain's devnet. Use `solana config get` to verify your settings.
* **Delay in Token Receipt**: Network congestion or delays can occasionally affect the timing of the airdrop. If your balance doesn't update immediately, give it a few minutes and check again.

**Next Steps**

* With the necessary devnet ETH in your account, you're prepared to move forward with deploying and testing your smart contracts on the Eclipse blockchain's devnet. This crucial step ensures that you can conduct thorough testing in a realistic but safe environment before any mainnet deployment.
