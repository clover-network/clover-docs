---
description: Returns the coinbase address to which mining rewards will go.
---

# eth\_coinbase

```
web3.eth.getCoinbase([callback])
```

#### Returns

The coinbase address set in the node for mining rewards

#### Example

```
> web3.eth.getCoinbase().then(console.log)
> 0x0000000000000000000000000000000000000000
```
