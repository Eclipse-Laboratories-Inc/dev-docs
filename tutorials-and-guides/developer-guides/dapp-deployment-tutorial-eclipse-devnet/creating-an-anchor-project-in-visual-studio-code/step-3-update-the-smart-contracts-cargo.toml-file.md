# Step 3: Update the Smart Contract's Cargo.toml File

This step focuses on precisely configuring the `Cargo.toml` file for your smart contract, located in the `programs/nftminter` directory. Updating this file ensures your project has the correct dependencies and settings for compilation and deployment.

**Instructions**

1. **Open the Smart Contract's `Cargo.toml`**:
   * In Visual Studio Code, navigate to the `programs/nftminter` directory within your project.
   * Open the `Cargo.toml` file located in this directory.
2. **Update the File with the Following Configuration**:
   *   Replace the existing content of the `Cargo.toml` file with the config below or your own:

       ```toml
       [package]
       name = "nft-minter"
       version = "0.1.0"
       description = "Created with Anchor"
       edition = "2021"

       [lib]
       crate-type = ["cdylib", "lib"]
       name = "nft_minter"

       [features]
       no-entrypoint = []
       no-idl = []
       no-log-ix-name = []
       cpi = ["no-entrypoint"]
       default = []

       [dependencies]
       anchor-lang = "0.29.0"
       anchor-spl = "0.29.0"
       ```
   * This configuration sets up your project with specific settings and dependencies:
     * **Package Information**: Names your project `nft-minter`, sets the version, and specifies it was created with Anchor.
     * **Library Settings**: Defines the type of Rust library being created and its name.
     * **Features**: Configures various Anchor features and compilation options.
     * **Dependencies**: Specifies the versions of `anchor-lang` and `anchor-spl` that your project depends on, ensuring compatibility with your Anchor framework version.
3.  **Save Your Changes**:

    * After updating the `Cargo.toml` file, save it to apply the changes.

    <figure><img src="../../../../.gitbook/assets/image (38).png" alt=""><figcaption><p>Loaded &#x26; Verified Dependencies</p></figcaption></figure>

