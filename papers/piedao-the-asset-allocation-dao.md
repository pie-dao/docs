---
description: >-
  **DRAFT v0.1** - Beware: This document is still in proposal stage and DOES NOT
  represents how PieDAO governance works right now. It is likely to change in
  the near future.
---

# PieDAO - The Asset Allocation DAO



### Abstract

PieDAO is an asset allocation Decentralized Autonomous Organization \(DAO\) for governing tokenized portfolio allocations.

In this paper we introduce an Ethereum decentralized autonomous organization \(DAO\) initially focused on creating tokenized asset allocations called PIEs where weights are collectively governed by DAO members, allowing users to frictionlessly get exposure to different allocations.

### Introduction

In the last decade, a significant shift in people’s relationship with money has occurred, due to a combination of macro-trends in economic, political, and technological forces. People have become increasingly mindful of domestic systemic risk and been seeking passive investment tools designed to mitigate that risk.

In finance, index funds guarantee investors a controlled exposure to a portfolio. These products, however, are centralized solutions for portfolio management and offer little exposure to new asset classes like crypto and DeFi.

Liquidity pools offer a new standard for efficiently trading assets while allowing investors to retain some properties of an index fund with automated rebalancing while earning a yield on their holdings.

In this paper, we introduce a decentralized governance system for tokenized Smart Pools representing a portfolio allocation and frictionless exposure to a diversified basket of tokens, implemented as, but not exclusively, a liquidity pool.

## Pie Smart Pools

Pie Smart Pools are non-custodial smart contracts, the first implementation of a DAO-governed AMM pool. They add extra functionality on top of vanilla AMMs pools.

Providing liquidity to one of these Pies gets you tokenized exposure to the underlying assets and additionally generates yield from the liquidity in these pools to perform token swaps.

The Pie Smart Pools are asset management agnostic. At the time of writing, Pie Smart Pools are compatible with the Balancer interface.

### Background

Balancer is an Automated Market Maker protocol, similar to Uniswap, which allows anyone to create a self-balancing index fund-like token. Those pools can be either Controlled or Finalized Pools.

1. Controlled pools are configurable by a “controller” address. Only this address can add or remove liquidity to the pool \(call join or exit\). This type of pool allows for great flexibility and implementation of custom functionality besides allowing the change of fees, pool assets, and their weights.
2. Finalized pools have fixed pool asset types, weights, and fees.

Any pool created on the Balancer protocol is used to aggregate liquidity when users want to swap one token for another.

### Supplying Assets

Suppling assets to a Pie will mint the Pie to your Ethereum wallet. Pies are represented by an ERC-20 token balance, which entitles the owner to a proportional share to an increasing quantity of the underlying asset.

There are two ways of supplying assets:

1. Weighted-asset deposit/withdrawal
2. Single-asset deposit/withdrawal

Represented in the smart contracts by the following functions:

`joinPool` allows deposits where all the underlying quantities of assets are according to weights.

```text
function joinPool(uint256 _amount) external virtual override ready noReentry
```

`joinswapExternAmountIn` allows a deposit where only one of the underlying quantities of assets is provided. Depositing a single asset is equivalent to depositing all pool assets proportionally by swapping the provided assets, which includes slippage.

### Redeeming Pies

Redeeming assets in the Pie is equivalent to burning Pie tokens in the non-custodial smart-contract.

`exitPool` allows Pie holders to burn Pie tokens and receive the underlying quantities of assets according to weights.

```text
function exitPool(uint256 _amount) external override ready noReentry {
```

`exitswapPoolAmountIn` allows Pie holders to burn Pie tokens and receive only one of the underlying asset. Exiting to a single asset is equivalent to swapping the other underlying assets for the chosen one, which includes slippage.

On a standard finalized Balancer pool if an asset transfer function reverts it is not possible to redeem any of the underlying assets.

Pie Smart Pools allow exiting the pool even with multiple frozen assets in it by using a specific function designed to limit the loss of frozen assets.

```text
@param _lossTokens Tokens skipped on redemption

function exitPoolTakingloss(uint256 _amount, address[] calldata _lossTokens)
```

### Features & Use Cases

Pie Smart Pools implement a wide range of additional features, from safeguards to flash loans, designed for a different use case that a specific allocation might benefit from.

#### Flash Loans

A Flash Loan allows you to borrow an asset without the need for collateral as long as the asset is returned within the same transaction.

Flash Loan technology allows users to use fewer transactions, thereby reducing fees.

They allow users to perform a variety of operations including arbitrage, refinancing, and more. Borrowers of Flash Loans might have to pay fees when returning the funds.

Pie Smart Pools allows for Flash Loans via the `PFlashLoans` smart contract pool.

Unlike other platforms offering Flash Loans, for security reasons `PFlashLoans` does not imply funds have been returned after a smart contract call by looking at the balances at the end of a transaction. It explicitly sends funds back to the Pie Smart Pool.

In some cases, `PFlashLoans` Pools could be used for more reasons than borrowing funds, for instance, a Pie containing governance tokens of some DAOs could use the Flash Loan mechanics to vote by owning the Pie instead of the governance token.

#### Dynamic Fees

The DAO governed function `setSwapFee()` in PieDAO Smart Pools allows for a dynamic fee system that can respond quickly to the expansion and contraction of demand for specific assets in the pool according to external factors.

Fees can be either decided directly by DAO participants or algorithmically determined according to specific factors.

Additionally, a dynamic fee system can be used to influence trading frequency within the pool, and either incentivize or disincentivize rebalancing until a certain maximum deviation from the nominal % allocations is reached.

Similarly to other industries, the ability to set pricing rules with a powerful algorithm that takes the price elasticity of tokens into account is likely to become an essential property of successful pools.

#### Dynamic Weights

Pies are different in nature, some will be designed to boost diversification, others to maximize yield. Pie Smart Pools allow time-based weight adjustments to run off-chain and be notarized by DOUGH token holders.

One early example is USD++, in which weight function takes into consideration several different factors including the volatility compared to the peg of $1, trust minimization, and market risk, allowing weights to be adjusted every quarter by the DAO.

Dynamic Weights can be used to roll expiring futures contracts, offering perpetual exposure to an asset class, and even replicate derivatives such as protective puts and calls.

ie: Adjust the weight of the underlying asset to always equal ΔPrice/ΔValue aka the elasticity or ‘omega’ of the derivative.

More on [Liquidity Provider Returns in Geometric Mean Markets](https://arxiv.org/abs/2006.08806).

#### Safeguards - Halting trading

Unlike finalized pools in Uniswap or Balancer, trading in Pie Smart Pools can be halted. Emergency transactions can be prepared via a vote of DAO token holders and broadcast in the future under specific conditions that require trading to be halted.

These transactions can be triggered based on multiple conditions by bots monitoring the mempool for abnormal behavior regarding the price and total supply of the underlying assets, in order to minimize the risk of a single asset collapse draining the entire pool.

Over time, we believe live risk statistics and predictive analysis regarding the likelihood of a specific asset defaulting will be essential tools for liquidity pools to succeed.

#### Safeguards - Capped Pools

Pie Smart Pools can include a maximum cap for experimental pools.

### Parameters to Govern

Pie Smart Pools have a range of parameters governed by DOUGH holders.

* Pie Swap Fee
* Pie Weights Function
* Pie Trading Switch
* Pie Flash Loan Fee
* Pie Single asset entry/exit Fee
* Pie Management Fee
* Pie Cap Value
* Pie Implementation Address

