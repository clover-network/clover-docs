---
description: Using Web3.js to interact with Clover
---

# Web3.js

### Introduction <a id="introduction"></a>

This guide walks through the process of using [web3.js](https://github.com/ethereum/web3.js/) to manually sign and send a transaction to a Clover standalone node. For this example, we will use Node.js and straightforward JavaScript code.

The guide assumes that you have a local Clover node running in `--dev` mode. You can find instructions to set up a local Clover node [here](https://clover-network.gitbook.io/portal/quick-start/local-node/setting-up-a-node).

### Checking Prerequisites <a id="checking-prerequisites"></a>

#### 1. Start Clover node

Start your standalone Clover node, which can successfully produce blocks in your local environment.

#### 2. Install Node.js and NPM

To install Node.js \(we'll go for v12.x, you can choose other versions, such as the latest version v15.x\)  and NPM package manager. You can do this by running the following commands in your terminal:

For Mac users:

```text
brew update
brew install node
```

For Debian users:

```text
curl -sL https://deb.nodesource.com/setup_12.x | sudo bash -
sudo apt-get install -y nodejs
```

For Centos/Redhat users:

```text
curl -sL https://deb.nodesource.com/setup_12.x | sudo bash -
sudo yum install nodejs
```

We can verify that everything installed correctly by querying the version for each package:

```text
node -v
npm -v
```

#### 3. Create your Node.js project

You can create a repository on your local environment by running:

```text
mkdir clover-web3 && cd clover-web3/
npm init --yes
npm install web3 --save
```

