---
description: DRAFT v0.1
---

# PieDAO

### Abstract

PieDAO is an asset allocation decentralized autonomous organization \(DAO\) for governing tokenized portfolio allocations.

In this paper we introduce an Ethereum decentralized autonomous organization \(DAO\) initially focused on creating tokenized asset allocations called PIEs where weights are collectively governed by DAO members, allowing users to frictionlessly get exposure to different allocations.

### Introduction

In the last decade, a significant shift in people’s relationship with money has occurred, due to a combination of macro-trends in economic, political, and technological forces. People have become increasingly mindful of domestic systemic risk and been seeking passive investment tools designed to mitigate that risk.

In finance, index funds guarantee investors a controlled exposure to a portfolio. These products, however, are centralized solutions for portfolio management and offer little exposure to new asset classes like crypto and DeFi.

Liquidity pools offer a new standard for efficiently trading assets while allowing investors to retain some properties of an index fund with automated rebalancing while earning a yield on their holdings.

In this paper, we introduce a decentralized governance system for tokenized Smart Pools representing a portfolio allocation and frictionless exposure to a diversified basket of tokens, implemented as, but not exclusively, a liquidity pool.

## Pie Smart Pools

Pie Smart Pools are non-custodial smart contracts, they are the first implementation of a DAO-governed AMM pool. They add extra functionality on top of vanilla AMMs pools.

Providing liquidity to one of these Pie gets you tokenized exposure to the underlying assets and additionally generates yield from the liquidity in these pools to perform token swaps.

The Pie Smart Pools are asset management agnostic, at the time of writing, Pie Smart Pools are compatible with the Balancer interface.

### Background

Balancer is an Automated Market Maker protocol, similar to Uniswap, which allows anyone to create a self-balancing index fund like token. Those pools can be either Controlled vs Finalized Pools.

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

`joinswapExternAmountIn` allows a deposit where only one of the underlying quantities of assets is provided. Depositing a single asset is equivalent to depositing all pool assets proportionally by swapping the provided assets, which included slippage.

### Redeeming Pies

Redeeming assets in the Pie is equivalent to burn Pie tokens in the non-custodial smart-contract.

`exitPool` allows Pie holders to burn Pie tokens and receive the underlying quantities of assets according to weights.

```text
function exitPool(uint256 _amount) external override ready noReentry {
```

`exitswapPoolAmountIn` allows Pie holders to burn Pie tokens and receive only one of the underlying asset. Exiting to a single asset is equivalent to swapping the other underlying assets for the chosen one, which included slippage.

On a standard finalized Balancer pool if an asset transfer function reverts is not possible to redeem any of the underlying assets.

Pie smart pool allows exiting the pool even with multiple frozen assets in it by using a specific function designed to limit the loss of frozen assets.

```text
@param _lossTokens Tokens skipped on redemption

function exitPoolTakingloss(uint256 _amount, address[] calldata _lossTokens)
```

### Features & Use Cases

Pie Smart Pools implement a wide range of additional features, from safe-guards to flash loans, designed for a different use-case that a specific allocation might benefit from.

#### Flash loans

A flash loan allows you to borrow an asset without the need for collateral as long as the asset is returned within the same transaction.

Flash-loan technology allows users to use fewer transactions, thereby reducing fees.

They allow users to perform a variety of operations including arbitrage, refinancing, and more. Borrowers of a Flash Loans might have to pay fees when returning the funds.

Pie Smart Pools allows for flash loans via the `PFlashLoans` smart contract pool.

Unlike other platforms offering Flash Loans, for security reasons `PFlashLoans` does not imply funds have been returned after a smart contract call by looking at the balances at the end of a transaction. It explicitly sends funds back to the Pie Smart Pool.

In some cases, `PFlashLoans` Pools could be used for more reasons than borrowing funds, for instance, a Pie containing governance tokens of some DAOs could use the flash loan mechanics to vote by owning the Pie instead of the governance token.

#### Dynamic Fees

DAO governed function `setSwapFee()` in PieDAO Smart Pools allows for a dynamic fee system that can respond quickly to the expansion and contraction of demand for specific assets in the pool according to external factors.

Fees can be either decided directly by DAO participants or algorithmically determined according to specific factors.

Additionally, a dynamic fee system can be used to influence trading frequency withing the pool, and either incentivize or disincentivize rebalancing until a certain maximum deviation from the nominal % allocations is reached.

Similarly to other industries, the ability to set pricing rules with a powerful algorithm that takes the price elasticity of tokens into account is likely to become an essential property of successful pools.

#### Dynamic Weights

Pies are different in nature, some will be designed to be boost diversification, others to maximize yield. Pie Smart Pools allow timed based weights adjustments to run off-chain and be notarized by DOUGH token holders.

One early example is USD++, in which weight function takes into consideration several different factors including the volatility compared to the peg of $1, trust minimization, and market risk, allowing weights to be adjusted every quarter by the DAO.

Dynamic Weights can be used to roll expiring futures contracts, offering perpetual exposure to an asset class, and even replicate derivatives such as protective puts and calls.

ie: Adjust the weight of the underlying asset to always equal `Δ * Price / Value` aka the elasticity or ‘omega’ of the derivative.

More on [Liquidity Provider Returns in Geometric Mean Markets](https://arxiv.org/abs/2006.08806).

#### Safeguards - Halting trading

Unlike finalized pools in Uniswap or Balancer, trading in Pie Smart Pools can be halted. Emergency transactions can be prepared via a vote of DAO token holders and broadcast in the future under specific conditions that require trading to be halted.

These transactions can be triggered based on multiple conditions by bots monitoring the mempool for abnormal behavior regarding the price and total supply of the underlying assets, in order to minimize the risk of a single asset collapsing draining the entire pool.

Over time, we believe live risk statistics and predictive analysis regarding the likelihood of a specific asset defaulting will be essential tools for liquidity pools to succeed.

#### Safeguards - Capped Pools

Pie smart pool can include a maximum cap for experimental pools.

### Parameters to Govern

Pie Smart Pools have a range of parameters governed by DOUGH holders.

* Pie Swap Fee
* Pie Weights Function
* Pie Trading Switch
* Pie Flashloan Fee
* Pie Single asset entry/exit Fee
* Pie Management Fee
* Pie Cap Value
* Pie Implementation Address

## Governance

DAOs are designed to disintermediate the hierarchy structure of organizations through their transparency, effectively giving direct control to shareholders. Key features are transparency, the immutability of the organization, and the accessible nature of the platform they are built upon.

Governance models in DAOs however still present challenges.

### Background

Governance is at the core of many DAOs. Unlike solidified protocols like Bitcoin, DeFi projects might need a greater level of granularity in decision making. It’s imperative that the process of decision making is transparent and incentivized accordingly.

Delegation mechanisms can play a positive role in reducing the amount of work required by token holders. Parties with significant delegated tokens need to have enough reasons to not be malicious.

Stimulating participation in governance is a combination of reducing friction from a user perspective, ensuring the distribution of information about governance proposals to the involved token holder, and incentives.

The proposed model below is designed to free the average DAO token holder from day to day work, while ensuring continuity in governance for specific tasks. Average token holders can any time remove decision power from delegated parties.

### A staking model for active delegated governance

In the following pages, we introduce a DAO governance model that is designed to create an operative commission within the DAO to work on a specific task, with aligned incentives via staking to boost participation and punish misbehave via slashing.

The models aim to mitigate a common condition where due to increased decentralization \(larger number of token holders\) participation in voting decreases, making decision making less agile and prone to answer quickly to challenges.

### Stakeholders

**DOUGH Token Holders**: DOUGH is the most important asset for decision making. Any decision can be taken or revoked using DOUGH. DOUGH is a non-transferable asset that has the sole purpose of facilitating participation in the PieDAO governance mechanism. Holders of DOUGH have the ability to vote on decisions in the DAO and exit the DAO \(aka ragequit\) whenever there is not an active vote that they initiated.

**FLOUR Token Holders**: DOUGH holders can burn DOUGH for FLOUR at any time. FLOUR is free to transfer, trade, and stake. One DOUGH can always be burned for one FLOUR.

**Committee member**: In order to become a Committee member a candidate has to stake FLOUR into the election contract, the candidate's stake sets the upper limit on what others can stake towards their candidacy. The Candidate Stake represents 5% of the maximum amount on what others can stake towards their candidacy.

Elected members must vote on questions raised by members of the PieDAO community. They may vote YES, NO, or ABSTAIN. Failure to vote results in the slashing of a portion of that member's stake.

A Committee member can charge a fee on rewards generated by delegated funds. Such fee is extracted to the Committee member at the end of each epoch providing the Committee member has not been slashed for lack of participation.

**Delegators:** FLOUR holders delegating to a `Committee member` in order to generate more DOUGH.

### Commission election

Once per epoch `t`, per committee, an election is held to pick an amount `n` of committee members. The `n` candidates with the most FLOUR staked on them win the election and serve for the epoch unless removed.

### Misconduct and Slashing

The system recognizes 3 security threat levels to the system, each level of penalty, represented in percentage point, indicated the amount slashed according to level.

**Level 1** Misconducts that are likely to happen eventually to most participants. Proposed slashing 10.0% of the stake.

**Level 2** Misconducts that can occur in good faith, but show bad practices. Proposed slashing 50.0% of the stake.

**Level 3** Misconducts that are unlikely to happen in good faith or by accident, considered malicious. Proposed slashing is 100% of the stake.

Right after an election, every Committee member starts at Level 1. If they fail to fulfill their role or generally act out of bad faith \(i.e. commit excessive misconduct\), DOUGH Token Holders can decide to raise their penalty level.

It's in each `Delegator`'s interest to make sure their `Committee member` performs work required. Otherwise, a `Delegator` should ask DOUGH Token Holders to increase the penalty level of their `Committee member`.

`Levels of Misconduct` are multiplied by the Member Participation Percentage, applied retroactively at the end of the quarter.

`Delegators` are slashed a maximum % value `Delegator_Penalty` of the amount their `Committee member` is slashed. Proposed: 40%.

### Baking DOUGH, inflation module

Through the process of staking FLOUR, new DOUGH is created.

The inflation rate is decided by governance which jointly decides a `Quarterly Reward` amount and `Bonus Reward`.

Commission members with good performance at the end of the year and users delegating to them are entitled to the `Bonus Reward`.

Good actors are rewarded for bootstrapping the governance mechanism and participating in governance.

`Quarterly Reward` is distributed every quarter according to the number of votes the Committee Member participated in compared to the total number of votes in the quarter.

### Rewards / Slashing Function

```text
CM = Commission Member.
DL = Delegator to Member.
MPP = Member Participation Percentage.
MPP = Cm_Total_Votes/TotalVotesInQuarter.

MIP = Member Informed Percentage
MIP = (Yes_CM + No_CM) / TotalVotesInQuarter.

// MPP and MIP are calculated every quarter.

Q_Rewards_CM_% = 1 - (1 - MPP) * Level_of_Misconduct_%;
Q_Rewards_CM = ((CM_Stake / 4) * (CM_Stake * QuarterlyReward)) * Q_Rewards_CM_%;

Q_Rewards_DL = 1 - (1 - MPP) * Level_of_Misconduct_% * Delegator_Penalty ;

MIP_CM = MIP_q1 + MIP_q2 + MIP_q3 + MIP_q4;
CM_Bonus_Reward = CM_Stake * Bonus_Reward * MIP_CM;
Slash_CM = CM_Bonus_Reward - CM_Stake;
```

### DAO Architecture

PieDAO is an Aragon DAO running the Dandelion organization structure.

In any voting-based system, there are cases for possible collusion or unavailability of stakeholders. For this reason, PieDAO implements rage-quit, a functionality popularized by MolochDAO which guarantees that participants can exit if they disagree with the decisions other members are making.

#### Governance parameters

| Time | Action | MIN Approval | Support |
| :--- | :--- | :--- | :--- |
| 72H | Voting | 10% | 60% |

#### Apps installed

* [Redemptions](https://github.com/1Hive/redemptions-app): Allows users to manage a list of eligible assets held within an organization's Vault and allow members of the organization to redeem \(burn\) organization token in exchange for a proportional amount of the eligible assets.
* [Token Request](https://github.com/1Hive/token-request-app): Allows users to propose minting tokens in exchange for a payment to the organization, subject to the approval of existing members.
* [Time Lock](https://github.com/1Hive/time-lock-app): Allows an organization to require users to lock a configure amount of tokens for a configurable amount of time in order to forward an intent.
* [Dandelion Voting](https://github.com/1Hive/dandelion-voting-app) An enhanced version of Aragon One's voting app which implements an ACL Oracle which allows an organization to configure permissions that restrict actions based on whether an address has recently voted Yes.

#### Permissions

| App | Permission | Grantee | Manager | ACL Oracle |
| :--- | :--- | :--- | :--- | :--- |
| Kernel | APP\_MANAGER | Dandelion Voting | Dandelion Voting | None |
| ACL | CREATE\_PERMISSIONS | Dandelion Voting | Dandelion Voting | None |
| EVMScriptRegistry | REGISTRY\_MANAGER | Dandelion Voting | Dandelion Voting | None |
| EVMScriptRegistry | REGISTRY\_ADD\_EXECUTOR | Dandelion Voting | Dandelion Voting | None |
| Dandelion Voting | CREATE\_VOTES | Time Lock | Dandelion Voting | None |
| Dandelion Voting | MODIFY\_QUORUM | Dandelion Voting | Dandelion Voting | None |
| Dandelion Voting | MODIFY\_SUPPORT | Dandelion Voting | Dandelion Voting | None |
| Dandelion Voting | MODIFY\_BUFFER | Dandelion Voting | Dandelion Voting | None |
| Dandelion Voting | MODIFY\_EXECUTION\_DELAY | Dandelion Voting | Dandelion Voting | None |
| Agent or Vault | TRANSFER | Finance and Redemptions | Dandelion Voting | None |
| Finance | CREATE\_PAYMENTS | Dandelion Voting | Dandelion Voting | None |
| Finance | EXECUTE\_PAYMENTS | Dandelion Voting | Dandelion Voting | None |
| Finance | MANAGE\_PAYMENTS | Dandelion Voting | Dandelion Voting | None |
| Token Manager | MINT | Token Request | Dandelion Voting | None |
| Token Manager | BURN | Redemptions | Dandelion Voting | None |
| Redemptions | ADD\_TOKEN | Dandelion Voting | Dandelion Voting | None |
| Redemptions | REMOVE\_TOKEN | Dandelion Voting | Dandelion Voting | None |
| Redemptions | REDEEM | ANY ENTITY | Dandelion Voting | Dandelion Voting |
| Token Request | SET\_TOKEN\_MANAGER | Dandelion Voting | Dandelion Voting | None |
| Token Request | SET\_VAULT | Dandelion Voting | Dandelion Voting | None |
| Token Request | MODIFY\_TOKENS | Dandelion Voting | Dandelion Voting | None |
| Token Request | FINALISE\_TOKEN\_REQUEST | Dandelion Voting | Dandelion Voting | None |
| Time Lock | CHANGE\_DURATION | Dandelion Voting | Dandelion Voting | None |
| Time Lock | CHANGE\_AMOUNT | Dandelion Voting | Dandelion Voting | None |
| Time Lock | CHANGE\_SPAM\_PENALTY | Dandelion Voting | Dandelion Voting | None |
| Time Lock | LOCK\_TOKENS\_ROLE | ANY ENTITY | Dandelion Voting | Token Oracle |
| Token Oracle | SET\_TOKEN | Dandelion Voting | Dandelion Voting | None |
| Token Oracle | SET\_MIN\_BALANCE | Dandelion Voting | Dandelion Voting | None |
| Agent | RUN\_SCRIPT | Dandelion Voting | Dandelion Voting |  |
| Agent | EXECUTE | Dandelion Voting | Dandelion Voting |  |

#### Staking Aragon App Architecture

TBD

#### Election Aragon App Architecture

TBD

## Appendix

### One year staking journey

New elections for Commission A is about to open with parameters:

| Election Values | Val |
| :--- | :--- |
| Quarterly Reward | 2.5% |
| Bonus Reward | 10% |
| Delegator Penalty | 40% |

#### Summary Q1

Alice registers as a Committee member candidate by staking 10,000 FLOUR tokens. Since Alice staked 10,000 FLOUR, up to 200,000 FLOUR can delegated towards her candidacy.

Bob delegated 200,000 FLOUR to Alice.

_End of Q1_

| Alice Q1 | Votes |
| :--- | :--- |
| Q1 Yes Votes | 0 |
| Q1 Abstain Votes | 1 |
| Q1 No Votes | 0 |
| Q1 Total Votes | 2 |
| Q1 MPP | 50.00% |
| Q1 MIP | 0.00% |
| Q1 Penalty Level | 1 |

Alice has only voted in one of the 2 votes, her Member Participation Percentage \(MPP\) is 50%. Alice closed the quarter with `Level_of_Misconduct` of 1.

```text
MPP = AliceVotes / TotalVotesInQuarter
```

To calculate Alice Q1 rewards, we then use the following formula:

```text
Q1_Rewards% = 1 - (1 - MPP) * Level_of_Misconduct%;
Q1_Rewards% = 1 - (1 - 50%) * 10%;
Q1_Rewards% = 95%;
Q1_Rewards_Alice = ((AliceStake / 4) * (AliceStake * QuarterlyReward)) * Q1_Rewards_%;
Q1_Rewards_Alice = ((10000 / 4) + (10000 * 2.5%)) * 95%;
Q1_Rewards_Alice = 2612.5;
```

Alice Scenario at the end of Q1

| Max Possible | Realized | % of max |
| :--- | :--- | :--- |
| 2,750 | 2,613 | 95.00% |

Bob is penalized by Alice bad behavior, to calculate Bob Q1 rewards, we then use the following formula:

```text
Q1_Rewards% = 1 - (1 - MPP) * Level_of_Misconduct_% * Delegator_Penalty;
Q1_Rewards% = 1 - (1 - 50%) * 10% * 40%;
Q1_Rewards% = 98%;
Q1_Rewards_Bob = ((DelegatorStake / 4) * (DelegatorStake * QuarterlyReward)) * Q1_Rewards%;
Q1_Rewards_Bob = 53,900;
```

Bob Scenario at the end of Q1

| Max Possible | Realized | % of max |
| :--- | :--- | :--- |
| 55,000 | 53,900 | 98.00% |

#### Summary Q2

| Alice Q2 | Votes |
| :--- | :--- |
| Q2 Yes Votes | 0 |
| Q2 Abstain Votes | 1 |
| Q2 No Votes | 0 |
| Q2 Total Votes | 10 |
| Q2 MPP | 10.00% |
| Q2 MIP | 0.00% |
| Q2 Penalty Level | 3 |

Alice is performing poorly, participating to only 10% of the votes.

Under Bob sollicitude, DOUGH holder during quarter 2, have raised `Level_of_Misconduct` for Alice to 3.

Alice Scenario at the end of Q2

| Max Possible | Realized | % of max |
| :--- | :--- | :--- |
| 2,750 | 275 | 10.00% |

Bob Scenario at the end of Q2

| Max Possible | Realized | % of max |
| :--- | :--- | :--- |
| 55,000 | 35,200 | 64.00% |

#### Summary Q3

| Alice Q3 | Votes |
| :--- | :--- |
| Q3 Yes Votes | 2 |
| Q3 Abstain Votes | 21 |
| Q3 No Votes | 2 |
| Q3 Total Votes | 25 |
| Q3 MPP | 100.00% |
| Q3 MIP | 16.00% |
| Q3 Penalty Level | 3 |

Alice is performing better, partecipating to 100% of the votes.

Alice closed the quarter with `Level_of_Misconduct` of 3 as a preventive behaviour.

Alice Scenario at the end of Q3

| Max Possible | Realized | % of max |
| :--- | :--- | :--- |
| 2,750 | 2,750 | 100.00% |

Bob Scenario at the end of Q3

| Max Possible | Realized | % of max |
| :--- | :--- | :--- |
| 55,000 | 55,000 | 100.00% |

#### Summary Q4

| Alice Q4 | Votes |
| :--- | :--- |
| Q4 Yes Votes | 1 |
| Q4 Abstain Votes | 0 |
| Q4 No Votes | 0 |
| Q4 Total Votes | 1 |
| Q4 MPP | 100.00% |
| Q4 MIP | 100.00% |
| Q4 Penalty Level | 2 |

Alice is back on track with 100% MPP.

Alice closed the quarter with `Level_of_Misconduct` of 2, since she was doing better in Q3.

Alice Scenario at the end of Q4

| Max Possible | Realized | % of max |
| :--- | :--- | :--- |
| 2,750 | 2,750 | 100.00% |

Bob Scenario at the end of Q4

| Max Possible | Realized | % of max |
| :--- | :--- | :--- |
| 55,000 | 55,000 | 100.00% |

#### Bonus Rewards calculation

The bonus is calculated by dividing the number of votes in which the member took an informed position \(YES or NO\) by the total votes brought to get the Member Informed Percentage \(MIP\). The final bonus is equal to ten percent of the initial stake, multiplied by the Member Informed Percentage.

```text
MIP_Alice = MIP_q1 + MIP_q2 + MIP_q3 + MIP_q4;
MIP_Alice = 29.00%;
Alice_Bonus_Reward = AliceStake*Bonus_Reward*MIP_Alice;
Alice_Bonus_Reward = 8,678;
Alice_Slash = Alice_Bonus_Reward - AliceStake;
Alice_Net = -1,323
```

Alice Bonus reward

| Staked | Realized | % of Slash |
| :--- | :--- | :--- |
| 10,000 | -1,323 | 13.22% |

Bob Bonus reward

| Staked | Realized | % of Slash |
| :--- | :--- | :--- |
| 200,000 | 4,900 | 0% |

**Conclusion**

* Alice didn't do great on Q1 and Q2.
* Alice did better on Q3 and Q4 and managed to save most of her stake.
* Bob was impacted by Alice behavious but ends net positive.
* Bob had to make sure Alice was voting enough.
* DOUGH Holders have renounced to Staking rewards for the possibility of staying liquid and possibly exit the DAO at any time.
* FLOUR Holders have renounced to direct governace in exchange for staking rewards.

