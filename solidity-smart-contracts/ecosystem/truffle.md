# Truffle

### Create A Project <a id="create-a-project"></a>

The first step is to create a Truffle project. We'll use the MegaCoin as an example, which creates a token that can be transferred between accounts:

* Create a new directory for your Truffle project

```text
mkdir MegaCoin
cd MegaCoin
```

* Initialize your project:

```text
truffle init
```

Once this operation is completed, you'll now have a project structure with the following items:

* contracts/: Directory for Solidity contracts
* migrations/: Directory for scriptable deployment files
* test/: Directory for test files for testing your application and contracts
* truffle-config.js: Truffle configuration file

#### Create Contract <a id="create-contract"></a>

You can write your own smart contract or download the CEP20 token smart contract template.

#### Compile Contract <a id="compile-contract"></a>

To compile a Truffle project, change to the root of the directory where the project is located and then type the following into a terminal:

```text
truffle compile
```

#### Config Truffle for CLV <a id="config-truffle-for-bsc"></a>

* Go to truffle-config.js
* Update the truffle-config with clv-network-crendentials.

```text
const HDWalletProvider = require('truffle-hdwallet-provider');
const fs = require('fs');
const mnemonic = fs.readFileSync(".secret").toString().trim();

module.exports = {
  networks: {
    development: {
      host: "127.0.0.1",     // Localhost (default: none)
      port: 8545,            // Standard CLV port (default: none)
      network_id: "*",       // Any network (default: none)
    },
    testnet: {
      provider: () => new HDWalletProvider(mnemonic, `https://rpc.clover.finance`),
      network_id: 1337,
      confirmations: 10,
      timeoutBlocks: 200,
      skipDryRun: true
    },
  },

  // Set default mocha options here, use special reporters etc.
  mocha: {
    // timeout: 100000
  },

  // Configure your compilers
  compilers: {
    solc: {
    }
  }
}
```

Notice, it requires mnemonic to be passed in for Provider, this is the seed phrase for the account you'd like to deploy from. Create a new .secret file in root directory and enter your 12 word mnemonic seed phrase to get started. To get the seedwords from metamask wallet you can go to Metamask Settings, then from the menu choose Security and Privacy where you will see a button that says reveal seed words.

### Deploying on CLV Network <a id="deploying-on-bsc-network"></a>

Run this command in root of the project directory:

```text
$ truffle migrate --network testnet
```

Contract will be deployed on Clover Chain Chapel Testnet, it look like this:

```text
1_initial_migration.js
======================

   Deploying 'Migrations'
   ----------------------
   > transaction hash:    0xaf4502198400bde2148eb4274b08d727a17080b685cd2dcd4aee13d8eb954adc
   > Blocks: 3            Seconds: 9
   > contract address:    0x81eCD10b61978D9160428943a0c0Fb31a5460466
   > block number:        3223948
   > block timestamp:     1604049862
   > account:             0x623ac9f6E62A8134bBD5Dc96D9B8b29b4B60e45F
   > balance:             6.24574114
   > gas used:            191943 (0x2edc7)
   > gas price:           20 gwei
   > value sent:          0 ETH
   > total cost:          0.00383886 ETH

   Pausing for 5 confirmations...
   ------------------------------
   > confirmation number: 2 (block: 3223952)
   > confirmation number: 3 (block: 3223953)
   > confirmation number: 4 (block: 3223954)
   > confirmation number: 6 (block: 3223956)

   > Saving migration to chain.
   > Saving artifacts
   -------------------------------------
   > Total cost:          0.00383886 ETH


Summary
=======
> Total deployments:   1
> Final cost:          0.00383886 ETH
```

> Remember your address, transaction\_hash and other details provided would differ, Above is just to provide an idea of structure.

**Congratulations!** You have successfully deployed CEP20 Smart Contract. Now you can interact with the Smart Contract.

You can check the deployment status here:[https://apps.clover.finance/\#/explorer](https://apps.clover.finance/#/explorer)

