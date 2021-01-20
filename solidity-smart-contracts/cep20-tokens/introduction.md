# Introduction

## CEP20 Token <a id="bep20-token"></a>

A CEP20 token must implement the interface `ICEP20` in ICEP20.sol. This is a template contract CEP20Token.template. Users just need to fill in `_name`, `_symbol`, `_decimals` and `_totalSupply` according to their own requirements:

```text
  constructor() public {
    _name = {{TOKEN_NAME}};
    _symbol = {{TOKEN_SYMBOL}};
    _decimals = {{DECIMALS}};
    _totalSupply = {{TOTAL_SUPPLY}};
    _balances[msg.sender] = _totalSupply;

    emit Transfer(address(0), msg.sender, _totalSupply);
  }
```

Then users can use [Remix IDE](https://remix.ethereum.org/) and Metamask to compile and deploy the CEP20 contract to CLV.

### Interact with Contract with [Web3](https://www.npmjs.com/package/web3) and NodeJS. <a id="interact-with-contract-with-web3-and-nodejs"></a>

#### Connect to Clover Chain's public RPC endpoint <a id="connect-to-binance-smart-chains-public-rpc-endpoint"></a>

```text
const Web3 = require('web3');

// testnet
const web3 = new Web3('https://rpc.clover.finance');
```

#### Create a wallet <a id="create-a-wallet"></a>

```text
web3.eth.accounts.create([entropy]);
```

#### Output:

```text
web3.eth.accounts.create();
{
  address: '0x883591a195537e90FE878a62363758C74fFd9E95',
  privateKey: '0x5a7a2e98e2c63b8d3f9bcfd8d4b41846e28c1d01f6184550d42b688897c9bf4c',
  signTransaction: [Function: signTransaction],
  sign: [Function: sign],
  encrypt: [Function: encrypt]
}
```

#### Recover a wallet <a id="recover-a-wallet"></a>

```text
const account = web3.eth.accounts.privateKeyToAccount("0x5a7a2e98e2c63b8d3f9bcfd8d4b41846e28c1d01f6184550d42b688897c9bf4c")
```

#### Check balance <a id="check-balance"></a>

```text
web3.eth.getBalance(holder).then(console.log);
```

#### Output:

The balance will be bumped by e18 for CLV.

```text
6249621999900000000
```

#### Create transaction <a id="create-transaction"></a>

**Parameters**

* Object - The transaction object to send:
* from - String\|Number: The address for the sending account. Uses the web3.eth.defaultAccount property, if not specified. Or an address or index of a local wallet in web3.eth.accounts.wallet.
* to - String: \(optional\) The destination address of the message, left undefined for a contract-creation transaction.
* value - Number\|String\|BN\|BigNumber: \(optional\) The value transferred for the transaction in wei, also the endowment if it’s a contract-creation transaction.
* gas - Number: \(optional, default: To-Be-Determined\) The amount of gas to use for the transaction \(unused gas is refunded\).
* gasPrice - Number\|String\|BN\|BigNumber: \(optional\) The price of gas for this transaction in wei, defaults to web3.eth.gasPrice.
* data - String: \(optional\) Either a ABI byte string containing the data of the function call on a contract, or in the case of a contract-creation transaction the initialisation code.
* nonce - Number: \(optional\) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.

```text
    // // Make a transaction using the promise
    web3.eth.sendTransaction({
        from: holder,
        to: '0x063eBCD1dB02320814Acc0721e65f14b8755Ff41',
        value: '1000000000000000000',
        gas: 5000000,
        gasPrice: 18e9,
    }, function(err, transactionHash) {
      if (err) {
        console.log(err);
        } else {
        console.log(transactionHash);
       }
    });
```

