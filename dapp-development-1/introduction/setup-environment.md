# Setup environment

## Install required tools

We're going to develop DApp using truffle and react. So you need to have at least the following tools installed.

### :tangerine: [Nodejs](https://nodejs.org)

Nodejs is the essential tool to develop both smart contract and create the front end app. Install the recent version of node would be enough, by writing this document, we're using v15.4.0.

{% hint style="info" %}
&#x20;Super-powers are granted randomly so please submit an issue if you're not happy with yours.
{% endhint %}

### :tools: [Truffle](https://www.trufflesuite.com/truffle)&#x20;

Truffle is a world class development environment, testing framework and asset pipeline for blockchains using the Ethereum Virtual Machine (EVM), aiming to make life as a developer easier.&#x20;



{% hint style="info" %}
Clover provides fully compatible with Ethereum tools and we can use existing power tools like truffle, remix to develop smart contracts on Clover.
{% endhint %}

```bash
npm install truffle -g
```

### :lemon: [Create-React-App](https://github.com/facebook/create-react-app)

{% hint style="info" %}
Create React App is an officially supported way to create single-page React applications. It offers a modern build setup with no configuration.
{% endhint %}

```bash
# No need to install create-react-app,
# just use 'npx create-react-app' to invoke it.
npx create-react-app
```

## :rocket: [Clover local node](../../quick-start/local-node/)

We assume you start your clover local node using below command, you might need to adjust some settings if you start clover use a different command arguments.

```bash
./target/release/clover --dev --alice
```
