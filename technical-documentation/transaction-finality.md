# Transaction Finality

PolkadotJS API provides **signAndSend** function to send a transaction,  the callback will yield information around the transaction pool status as well as any events when isInBlock or isFinalized.  If you receive the isFinalized event, then your transaction is considered as finality and will never be reverted.

## Sample Code

```javascript
const API = require("@polkadot/api");
const cloverTypes = require('@clover-network/node-types');

async function sendCLV(address, amount) {
  const wsProvider = new API.WsProvider('wss://api-ivy.clover.finance');
  const api = await API.ApiPromise.create({
    provider: wsProvider,
    types: cloverTypes
  });
  const nonce = await api.rpc.system.accountNextIndex('your CLV address configured somewhere');
  const keyring = new API.Keyring({ type: 'sr25519' });
  const signer = keyring.addFromUri('your seeds configured somewhere');
  return new Promise( (resolve, reject) => {
    api.tx.balances
      .transfer(address, amount)
      .signAndSend(signer, {
        nonce,
      }, ({ events = [], status }) => {
        if (status.isFinalized) {
          // filter events and check balances.transfer is succeed
          resolve({ success: true });
        }
        if (status.isDropped || status.isInvalid || status.isUsurped) {
          resolve({ success: false });
        }
      });
  });
}
```

