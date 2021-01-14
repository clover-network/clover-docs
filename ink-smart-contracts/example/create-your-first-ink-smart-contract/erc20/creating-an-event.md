# Creating an Event

Recall that contract calls cannot directly return a value to the outside world when submitting a transaction. However, often we will want to indicate to the outside world that something has taken place \(e.g. a transaction has occurred or a certain state has been reached\). We can alert others that this has occurred using an `event`.

### [Declaring Events](https://substrate.dev/substrate-contracts-workshop/#/2/creating-an-event?id=declaring-events) <a id="declaring-events"></a>

An event can communicate an arbitrary amount of data, defined in a similar manner as a `struct`. Events should be declared using the `#[ink(event)]` attribute.

For example,

```text
#[ink(event)]
pub struct Transfer {
    #[ink(topic)]
    from: Option<AccountId>,
    #[ink(topic)]
    to: Option<AccountId>,
    value: Balance,
}Copy to clipboardErrorCopied
```

This `Transfer` event will contain three pieces of data - a value of type `Balance` and two Option-wrapped `AccountId` variables indicating the `from` and `to` accounts. For faster access to the event data they can have _indexed fields_. We can do this by using the `#[ink(topic)]` attribute tag on that field.

One way of retrieving data from an Option variable is using the `.unwrap_or()` function. You may recall using this in the `my_value_or_zero()` and `balance_of_or_zero()` functions in this project and the Incrementer project.

### [Emitting Events](https://substrate.dev/substrate-contracts-workshop/#/2/creating-an-event?id=emitting-events) <a id="emitting-events"></a>

Now that we have defined what data will be contained within the event and how to declare it, it's time to actually emit some events. We do this by calling `self.env().emit_event` and include an event as the sole argument to the method call.

Remember that since the `from` and `to` fields are Option, we can't just set them to particular values. Let's assume we want to set an value of 100 for the initial deployer. This value does not come from any other account, and so the `from` value should be `None`.

```text
self.env()
    .emit_event(
        Transfer {
            from: None,
            to: Some(self.env().caller()),
            value: 100,
        });Copy to clipboardErrorCopied
```

> Note: `value` does not need a `Some()`, as the value is not stored in an `Option`.

We want to emit a Transfer event every time that a transfer takes place. In the ERC-20 template that we have been working on, this occurs in two places: first, during the `new` call, and second, every time that `transfer_from_to` is called.

### [Your Turn!](https://substrate.dev/substrate-contracts-workshop/#/2/creating-an-event?id=your-turn) <a id="your-turn"></a>

Follow the ACTIONs in the template code to emit a `Transfer` event every time a token transfer occurs.

Remember to run `cargo +nightly test` to test your work.[  
](https://substrate.dev/substrate-contracts-workshop/#/2/transferring-tokens)

