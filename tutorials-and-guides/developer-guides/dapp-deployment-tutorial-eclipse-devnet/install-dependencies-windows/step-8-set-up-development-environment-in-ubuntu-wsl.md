# Step 8: Set Up Development Environment in Ubuntu WSL

After installing the necessary tools and extensions, it's time to switch your development environment to Ubuntu on WSL and set up Rust, Cargo, and Solana. This step ensures you have all the necessary components for Solana blockchain development within a Linux environment.

**Switch Visual Studio Code to Use Ubuntu WSL**

1. **Open Visual Studio Code**:
   * Launch Visual Studio Code on your Windows system.
2. **Open a New Terminal**:
   * Open a new terminal in VS Code by going to `Terminal` > `New Terminal` or using the shortcut `` Ctrl+` `` .
3.  **Select WSL: Ubuntu from the Terminal Dropdown**:

    * In the new terminal window, you'll see a dropdown menu at the top right corner. Click on it and select "Select Default Profile".
    * Choose "WSL: Ubuntu" (or the specific version of Ubuntu you installed) from the list. This changes the terminal to use the Ubuntu environment within WSL for the current and future terminal sessions.

    <figure><img src="../../../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>
4. **Navigate to Your Project Directory**:
   *   Use the `cd` command to change to your project directory. For example:

       ```bash
       cd /path/to/your/project
       ```

**Install Rust on WSL**

1. **Install Rust**:
   *   Execute the following command in the Ubuntu terminal to install Rust:

       ```bash
       curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
       ```
   * Follow the on-screen instructions to complete the Rust installation process.

**Install Cargo on WSL**

1. **Verify Cargo Installation**:
   *   Installing Rust with rustup also installs Cargo. You can verify Cargo is installed by running:

       ```bash
       cargo --version
       ```
   * This command should display the version of Cargo installed, confirming its availability.

**Install Solana on WSL**

1. **Install Solana Toolkit**:
   *   To install the Solana toolkit, including the CLI tools, run:

       ```bash
       sh -c "$(curl -sSfL https://release.solana.com/stable/install)"
       ```
   * This command downloads and installs the latest stable version of the Solana toolkit.
2. **Verify Solana Installation**:
   *   Ensure the installation was successful by checking the Solana version with:

       ```bash
       solana --version
       ```
   * The command output should display the version of Solana installed, indicating that it is ready for use in your development projects.
