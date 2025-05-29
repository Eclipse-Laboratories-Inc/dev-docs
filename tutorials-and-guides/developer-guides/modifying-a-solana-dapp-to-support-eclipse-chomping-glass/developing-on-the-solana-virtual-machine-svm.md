# Developing on the Solana Virtual Machine (SVM)

#### What is Solana?[​](https://icarus131.github.io/devcookbook/docs/SVM#what-is-solana) <a href="#what-is-solana" id="what-is-solana"></a>

Solana is a high-performance blockchain platform designed for decentralized applications (dApps) and crypto-native projects. It distinguishes itself by offering fast transaction speeds, low fees, and scalability without sacrificing decentralization. At the core of Solana uses a proof-of-stake architecture, which allows for high throughput and efficiency.

#### Solana Programs[​](https://icarus131.github.io/devcookbook/docs/SVM#solana-programs) <a href="#solana-programs" id="solana-programs"></a>

In the context of Solana, a "program" refers to a smart contract or application deployed on the Solana blockchain. Solana programs are typically written in Rust or C, and they execute within the Solana Virtual Machine (SVM). Each program is associated with a specific address on the blockchain and contains the logic for handling transactions, updating state, and interacting with other programs.

#### Developing on Solana[​](https://icarus131.github.io/devcookbook/docs/SVM#developing-on-solana) <a href="#developing-on-solana" id="developing-on-solana"></a>

Developing on Solana involves writing smart contracts or programs that run on the Solana blockchain. Unlike traditional centralized applications, Solana development requires understanding blockchain concepts such as consensus mechanisms, transaction processing, and decentralized data storage. Developers use programming languages like Rust or C to write Solana programs, and they interact with the blockchain using the Solana Command Line Tool (CLI) or specialized SDKs.

#### SVM vs. EVM?[​](https://icarus131.github.io/devcookbook/docs/SVM#svm-vs-evm) <a href="#svm-vs-evm" id="svm-vs-evm"></a>

**Parallel Execution Capability:**[**​**](https://icarus131.github.io/devcookbook/docs/SVM#parallel-execution-capability)

The SVM executes transactions in parallel, while the EVM processes them sequentially. This parallel execution of SVM addresses gas cost issues, especially during concurrent transactions.

**Mitigation of Gas Costs:**[**​**](https://icarus131.github.io/devcookbook/docs/SVM#mitigation-of-gas-costs)

EVM's sequential processing can lead to increased gas costs for unrelated transactions due to congestion. In contrast, SVM's parallel execution ensures that highly contested applications don't impact others, mitigating the "noisy neighbor problem."

**State Handling:**[**​**](https://icarus131.github.io/devcookbook/docs/SVM#state-handling)

EVM allows unrestricted state access per transaction, potentially causing slower lookups as the rollup state expands. SVM requires specifying necessary state for each transaction, offering a more efficient approach.

**Performance:**[**​**](https://icarus131.github.io/devcookbook/docs/SVM#performance)

Leveraging Solana's execution engine within the Ethereum ecosystem represents a significant enhancement in scalability and performance for decentralized applications deployed on the Eclipse rollup.

#### How does the SVM fit into the Eclipse workflow?[​](https://icarus131.github.io/devcookbook/docs/SVM#how-does-the-svm-fit-into-the-eclipse-workflow) <a href="#how-does-the-svm-fit-into-the-eclipse-workflow" id="how-does-the-svm-fit-into-the-eclipse-workflow"></a>

**Transaction Execution:**[**​**](https://icarus131.github.io/devcookbook/docs/SVM#transaction-execution)

Within the Eclipse workflow, transactions are processed using the Solana Virtual Machine (SVM). Unlike the traditional Ethereum rollups that rely on the Ethereum Virtual Machine (EVM), Eclipse leverages Solana's execution engine for transaction execution.

**Parallel Transaction Processing:**[**​**](https://icarus131.github.io/devcookbook/docs/SVM#parallel-transaction-processing)

The SVM is distinguished by its capability to execute transactions in parallel, without overlapping states. This stands in contrast to the sequential processing nature of the EVM. With SVM's parallel execution, multiple transactions can occur simultaneously, addressing potential gas cost issues and enhancing overall efficiency.

**Efficient State Handling:**[**​**](https://icarus131.github.io/devcookbook/docs/SVM#efficient-state-handling)

SVM requires specific state specification for each transaction, ensuring efficient processing. In contrast, the EVM allows unrestricted state access per transaction, which can lead to slower processing as the rollup state expands. By mandating precise state specification, SVM optimizes transaction execution within the Eclipse workflow.

**Integration with Eclipse's Workflow:**[**​**](https://icarus131.github.io/devcookbook/docs/SVM#integration-with-eclipses-workflow)

As transactions are dispatched to a sequencer within Eclipse, they are subsequently processed using SVM. The parallel execution capability of SVM contributes to the scalability and performance of the Eclipse rollup. This integration showcases the innovative potential of combining Solana's execution engine with Ethereum's ecosystem, ultimately enhancing the functionality and efficiency of decentralized applications deployed on the Eclipse platform.

## Sub Page - Development guide with examples

### How to use the Solana CLI[​](https://icarus131.github.io/devcookbook/docs/SVM#how-to-use-the-solana-cli) <a href="#how-to-use-the-solana-cli" id="how-to-use-the-solana-cli"></a>

The Solana Command Line Interface (CLI) is a powerful tool that allows developers and users to interact with the Solana blockchain. It provides a comprehensive set of commands for various tasks such as deploying smart contracts, managing accounts, and monitoring network activity. Here's a hands-on guide to using the Solana CLI:

Follow this guide to install the Solana CLI on your machine: [Solana CLI Installation Docs](https://docs.solanalabs.com/cli/install)

Usage:

Once installed, you can use the Solana CLI to interact with the Solana blockchain. Here are some common commands:

Check Cluster Status:

```
solana cluster-version
```

Create a Wallet:

```
solana-keygen new --outfile ~/my-wallet.json
```

Show Account Balance:

```
solana balance <account_address>
```

Send SOL to Another Account:

```
solana transfer <recipient_address> <amount>
```

Deploy a Smart Contract:

```
solana deploy <program_binary>
```

Interact with a Smart Contract:

```
solana program <program_address>
```

View Transaction Logs:

```
solana logs
```

### Setting up your Development Environment[​](https://icarus131.github.io/devcookbook/docs/SVM#setting-up-your-development-environment) <a href="#setting-up-your-development-environment" id="setting-up-your-development-environment"></a>

Before you can start developing smart contracts for SVM, you need to set up your development environment. This involves installing the necessary tools and libraries. Follow the instructions in this section to get started.

* Install Rust and Cargo, the package manager for Rust, by following the official Rust installation guide: rustup.rs.
* Install the Solana Command Line Tool (CLI) by following the instructions in the Solana documentation: solana.com/docs/.
* Install the Solana SDK for Rust by adding the following dependency to your Cargo.toml file:

```toml
[dependencies]
solana-sdk = "1.9.0"
```

* Initialize a new Rust project using Cargo

```bash
cargo new my_project
cd my_project
```

* All set, now you can begin writing out the smart contract

### Creating Your First Solana Smart Contract[​](https://icarus131.github.io/devcookbook/docs/SVM#creating-your-first-solana-smart-contract) <a href="#creating-your-first-solana-smart-contract" id="creating-your-first-solana-smart-contract"></a>

In this section, you will learn how to create a simple Solana smart contract using Rust. Follow these steps to create a basic "Hello, World!" smart contract:

* Create a new Rust file in your project directory:

```bash
touch src/lib.rs
```

* Open src/lib.rs in your favorite text editor and add the following code:

```rust
#![cfg_attr(not(feature = "std"), no_std)]

use solana_program::{
    account_info::{next_account_info, AccountInfo},
    entrypoint,
    entrypoint::ProgramResult,
    msg,
    pubkey::Pubkey,
};

entrypoint!(process_instruction);

pub fn process_instruction(
    _program_id: &Pubkey,
    accounts: &[AccountInfo],
    _instruction_data: &[u8],
) -> ProgramResult {
    msg!("Hello, Solana!");
    Ok(())
}
```

* Save the file and exit your text editor.

### Deploying Smart Contracts on the Solana Blockchain[​](https://icarus131.github.io/devcookbook/docs/SVM#deploying-smart-contracts-on-the-solana-blockchain) <a href="#deploying-smart-contracts-on-the-solana-blockchain" id="deploying-smart-contracts-on-the-solana-blockchain"></a>

Once you've created your smart contract, the next step is to deploy it on the Solana blockchain.

Follow these steps to deploy your contract using the Solana CLI:

* Build your smart contract:

```bash
cargo build-bpf
```

* Deploy your smart contract:

```bash
solana deploy target/deploy/my_project.so
```

Your smart contract is now deployed on the Solana blockchain!

### Interacting with Smart Contracts[​](https://icarus131.github.io/devcookbook/docs/SVM#interacting-with-smart-contracts) <a href="#interacting-with-smart-contracts" id="interacting-with-smart-contracts"></a>

Now that your smart contract is deployed, you can interact with it using various tools and libraries. Here are some common ways to interact with Solana smart contracts:

* Using the Solana CLI:

```bash
solana program call <contract_address> <instruction_data>
```

### Advanced Smart Contract Development Techniques[​](https://icarus131.github.io/devcookbook/docs/SVM#advanced-smart-contract-development-techniques) <a href="#advanced-smart-contract-development-techniques" id="advanced-smart-contract-development-techniques"></a>

In this section, you will learn advanced techniques for developing smart contracts on Solana, including:

* State management: Let us see how to use account states efficiently, minimizing storage costs and implement state transitions carefully to maintain data consistency.

```rust
pub struct MyContractState {
    pub balance: u64,
}

impl MyContractState {
    pub fn new(balance: u64) -> Self {
        Self { balance }
    }

    pub fn update_balance(&mut self, amount: u64) {
        self.balance += amount;
    }
}

```

* Event handling: You can emit events from your smart contract to notify external systems about state changes or other important events.
* Here is a simple example:

```rust
use solana_program::program::invoke;

fn emit_event() {
    // Emit event
    let event_data = vec![1, 2, 3]; // Example event data
    let event_instruction = solana_program::system_instruction::log(&event_data);
    invoke(&event_instruction, &[]).unwrap();
}

```

* Error handling: You can define custom error types and handle errors gracefully in your smart contracts.

```rust
#[derive(Debug)]
pub enum MyError {
    InsufficientFunds,
    InvalidInstruction,
}

pub fn process_instruction(
    instruction_data: &[u8],
) -> Result<(), MyError> {
    if instruction_data.len() < 4 {
        return Err(MyError::InvalidInstruction);
    }

    // Check for sufficient funds
    if balance < amount {
        return Err(MyError::InsufficientFunds);
    }

    Ok(())
}

```

* Gas optimization: You can optimize gas usage by batching operations and minimizing storage costs.

```rust
// gas optimization by batching operations
let mut ix_batch = Vec::new();
for (account, amount) in transfers.iter() {
    let ix = solana_program::system_instruction::transfer(source, account, *amount);
    ix_batch.push(ix);
}
let batch_instruction = solana_program::instruction::Instruction::new_batch(&ix_batch);
invoke(&batch_instruction, &[]).unwrap();
```

* Upgrading smart contracts safely: You can implement a safe upgrade mechanism to update your smart contract's program ID without losing data or funds.

```rust
pub fn upgrade_contract(new_program_id: &Pubkey) {
    // Update contract's program ID to new_program_id
}
```

### Testing and Debugging Smart Contracts[​](https://icarus131.github.io/devcookbook/docs/SVM#testing-and-debugging-smart-contracts) <a href="#testing-and-debugging-smart-contracts" id="testing-and-debugging-smart-contracts"></a>

Testing and debugging are critical aspects of smart contract development. In this section, you will learn how to write tests for your smart contracts and debug common issues using tools like the Solana CLI

* Debugging: You can use the Solana CLI to debug your smart contracts by inspecting account states and transaction logs.

```rust
fn process_instruction(instruction_data: &[u8]) -> ProgramResult {
    msg!("Received instruction: {:?}", instruction_data);
    // Process instruction...
    Ok(())
}
```

* Testing: You can write unit tests and integration tests for your smart contracts using the Rust testing framework.

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_process_instruction() {
        let instruction_data = vec![1, 2, 3]; // Example instruction data
        assert_eq!(process_instruction(&instruction_data), Ok(()));
    }
}
```

* Integration testing: You can write integration tests to deploy and interact with multiple smart contracts in a simulated environment.

```rust
#[cfg(test)]
mod integration_tests {
    use super::*;

    #[test]
    fn test_contract_integration() {
        // Deploy and interact with multiple contracts...
    }
}
```

#### What is Anchor[​](https://icarus131.github.io/devcookbook/docs/SVM#what-is-anchor) <a href="#what-is-anchor" id="what-is-anchor"></a>

Anchor is a framework for building Solana smart contracts using the Rust programming language. It simplifies the development process by providing high-level abstractions and utilities for interacting with the Solana blockchain. Here's an overview of using Anchor for Rust development in Solana: Installation:

You can install Anchor using Cargo, the Rust package manager:

```bash
cargo install --git https://github.com/project-serum/anchor --tag <latest_version>
```

Usage:

* Create a New Anchor Project:

```bash
anchor init my_project
cd my_project
```

* Define Your Anchor Program:

Anchor programs are defined using a combination of Rust and a domain-specific language (DSL) provided by Anchor. You define your program's state, instructions, and events in Rust structs annotated with Anchor macros.

```rust
use anchor_lang::prelude::*;

#[program]
mod my_program {
    use super::*;

    #[state]
    pub struct MyState {
        pub value: u64,
    }

    impl MyState {
        pub fn new(ctx: Context<Initialize>, value: u64) -> Result<Self> {
            let state = &mut ctx.accounts.state;
            state.value = value;
            Ok(())
        }

        pub fn update(ctx: Context<Update>, value: u64) -> Result<()> {
            let state = &mut ctx.accounts.state;
            state.value = value;
            Ok(())
        }
    }
}
```

* Build and Deploy Your Program:

```bash
anchor build
anchor deploy
```

Interact with Your Program:

Anchor generates client libraries that you can use to interact with your Solana program from other Rust code or external applications. You can use these libraries to send transactions, call methods, and query state on the blockchain.

```rust
use my_project::my_program::MyProgram;
use anchor_lang::prelude::*;

let program_id = Pubkey::new_from_array([0; 32]);
let payer = Keypair::new();
let client = RpcClient::new("https://api.devnet.solana.com".to_string());

let mut transaction = Transaction::new_with_payer(
    &[Instruction::new_with_bincode(
        program_id,
        &MyProgram::Instruction::Update { value: 42 },
        vec![],
    )],
    Some(&payer.pubkey()),
);
transaction.sign(&[&payer], recent_blockhash);
client.send_and_confirm_transaction(&transaction);
```

In conclusion, understanding how to develop for the Solana Virtual Machine (SVM) within Eclipse will give you significant advantages in the Ethereum ecosystem. By leveraging the SVM's highly performant parallelized architecture, developers can unlock improved scalability, lower fees, and enhanced throughput for their decentralized applications. This not only ensures a smoother user experience but also allows you to build more efficient and scalable applications with Eclipse.
