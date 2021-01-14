# Setup

To follow this tutorial, you will need to set up some stuff on your computer.

### [Substrate Prerequisites](https://substrate.dev/substrate-contracts-workshop/#/0/setup?id=substrate-prerequisites) <a id="substrate-prerequisites"></a>

Follow the [official installation steps](https://substrate.dev/docs/en/knowledgebase/getting-started/) from the Substrate Developer Hub Knowledge Base.

```text
rustup component add rust-src --toolchain nightly
rustup target add wasm32-unknown-unknown --toolchain stableCopy to clipboardErrorCopied
```

### [Installing The Canvas Node](https://substrate.dev/substrate-contracts-workshop/#/0/setup?id=installing-the-canvas-node) <a id="installing-the-canvas-node"></a>

We need to use a Canvas node with the built-in Contracts module. For this workshop we'll use the pre-designed Substrate node client.

```text
cargo install canvas-node --git https://github.com/paritytech/canvas-node.git --tag v0.1.4 --force --lockedCopy to clipboardErrorCopied
```

### [ink! CLI](https://substrate.dev/substrate-contracts-workshop/#/0/setup?id=ink-cli) <a id="ink-cli"></a>

The final tool we will be installing is the ink! command line utility which will make setting up Substrate smart contract projects easier.

You can install the utility using Cargo with:

```text
cargo install cargo-contract --vers 0.8.0 --force --lockedCopy to clipboardErrorCopied
```

You can then use `cargo contract --help` to start exploring the commands made available to you.

