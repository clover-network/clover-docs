# Setup truffle

## 📝 Truffle config

create and edit `truffle-config.js`:

{% code title="truffle-config.js" %}
```javascript
module.exports = {
  contracts_build_directory: "./src/contract_build",
  compilers: {
    solc: {
      version: '0.5.2'
    }
  },
};
```
{% endcode %}

We specified to use `solc` version `0.5.2` as the contract compiler and we set the contract build directory to `./src/contract_build` because the default build directory is `./build` which conflicts with react application build folder. Another reason is later on we'll use react to load the `Counter.json` from the build directory which must be located inside the `src` directory.

## 🚗 Test out

You can check if it's setup correctly by running `truffle compile`

```bash
truffle compile
```

The command will success and print something like:

```bash
Compiling your contracts...
===========================
> Everything is up to date, there is nothing to compile.
```

{% hint style="info" %}
truffle compile will check the contracts which needs compile to evm byte code. Truffle thinks everything is update to date since we're not having any smart contract in this project.
{% endhint %}

