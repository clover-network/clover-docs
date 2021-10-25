# Running a validator on Clover Network

This guide will instruct you how to set up a Clover validator node on Clover networks (Testnet/Sakura/Mainnet).&#x20;

## :point\_right: Must Read Before Start....

Running a validator is a serious thing, you have a lot responsibility for the staked tokens of you and nominators.  You take the risk of losing your staked tokens as a slash might happen if your validator node is not properly configured. Please make sure you or your team have the necessary knowledge to run a validator node.

[Polkadot Wiki ](https://wiki.polkadot.network/docs/en/maintain-guides-how-to-validate-polkadot)has an awesome introduction of running a validator node on the [Polkadot network](https://polkadot.network).  As a member of the Polkadot ecosystem, Clover follows the similar process to run and setup a validator node. We may skip some basics steps in this tutorial.

## :tools: Hardware Requirements

* **CPU** - Recent released high end cpu, e.g. Intel 10700/Amd 5800X.
* **Memory** - 32GB for Testnet, 64GB for Sakura and Mainnet.
* **Storage** - 300GB SSD, Storage usage could increase by time, you might need to increase the capacity as the chain data grows..
* **OS**: Linux, Debian/Ubuntu LTS distributions are recommended.

## :wrench: Prepare Environment

We'll use [docker](https://docs.docker.com/engine/) and [docker-compose](https://docs.docker.com/compose/) to run the validator in this guide. You need to install docker and docker-compose firstly.  Please follow the installation guide in the docs.

* [Docker Install Document](https://docs.docker.com/engine/install/)
* [Docker-Compose Install Document](https://docs.docker.com/compose/install/)

{% hint style="info" %}
We're using docker to simplify the setup process. You can use the tools which you're familiar with.
{% endhint %}

### :satellite\_orbital: Firewall Setup

Below ports need to be exposed:

* **30333** - The p2p port of the chain

### :file\_folder: Create Directories

Create the config and data directories using below command:

```bash
sudo mkdir -p /opt/data/
sudo mkdir -p /opt/compose/
# secure the data access
sudo chmod 0700 /opt/data
sudo chmod 0700 /opt/compose 
```

## :gear: Setup Clover Validator Node

Currently we only have **Clover Testnet**(iris) and **Clover Mainnet**(ivy) launched. Clover Testnet opens for validators to join.  _Clover Mainnet operates in the POA mode and maintained by 6 nodes  belongs to Clover foundation_.&#x20;

Validator Configuration for Clover Mainnet will be updated later once it's ready for staking and validators can join.

### :pencil: Create the Compose configure file

Create `/opt/compose/docker-compose.yaml` and set the content as below:

```yaml
version: "3.8"
services:
  clover-validator:
    image: "cloverio/clover-iris:0.1.14"
    restart: always
    command:
      - /opt/clover/bin/clover
      - --chain 
      - /opt/specs/clover-preview-iris.json  
      - --base-path 
      - /opt/chaindata
      - --validator
      - --name 
      - 🍀clover-validatornode
      - --port 
      - "30333"
      - --ws-port 
      - "9944"
      - --rpc-port 
      - "9933"
      - --rpc-cors=all 
    ports:
      - "30333:30333"
      - "9933:9933"
      - "9944:9944"
    volumes:
      - /opt/data/clover:/opt/chaindata
```

{% hint style="info" %}
You can edit the `docker-compose.yaml `and include your customizations by updating below arguments:

* image: the docker image used to launch the node, for Clover Testnet, use `cloverio/clover-iris:0.1.14.`For a full list of clover networks please check out the [Clover Network List](../quick-start/clover-network-list.md) page.
* \--_name_:  The node name of your validator, the name could be found in the telemetry node list.
* _--unsafe-rpc-external:_  You might need this flag to call the `author_rotateKeys` api, make sure to remove this flag later on for better security.
{% endhint %}

## :rocket: Bring up the validator node

Use below command to bring up the validator node:

```bash
cd /opt/compose # goto the compose file directory
docker-compose up # bring up the validator node in the foreground
## check whether the node starts up normally
## Ctrl-C stop the node
docker-compose up -d # start the validator node in the daemon mode.
```

You need to check the node logs using `docker-compose logs ` command. Wait until the node is synced and the block numbers syncs with the latest number on the chain.

## :chart: Bond CLV

Checkout[ Polkadot Bond ](https://wiki.polkadot.network/docs/en/maintain-guides-how-to-validate-polkadot#bond-dot)documentation.

## :key2: Set the session keys

Checkout [Polkadot Session Keys](https://wiki.polkadot.network/docs/en/maintain-guides-how-to-validate-polkadot#option-2-cli) documentation.

## :stars: Validate

Checkout [Polkadot Validate](https://wiki.polkadot.network/docs/en/maintain-guides-how-to-validate-polkadot#validate) documentation.

## :trident: Links

* [Clover Testnet Apps](https://apps.clover.finance)
* [Clover Mainnet Apps](https://apps-ivy.clover.finance)
