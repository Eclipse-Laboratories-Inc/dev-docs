# Unified Restaking Tokens (URTs)

## Turbo ETH (tETH)

Eclipse is launching a unified restaking token, tETH, in partnership with [Nucleus](https://twitter.com/nucleusearn). tETH combines the largest yield-generating protocols on Ethereum into one easy-to-use default yield token.&#x20;

{% embed url="https://teth.eclipse.xyz/" %}

## Overview

Users can now earn restaking rewards without the complexity of fragmented liquidity, time, and attention. Onchain users finally have an index LRT token that diversifies risk and maximizes reward exposure. This is just the beginning of our LRT hub, dedicated to simplifying the process of earning rewards for users and unifying liquidity across LRTs.&#x20;

The five LRTs that users can deposit to mint tETH are:

1. Wrapped ETH - WETH
2. EtherFi - weETH
3. Renzo Protocol - ezETH
4. Swell Network - rswETH
5. Dinero - apxETH
6. Puffer Finance - pufETH

## tETH Mechanisms

### **Minting and Bridging:**&#x20;

* Eligible LRT deposits mint tETH on Ethereum mainnet, then bridge to Eclipse via Hyperlane. tETH is an exchange-rate-bearing token, similar to Compound cTokens and Lido’s wstETH.

### Yield and Rewards:

* ETH-based yield increases the exchange rate over time (barring slashing events).
* Non-ETH rewards (e.g., AVS rewards) don’t impact the exchange rate; users claim these separately through a claim interface.
* Rewards are periodically mapped to holder addresses and remain claimable while holding the asset.

### Redemption Options \[COMING SOON]:

1. Standard Redemption: Redeem tETH for any LRT on Ethereum mainnet at full value.
2. Expedited Redemption: Solvers fill orders for quicker redemption on Ethereum mainnet or Eclipse.

### Exchange Rate Oracle:

* Tracks the tETH to ETH rate by querying underlying ETH balances of LRTs.
* Used during minting and redemption, initially managed by Nucleus, with plans for future decentralization.
* In case of an LRT price depeg, the oracle only tracks ETH balance, making tETH unaffected by market volatility.
* For slashing events, the oracle reflects the slashed ETH, with loss capped to the affected LRT’s exposure in the pool.
