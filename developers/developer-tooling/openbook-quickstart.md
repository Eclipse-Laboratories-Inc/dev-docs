# OpenBook Quickstart

{% hint style="info" %}
The goal of this section is to develop a smart contract to interact with the OpenBook deployment on the Eclipse Devnet
{% endhint %}

### Prerequisites[​](https://icarus131.github.io/devcookbook/docs/DevCookBook#prerequisites) <a href="#prerequisites" id="prerequisites"></a>

**Install and configure Rust**[**​**](https://icarus131.github.io/devcookbook/docs/DevCookBook#install-and-configure-rust)

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

**Check if Rust is installed**[**​**](https://icarus131.github.io/devcookbook/docs/DevCookBook#check-if-rust-is-installed)

```bash
rustc --version
cargo --version
```

{% hint style="warning" %}
WARNING

**Make sure you add the Rust toolchain to your PATH**
{% endhint %}

**Make sure you add the Rust toolchain to your PATH**

**Finally, let's go ahead and install the Solana CLI tools**[**​**](https://icarus131.github.io/devcookbook/docs/DevCookBook#finally-lets-go-ahead-and-install-the-solana-cli-tools)

```bash
sh -c "$(curl -sSfL https://release.solana.com/stable/install)"
```

### Switching to the Eclipse Devnet[​](https://icarus131.github.io/devcookbook/docs/DevCookBook#switching-to-the-eclipse-devnet) <a href="#switching-to-the-eclipse-devnet" id="switching-to-the-eclipse-devnet"></a>

**Now set your solana cli to the Eclipse Devnet**[**​**](https://icarus131.github.io/devcookbook/docs/DevCookBook#now-set-your-solana-cli-to-the-eclipse-devnet)

```bash
solana config set --url https://staging-rpc.dev2.eclipsenetwork.xyz
```

**Tokens**[**​**](https://icarus131.github.io/devcookbook/docs/DevCookBook#tokens)

```bash
solana airdrop 10
```

**Now you should be good to go!**[**​**](https://icarus131.github.io/devcookbook/docs/DevCookBook#now-you-should-be-good-to-go)

***

### Writing the smart contract:[​](https://icarus131.github.io/devcookbook/docs/DevCookBook#writing-the-smart-contract) <a href="#writing-the-smart-contract" id="writing-the-smart-contract"></a>

#### Interfacing with OpenBook[​](https://icarus131.github.io/devcookbook/docs/DevCookBook#interfacing-with-openbook) <a href="#interfacing-with-openbook" id="interfacing-with-openbook"></a>

* We need our smart contract to connect with the OpenBook deployment on Eclipse
* OpenBook essentially allows us keep records of the trades and transactions that take place on our network.
* The Eclipse OpenBook deployment has its own API which is essential for us to interact with it.
* We will mostly follow the standard deployment process for any Solana app
* We will also need the programId for the OpenBook deployment on the Eclipse Devnet, which is as follows: `xY9r3jzQQLYuxBycxfT31xytsPDrpacWnyswMskn16s`
* Let's begin by writing out our `lib.rs` file.
* First we need to include the anchor crates and declare the programId

```rust
use anchor_lang::prelude::*;

declare_id!("<smart_contract_programId>");

...

```

* Now let's go ahead and write the logic for our smart contract here.
* Since everything is an account on Solana, we need to define our accounts here.
* We can also add any required function and its logic here

```rust
...

#[program]
pub mod my_smart_contract {
    use super::*;

    #[derive(Accounts)]
    pub struct MyAccounts<'info> {
    }

    #[access_control(allow)]
    pub fn trade(ctx: Context<MyAccounts>) -> ProgramResult {
        Ok(())
    }
}
...
```

* Now let's go ahead and interact with the OpenBook deployment.
* First let's connect to the Eclipse devnet by supplying the RPC url
* Now we can use the programId to connect to the OpenBook deployment
* After this we can write whatever API calls we need

```rust
...

#[tokio::main]
async fn main() {
    let ecl_rpc = "https://staging-rpc.dev2.eclipsenetwork.xyz";
    let provider = Provider::new(ecl_rpc).await.unwrap();

    let dex_program_id = Pubkey::from_str("xY9r3jzQQLYuxBycxfT31xytsPDrpacWnyswMskn16s").unwrap();
    // First we need to perform authorization based on the token. This is the first account.
    struct Identity;

    impl Identity {
      fn prepare_pda<'info>(acc_info: &AccountInfo<'info>) -> AccountInfo<'info> {
        let mut acc_info = acc_info.clone();
        acc_info.is_signer = true;
        acc_info
      }
  }
    // Now to interact with the deployment
  impl MarketMiddleware for Identity {
    // Here is an example API call
        fn new_order_v3(&self, ctx: &mut Context, _ix: &mut NewOrderInstructionV3) -> ProgramResult {
        verify_and_strip_auth(ctx)
    }
    // Note that these calls are based on the public crate that is available for the deployment. This example assumes the availability of the default crates from a standard openbook deployment.
  }
}

...

```

#### Deploying the smart contract:[​](https://icarus131.github.io/devcookbook/docs/DevCookBook#deploying-the-smart-contract) <a href="#deploying-the-smart-contract" id="deploying-the-smart-contract"></a>

* To deploy the smart contract with anchor we can just do the following:

```
  anchor build
```

And then

```
  anchor deploy
```

* Using this template you can write your own trade functions and deploy it on the Eclipse Devnet.
