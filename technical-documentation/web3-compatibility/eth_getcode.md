---
description: Returns the code at given address at given time (block number)
---

# eth\_getCode

```text
web3.eth.getCode(address [, defaultBlock] [, callback])
```

#### Parameters

1. The address to get the code from.
2. \(optional\) If you pass this parameter it will not use the default block set with [web3.eth.defaultBlock](https://web3js.readthedocs.io/en/v1.3.0/web3-eth.html#eth-defaultblock). Pre-defined block numbers as `"earliest"`, `"latest"` and `"pending"` can also be used.
3.  Optional callback, returns an error object as first parameter and the result as second.

#### Returns

The data at given address `address`

#### Example

```text
> web3.eth.getCode("0x063eBCD1dB02320814Acc0721e65f14b8755Ff41").then(console.log);
> 0x
```

