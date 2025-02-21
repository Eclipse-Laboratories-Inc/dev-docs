# Step 1: Initialize Anchor Project

Anchor is a framework that simplifies SVM development, providing tools and abstractions for building Eclipse programs. Initializing an Anchor project sets up the necessary project structure and configurations.

**Prerequisites**

* Ensure you have Anchor and Solana CLI installed. If not, refer to earlier sections of this guide for installation instructions.
* Visual Studio Code (VS Code) should be installed and configured to use WSL (Windows Subsystem for Linux) if you are on a Windows machine.

**Project Initialization**

1. **Open Visual Studio Code**:
   * Launch Visual Studio Code. If you're working within WSL, make sure VS Code is connected to your WSL instance.
2. **Create a New Directory**:
   * Open a terminal in VS Code (`Terminal` > `New Terminal`) and navigate to the location where you want to create your project.
   *   Create a new directory for your project with:

       ```bash
       dir NFTMinter
       ```
   *   Navigate into your newly created directory:

       ```bash
       cd NFTMinter
       ```
3. **Initialize Anchor Project**:
   *   Within your project directory, initialize a new Anchor project by running:

       ```bash
       anchor init NFTMinter --javascript
       ```
   *   This command creates a new Anchor project named "NFTMinter" and sets it up to use JavaScript for the project's scripts. Anchor will generate a basic project structure, including a lib folder for your program's logic and a tests folder for your project's test scripts.

       <figure><img src="https://lh7-us.googleusercontent.com/hJwQd3QQkLb-0DlplMDmedSTy8IXpwtEKm-qU7ZsMyER-AXXbzO1OLSey58Uw31X3L4oJVi8p3_aAYbvFgiBZwvFP_08MlvrrYl4MGyU6Q02BQwkfs42sfvW8kjtsmx-ohTd4UNNC3uehd0L2O3zEcU" alt=""><figcaption><p>Example Output</p></figcaption></figure>
4. **Examine Project Structure**:
   *   After initialization, take a moment to explore the created project structure. You can use the VS Code explorer pane to navigate through the files and folders. You’ll find:

       * `Anchor.toml`: The configuration file for your Anchor project.
       * `programs/`: Directory where your program source code will reside.
       * `tests/`: Directory for your project's test scripts.
       * `app/`: A template for a front-end application if you decide to develop one.\


       <figure><img src="../../../../.gitbook/assets/image (41).png" alt=""><figcaption><p>Example Directory</p></figcaption></figure>

**Next Steps**

* With your Anchor project initialized, you’re ready to begin smart contract development. The next steps involve writing your smart contract logic within the `programs/` directory and creating tests to ensure your contract works as expected.
