# Step 3: Generate a New Solana Keypair

A keypair in Solana/Eclipse consists of a public key (wallet address) and a private key (secret key), which are essential for interacting with the blockchain. The public key is used to receive funds or identify your account, while the private key is used to sign transactions securely. If you're new to the Solana CLI, generating a new keypair is one of the first steps you'll need to take to start working with the Eclipse blockchain.

**Why You Need a Keypair**

* **Identity**: Your keypair serves as your identity on the blockchain, allowing you to own and transact with ETH and other tokens.
* **Security**: The private key in the keypair is used to sign transactions, ensuring that only the key owner can authorize actions associated with the account.

**Generating a Keypair**

1. **Open Your Terminal**:
   * Ensure you have access to your terminal or command line interface where the Solana CLI is installed. Windows users should use WSL for this process.
2. **Run the Keypair Generation Command**:
   *   Enter the following command to generate a new keypair:

       ```bash
       solana-keygen new
       ```
   * The Solana CLI will generate a new keypair and save the keys to a file in your home directory (by default). It will also display the public key, which represents your new Eclipse blockchain address.
3. **Secure Your Keypair**:
   *   Upon generating a new keypair, the CLI will prompt you to save the seed phrase associated with your keypair. It's crucial to write down this seed phrase and store it in a secure location. The seed phrase can be used to recover your keypair in case you lose access to your computer or the file where your keys are stored.\


       <figure><img src="https://lh7-us.googleusercontent.com/YZlZWEZprfTTBgrIhOsY3TbvsDDcToU7xvclS01cqBvN5Xe4qtEYVFNKi5kdFmU0OuSUI_CnjUkwRqZ0zhLcmgWRrTvxIUOo02Ebqq6XmWI-Du-MyWgZyVLaQRLt-ippxh7PwRsU0RuKMkpbj2quFLw" alt=""><figcaption><p>Example Output</p></figcaption></figure>

**Next Steps**

* With your new keypair generated, you're now equipped with a fundamental tool required for Eclipse blockchain development. This keypair will be used for various tasks, including sending and receiving ETH, deploying programs, and interacting with contracts on the blockchain.
* Remember to keep your private key and seed phrase secure at all times. Anyone with access to your private key can control your blockchain assets.

**Troubleshooting**

* If you encounter any issues during the keypair generation process, ensure that your Solana CLI is correctly installed and updated to the latest version. Running `solana --version` can verify your CLI version.
