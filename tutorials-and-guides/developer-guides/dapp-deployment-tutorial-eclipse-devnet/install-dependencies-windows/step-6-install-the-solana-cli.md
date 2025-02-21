# Step 6: Install the Solana CLI

The Solana Command Line Interface (CLI) is a powerful tool that enables developers to interact with the SVM based blockchains, deploy programs, and manage accounts. This step guides you through the installation of the Solana CLI on your Windows system.

1. **Run Visual Studio Code as Administrator**:
   * Before proceeding with the installation commands, ensure that you run Visual Studio Code as an administrator. This permission level is required to install the Solana CLI tools correctly.
2. **Install Solana CLI**:
   *   Open a new terminal in Visual Studio Code (run as administrator) and execute the following commands to download and install the Solana CLI:

       *   Download the Solana Install Initiation Script:

           ```
           cmd /c "curl https://release.solana.com/v1.18.3/solana-install-init-x86_64-pc-windows-msvc.exe --output C:\solana-install-tmp\solana-install-init.exe --create-dirs"
           ```
       *   Run the Installer:

           ```cmd
           C:\solana-install-tmp\solana-install-init.exe v1.18.3
           ```

       These commands download the Solana installer to a temporary directory and then execute it to install the Solana CLI of the specified version.
3. **Verify Installation**:
   *   After the installation process completes, you can verify the installation by running the following command in the terminal:

       ```bash
       solana --version
       ```
   * This command should output the version of the Solana CLI that you have installed (v1.18.3), confirming that the installation was successful.

<figure><img src="https://lh7-us.googleusercontent.com/SUvOhmEeBSbN810uMqgX_0kIWNQLMPdbB7VkRdCsxbaJjKSKDumgne8VX0gFQREWHOlZG5DRB2GwJsABj9KgfgDfhQOBLnp13P465Ze3a15Ni1GDv0qMAvSWbcLRZnEcpWErQRPiZB1Jc0dANLQ3Uyw" alt=""><figcaption><p>Example Output</p></figcaption></figure>
