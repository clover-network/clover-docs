---
description: 'Sends signed transaction, returning its hash'
---

# eth\_sendSignedTransaction

```text
web3.eth.sendSignedTransaction(signedTransactionData [, callback])
```

#### Parameters

1. Signed transaction data in HEX format
2. \(optional\) Optional callback, returns an error object as first parameter and the result as second

#### Returns

The callback will return the 32 bytes transaction hash.

#### Example

```text
> web3.eth.accounts.signTransaction({
        from : "0x063eBCD1dB02320814Acc0721e65f14b8755Ff41",
        to : "0x69d8fa34508C43C8533a32ab278aDDDE2820556b",
        value : web3.utils.toWei('0.1', 'ether'),
        gas : web3.utils.toHex(21000),
        gasPrice : web3.utils.toWei("1", "gwei"),
        nonce : 19
    },account1_key,function(err,raw){
        if(!err){
            web3.eth.sendSignedTransaction(raw.rawTransaction, function(error, result){
                if(!error){
                    console.log(result)
                }else{
                    console.log(error)
                }
            })
        }else{
            console.log(err)
        }
    });
> 0x52e5f6347bdbf6fe1d8acb07bb460ce31d82bb36e80acfdc8aca798e28415334
```

