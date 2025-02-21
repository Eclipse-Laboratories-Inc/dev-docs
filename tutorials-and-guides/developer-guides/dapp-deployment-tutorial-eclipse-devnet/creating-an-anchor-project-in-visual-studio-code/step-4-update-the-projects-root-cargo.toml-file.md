# Step 4: Update the Project's Root Cargo.toml File

This step involves editing the `Cargo.toml` file located at the root of your Anchor project. The modifications will define the project workspace and optimize the Rust compiler's release profile settings for your smart contract, ensuring efficient compilation and deployment.

**Instructions**

1. **Locate the Root `Cargo.toml` File**:
   * In Visual Studio Code, go to the root directory of your Anchor project.
   * Find and open the `Cargo.toml` file. This is different from the `Cargo.toml` within the `programs/nft-minter` directory.
2. **Replace Existing Content with the Following Configuration**:
   *   Clear the current contents of the file and paste the new configuration:

       ```toml
       [workspace]
       members = [
           "programs/nft-minter"
       ]
       resolver = "2"

       [profile.release]
       overflow-checks = true
       lto = "fat"
       codegen-units = 1

       [profile.release.build-override]
       opt-level = 3
       incremental = false
       codegen-units = 1
       ```
   * This updated configuration includes:
     * **Workspace Members**: Specifies the paths to workspace members, ensuring Cargo recognizes your smart contract as part of the project.
     * **Resolver Version**: Uses version 2 of Cargo's feature resolver, which can affect how dependencies are selected and compiled.
     * **Release Profile Optimizations**: Adjusts settings for the release build, including enabling overflow checks, using "fat" Link Time Optimization (LTO) for better optimization, and setting code generation units to 1 for more efficient compilation.
     * **Build-Override Settings**: Further optimizes release builds by setting the optimization level to 3, disabling incremental compilation, and enforcing a single code generation unit for consistency across builds.
