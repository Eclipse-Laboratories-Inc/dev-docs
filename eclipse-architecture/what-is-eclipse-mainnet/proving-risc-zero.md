# Proving - RISC Zero

## Introduction to SVM Fraud Proofs and State Serialization

Our proving strategy draws inspiration from Anatoly's SVM fraud proofs SIMD and builds upon John Adler's insight regarding the costly nature of state serialization. Adler identified that it's feasible to bypass this expense, a concept integral to our approach.

## Avoiding Merkle Trees in SVM

In our initial experiments with the SVM, we integrated a Sparse Merkle Tree. However, updating the Merkle tree after each transaction led to significant performance setbacks. To enhance efficiency, we decided against reintroducing a Merkle tree into the SVM. This decision moves us away from traditional generalist rollup frameworks like the OP Stack and necessitates a more innovative fault proof architecture.

## Fault Proof Requirements

Fault proof in our framework requires:

1. **Commitment to Transaction Inputs**: Ensuring all inputs for a transaction are committed before execution.
2. **Transaction Execution**: The transaction itself, as executed.
3. **Output Verification**: Proof that re-executing the transaction yields a different outcome than what was recorded on the blockchain.

Instead of using a Merkle root for input commitments, our executor will post a detailed list of inputs and outputs for each transaction. This includes account hashes and relevant global state details, along with indices tracking the origin of each input. Transactions are recorded on Celestia, allowing any full node to verify the inputs and outputs by referencing their own state data to ensure the commitment on Ethereum is accurate.

## Identifying and Addressing Major Faults

There are two primary types of faults we anticipate:

1. **Incorrect Outputs**: If incorrect outputs are detected, the verifier will submit a Zero-Knowledge (ZK) proof directly on the blockchain, demonstrating the correct outputs. We utilize RISC Zero to generate these ZK proofs based on SVM execution, extending our previous efforts in proving BPF bytecode execution. This approach enables our settlement contract to verify correctness without executing the transactions on-chain.
2. **Incorrect Inputs**: In cases where the inputs are misrepresented, the verifier will reference historical data on-chain to prove the discrepancy. Utilizing Celestia's Quantum Gravity Bridge, our settlement contract can then verify that this historical data substantiates a fraud claim.

## Methodology

This methodology enhances the security and efficiency of SVM rollups by eliminating the need for expensive state serialization and reducing the dependency on traditional rollup frameworks. Our approach ensures that even without running transactions directly on the blockchain, the integrity and correctness of each transaction can be effectively verified, fostering a robust and scalable rollup solution.
