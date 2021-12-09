# Substrate dApp Integration

Clover Extension Wallet use method 'injectExtension' of **@polkadot/extension-inject** to inject a object into dApp's web page, then it could be used by calling web3Enable() as official document said. Sample code is below:

```typescript
import { web3Accounts, web3Enable } from '@polkadot/extension-dapp';

// find clover extension wallet injector
const findCloverInjected = async () => {
  const injected = await web3Enable('clv');
  if (!injected.length) {
    return {
      message: "Not found wallet",
      status: 'error'
    };
  }

  const cloverInjector = injected.find(injected, (w) => isCloverWallet(w))
  return cloverInjector
}

// sign message
const polkadotSignMessage = async () => {
    const polkadotAddress = await web3Accounts({ ss58Format: 42 });
    const wrapped = u8aWrapBytes(polkadotAddress.toLowerCase());
    const ret = await currentInjected.signer.signRaw({
      data: u8aToHex(wrapped),
      address: polkadotAddress,
      type: "bytes",
    });
  }
  
// send transaction
polkadotSignTransaction = async () => {
  const wsProvider = new WsProvider('wss://rpc.polkadot.io');
  const api = await ApiPromise.create({provider: wsProvider});
  const polkadotAddress = await web3Accounts({ ss58Format: 42 });
  const currentClvAccount = polkadotAddress
  const injected = await web3FromAddress(currentClvAccount)
  api.setSigner(injected.signer)
  const unsub = await api.tx.balances
    .transfer(currentClvAccount, 0)
    .signAndSend(currentClvAccount, (result) => {

      if (result.status.isInBlock) {
        // in block
      } else if (result.status.isFinalized) {
        unsub();
      }
    })
  }
```
