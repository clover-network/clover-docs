# DApp Integration

Clover Extension Wallet injected into web pages a varible, which named 'clover'. DApp developer could integrate with the wallet with window.clover. Below snippet shows how to use it to interact between dapp and the wallet.

```text
const example = async () => {
    // connect to wallet and get accounts
    const accounts = await window.clover.request({ method: 'eth_requestAccounts' })
    
    // the first account is the selected account
    const currAccount = accounts[0]
        
    // get chain id
    const chainId = await window.clover.request({ method: 'eth_chainId' });
    
    const transactionParameters = {
        nonce: '0x05',
        gasPrice: '0x3e95ba80', // could set by user
        gas: '0x2710', // could set by user
        to: '0x66cb476bdbd6b55804644072255a1156e6977b23',
        from: currAccount,
        value: '0x2386f26fc10000',
        chainId: chainId,
    };
    
    const txHash = await window.clover.request({
      method: 'eth_sendTransaction',
      params: [transactionParameters],
    });
}

const handleAccountsChanged = async (accounts) => {
    // here could set current account with accounts[0]
}

window.clover.on('accountsChanged', handleAccountsChanged);
```

