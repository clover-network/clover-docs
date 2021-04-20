# Run a Clover node

If you're building dapps or products on a Substrate-based chain like Clover or a custom Substrate implementation, you probably want the ability to run a node-as-a-back-end. After all, it's always better to rely on your own infrastructure than on a third-party-hosted one in this brave new decentralized world.

This guide will show you how to connect to Clover network, but the same process applies to any other [Substrate](https://substrate.dev/docs/en/)-based chain. First, let's clarify the term _full node_.

### Using Docker and Docker Compose

Make sure you have docker and docker-compose installed using below command:

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```

To run a clover full node, create a `docker-compose.yaml` file with below content:

```yaml
version: "3.8"
services:
  clover-node:
    image: "cloverio/clover-iris:0.1.14"
    restart: always
    environment:
        ARGS: "--base-path /opt/chaindata --chain /opt/specs/clover-preview-iris.json --port 30333 --ws-port 9944 --rpc-port 9933 --name "clover-node" --rpc-cors=all --validator --unsafe-ws-external --unsafe-rpc-external --rpc-methods=Unsafe"
    ports:
      - "9933:9933"
      - "9944:9944"
      - "30333:30333"
      - "9615:9615"
    volumes:
      - /opt/data/dev:/opt/chaindata
```

You're free to edit the name of this node in the ARGS field. 

Now launch the node with below command:

```bash
docker-compose up -d 
```

You should get clover node runs and syncing data from the test net now. You can monitor the logs with:

```bash
docker-compose logs -f --tail 10 clover-node
```

## Get Substrate

Follow instructions as outlined [here](https://substrate.dev/docs/en/knowledgebase/getting-started) - note that Windows users will have their work cut out for them. It's better to use a virtual machine instead.

Test if the installation was successful by running `cargo --version`.

```text
λ cargo --version
cargo 1.41.0 (626f0f40e 2019-12-03)
```

## Clone and Build

The [clover-network/clover](https://github.com/clover-network/clover) repo's master branch contains the latest Clover code.

```text
git clone https://github.com/clover-network/clover
cd clover
./scripts/init.sh
cargo build --release
```

Alternatively, if you wish to use a specific release, you can check out a specific tag \(`v0.8.3` in the example below\):

```text
git clone https://github.com/clover-network/clover
cd clover
git checkout tags/v0.8.3
./scripts/init.sh
cargo build --release
```

## Run

The built binary will be in the `target/release` folder, called clover.

```text
./target/release/clover --name "My node's name" --chain specs/clover-cc1-raw.json
```

Use the `--help` flag to find out which flags you can use when running the node. For example, if [connecting to your node remotely](https://app.gitbook.com/@clover-network/s/portal/maintain/nodes-and-dapps/set-up-secure-websocket-for-remote-connections/@drafts), you'll probably want to use `--ws-external` and `--rpc-cors all`.

The syncing process will take a while depending on your bandwidth, processing power, disk speed and RAM. On a $10 DigitalOcean droplet, the process can complete in some 36 hours.

Congratulations, you're now syncing with Clover. Keep in mind that the process is identical when using any other Substrate chain.

## Running an Archive Node

When running as a simple sync node \(above\), only the state of the past 256 blocks will be kept. When validating, it defaults to archive mode. To keep the full state use the `--pruning` flag:

```text
./target/release/clover --name "My node's name" --pruning archive
```

It is possible to almost quadruple synchronization speed by using an additional flag: `--wasm-execution Compiled`. Note that this uses much more CPU and RAM, so it should be turned off after the node is in sync.

## Using Docker

Finally, you can use Docker to run your node in a container. Doing this is a bit more advanced so it's best left up to those that either already have familiarity with docker, or have completed the other set-up instructions in this guide. If you would like to connect to your node's WebSockets ensure that you run you node with the `--rpc-external` and `--ws-external` commands.

```text
docker run -p 9944:9944 clover-network/clover-iris:0.1.14 --name "calling_home_from_a_docker_container" --rpc-external --ws-external
```

