# Experinator

Converts Pie Smart Pools into ExperiPies 🥧

### External deployed contracts

```text
Diamond implementation: 0x1f863776975a69b6078fdafab6298d3e823e0190
Balancer factory: 0x9424B1412450D0f8Fc2255FAf6046b98213B76Bd
Smart pool implementation: 0x706f00ea85a71eb5d7c2ce2ad61dbbe62b616435
```

### Deployed contracts

```text
experinator: 0xd6a2AAeb7ee0243D7d3148cCDB10C0BD1bb56336
smartpool storage doctor: 0xd7Db1aE8193A12D0ee5e1cf53D7Bcf0f20D09757
experiPie storage doctor: 0xCA4Dc78E1BB0520606195DF3BBD24638fF996852
```

### How to migrate a Pie Smart Pool to a PieVault

Set the proxy owner and smart pool controller to the experinator contract address `0xd6a2AAeb7ee0243D7d3148cCDB10C0BD1bb56336`

Migrate pie

```text
npx harhat to-experipie --experinator 0xd6a2AAeb7ee0243D7d3148cCDB10C0BD1bb56336 --pie [smart pool address]
```

Afterwards you need to set the fees, cap and other params if necesary  
  
Repo: [https://github.com/pie-dao/experinator](https://github.com/pie-dao/experinator)

