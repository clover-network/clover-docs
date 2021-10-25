---
description: Returns content of the storage at given address
---

# eth\_getStorageAt

```
web3.eth.getStorageAt(address, position [, defaultBlock] [, callback]
```

#### Parameters

1、The address to get the storage from.

2、The index position of the storage.

3、(optional) If you pass this parameter it will not use the default block set with [web3.eth.defaultBlock](https://web3js.readthedocs.io/en/v1.3.0/web3-eth.html#eth-defaultblock). Pre-defined block numbers as `"earliest"`, `"latest"` and `"pending"` can also be used.

4、(optional) Optional callback, returns an error object as first parameter and the result as second.

#### Returns

The value in storage at the given position.

#### Example

```
> web3.eth.getStorageAt("0x063eBCD1dB02320814Acc0721e65f14b8755Ff41", 0).then(console.log)
> 0x0000000000000000000000000000000000000000000000000000000000000000
```
