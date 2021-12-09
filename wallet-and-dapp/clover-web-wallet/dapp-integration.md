# dApp Integration

dApps can easily integrate Clover Web Wallet for transaction & message signing. The SDK **@clover-network/web-wallet-sdk** is **** needed for the integration.

The SDK can be installed as follows:

```shell
yarn add @clover-network/web-wallet-sdk
```

### Substrate Blockchains Integration

Clover web wallet supports dApps on substrate based blockchains, such as Polkadot, Kusama, Acala, etc.  The sample code is as follows:

```typescript
import CloverWebInjected from '@clover-network/web-wallet-sdk';
import { web3Enable, web3Accounts, web3FromAddress } from "@polkadot/extension-dapp";

const clvInject = new CloverWebInjected({ zIndex: 99999 });

const initInjector = async () => {
    await clvInject.init({
      network: {
        chainId: '0x1',
      },
      enableLogging: true,
    });
    
    await clvInject.polkadotLogin(); // After this success, the injector has been injected into web page
    const injected: any = await web3Enable('clv'); // injector could be get in standard way
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

### EVM Blockchains Integration

Clover web wallet supports dApps on EVM blockchains, such as Ethereum, Moonbeam. The sample code is as follows:

```typescript
import CloverWebInjected from '@clover-network/web-wallet-sdk';

const clvInject = new CloverWebInjected({ zIndex: 99999 });

const initInjector = async () => {
    await clvInject.init({
      network: {
        chainId: '0x1',
      },
      enableLogging: true,
    });
    
    await clvInject.login();
    clvInject.provider.on('accountsChanged', (accounts) => {
      // do something
    });
}tye

// send transaction
const send = (): void => {
  const web3 = new Web3(clvInject.provider)
  const accounts = await web3.eth.getAccounts();
  const publicAddress = accounts[0]
  web3.eth.sendTransaction({ 
      from: publicAddress, 
      to: publicAddress, 
      value: web3.utils.toWei('0.01') 
    })
}

// eth_sign
const signMessage = (): void => {
  const web3 = new Web3(clvInject.provider)
  const accounts = await web3.eth.getAccounts();
  const publicAddress = accounts[0]
  // hex message
  const message = '0x000000000000000000000000000000000000';
  web3.currentProvider.send(
    {
      method: 'eth_sign',
      params: [publicAddress, message],
      jsonrpc: '2.0',
    },
    (err: Error, result: any) => {
      // error handle
    },
  );
}

// personal message
const signPersonalMsg = async () => {
    try {
      const web3 = new Web3(clvInject.provider)
      const accounts = await web3.eth.getAccounts();
      const publicAddress = accounts[0]
      const message = 'Example';
      const hash = web3.utils.sha3(message);
      const sig = await web3.eth.personal.sign(hash, publicAddress, '');
    } catch (error) {
      //error
    }
  }

// eth_signTypedData, eth_signTypedData_v3, eth_signTypedData_v4 are also supported

```

### Solana Blockchain Integration

Clover web wallet supports dApps on Solana. The sample code is as follows:

```typescript
import CloverWebInjected from '@clover-network/web-wallet-sdk';

const clvInject = new CloverWebInjected({ zIndex: 99999 });

const initInjector = async () => {
  await clvInject.init({
    network: {
      chainId: '0x1',
    },
    enableLogging: true,
  });
  
  await clvInject.solLogin();
}

const sendSolana = async () => {
  const solAddress = await clvInject.clover_solana.getAccount();
  const connection = new solanaWeb3.Connection(
    solanaWeb3.clusterApiUrl('mainnet-beta'),
    'confirmed',
  );
  const fromPubkey = new solanaWeb3.PublicKey(solAddress);
  const toPubkey = new solanaWeb3.PublicKey(solAddress);
  const transaction = new solanaWeb3.Transaction().add(
    solanaWeb3.SystemProgram.transfer({
      fromPubkey: fromPubkey,
      toPubkey: toPubkey,
      lamports: solanaWeb3.LAMPORTS_PER_SOL * 0,
    }),
  );

  const block = await connection.getRecentBlockhash('max');
  transaction.recentBlockhash = block.blockhash;
  transaction.setSigners(fromPubkey);

  const sss = await clvInject.clover_solana.signTransaction(transaction);
  const rawTransaction = sss.serialize();
  const a = await connection.sendRawTransaction(rawTransaction, {
    skipPreflight: false,
    preflightCommitment: 'single',
  });

  this.console('transaction hash:' + a);
}

const sendSolanaAll = async () => {
  const solAddress = await clvInject.clover_solana.getAccount();
  const connection = new solanaWeb3.Connection(
    solanaWeb3.clusterApiUrl('mainnet-beta'),
    'confirmed',
  );
  const fromPubkey = new solanaWeb3.PublicKey(solAddress);
  const toPubkey = new solanaWeb3.PublicKey(solAddress);
  const transaction = new solanaWeb3.Transaction().add(
    solanaWeb3.SystemProgram.transfer({
      fromPubkey: fromPubkey,
      toPubkey: toPubkey,
      lamports: solanaWeb3.LAMPORTS_PER_SOL * 0,
    }),
  );

  const block = await connection.getRecentBlockhash('max');
  transaction.recentBlockhash = block.blockhash;
  transaction.setSigners(fromPubkey);

  const sss = await clvInject.clover_solana.signAllTransactions([transaction]);
  const rawTransaction = sss[0].serialize();
  const a = await connection.sendRawTransaction(rawTransaction, {
    skipPreflight: false,
    preflightCommitment: 'single',
  });

  this.console('transaction hash:' + a);
}

```
