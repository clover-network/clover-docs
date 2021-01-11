---
description: >-
  Returns the current gas price oracle. The gas price is determined by the last
  few blocks median gas price.
---

# eth\_gasPrice

```text
web3.eth.getGasPrice([callback])
```

#### Returns

Number string of the current gas price in wei

#### Example

```text
> web3.eth.getGasPrice().then(console.log)
> 1000000000
```

