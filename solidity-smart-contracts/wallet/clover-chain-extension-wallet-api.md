# Developer

Clover Chain Wallet injects a global API into websites visited by its users at `window.CloverChain`.

This API specification borrows heavily from API MetaMask provided, considering the massive adoption. So Web3 site developers can easily connect their dApps with the Clover Chain Wallet. The APIs allow websites to request users' Clover Chain addresses, read data from the blockchain the user is connected to, and prompt the users to sign messages and transactions.

The presence of the provider object `window.CloverChain` indicates a Clover Chain/Clover Chain user.

The API this extension wallet provides includes API specified by [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193) and API defined by [MetaMask](https://docs.metamask.io/guide/ethereum-provider.html) \(including some massively relied legacy ones\).

## Development Progress <a id="development-progress"></a>

Currently \(version 1.112.8\) as Clover Chain Wallet natively supports Clover Chain, we are planning to open a series of APIs for dApp developers to interact with Clover Chain. At the end of the day, most [APIs available in Clover Chain javascript sdk](https://github.com/clover-network/clover-sdk) would be available.

Currently, only the following is supported:  `transfer`

Warning

Please read through this section if you are a web3 developer who has integrated with MetaMask and interested in integrating with Clover Chain Wallet.

### Inpage injected object <a id="inpage-injected-object"></a>

The biggest difference between Clover Chain Wallet and MetaMask is we inject Clover`Chain` rather than `ethereum` \(or `web3`\) to the web page. So user could keep two extensions at the same time.

### CloverChain.request\({method: "eth\_sign", params: \["address", "message"\]\) <a id="binancechainrequestmethod-eth_sign-params-address-message"></a>

We haven't supported the full complex [signing data](https://docs.metamask.io/guide/signing-data.html#signing-data-with-metamask) APIs MetaMask provided, while we only provide standard [`eth_sign`](https://eth.wiki/json-rpc/API#eth_sign) JSON-RPC call.

The `message` item in params for this request on MetaMask has to be hex-encoded keccak256 hash \(otherwise the API would silent failure for dapp https://www.reddit.com/r/Metamask/comments/9wp7kj/eth\_sign\_not\_working/\). In contrast, web3 developers could pass any message in plaintext to this method, and our UI would render it as it is to the wallet users.

### CloverChain.request\({method: "eth\_accounts"}\) <a id="binancechainrequestmethod-eth_accounts"></a>

When this API is invoked without the user's approval, MetaMask would return an empty array.

In contrast, we would ask the user to unlock his wallet and return the address user connected to.

## Upcoming Breaking Changes <a id="upcoming-breaking-changes"></a>

Warning

Important Information

On **November 16, 2020**, MetaMask is making changes to their provider API that will be breaking for some web3 sites. These changes are _upcoming_, but you can prepare for them today. Follow [this GitHub issue](https://github.com/MetaMask/metamask-extension/issues/8077) for updates.

In this implementation, some APIs will be supported as Legacy API \(For example we will still implement the `chainIdChanged` on CloverChain object until MetaMask formally deprecates it\).

## Basic Usage <a id="basic-usage"></a>

For any non-trivial Clover Chain web application — a.k.a. web3 site — to work, you will have to:

1. Detect the Clover Chain provider \(`window.CloverChain`\)
2. Detect which Clover Chain network the user is connected to
3. Get the user's Clover Chain account\(s\)

You can learn how to accomplish the `2` and `3` from above list by reviewing the snippet in the Using the Provider section.

The provider API is all you need to create a full-featured web3 application.

That said, many developers use a convenience library, such as ethers and web3.js, instead of using the provider directly. If you need higher-level abstractions than those provided by this API, we recommend that you use a convenience library.

## Chain IDs <a id="chain-ids"></a>

Warning

At the moment, the `CloverChain.chainId` property and the `chainChanged` event should be preferred over the `eth_chainId` RPC method.

Their chain ID values are correctly formatted, per the table below.

`eth_chainId` returns an incorrectly formatted \(0-padded\) chain ID for the chains in the table below, e.g. `0x01` instead of `0x1`. See the Upcoming Breaking Changes section for details on when the `eth_chainId` RPC method will be fixed.

Custom RPC endpoints are not affected; they always return the chain ID specified by the user.

These are the IDs of the Clover chains that Clover Chain Wallet supports by default.

| Hex | Decimal | Network |
| :--- | :--- | :--- |
| 0x3ff | 1023 | Clover Chain Preview Network \(clv-previewnet\) |

This API can also return chain ids of Clover Chains if users switch to them. The possible return value would be: \| Chain Id \| Network \| \| -------------------- \| ---------------------------------------- \| \|  Clover Chain Preview Network \(clv-previewnet\) \|

## Properties <a id="properties"></a>

### CloverChain.chainId <a id="binancechainchainid"></a>

Warning

The value of this property can change at any time, and should not be exclusively relied upon. See the `chainChanged` event for details.

**NOTE:** See the Chain IDs section for important information about the Clover Chain Wallet provider's chain IDs.

A hexadecimal string representing the current chain ID.

### CloverChain.autoRefreshOnNetworkChange <a id="binancechainautorefreshonnetworkchange"></a>

As the consumer of this API, it is your responsibility to handle chain changes using the `chainChanged` event. We recommend reloading the page on `chainChange` unless you have a good reason not to.

Please refer to [MetaMask Doc](https://docs.metamask.io/guide/ethereum-provider.html#ethereum-autorefreshonnetworkchange), the only difference is we injected a different object.

To disable this behavior, set this property to `false` immediately after detecting the provider:

```text
CloverChain.autoRefreshOnNetworkChange = false;
```

## Methods <a id="methods"></a>

### CloverChain.isConnected\(\) <a id="binancechainisconnected"></a>

Please refer to [MetaMask Doc](https://docs.metamask.io/guide/ethereum-provider.html#ethereum-isconnected), the only difference is we injected a different object.

```text
CloverChain.isConnected(): boolean;
```

### CloverChain.request\(args\) <a id="binancechainrequestargs"></a>

Please refer to [MetaMask Doc](https://docs.metamask.io/guide/ethereum-provider.html#ethereum-request-args), the only difference is we injected a different object.

We use this method to wrap an RPC API, Please see [the Ethereum wiki](https://eth.wiki/json-rpc/API#json-rpc-methods).

Important methods from this API include:

* [`eth_accounts`](https://eth.wiki/json-rpc/API#eth_accounts)
* [`eth_call`](https://eth.wiki/json-rpc/API#eth_call)
* [`eth_getBalance`](https://eth.wiki/json-rpc/API#eth_getBalance)
* [`eth_sendTransaction`](https://eth.wiki/json-rpc/API#eth_sendTransaction)
* [`eth_sign`](https://eth.wiki/json-rpc/API#eth_sign)

```text
interface RequestArguments {
  method: string;
  params?: unknown[] | object;
}

CloverChain.request(args: RequestArguments): Promise<unknown>;
```

#### Example <a id="example"></a>

The code snippet below is as same as [MetaMask's example](https://docs.metamask.io/guide/ethereum-provider.html#example), the only difference is we injected a different object.

```text
params: [
  {
    from: '0x063eBCD1dB02320814Acc0721e65f14b8755Ff41',
    to: '0x883591a195537e90FE878a62363758C74fFd9E95',
    gas: '0x76c0', // 30400
    gasPrice: '0x9184e72a000', // 10000000000000
    value: '0x9184e72a', // 2441406250
    data:
      '0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675',
  },
];

CloverChain
  .request({
    method: 'eth_sendTransaction',
    params,
  })
  .then((result) => {
    // The result varies by by RPC method.
    // For example, this method will return a transaction hash hexadecimal string on success.
  })
  .catch((error) => {
    // If the request fails, the Promise will reject with an error.
  });
```

### CloverChain.clvSign\(address: string, message: string\): Promise&lt;{publicKey: string, signature: string}&gt; <a id="binancechainbnbsignaddress-string-message-string-promisepublickey-string-signature-string"></a>

Like `eth_sign` enable dapp to verify a user has control over an ethereum address by signing an arbitrary message. We provide this method for dapp developers to request the signature of arbitrary messages for Clover Chain \(CLV\).

If `address` parameter is a clover chain address \(start with `0x`\), the message would be hashed in the same way with [`eth_sign`](https://eth.wiki/json-rpc/API#eth_sign).

The returned `publicKey` would be a compressed encoded format \(a hex string encoded 33 bytes starting with "0x02", "0x03"\) for CloverChain. And uncompressed encoded format \(a hex string encoded 65 bytes starting with "0x04"\).

The returned `signature` would be bytes encoded in hex string starting with `0x`. For CloverChain, its r,s catenated 64 bytes in total. For Clover Chain, like `eth_sign`, its r, s catenated 64 bytes and a recover byte in the end.

Warning

DApp developers should verify whether the returned public key can be converted into the address user claimed in addition to an ECDSA signature verification. Because any plugin can inject the same object `CloverChain` as Clover Chain Wallet.

### CloverChain.switchNetwork\(networkId: string\): Promise&lt;{networkId: string}&gt; <a id="binancechainswitchnetworknetworkid-string-promisenetworkid-string"></a>

As Clover Chain Wallet natively supports Clover Chain and Clover Chain which are heterogeneous blockchains run in parallel. APIs would be different in any situation. \(We would open APIs for Clover Chain very soon\).

Developers could judge which network is selected by user currently via Clover`Chain.chainId` or listening to the `chainChanged` event via `CloverChain.on('chainChanged', callback)`.

To request for network switching, developers could invoke this API with clv`-testnet` \(Clover Test Network\) to prompt user to agree on network switching.

### CloverChain.requestAccounts\(\) <a id="binancechainrequestaccounts"></a>

Request all accounts maintained by this extension.

The `id` in response would be used as `accountId` for the APIs for Clover Chain.

This method would return an array of Account:

```text
{
  addresses: [{address: string, type: string}],
  icon: string,
  id: string,
  name: string
}
```

For example:

```text
[
    {
        "id":"fba0b0ce46c7f79cd7cd91cdd732b6c699440acf8c539d7e7d753d38c9deea544230e51899d5d9841b8698b74a3c77b79e70d686c76cb35dca9cac0e615628ed",
        "name":"Account 1",
        "icon":"data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIGZpbGw9Im5vbmUiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgY2xhc3M9InNjLXBraElSIGhnRUNmUyI+PHJlY3Qgd2lkdGg9IjI0IiBoZWlnaHQ9IjI0IiByeD0iOCIgZmlsbD0iI2ZjNmU3NSI+PC9yZWN0Pjx0ZXh0IHg9IjUwJSIgeT0iNTAlIiBkb21pbmFudC1iYXNlbGluZT0iY2VudHJhbCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZmlsbD0iIzFlMjAyNiIgc3R5bGU9ImZvbnQtc3R5bGU6bm9ybWFsO2ZvbnQtc2l6ZToxNHB4O2ZvbnQtZmFtaWx5OkJpbmFuY2VQbGV4LCAtYXBwbGUtc3lzdGVtLCAmI3gyNzsuU0ZOU1RleHQtUmVndWxhciYjeDI3OywgJiN4Mjc7U2FuIEZyYW5jaXNjbyYjeDI3OywKQmxpbmtNYWNTeXN0ZW1Gb250LCAmI3gyNzsuUGluZ0ZhbmctU0MtUmVndWxhciYjeDI3OywgJiN4Mjc7TWljcm9zb2Z0IFlhSGVpJiN4Mjc7LCAmI3gyNztTZWdvZSBVSSYjeDI3OywgJiN4Mjc7SGVsdmV0aWNhIE5ldWUmI3gyNzssCkhlbHZldGljYSwgQXJpYWwsIHNhbnMtc2VyaWYiPkE8L3RleHQ+PC9zdmc+",
        "addresses":[
            {
                "type":"eth",
                "address":"0x883591a195537e90FE878a62363758C74fFd9E95"
            }
        ]
    }
]
```

### CloverChain.transfer\({fromAddress:string, toAddress:string, asset:string, amount:number, memo?: string, sequence?: number, accountId:string, networkId:string}\)&gt; <a id="binancechaintransferfromaddressstring-toaddressstring-assetstring-amountnumber-memo-string-sequence-number-accountidstring-networkidstring"></a>

Transfer certain `amount` of `asset` \(CEP20\) on CloverChain.

`accountId` could be retrieved from the `CloverChain.requestAccounts` API \(`id` field of each account\)

`networkId` could be clv`-previewnet`

For example:

1. This will ask the user's approval for transferring 1 CLV to himself. `CloverChain.transfer({fromAddress:"0x063eBCD1dB02320814Acc0721e65f14b8755F41", toAddress:"0x883591a195537e90FE878a62363758C74fFd9E95", asset:"CLV", amount:1, accountId:"fba0b0ce46c7f79cd7cd91cdd732b6c699440acf8c539d7e7d753d38c9deea544230e51899d5d9841b8698b74a3c77b79e70d686c76cb35dca9cac0e615628ed", networkId:"clv-testnet"})`
2. This will ask the user's approval for transferring 1 CUSD to himself. `CloverChain.transfer({fromAddress:"0x063eBCD1dB02320814Acc0721e65f14b8755F41", toAddress:"0x883591a195537e90FE878a62363758C74fFd9E95", asset:"CUSD-BAF", amount:1, accountId:"fba0b0ce46c7f79cd7cd91cdd732b6c699440acf8c539d7e7d753d38c9deea544230e51899d5d9841b8698b74a3c77b79e70d686c76cb35dca9cac0e615628ed", networkId:"clv-testnet"})`

## Events <a id="events"></a>

Please refer to [MetaMask Doc](https://docs.metamask.io/guide/ethereum-provider.html#events), the only difference is we injected a different object.

```text
CloverChain.on('accountsChanged', (accounts) => {
  // Handle the new accounts, or lack thereof.
  // "accounts" will always be an array, but it can be empty.
});

CloverChain.on('chainChanged', (chainId) => {
  // Handle the new chain.
  // Correctly handling chain changes can be complicated.
  // We recommend reloading the page unless you have a very good reason not to.
  window.location.reload();
});
```

### connect <a id="connect"></a>

Please refer to [MetaMask Doc](https://docs.metamask.io/guide/ethereum-provider.html#connect), the only difference is we injected a different object.

```text
interface ConnectInfo {
  chainId: string;
}

CloverChain.on('connect', handler: (connectInfo: ConnectInfo) => void);
```

### disconnect <a id="disconnect"></a>

Please refer to [MetaMask Doc](https://docs.metamask.io/guide/ethereum-provider.html#disconnect), the only difference is we injected a different object.

```text
CloverChain.on('disconnect', handler: (error: ProviderRpcError) => void);
```

### accountsChanged <a id="accountschanged"></a>

Please refer to [MetaMask Doc](https://docs.metamask.io/guide/ethereum-provider.html#accountschanged), the only difference is we injected a different object.

```text
CloverChain.on('accountsChanged', handler: (accounts: Array<string>) => void);
```

### chainChanged <a id="chainchanged"></a>

Please refer to [MetaMask Doc](https://docs.metamask.io/guide/ethereum-provider.html#chainchanged), the only difference is we injected a different object.

```text
CloverChain.on('chainChanged', handler: (chainId: string) => void);
```

```text
CloverChain.on('chainChanged', (_chainId) => window.location.reload());
```

### message <a id="message"></a>

Please refer to [MetaMask Doc](https://docs.metamask.io/guide/ethereum-provider.html#message), the only difference is we injected a different object.

```text
interface ProviderMessage {
  type: string;
  data: unknown;
}

CloverChain.on('message', handler: (message: ProviderMessage) => void);
```

## Errors <a id="errors"></a>

Please refer to [MetaMask Doc](https://docs.metamask.io/guide/ethereum-provider.html#errors), the only difference is we injected a different object.

## Using the Provider <a id="using-the-provider"></a>

This snippet explains how to accomplish the three most common requirements for web3 sites:

* Detect which CloverChain network the user is connected to
* Get the user's CloverChain account\(s\)

```text
/**********************************************************/
/* Handle chain (network) and chainChanged (per EIP-1193) */
/**********************************************************/

// Normally, we would recommend the 'eth_chainId' RPC method, but it currently
// returns incorrectly formatted chain ID values.
let currentChainId = CloverChain.chainId;

CloverChain.on('chainChanged', handleChainChanged);

function handleChainChanged(_chainId) {
  // We recommend reloading the page, unless you must do otherwise
  window.location.reload();
}

/***********************************************************/
/* Handle user accounts and accountsChanged (per EIP-1193) */
/***********************************************************/

let currentAccount = null;
CloverChain
  .request({ method: 'eth_accounts' })
  .then(handleAccountsChanged)
  .catch((err) => {
    // Some unexpected error.
    // For backwards compatibility reasons, if no accounts are available,
    // eth_accounts will return an empty array.
    console.error(err);
  });

// Note that this event is emitted on page load.
// If the array of accounts is non-empty, you're already
// connected.
CloverChain.on('accountsChanged', handleAccountsChanged);

// For now, 'eth_accounts' will continue to always return an array
function handleAccountsChanged(accounts) {
  if (accounts.length === 0) {
    // Clover Chain Wallet is locked or the user has not connected any accounts
    console.log('Please connect to Clover Chain Wallet.');
  } else if (accounts[0] !== currentAccount) {
    currentAccount = accounts[0];
    // Do any other work!
  }
}

/*********************************************/
/* Access the user's accounts (per EIP-1102) */
/*********************************************/

// You should only attempt to request the user's accounts in response to user
// interaction, such as a button click.
// Otherwise, you popup-spam the user like it's 1999.
// If you fail to retrieve the user's account(s), you should encourage the user
// to initiate the attempt.
document.getElementById('connectButton', connect);

function connect() {
  CloverChain
    .request({ method: 'eth_requestAccounts' })
    .then(handleAccountsChanged)
    .catch((err) => {
      if (err.code === 4001) {
        // EIP-1193 userRejectedRequest error
        // If this happens, the user rejected the connection request.
        console.log('Please connect to MetaMask.');
      } else {
        console.error(err);
      }
    });
}
```

## Legacy API <a id="legacy-api"></a>

Warning

You should **never** rely on any of these methods, properties, or events in practice.

This section documents MetaMask's legacy provider API.

To be compatible with existing dApps that support MetaMask, Clover Chain Wallet implements them as well, but please don't rely on them. We may deprecate them soon in the future.

## Legacy Properties <a id="legacy-properties"></a>

### CloverChain.networkVersion \(DEPRECATED\) <a id="binancechainnetworkversion-deprecated"></a>

Please refer to [MetaMask Doc](https://docs.metamask.io/guide/ethereum-provider.html#legacy-properties), the only difference is we injected a different object.

### CloverChain.selectedAddress \(DEPRECATED\) <a id="binancechainselectedaddress-deprecated"></a>

Please refer to [MetaMask Doc](https://docs.metamask.io/guide/ethereum-provider.html#ethereum-selectedaddress-deprecated), the only difference is we injected a different object.

## Legacy Methods <a id="legacy-methods"></a>

### CloverChain.enable\(\) \(DEPRECATED\) <a id="binancechainenable-deprecated"></a>

Please refer to [MetaMask Doc](https://docs.metamask.io/guide/ethereum-provider.html#ethereum-enable-deprecated), the only difference is we injected a different object.

### CloverChain.sendAsync\(\) \(DEPRECATED\) <a id="binancechainsendasync-deprecated"></a>

Please refer to [MetaMask Doc](https://docs.metamask.io/guide/ethereum-provider.html#ethereum-sendasync-deprecated), the only difference is we injected a different object.

```text
interface JsonRpcRequest {
  id: string | undefined;
  jsonrpc: '2.0';
  method: string;
  params?: Array<any>;
}

interface JsonRpcResponse {
  id: string | undefined;
  jsonrpc: '2.0';
  method: string;
  result?: unknown;
  error?: Error;
}

type JsonRpcCallback = (error: Error, response: JsonRpcResponse) => unknown;

CloverChain.sendAsync(payload: JsonRpcRequest, callback: JsonRpcCallback): void;
```

### CloverChain.send\(\) \(DEPRECATED\) <a id="binancechainsend-deprecated"></a>

Please refer to [MetaMask Doc](https://docs.metamask.io/guide/ethereum-provider.html#ethereum-send-deprecated), the only difference is we injected a different object.

```text
CloverChain.send(
  methodOrPayload: string | JsonRpcRequest,
  paramsOrCallback: Array<unknown> | JsonRpcCallback,
): Promise<JsonRpcResponse> | void;
```

This method behaves unpredictably and should be avoided at all costs. It is essentially an overloaded version of `CloverChain.sendAsync()`.

`CloverChain.send()` can be called in three different ways:

```text
// 1.
CloverChain.send(payload: JsonRpcRequest, callback: JsonRpcCallback): void;

// 2.
CloverChain.send(method: string, params?: Array<unknown>): Promise<JsonRpcResponse>;

// 3.
CloverChain.send(payload: JsonRpcRequest): unknown;
```

You can think of these signatures as follows:

1. This signature is exactly like `CloverChain.sendAsync()`
2. This signature is like an async `CloverChain.sendAsync()` with `method` and `params` as arguments, instead of a JSON-RPC payload and callback
3. This signature enables you to call the following RPC methods synchronously:
4. `eth_accounts`
5. `eth_coinbase`
6. `eth_uninstallFilter`
7. `net_version`

## Legacy Events <a id="legacy-events"></a>

### close \(DEPRECATED\) <a id="close-deprecated"></a>

Please refer to [MetaMask Doc](https://docs.metamask.io/guide/ethereum-provider.html#close-deprecated), the only difference is we injected a different object.

```text
CloverChain.on('close', handler: (error: Error) => void);
```

### chainIdChanged \(DEPRECATED\) <a id="chainidchanged-deprecated"></a>

Please refer to [MetaMask Doc](https://docs.metamask.io/guide/ethereum-provider.html#chainidchanged-deprecated), the only difference is we injected a different object.

```text
CloverChain.on('chainChanged', handler: (chainId: string) => void);
```

### networkChanged \(DEPRECATED\) <a id="networkchanged-deprecated"></a>

Please refer to [MetaMask Doc](https://docs.metamask.io/guide/ethereum-provider.html#networkchanged-deprecated), the only difference is we injected a different object.

```text
CloverChain.on('networkChanged', handler: (networkId: string) => void);
```

### notification \(DEPRECATED\) <a id="notification-deprecated"></a>

Please refer to [MetaMask Doc](https://docs.metamask.io/guide/ethereum-provider.html#notification-deprecated), the only difference is we injected a different object.

```text
CloverChain.on('notification', handler: (payload: any) => void);
```

