---
description: Use Web3.js to Send Transaction on Clover
---

# Send Transaction

For our example, we only need a single JavaScript file (arbitrarily named _transaction.js_) to create and send the transaction, which we will run using the `node` command in the terminal. The script will transfer 100 CLV from the genesis account to another address. For simplicity, the file is divided into three sections: variable definition, create transaction, and send transaction.

We need to set a couple of values in the variable definitions, then construct and sign the transaction:

1. Create your Web3 constructor (`Web3`).
2. Specify the received address, CLV amount, gas price and gas limit.
3. Sign the transaction and broadcast it the Clover chain

The code looks like:

```javascript
const Web3 = require('web3')

const web3 = new Web3(new Web3.providers.HttpProvider('http://localhost:9933'));
const GENESIS_ACCOUNT = '0xe6206C7f064c7d77C6d8e3eD8601c9AA435419cE';

async function webTransfer(account, etherValue) {
  try {
    const before = await web3.eth.getBalance(account);
    console.log(`before transfer: ${account}, has balance ${web3.utils.fromWei(before, "ether")}`);

    const nonce = await web3.eth.getTransactionCount(GENESIS_ACCOUNT);
    const signedTransaction = await web3.eth.accounts.signTransaction({
      from: GENESIS_ACCOUNT,
      to: account,
      value: web3.utils.toWei(etherValue.toString(), "ether"),
      gasPrice: web3.utils.toWei("100", "gwei"),
      gas: "0x5208",
      nonce: nonce
    }, 'your private key configured somewhere');
    await web3.eth.sendSignedTransaction(signedTransaction.rawTransaction);
    const after = await web3.eth.getBalance(account);
    console.log(`after transfer: ${account}, has balance ${web3.utils.fromWei(after, "ether")}`);
    return { success: true };
  } catch (e) {
    return { success: false, message: e.toString() };
  }
}

webTransfer('0x1874FC5f915aa9Fd24C379fE7F9ec40607CEf78A', 100).then(console.log)
```

## Running the Script

You can run the above script using command :

```
node transaction.js
```

