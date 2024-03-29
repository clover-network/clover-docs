---
description: >-
  This section shows how to connect to Clover mainnet and query the account
  balance
---

# Query Balance

## **Prerequisites**

Install @polkadot/api, the suggested version is ^4.15.1 (equal or above 4.15.1)

## Sample code

```javascript
const API = require("@polkadot/api")
const cloverTypes = require('@clover-network/node-types');

async function getBalance(address) {
    const wsProvider = new API.WsProvider('wss://api-ivy.clover.finance');
    const api = await API.ApiPromise.create({
        provider: wsProvider,
        types: cloverTypes
    });
    const { parentHash } = await api.rpc.chain.getHeader();
    const {
        data: { free: balance },
    } = await api.query.system.account.at(parentHash, address);
    return balance.toString();
}

getBalance('5DZabq7G2m12g1GUuqs66CCAVcmzdWRkXsGxpi4dgXFEcLEb').then(console.log)
```

Furthermore you may use **formatBalance** from **@polkadot/util** to format the display



 
