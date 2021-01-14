# Storing a Value

The first thing we are going to do to the contract template is introduce some storage values.

Here is how you would store some simple values in storage:

```text
#[ink(storage)]
pub struct MyContract {
    // Store a bool
    my_bool: bool,
    // Store some number
    my_number: u32,
}
/* --snip-- */Copy to clipboardErrorCopied
```

### [Supported Types](https://substrate.dev/substrate-contracts-workshop/#/1/storing-a-value?id=supported-types) <a id="supported-types"></a>

Contract may store types that are encodable and decodable with [Parity Codec](https://github.com/paritytech/parity-codec) which includes the most common types such as `bool`, `u{8,16,32,64,128}`, `i{8,16,32,64,128}`, `String`, tuples, and arrays.

ink! provides smart contracts Substrate specific types like `AccountId`, `Balance`, and `Hash` as if they were primitive types. Also ink! provides storage types for more elaborate storage interactions through the storage module:

```text
use ink_storage::collections::{Vec, HashMap, Stash, Bitvec};Copy to clipboardErrorCopied
```

Here is an example of how you would store an `AccountId` and `Balance`:

```text
// We are importing the default ink! types
use ink_lang as ink;

#[ink::contract]
mod MyContract {

    // Our struct will use those default ink! types
    #[ink(storage)]
    pub struct MyContract {
        // Store some AccountId
        my_account: AccountId,
        // Store some Balance
        my_balance: Balance,
    }
    /* --snip-- */
}Copy to clipboardErrorCopied
```

You can find all the supported Substrate types in [`crates/storage/src/lib.rs`](https://github.com/paritytech/ink/blob/master/crates/storage/src/lib.rs).

### [Contract Deployment](https://substrate.dev/substrate-contracts-workshop/#/1/storing-a-value?id=contract-deployment) <a id="contract-deployment"></a>

Every ink! smart contract must have a constructor which is run once when a contract is created. ink! smart contracts can have multiple constructors:

```text
use ink_lang as ink;

#[ink::contract]
mod mycontract {

    #[ink(storage)]
    pub struct MyContract {
        number: u32,
    }

    impl MyContract {
        /// Constructor that initializes the `u32` value to the given `init_value`.
        #[ink(constructor)]
        pub fn new(init_value: u32) -> Self {
            Self {
                number: init_value,
            }
        }

        /// Constructor that initializes the `u32` value to the `u32` default.
        ///
        /// Constructors can delegate to other constructors.
        #[ink(constructor)]
        pub fn default() -> Self {
            Self {
                number: Default::default(),
            }
        }
    /* --snip-- */
    }
}Copy to clipboardErrorCopied
```

### [Your Turn!](https://substrate.dev/substrate-contracts-workshop/#/1/storing-a-value?id=your-turn) <a id="your-turn"></a>

Follow the `ACTION`s in the template.

Remember to run `cargo +nightly test` to test your work.[  
](https://substrate.dev/substrate-contracts-workshop/#/1/contract-template)

