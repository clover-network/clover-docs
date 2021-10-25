---
description: >-
  Returns the chain ID used for transaction signing at the current best block.
  None is returned if not available
---

# eth\_chainId



```
web3.eth.getChainId([callback])
```

#### Returns

Returns chain ID.

#### Example

```
> web3.eth.getChainId().then(console.log);
> 1023
```
