# DApp Integration

### Integrated with JS

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

### Integrated with web3-react

First, Web3ReactProvider and getLibrary should be used as provider as below:

```text
<Web3ReactProvider getLibrary={getLibrary}>
    {/* <...> */}
</Web3ReactProvider>

```

Second, initialize InjectedConnector, which could be used as connector to connect to Clover Extension Wallet.

```text
const injectedConnector = new InjectedConnector({ supportedChainIds: [1, 3] })
```

At last, we could use injectedConnector to connect to and communicate with the wallet

```text
const { activate, deactivate, library, account, error } = useWeb3React()

useEffect(() => {
    activate(injectedConnector, async (error) => {
        if (error instanceof UnsupportedChainIdError) {
            setToast('error', 'Unsupported chain id')
        } else {
            if (error instanceof NoEthereumProviderError) {
                setToast('error', 'No provider was found')
            } else if (
                error instanceof UserRejectedRequestErrorInjected
            ) {
                setToast('error', 'Please authorize to access your account')
            } else {
                setToast('error', error.message)
            }
            }
    })
}, [activate])

useEffect(() => {
    const send = async () => {
        if (account !== undefined) {
            const chainId = await library.request({ method: 'eth_chainId' });
    
            const transactionParameters = {
                nonce: '0x05',
                to: account,
                from: account,
                value: '0x2386f26fc10000',
                chainId: chainId,
            };
            
            const txHash = await library.request({
              method: 'eth_sendTransaction',
              params: [transactionParameters],
            });
        }
    }
    send()
}, [account, library])

```



