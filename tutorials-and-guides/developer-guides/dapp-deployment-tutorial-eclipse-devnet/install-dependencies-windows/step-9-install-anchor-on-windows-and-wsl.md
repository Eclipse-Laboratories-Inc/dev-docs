# Step 9: Install Anchor on Windows and WSL

Anchor is a framework for Solana smart contract development. It simplifies the process of writing, testing, and deploying programs on SVM blockchains. To ensure compatibility and flexibility in your development environment, it's recommended to install Anchor both on your Windows system and within WSL. This section covers the installation process for both environments.

**Install Anchor in Windows Terminal**

1. **Open Windows Terminal as Administrator**:
   * This is necessary to ensure that the installation process has the required permissions.
2. **Install Anchor CLI**:
   *   Run the following command in the terminal:

       ```bash
       cargo install --git https://github.com/project-serum/anchor anchor-cli --locked
       ```
   *   This command downloads and installs the latest version of Anchor CLI from the official GitHub repository.\


       <figure><img src="https://lh7-us.googleusercontent.com/BQn236M-Z9efjwwQjW0MJTgpGtpvJ1W94QYCi22gdeP7DGfwEIGmukF0fN1DXS7RYfmy-I8d2MqJni2mTxdxFwf2alV583TyyW5LFQQlouPU4pqMMsA_2uv-SsRLg9qVUsRAqYtPb0_Dm9nU5um2zms" alt=""><figcaption><p>Example Output</p></figcaption></figure>
3. **Verify Installation**:
   *   To check that Anchor was installed correctly, type:

       ```bash
       anchor --version
       ```
   * The terminal should display the version of Anchor CLI installed.

**Install Anchor in WSL**

To install Anchor within your WSL environment, follow these steps:

1. **Open WSL Terminal**:
   * Launch your WSL terminal from the Start menu or through VS Code's Remote - WSL extension.
2. **Install Anchor CLI**:
   *   Execute the same Cargo command within your WSL terminal:

       ```bash
       cargo install --git https://github.com/project-serum/anchor anchor-cli --locked
       ```
   * This ensures that Anchor CLI is available in your Linux environment, offering consistency across your development platforms.
3. **Verify Installation**:
   *   Confirm the installation by checking the Anchor version in WSL:

       ```bash
       anchor --version
       ```
   * The version number indicates that Anchor CLI is ready for use within WSL.
