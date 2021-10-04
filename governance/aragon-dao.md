# Aragon DAO

PieDAO is originally based on the Aragon [Dandelion Template](https://github.com/1Hive/dandelion-template).  
DOUGHv1 was the original non-transferable governance token of the organization.

### Configuration

| Parameter | Description | Value |
| :--- | :--- | :--- |
| _Support_ | Relative percentage of tokens that are required to vote “Yes” for a proposal to be approved. For example, if “Support” is set to 50%, then more than 50% of the tokens used to vote on a proposal must vote “Yes” for it to pass. | 60% |
| _Minimum Approval_ | Percentage of the total token supply that is required to vote “Yes” on a proposal before it can be approved. For example, if the “Minimum Approval” is set to 20%, then more than 20% of the outstanding token supply must vote “Yes” on a proposal for it to pass. | 10% |
| _Vote Duration_ | Length of time that the vote will be open for participation. For example, if the Vote Duration is set to 7 days, then token holders have 7 days to participate in the vote. | 3 days |

Following PIP-12, DOUGH started migrating to DOUGHv2 at will, with a rate of 1 DOUGHv1 per 1 DOUGHv2. During the migration, governance has been migrated to [pie-crust](https://github.com/pie-dao/pie-crust), a module that calculates the voting weight of holders based on the sum of DOUGHv1 and DOUGHv2 both.  
  
`Vote weight = (DOUGH+DOUGHv2)`

### PIP-60 Release Impact — October 4th 2021

Following [PIP-60](https://forum.piedao.org/t/pip-60-dough-staking-governance-mining/954) and the introduction of Governance Mining, veDough becomes PieDAO's Governance Token, and voting will be exclusively done through [Snapshot](https://snapshot.org/#/piedao).

`Vote weight = veDough`

More about Governance Mining [here](governance-mining.md).

