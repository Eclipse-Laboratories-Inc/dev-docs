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

# RPC & Block Explorers

{% hint style="info" %}
Submit your deployment address to this [form](https://forms.gle/yJfFABQDPmpvgzAf7). We will provide engineering support and prioritize specific infrastructure needs.
{% endhint %}

## Eclipse Mainnet

<table><thead><tr><th width="199">Network Name</th><th>Eclipse Mainnet</th></tr></thead><tbody><tr><td>Description</td><td>Eclipse Mainnet with real economic value. </td></tr><tr><td>RPC</td><td><p>Free (good for testing but do not autoscale and are rate-limited):</p><ul><li><p><code>https://mainnetbeta-rpc.eclipse.xyz</code></p><ul><li>Rate limit: 3 reqs/s</li><li>Exceeding the rate limit can lead to a temporary IP ban</li></ul></li><li><p><code>https://eclipse.helius-rpc.com</code></p><ul><li>Rate limit: 25 reqs/s per ip</li></ul></li></ul><p>Private (recommended):</p><ul><li><a href="https://docs.google.com/forms/d/e/1FAIpQLSdrpgSCa4PSOWFKH8elf4UBptCHKbWN0BL0NxFLV_z_K2DF-g/viewform">Triton</a> (shared and dedicated tiers available)</li><li><a href="https://www.helius.dev/">Helius</a> (dedicated option only)</li><li><a href="./#rpc-requirements">Run your own node</a></li></ul></td></tr><tr><td>Block Explorers</td><td><ul><li><a href="https://eclipsescan.xyz/">EclipseScan</a></li><li><a href="https://explorer.eclipse.xyz/">Eclipse Dev Explorer</a></li><li><a href="https://www.eclipsexray.id/">Eclipse XRAY</a></li></ul></td></tr><tr><td>Celestia Namespace</td><td><ul><li><a href="https://celenium.io/namespace/00000000000000000000000000000000000000000065636c69707365?tab=Blobs">Celenium</a></li></ul></td></tr></tbody></table>

## Eclipse Testnet

<table><thead><tr><th width="199">Network Name</th><th>Eclipse Testnet</th></tr></thead><tbody><tr><td>Description</td><td>Realistic to Mainnet but with no real economic value.</td></tr><tr><td>RPC</td><td><code>https://testnet.dev2.eclipsenetwork.xyz</code></td></tr><tr><td>Block Explorers</td><td><ul><li><a href="https://eclipsescan.xyz/?cluster=testnet">EclipseScan Testnet</a></li><li><a href="https://explorer.dev.eclipsenetwork.xyz/?cluster=testnet">Eclipse Explorer Testnet</a></li><li><a href="https://solscan.io/?cluster=custom&#x26;customUrl=https%3A%2F%2Ftestnet.dev2.eclipsenetwork.xyz">Solscan Testnet</a></li></ul></td></tr><tr><td>Celestia Namespace</td><td><ul><li><a href="https://mocha-4.celenium.io/namespace/0000000000000000000000000000000000000000000065636c74330a?tab=Blobs">Celenium</a></li></ul></td></tr></tbody></table>

## Eclipse Devnet2

<table><thead><tr><th width="200">Network Name</th><th>Eclipse Devnet2 (Devnet1 is being deprecated)</th></tr></thead><tbody><tr><td>Description</td><td>Simulates developer experience.</td></tr><tr><td>RPC</td><td><code>https://staging-rpc.dev2.eclipsenetwork.xyz</code><br><code>https://staging-rpc-eu.dev2.eclipsenetwork.xyz</code></td></tr><tr><td>Block Explorers</td><td><ul><li><a href="https://eclipsescan.xyz/?cluster=devnet">EclipseScan Devnet</a></li><li><a href="https://explorer.dev.eclipsenetwork.xyz/">Eclipse Explorer Devnet</a></li><li><a href="https://solscan.io/?cluster=custom&#x26;customUrl=https%3A%2F%2Fstaging-rpc.dev2.eclipsenetwork.xyz">Solscan Devnet</a></li></ul></td></tr></tbody></table>

## RPC Requirements

* CPU
  * 12 cores / 24 threads, or more
  * 2.8GHz base clock speed, or faster
* RAM
  * 256GB or more
* Disk
  * Ledger: 2TB or larger. SSD suggested
  * Accounts: 500G or larger. SSD suggested

For instructions on how to run mainnetbeta RPC node, follow this [document](https://eclipsebuilders.notion.site/How-to-run-mainnetbeta-RPC-node-19eb6f80e8d34b54838b03995d3f9865).
