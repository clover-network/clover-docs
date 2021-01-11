---
description: >-
  Returns the chain ID used for transaction signing at the current best block.
  None is returned if not available
---

# eth\_chainId



```text
web3.eth.getChainId([callback])
```

#### Returns

Returns chain ID.

#### Example

```text
> web3.eth.getChainId().then(console.log);
> 1337
```

