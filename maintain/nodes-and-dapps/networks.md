# Networks

Clover is built on top of Substrate, a modular framework for blockchains. One feature of Substrate is to allow for connection to different networks using a single executable and configuring it with a start-up flag. Here are some of the networks associated with Clover or Substrate that you may want to connect to and join.

## Clover Networks

To connect to a Clover network please follow the [instructions](https://app.gitbook.com/@clover-network/s/portal/maintain/nodes-and-dapps/set-up-a-full-node/@drafts) for installing the Clover executable.

### Clover Mainnet

The Clover mainnet is not released yet. Instructions will be here once the network is available.

### Clover Test Network

Westend is the latest test network for Clover. The tokens on this network are called _Westies_ and they purposefully hold no economic value.

Currently Westend is built from the tip of master and requires a commandline flag to access.

Run the Clover binary with a `chain` option:

```text
clover
```

and you will connect and start syncing to Westend.

#### Clover Faucet

Follow the instruction here for instructions on acquiring Westies.

Check your node is connected by viewing it on [Telemetry.](https://apps.clover.finance/#/explorer)

## Substrate Networks

To connect to a Substrate public network first follow the [instructions](https://substrate.dev/docs/en/knowledgebase/getting-started) for installing the Substrate executable.

### Flaming Fir

Flaming Fir is the public Substrate test network. It contains some pallets that will not be included in the Clover runtime.

Flaming Fir is built from the tip of master and is the default option when running the Substrate executable.

Run Substrate without a flag or explicitly state `fir`:

```text
substrate 
```

and you will connect and start syncing Flaming Fir.

## Telemetry Dashboard

If you connect to the public networks, the default configuration for your node will connect it to the public [Telemetry](https://telemetry.polkadot.io/) service.

You can verify that your node is connected by navigating to the correct network on the dashboard and finding the name of your node.

There is a built-in search function for the nodes page. Simply start typing keystrokes in the main window to make it available.

