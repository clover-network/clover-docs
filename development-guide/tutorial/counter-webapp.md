# Counter Webapp

Now we have the `Counter` smart contract and deployed it to the clover local node. It's time to make changes to the web app so that it can interact with the `Counter` smart contract instance.&#x20;

## Preparation

### Setup the browser wallet

As it's a web application, make sure you've setup a `browser wallet` that connects to the local node. Follow the tutorials in the [Quick Start](broken-reference) section if you have set it up yet. In this guide, we assume you connect to the local node using the [MetaMask](../using-metamask.md) wallet.&#x20;

{% hint style="info" %}
&#x20;MetaMask is the most commonly used browser wallet for Ethereum like blockchain networks. We can use MetaMask to connect to Clover network since Clover is fully compatible with Ethereum.&#x20;
{% endhint %}

After you have the wallet installed, import the `dev account` , you can import the dev account using the seed phase:

> bottom drive obey lake curtain smoke basket hold race lonely fit walk

Or using just the private key:

> 0x03183f27e9d78698a05c24eb6732630eb17725fcf2b53ee3a6a635d6ff139680

{% hint style="danger" %}
Do not use this key or seed phase in any real chain to store your assets! It's for test only, you will lose your money if you used them  in any real chain!
{% endhint %}

### Install Packges

Install necessary packages before we start:

```bash
$ yarn add @web3-react/core @web3-react/injected-connector @ethersproject/providers
```

The `@web3-react` packages provide pretty good web3 utilities for react application to talk with web3 compatible blockchain. `@ethersproject/providers` give use the web3 providers.

## Connecting to the wallet

The first step is connect our Counter DApp to the browser wallet. Firstly we need to create several files.

{% code title="connector.js" %}
```javascript
import { InjectedConnector } from '@web3-react/injected-connector'

export const injected = new InjectedConnector({ supportedChainIds: [1337] })
```
{% endcode %}

We make a `InjectedConnector` and specified the supported chain id to include the Clover chain id `1337`.

{% code title="hooks.js" %}
```javascript
import { useState, useEffect } from 'react'
import { useWeb3React } from '@web3-react/core'
import { injected } from './connectors'

export function useEagerConnect() {
  const { activate, active } = useWeb3React()

  const [tried, setTried] = useState(false)

  useEffect(() => {
    injected.isAuthorized().then((isAuthorized) => {
      if (isAuthorized) {
        activate(injected, undefined, true).catch(() => {
          setTried(true)
        })
      } else {
        setTried(true)
      }
    })
  }, []) // intentionally only running on mount (make sure it's only mounted once :))

  // if the connection worked, wait until we get confirmation of that to flip the flag
  useEffect(() => {
    if (!tried && active) {
      setTried(true)
    }
  }, [tried, active])

  return tried
}

export function useInactiveListener(suppress = false) {
  const { active, error, activate } = useWeb3React()

  useEffect(() => {
    const { ethereum } = window
    if (ethereum && ethereum.on && !active && !error && !suppress) {
      const handleConnect = () => {
        console.log("Handling 'connect' event")
        activate(injected)
      }
      const handleChainChanged = (chainId) => {
        console.log("Handling 'chainChanged' event with payload", chainId)
        activate(injected)
      }
      const handleAccountsChanged = (accounts) => {
        console.log("Handling 'accountsChanged' event with payload", accounts)
        if (accounts.length > 0) {
          activate(injected)
        }
      }
      const handleNetworkChanged = (networkId) => {
        console.log("Handling 'networkChanged' event with payload", networkId)
        activate(injected)
      }

      ethereum.on('connect', handleConnect)
      ethereum.on('chainChanged', handleChainChanged)
      ethereum.on('accountsChanged', handleAccountsChanged)
      ethereum.on('networkChanged', handleNetworkChanged)

      return () => {
        if (ethereum.removeListener) {
          ethereum.removeListener('connect', handleConnect)
          ethereum.removeListener('chainChanged', handleChainChanged)
          ethereum.removeListener('accountsChanged', handleAccountsChanged)
          ethereum.removeListener('networkChanged', handleNetworkChanged)
        }
      }
    }
  }, [active, error, suppress, activate])
}

```
{% endcode %}

`hooks.js` provides several hooks to help connect with the wallet.

{% code title="Spinner.js" %}
```javascript
import React from 'react'

// <!-- By Sam Herbert (@sherb), for everyone. More @ http://goo.gl/7AJzbL -->
export function Spinner(props) {
  const { color, ...rest } = props
  return (
    <svg width="38" height="38" viewBox="0 0 38 38" xmlns="http://www.w3.org/2000/svg" stroke={color} {...rest}>
      <g fill="none" fillRule="evenodd">
        <g transform="translate(1 1)" strokeWidth="2">
          <circle strokeOpacity=".5" cx="18" cy="18" r="18" />
          <path d="M36 18c0-9.94-8.06-18-18-18">
            <animateTransform
              attributeName="transform"
              type="rotate"
              from="0 18 18"
              to="360 18 18"
              dur="1s"
              repeatCount="indefinite"
            />
          </path>
        </g>
      </g>
    </svg>
  )
}
```
{% endcode %}

`Spinner.js` implements a simple spinner component which could be used as the loading status.

Update the `App.js` to set its content to:

{% code title="App.js" %}
```javascript
import { Web3ReactProvider, useWeb3React, } from '@web3-react/core'
import { Web3Provider } from '@ethersproject/providers'
import { useEagerConnect, useInactiveListener } from './hooks'

import './App.css';

function getLibrary(provider) {
  const library = new Web3Provider(provider)
  library.pollingInterval = 5000
  return library
}

function ChainId() {
  const { chainId, library } = useWeb3React()

  return (
    <div className="ChainIdWrapper">
      <span>Chain Id</span>
      <span role="img" aria-label="chain">
        ⛓
      </span>
      <span className="ChainIdText">{chainId ?? 'Not Connected'}</span>
    </div>
  )
}

function App() {
  const triedEager = useEagerConnect()

  return (
      <div className="App">
        <header className="App-header">
          <h1>Counter Example </h1>
          <ChainId/>
          <p>
            Current value: n/a
        </p>
          <button className="CounterButton">Inc Counter</button>
          <button className="CounterButton">Dec Counter</button>
        </header>
      </div>
  );
}


export default function() {
  return (
    <Web3ReactProvider getLibrary={getLibrary}>
      <App />
    </Web3ReactProvider>
  )
}
```
{% endcode %}

We added the `Web3ReactProvider` to the root of the application and include the `useEagerConnect` hooks in the `App` component. We also includes the `ChainId` component which will show the connected chain id and show `not connected` if no connection detected.&#x20;

Start the application you will see the `not connected` in the ChainId component. It's find since we haven't implement the connection logic. But you can test it by manually connect to the web app from `MetaMask`, try to figure out how to do it by yourself.

### Add the Connect Button

Now let's add a button to trigger the wallet connect dialog.&#x20;

Edit `App.js` to add some imports:

```javascript
import React from 'react'
import { Spinner } from './Spiner'
import { injected } from './connectors'
```

And create the `ConnectChain` component:

```javascript
function ConnectChain(props) {
  const context = useWeb3React()
  const { connector, library, chainId, account, activate, deactivate, active, error } = context

  const [activatingConnector, setActivatingConnector] = React.useState()
  React.useEffect(() => {
    if (activatingConnector && activatingConnector === connector) {
      setActivatingConnector(undefined)
    }
  }, [activatingConnector, connector])

  const activating = injected === activatingConnector
  const connected = injected === connector
  const disabled = !props.triedEager || !!activatingConnector || !!error

  useInactiveListener(!props.triedEager || !!activatingConnector)

  let isDisconnect = !error && chainId
  const buttonText = isDisconnect ? 'Disconnect' : (activating ? 'Connectting' : 'Connect' )

  return (
    <button
      style={{
        borderColor: activating ? 'orange' : connected ? 'green' : 'unset',
        cursor: disabled ? 'unset' : 'pointer',
        position: 'relative',
      }}
      className="ConnectButton"
      disabled={disabled}
      onClick={() => {
        if (!isDisconnect) {
          setActivatingConnector(injected)
          activate(injected)
        } else {
          deactivate()
        }
      }}
    >
      <div
        style={{
          position: 'absolute',
          top: '0',
          left: '0',
          height: '100%',
          display: 'flex',
          alignItems: 'center',
          color: 'black',
          margin: '0 0 0 1rem'
        }}
      >
        {activating && <Spinner color={'red'} style={{ height: '50%', marginLeft: '-1rem' }} />}
      </div>
      { buttonText }
    </button>
  )
}
```

and the css class for the button

{% code title="App.css" %}
```css
.ConnectButton {
  background-color: #4CAF50;
  border: none;
  color: white;
  margin-top: 15px;
  margin-bottom: 15px;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  width: 200px;
}

.ConnectButton:disabled {
  background-color: grey;
  color: black;
}
```
{% endcode %}

The `ConnectChain` component simple renders a button if it's not connected, click it will trigger the web3 connection dialog.&#x20;

Let's add the `ConnectChain` component to the `App` component, you could place it under the `<h1>` title or somewhere else as you like.

```javascript
<ConnectChain triedEager={triedEager} />
```

Reload the page, the `Connect` button will show up and you can click it to open the connect dialog.  After connected to the wallet, click the button will disconnect it.

## Read contract state

First install the `@ethersproject/contracts` package, i

```bash
$ yarn add @ethersproject/contracts
```

{% hint style="info" %}
`@ethersproject/contracts` is a sub module of the ethers project.It is creating (at run-time) an object which interacts with an on-chain contract as a native JavaScript object.
{% endhint %}

We need add extra hooks to interact with the contract:

{% code title="hooks.js" %}
```javascript
import { Contract } from '@ethersproject/contracts'

export function useBlockNumber() {
  const { library }= useWeb3React()
  const [blockNumber, setBlockNumber] = useState(-1)

  useEffect(() =>  {
    if (!library) {
      return
    }
    const t = setInterval(async () => {
      try {
        setBlockNumber(await library.getBlockNumber())
      } catch(ex) {
        console.error('failed to get block number', ex)
      }
      return () => {
        clearInterval(t)
      }
    }, 1000)

  }, [library])
  return blockNumber
}

export function useContract(contractJson) {
  const { chainId, library, account}= useWeb3React()

  if (!chainId || !contractJson.networks || !contractJson.networks[chainId]) {
    return null
  }

  const signer = library.getSigner(account).connectUnchecked()
  return new Contract(contractJson.networks[chainId].address, contractJson.abi, signer)
}

export function useContractCallData(contract, methodName, args) {
  const blockNumber = useBlockNumber()
  const [ result, setResult ] = useState(null)
  useEffect(() => {
    if (!contract || !methodName) {
      return null
    }
    async function loadData() {
      try {
        const result = await contract[methodName](...args)
        setResult(result)
      } catch (ex) {
        console.log(`failed call contract method ${methodName}: `, ex)
      }
    }
    loadData()
  }, [blockNumber])
  return result
}
```
{% endcode %}

1. We add 3 hooks, the `useBlockNumber`hook returns the latest block number from the chain (we emulate the data with 1 second refresh interval).
2. The `useContract` hook creates a smart contract instance from the json definition which was created by truffle. It will automatically detect the smart contract address on the chain.
3. The `useContractCallData` hook calls the contract method and will keep up to date with the latest block.

Now edit `App.js` and update it's content:

```javascript
import { useEagerConnect, useInactiveListener, useContract, useContractCallData } from './hooks'


function App() {
  const triedEager = useEagerConnect()
  const counter = useContract(CounterContract)
  const currentValue = useContractCallData(counter, 'current_value', [])
  const currentValueText = (currentValue === undefined || currentValue === null) ? 'N/A' : currentValue

  return (
      <div className="App">
        <header className="App-header">
          <h1>Counter Example </h1>
          <ConnectChain triedEager={triedEager} />
          <ChainId/>
          <p>
            Current value: {currentValueText}
          </p>

          <button className="CounterButton">Inc Counter</button>
          <button className="CounterButton">Dec Counter</button>
        </header>
      </div>
  );
}
```

Here we create the counter contract using the `useContract` hook. And then we use the `useContractCallData` hook to fetch the `current_value` state from the smart contract.&#x20;

In the renderer function, we set the current value text to the value on the chain.&#x20;

Save `App.js` and reload the webpage, you should see the current counter value on the page.

## Write contract state

Now we can work on the inc/dec buttons, we'll add the `onClick` handler to them and call the `inc`/`dec` method correspondingly.&#x20;

```javascript
function App() {
  const triedEager = useEagerConnect()
  const counter = useContract(CounterContract)
  const [loading, setLoading] = useState(false)
  const currentValue = useContractCallData(counter, 'current_value', [])

  const currentValueText = (currentValue === undefined || currentValue === null) ? 'N/A' : currentValue
  const callMethod = async (name) => {
    if (loading) {
      return
    }
    setLoading(true)
    try {
      await counter[name]()
    } catch(ex) {
      console.error('transaction error: ', ex)
    } finally {
      setLoading(false)
    }
  }

  return (
      <div className="App">
        <header className="App-header">
          <h1>Counter Example </h1>
          <ConnectChain triedEager={triedEager} />
          <ChainId/>
          <p>
            Current value: {currentValueText}
          </p>

          {loading && <Spinner color={'red'} style={{ height: '40px', marginLeft: '-1rem' }} />}
          <button className="CounterButton" onClick={() => callMethod('inc')}>Inc Counter</button>
          <button className="CounterButton" onClick={() => callMethod('dec')}>Dec Counter</button>
        </header>
      </div>
  );
}
```

Besides calling the `inc`/`dec`method, we also added a loading state to indicate we're waiting for some operation.

Save `App.js` and reload the webpage.

Click the Inc or Dec button, the sign transaction dialog will show up, click confirm to sign and send the transaction. The current value will be updated after a short while (\~around 10 seconds).

That's it! We've implemented read/write the smart contracts in the DApp.

{% hint style="info" %}
The source code of this chapter could be found at the revision `fc93d686aab`in the [`counter-dapp`](https://github.com/clover-network/example-counter-dapp) source repo.
{% endhint %}

## Conclusion

In this tutorial we completed an e2e DApp development process which includes smart contract development, deployment and setup a frontend application to interact with the smart contract.

There're several things to improve which you can do:

1. The smart contract should send the update when the current value changes.
2. Implement a real time `useBlockNumber` hook
3. The current value was not updated in real time, improve it
4. The loading spinner shows when the `confirm transaction` dialog open, improve it so that it will wait for the transaction completes.

As we only demo a smallest DApp development, some code/functions are not written in a performant style, you should adjust them if you want to use them in a real project.
