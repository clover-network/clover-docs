---
description: Estimate gas needed for execution of given contract
---

# eth\_estimateGas

```
web3.eth.estimateGas(callObject [, callback])
```

#### Parameters

1. A transaction object.
2. (optional) Optional callback, returns an error object as first parameter and the result as second.

#### Returns

the used gas for the simulated call/transaction.

#### Example

```
web3.eth.estimateGas({
    from : account1,
    to : account2
}).then(console.log);
> 21000
```
