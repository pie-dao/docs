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

