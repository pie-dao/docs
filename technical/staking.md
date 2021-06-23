---
description: Guide to interacting with the staking smart contracts
---

# Staking

### Deposit

TODO

### Withdraw

TODO

### Distribute rewards

Any address is able to call the `distributeRewards` function on the `dToken` \(veDOUGH\) contract. A sufficient approval and balance for the `token` specified in the `dToken` contract is needed.

For the veDOUGH address please refer to the [deployed contract addresses](deployed-smart-contracts.md)

```text
function distributeRewards(uint256 amount) external;
```

### Participation

Participation is tracked in the participation merkle tree.  
  
There are three states an address can be in

* INACTIVE - included in the merkle tree as 0
* ACTIVE - included in the merkle tree as 1
* UNSPECIFIED - not included in the  merkle tree, default state

#### INACTIVE

An address which gets labeled as such can have its reward redistributed by other participants. This can be done by any other address and other stakers are incentivised to do so to increase their own rewards. This can be done in batches using the following smart contract call:

```text
function redistribute(address[] calldata accounts, bytes32[][] calldata proofs) 
```

#### ACTIVE

An address which gets labeled as such has been active in governance and is able to claim its accrued rewards:

```text
function claim(bytes32[] calldata proof) external;
```

Its also possible to claim rewards for another address:

```text
function claimFor(address account, bytes32[] memory proof) public;
```

#### UNSPECIFIED

Not included in the merkle tree. Rewards cannot be claimed but also not redistributed

