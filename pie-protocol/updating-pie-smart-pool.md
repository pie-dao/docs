---
description: >-
    How to update a PieDAO smart pool.
---


# Updating a PieDAO smart pool

All Pie DAO smart pools are upgradeable because they use a proxy contract. At PieDAO we use our implementation of a simple proxy contract called [pie-proxy](https://github.com/pie-dao/pie-proxy).

Upgrading a contract is done by setting the implementation address on the proxy contract to the address of the new logic contract. Using this pattern allows smart pools to be upgraded while keeping keeping the old storage.

## Deploying a new implementation

Clone the [pie-smart-pools repository](https://github.com/pie-dao/pie-smart-pools) and checkout master. Master contains the latest release which is considered stable and audited for mainnet use.

copy the contents of ``env.example`` to a new file called ``.env`` and edit the the relevant values inside. DO NOT share this file with anyone as it will contain sensitive data.

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

For simplicity sake we assume the address is controlled by a Metamask wallet and we upgrade the implementation from Etherscan.

1. Go to https://etherscan/address/[ADDRESS_OF_PIE_SMART_POOL]/#writeContract
2. Click "Write Contract".
3. Click "Connect Metamask".
4. Go to setImplementation.
5. Paste address of implementation in the input field.
6. Click write.
7. Confirm transaction in Metamask.
8. Enjoy your upgraded contract.

## Testing procedure

Because Pie smart pools deal with real value it is important to take extra precaution when upgrading them to minimize the risk to users funds.

For BTC++ there is a private testing pool on mainnet at: [0x21909429c586fb739653e8e0b913b23fe30bacfa](https://etherscan.io/token/0x21909429c586fb739653e8e0b913b23fe30bacfa).

Before upgrading any pool we do a test run using this pool first. To do this we take the following steps:

1. Deploy implementation (described above).
2. Set cap to zero.
3. Set implementation (described above).

### Automated testing

The automated test tests this functionality on mainnet:

1. Check enforcement of cap.
2. Setting cap from non controller.
3. Changing controller address.
4. Setting the cap.
5. joinPool.
6. exitPool.
7. exitPool taking loss,
8. Setting public swap from non public swap setter (should fail).
9. Setting public swap setter.
10. Setting public swap from right address.
11. Binding token from non tokenBinder address (should fail).
12. Unbinding token from non tokenBinder address (should fail).
13. Rebinding token from non tokenBinderAddress (should fail).
14. Changing tokenBinder address.
15. Binding token from tokenBinder address.
16. Unbinding token from tokenBinder address.
17. Rebinding token from tokenBinderAddress.

To run this test execute the following command:

```
npx buidler mainnet-test --pool [SMART_POOL_ADDRESS] --network mainnet
```

This command will throw an error if something is not as expected.

Only **AFTER** these tests should the implementation be set on the actual smart pool contract.