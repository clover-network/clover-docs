# Getting a Value

Now that we have created and initialized a storage value, we are going to start to interact with it!

### [Contract Functions](https://substrate.dev/substrate-contracts-workshop/#/1/getting-a-value?id=contract-functions) <a id="contract-functions"></a>

As you can see in the contract template, all of your contract functions are part of your contract module.

```text
impl MyContract {
    // Public and Private functions can go here
}Copy to clipboardErrorCopied
```

#### [Public and Private Functions](https://substrate.dev/substrate-contracts-workshop/#/1/getting-a-value?id=public-and-private-functions) <a id="public-and-private-functions"></a>

In Rust, you can make as many implementations as you want. As a stylistic choice, we recommend breaking up your implementation definitions for your private and public functions:

```text
impl MyContract {
    /// Public function
    #[ink(message)]
    pub fn my_public_function(&self) {
        /* --snip-- */
    }

    /// Private function
    fn my_private_function(&self) {
        /* --snip-- */
    }

    /* --snip-- */
}Copy to clipboardErrorCopied
```

You can also choose to split things up however is most clear for your project.

Note that all public functions must use the `#[ink(message)]` attribute.

### [Storage Value API](https://substrate.dev/substrate-contracts-workshop/#/1/getting-a-value?id=storage-value-api) <a id="storage-value-api"></a>

Without going into so much detail, storage values are a part of the underlying ink! core layer. In the background, they use a more primitive `cell` type which holds an `Option<T>`. When we try to get the value from storage, we `unwrap` the value, which is why it panics if it is not initialized!

```text
impl<T> Value<T>
where
    T: scale::Codec,
{
    /// Returns an immutable reference to the wrapped value.
    pub fn get(&self) -> &T {
        self.cell.get().unwrap()
    }

    /// Returns a mutable reference to the wrapped value.
    pub fn get_mut(&mut self) -> &mut T {
        self.cell.get_mut().unwrap()
    }

    /// Sets the wrapped value to the given value.
    pub fn set(&mut self, val: T) {
        self.cell.set(val);
    }
}Copy to clipboardErrorCopied
```

In that same file, you can find the other APIs exposed by storage values, however these three are the most commonly used.

### [Getting a Value](https://substrate.dev/substrate-contracts-workshop/#/1/getting-a-value?id=getting-a-value-1) <a id="getting-a-value-1"></a>

We already showed you how to initialize a storage value. Getting the value is just as simple:

```text
impl MyContract {
    #[ink(message)]
    pub fn my_getter(&self) -> u32 {
        self.number
    }
}Copy to clipboardErrorCopied
```

In Rust, if the last expression in a function does not have a semicolon, then it will be the return value.

### [Your Turn!](https://substrate.dev/substrate-contracts-workshop/#/1/getting-a-value?id=your-turn) <a id="your-turn"></a>

Follow the `ACTION`s on the code template provided.

Remember to run `cargo +nightly test` to test your work.[  
](https://substrate.dev/substrate-contracts-workshop/#/1/storing-a-value)

