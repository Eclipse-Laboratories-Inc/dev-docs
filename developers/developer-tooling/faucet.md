---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Faucet

{% hint style="info" %}
Submit your deployment address to this [form](https://forms.gle/yJfFABQDPmpvgzAf7). We will provide engineering support and prioritize specific infrastructure needs.
{% endhint %}

## Eclipse Testnet

### **Getting Sepolia ETH Tokens**

As a user, you can use Sepolia ETH to explore the Eclipse testnet and dApps deployed on the network. Sepolia ETH is not meant to be traded, and is only used to test applications. Sepolia ETH can be claimed from a number of faucets: Alchemy, QuickNode, and Infura.&#x20;

Here are instructions on how to claim Sepolia ETH on [Alchemy Sepolia ETH Faucet](https://sepoliafaucet.com/).&#x20;

1. Create an Alchemy account to request Sepolia ETH.
2. Visit the Alchemy Sepolia faucet and log in with your Alchemy account.
3. Enter your wallet in the provided box, complete the CAPTCHA verification, and click "Send Me ETH".

## Eclipse Devnet

### Requesting Devnet Tokens

You can request devnet tokens using this [faucet UI](https://eclipse-faucet-ui.vercel.app/).

To get devnet tokens programmatically, simply run this curl command in the terminal and replace `YOUR_SOLANA_VM_ACCOUNT` with the address of the wallet that you want to import tokens into.

{% code overflow="wrap" %}
```bash
curl https://staging-rpc.dev2.eclipsenetwork.xyz -X POST -H "Content-Type: application/json" -d '{"jsonrpc":"2.0","id":1, "method":"requestAirdrop", "params":["YOUR_SOLANA_VM_ACCOUNT", 1000000000]}'
```
{% endcode %}

The request is in "lamports," and you must request at least 1 SOL = 1000000000 lamports.

You can alternatively use the Solana CLI to airdrop yourself tokens:

```bash
solana config set --url https://staging-rpc.dev2.eclipsenetwork.xyz
solana airdrop 1
```

### Verifying Your Account Balance

{% code overflow="wrap" %}
```bash
curl https://staging-rpc.dev2.eclipsenetwork.xyz -X POST -H "Content-Type: application/json" -d ' {"jsonrpc":"2.0","id":1, "method":"getBalance", "params":["YOUR_SOLANA_VM_ACCOUNT"]}'
```
{% endcode %}

If you've just airdropped yourself some tokens, it might take a moment to show up.

### SPL Token Faucet

For generating custom tokens, you can use our [SPL Token Faucet](https://eclipse-dummy-token-faucet.vercel.app/) frontend.

You can also use the SPL Token CLI directly. You can reference the Solana docs on the [Token program](https://spl.solana.com/token), or you can follow the steps below to create a dummy fungible token:

### Creating An SPL Token

Make sure your Solana CLI is configured to Eclipse Devnet and [airdrop yourself](faucet.md#requesting-devnet-tokens) some tokens. Next, install the SPL token CLI:&#x20;

```
cargo install spl-token-cli
```

You can see all commands for this tool with `spl-token --help`.

Create the SPL token:

```bash
spl-token create-token
```

{% code title="Output" %}
```
Creating token 7RFaNSSdVD9Q2aYJf1XNCTz7xKfgN19fAPAsAqzaorqr under program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA

Address:  7RFaNSSdVD9Q2aYJf1XNCTz7xKfgN19fAPAsAqzaorqr
Decimals:  9

Signature: 2a13fRfFHbbBdpF9QSecqAGjSZ4BBWoPKTSurcoXAL2RW5ZchAzbcY1pgqfDLzfGhDvSFd88egof4FTPtrAr72sv
```
{% endcode %}

Take note of the token address, which happens to be `7RFaNSSdVD9Q2aYJf1XNCTz7xKfgN19fAPAsAqzaorqr` in this case.

### Create A Balance For Your SPL Token

Create an empty account to hold a balance of your newly created token:

```bash
spl-token create-account 7RFaNSSdVD9Q2aYJf1XNCTz7xKfgN19fAPAsAqzaorqr
```

{% code title="Output" %}
```
Creating account 4dYFmw8ytJmYDmZLvFnobT4CKC7Z5q1W5b9byEuVVap7

Signature: 25EsivN9f75yJNhrSrnM9TuPGHFx9tpyYwUhUomY5kydZpfvjcShJWCsUB5uk7hkP9QeTr6JpJtQV1DPjDjYtAjR
```
{% endcode %}

Mint tokens into the newly-created account:

```bash
spl-token mint 7RFaNSSdVD9Q2aYJf1XNCTz7xKfgN19fAPAsAqzaorqr 10000
```

{% code title="Output" %}
```
Minting 10000 tokens
  Token: 7RFaNSSdVD9Q2aYJf1XNCTz7xKfgN19fAPAsAqzaorqr
  Recipient: 4dYFmw8ytJmYDmZLvFnobT4CKC7Z5q1W5b9byEuVVap7

Signature: 3aVqfVgmZu333YxwJg7xieKkmebdw9NpPvmsCJgao3eQTDwQNmWYD3KBppp5g6Tbw6cN7pjQonN61Z2d2CFazzqp
```
{% endcode %}

See all of the tokens that you own:

```bash
spl-token accounts
```

{% code title="Output" %}
```
Token                                         Balance
-----------------------------------------------------
7RFaNSSdVD9Q2aYJf1XNCTz7xKfgN19fAPAsAqzaorqr  10000
```
{% endcode %}

### Transferring SPL Tokens

Transferring tokens to another account:

```bash
spl-token transfer 7RFaNSSdVD9Q2aYJf1XNCTz7xKfgN19fAPAsAqzaorqr 50 qp6oBVgpxfQDMP1GKzxgzDxKqEuZnuC8gZo5PK1qrdh
```

If it's the receiver's first time ever receiving tokens, then you'll get an error like this:

{% code title="Output" %}
```
Error: "Error: Recipient's associated token account does not exist. Add `--fund-recipient` to fund their account"
```
{% endcode %}

To fix it, you'll have to be the one to "fund" the recipient token account by adding the `--fund-recipient` flag:

```bash
spl-token transfer --fund-recipient AQoKYV7tYpTrFZN6P5oUufbQKAUr9mNYGe1TTJC9wajM 50 vines1vzrYbzLMRdu58
```

{% code title="Output" %}
```
Transfer 50 tokens
  Sender: 4dYFmw8ytJmYDmZLvFnobT4CKC7Z5q1W5b9byEuVVap7
  Recipient: qp6oBVgpxfQDMP1GKzxgzDxKqEuZnuC8gZo5PK1qrdh
  Recipient associated token account: 7G9XHxAjgXhkKnTmcb8Z24ssSgiXYNnTp8KqRuQVCi9D
  Funding recipient: 7G9XHxAjgXhkKnTmcb8Z24ssSgiXYNnTp8KqRuQVCi9D

Signature: 25cb1D1pxN3Sh3cqFhJSP2hmD6jAMRDxeg9jrvp3SH6GNpkY6Z4viEs7FFsHwVH5528xUZB8xCT5F1uVrXpcTBcN
```
{% endcode %}
