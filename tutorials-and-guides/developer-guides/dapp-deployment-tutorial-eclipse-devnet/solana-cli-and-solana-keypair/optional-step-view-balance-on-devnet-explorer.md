# Optional Step: View Balance on Devnet Explorer

After obtaining devnet tokens for transaction fees, an excellent way to verify your account's balance and ensure everything is set up correctly is by using the Eclipse blockchain's devnet explorer. This online tool allows you to view transaction history, account balances, and other blockchain data by simply entering your public key. This step is optional but recommended for a comprehensive check of your account status.

**Why Check Your Balance on the Explorer**

* **Verification**: Confirming your balance on the blockchain explorer provides an additional layer of verification outside the CLI environment.
* **Transparency**: Viewing your account on the explorer allows you to see transactions and balances as they are recorded on the blockchain, offering transparency and peace of mind.

**How to Check Your Balance**

1. **Access the Eclipse Blockchain Devnet Explorer**:
   * Open your web browser and navigate to the Eclipse blockchain's devnet explorer at [https://explorer.dev.eclipsenetwork.xyz/](https://explorer.dev.eclipsenetwork.xyz/).
2. **Find Your Public Key**:
   *   If you don’t already have your public key handy, you can retrieve it by running the following command in your terminal:

       ```bash
       solana address
       ```
   * This command displays the public key associated with your current Solana CLI configuration, which corresponds to your devnet account address.
3. **Search for Your Public Key**:
   * On the devnet explorer page, locate the search bar at the top. Enter your public key into this search bar and press Enter or click the search icon.
   *   The explorer will navigate to a page detailing the account associated with the public key you entered. Here, you can view your current balance, recent transactions, and other relevant account information.\


       <figure><img src="https://lh7-us.googleusercontent.com/Iu4JvARqrw5nPl64k3XIAxpBwnLkZijT3lo-Xucjajd8S6Ifj1rD9U-ypiW-l6VDiFL_AV-ECoZF0yFfxf8Eq_yEHM_dO9pG4T6PMm64kUonL59B9we-3Z_WE0fpticq6q-5xEnSX91i-Ki-CgbIcf4" alt=""><figcaption><p>Eclipse Devnet Explorer</p></figcaption></figure>

**Troubleshooting**

* **No Results Found**: If the explorer does not display any information for your public key, ensure that the key is correct and that you have successfully claimed devnet tokens. Remember, blockchain explorers can only display information for accounts that have been activated or have transaction history on the network.

**Next Steps**

* With your balance verified on the Eclipse blockchain's devnet explorer, you have an additional confirmation that your setup is correctly configured and ready for development activities. This check ensures that you are fully prepared to deploy and test smart contracts, interact with the blockchain, and engage in further development tasks with confidence.
