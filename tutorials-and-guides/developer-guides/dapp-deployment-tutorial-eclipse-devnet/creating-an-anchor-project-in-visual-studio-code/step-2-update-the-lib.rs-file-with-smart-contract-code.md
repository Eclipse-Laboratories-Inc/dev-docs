# Step 2: Update the lib.rs File with Smart Contract Code

In this step, you'll define the functionality for NFT minting within your project by updating the `lib.rs` file with provided Rust code. This smart contract code includes the structure and logic necessary for creating NFTs.

**Instructions**

1. **Access the `lib.rs` File**:
   * Use Visual Studio Code to navigate to `programs/nftminter/src/` in your project's directory.
   * Open the `lib.rs` file. Initially, this contains Anchor's default scaffolded code.
2. **Update the File with Provided Code**:
   *   Erase the existing content and insert the following example code or your own code:

       ```rust
       use anchor_lang::prelude::*;
       use anchor_spl::token::{self, MintTo, Token, TokenAccount};

       declare_id!("28yv9AxVwUtw1HtDYin7JS3F3x1Z2G9cqAdowUb3iCs6");

       #[program]
       pub mod nftminter {
           use super::*;
           pub fn mint_nft(ctx: Context<MintNft>, name: String, description: String, image_uri: String) -> Result<()> {
               let nft_info = &mut ctx.accounts.nft_info;
               nft_info.name = name;
               nft_info.description = description;
               nft_info.image_uri = image_uri;
               Ok(())
           }
       }

       #[derive(Accounts)]
       pub struct MintNft<'info> {
           #[account(init, payer = user, space = 1024)]
           pub nft_info: Account<'info, NftInfo>,
           #[account(mut)]
           pub user: Signer<'info>,
           pub system_program: Program<'info, System>,
       }

       #[account]
       pub struct NftInfo {
           pub name: String,
           pub description: String,
           pub image_uri: String,
       }
       ```
   * This code establishes the functionality for your NFT minter, including the `mint_nft` function and the `NftInfo` structure to hold NFT metadata
3. **Key Components Overview**:
   * The `use` statements bring necessary modules into scope.
   * `declare_id!` uniquely identifies your smart contract on the blockchain.
   * The `#[program]` attribute and subsequent module define the contract's executable functions.
   * `#[derive(Accounts)]` specifies the accounts context required for the `mint_nft` function.
   * `NftInfo` struct holds the NFT's metadata.

{% hint style="success" %}
If you already have your own code that differs from this NFT example, feel free to paste that instead.
{% endhint %}

**Next Steps**

* With the smart contract code in place, you're set to further develop, test, and eventually deploy your NFT minter.
