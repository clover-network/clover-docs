# Creating an ink! Project

We are going to use the ink! CLI to generate the files we need for a Substrate smart contract project.

Make sure you are in your working directory, and then run:

```text
cargo contract new flipperCopy to clipboardErrorCopied
```

This command will create a new project folder named `flipper` which we will explore:

```text
cd flipper/Copy to clipboardErrorCopied
```

**ink! Contract Project**

```text
flipper
|
+-- lib.rs                <-- Contract Source Code
|
+-- Cargo.toml            <-- Rust Dependencies and ink! Configuration
|
+-- .gitignoreCopy to clipboardErrorCopied
```

### [Contract Source Code](https://substrate.dev/substrate-contracts-workshop/#/0/creating-an-ink-project?id=contract-source-code) <a id="contract-source-code"></a>

The ink CLI automatically generates the source code for the "Flipper" contract, which is about the simplest "smart" contract you can build. You can take a sneak peak as to what will come by looking at the source code here:

[Flipper Example Source Code](https://github.com/paritytech/ink/blob/v3.0.0-rc1/examples/flipper/lib.rs)

The Flipper contract is nothing more than a `bool` which gets flipped from true to false through the `flip()` function. We won't go so deep into the details of this source code because we will be walking you through the steps to build a more advanced contract!

### [Testing Your Contract](https://substrate.dev/substrate-contracts-workshop/#/0/creating-an-ink-project?id=testing-your-contract) <a id="testing-your-contract"></a>

You will see at the bottom of the source code there is a simple test which verifies the functionality of the contract. We can quickly test that this code is functioning as expected using the **off-chain test environment** that ink! provides.

In your project folder run:

```text
cargo +nightly testCopy to clipboardErrorCopied
```

To which you should see a successful test completion:

```text
$ cargo +nightly test
    running 2 tests
    test flipper::tests::it_works ... ok
    test flipper::tests::default_works ... ok

    test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 0 filtered outCopy to clipboardErrorCopied
```

Now that we are feeling confident things are working, we can actually compile this contract to Wasm.[  
](https://substrate.dev/substrate-contracts-workshop/#/0/setup)
