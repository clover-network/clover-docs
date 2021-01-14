# Contract Template

  
Let's take a look at a high level what is available to you when developing a smart contract using ink!.

### [ink!](https://substrate.dev/substrate-contracts-workshop/#/1/contract-template?id=ink) <a id="ink"></a>

ink! is an [Embedded Domain Specific Language](https://wiki.haskell.org/Embedded_domain_specific_language) \(EDSL\) that you can use to write WebAssembly based smart contracts in the Rust programming language.

ink! is just standard Rust in a well defined "contract format" with specialized `#[ink(...)]` attribute macros. These attribute macros tell ink! what the different parts of your Rust smart contract represent, and ultimately allow ink! to do all the magic needed to create Substrate compatible Wasm bytecode!

### [Your Turn!](https://substrate.dev/substrate-contracts-workshop/#/1/contract-template?id=your-turn) <a id="your-turn"></a>

Let's start a new project for the Incrementer contract that you will build in this chapter.

Change into your working directory and run:

```text
cargo contract new incrementerCopy to clipboardErrorCopied
```

Just like in previous examples, this will create a new project folder named `incrementer` which we will use for the rest of this chapter.

```text
cd incrementer/Copy to clipboardErrorCopied
```

In the `lib.rs` file, replace the "Flipper" contract source code with the template code provided here.

Quickly check that it compiles and the trivial test passes with:

```text
cargo +nightly testCopy to clipboardErrorCopied
```

Also check that you can build the Wasm file by running:

```text
cargo +nightly contract buildCopy to clipboardErrorCopied
```

If everything looks good, then we are ready to start programming![  
](https://substrate.dev/substrate-contracts-workshop/#/1/introduction)

