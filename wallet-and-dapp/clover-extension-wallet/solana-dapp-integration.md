# Solana-dApp-Integration

Once Clover extension wallet is installed, it will inject the following to window object:

```
window.clover_solana = {
    isCloverWallet: true,
    getAccount: async() => {...}
    signTransaction: async (paload) => {...}
}
```

dApps can use the above injected object to integrate with Clover extension wallet.

## dApp Development Documents

### RPC document

[https://docs.solana.com/](https://docs.solana.com)

### Wallet Adapter API (lib for communicate with web wallet or extension)

{% embed url="https://github.com/project-serum/sol-wallet-adapter" %}

### Web3 JS (create transactions in frontend)

{% embed url="https://github.com/solana-labs/solana-web3.js" %}

