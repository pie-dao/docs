---
description: >-
    How to update a PieDAO smart pool.
---


# Updating a PieDAO smart pool

All Pie DAO smart pools are upgradeable because they use a proxy contract. At PieDAO we use our implementation of a simple proxy contract called [pie-proxy](https://github.com/pie-dao/pie-proxy).

Upgrading a contract is done by setting the implementation address on the proxy contract to the address of the new logic contract. Using this pattern allows smart pools to be upgraded while keeping the old storage.

## Deploying a new implementation

Clone the [pie-smart-pools repository](https://github.com/pie-dao/pie-smart-pools) and checkout master. Master contains the latest release which is considered stable and audited for mainnet use.

copy the contents of ``env.example`` to a new file called ``.env`` and edit the relevant values inside. DO NOT share this file with anyone as it will contain sensitive data.

Install all dependencies:
```
yarn
```

Build the project
```
yarn build
```

To deploy the implementation contract run the following command replacing the impl-name value with the name you would like to give to the implementation contract and the --network value with whichever network you want to deploy the contract to.
```
npx buidler deploy-smart-pool-implementation-complete --impl-name [IMPL-NAME] --network kovan
```

In your terminal you will see the address of the implementation, copy this for the next step.

## Setting the new implementation

The pie-proxy instance has a proxyOwner address, only this address is capable of changing the implementation contract by calling ``setImplementation(address _newImplementation)``.

The smart pools are currently controlled by a multisig wallet. To change the implementation you should do the following steps.

1. Go to [multi sig UI](https://wallet.gnosis.pm/#/wallet/0x3bFdA5285416eB06Ebc8bc0aBf7d105813af06d0)
2. Connect your wallet.
3. Add a transaction.
4. Set destination to pool address.
5. Set contract name.
6. Set ABI (can be found on Etherscan).
7. Select setImplementation method.
8. Fill in the implementation address in the input.
9. Send multi sig tx.
10. Get enough confirmations by other multi sig owners.
11. Enjoy your upgraded contract.

## Testing procedure

Because Pie smart pools deal with real value it is important to take extra precaution when upgrading them to minimize the risk to users funds.

For BTC++ there is a private testing pool on mainnet at: [0x21909429c586fb739653e8e0b913b23fe30bacfa](https://etherscan.io/token/0x21909429c586fb739653e8e0b913b23fe30bacfa).

Before upgrading any pool we do a test run using this pool first. To do this we take the following steps:

1. Deploy implementation (described above).
2. Set cap to zero.
3. Set implementation (described above).
4. Set Controller to testing address.

### Automated testing

The automated test tests this functionality on mainnet:

1. Check the controller.
2. Check if the cap is zero.
3. Exiting the pool.
4. Check if cap is enforced.
5. Setting the cap.
6. Joining the pool.
7. Setting cap to zero
8. Setting public swap from non public swap setter (should fail).
9. Setting public swap setter from controller.
10. Setting public swap to true.
11. Setting public swap to false.
12. Setting the tokenBinder
12. Unbinding token from non tokenBinder address (should fail).
11. Binding token from non tokenBinder address (should fail)
13. Rebinding token from non tokenBinderAddress (should fail).
17. Rebinding token from tokenBinderAddress.
15. Binding token from tokenBinder address.
16. Unbinding token from tokenBinder address.
14. Setting token binder to 0x000...0000.
15. Binding a token (should fail).


To run this test execute the following command:

```
POOL=[POOL_ADDRESS] npx buidler test ./mainnet-test/test.ts --network [rinkeby|mainnet]
```

This command will throw an error if something is not as expected.

Only **AFTER** these tests should the implementation be set on the actual smart pool contract. [See above](#setting-new-implementation) on how to do this.