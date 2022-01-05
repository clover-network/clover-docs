# The Counter Contract

## :pencil: The Code

Smart contracts are put at the contracts folder. Create the contract folder using:

```bash
mkdir contracts
```

{% hint style="info" %}
Placing smart contracts in the contracts folder is a convention of truffle. You can specify a different directory by modifying truffle configuration. Checkout the [conracts\_directory](https://www.trufflesuite.com/docs/truffle/reference/configuration#contracts\_directory) section in truffles document.
{% endhint %}

Once created the contracts folder , create the `Counter.sol` file with contents below:

{% code title="Counter.sol" %}
```bash
pragma solidity >=0.5.0 <0.7.0;

contract Counter {
  uint32 public current_value;

  function inc() public {
    require(current_value < 10000, "Counter: max value");
    current_value = current_value + 1;
  }

  function dec() public {
    require(current_value > 0, "Counter: min value");
    current_value = current_value - 1;
  }
}
```
{% endcode %}

## :star2: Explain

The contract code is quite self explain:&#x20;

* A state variable `current_value` which is an unsigned integer.&#x20;
* inc() method, which increase the current\_value by one.&#x20;
* dec() method, which decrease the current\_value by one.
* current\_value has a bound of \[0, 10000] which was checked in inc and dec methods.

## :red\_car: Test out

Run command `truffle compile`, it will find and compiles the Counter contract. you should looks outputs like:

```bash
Compiling your contracts...
===========================
> Compiling ./contracts/Counter.sol
> Artifacts written to ~/counter-dapp/build/contracts
> Compiled successfully using:
   - solc: 0.5.2+commit.1df8f40c.Emscripten.clang
```

{% hint style="info" %}
Truffle command will download solidity compiler at the first time. There could be some messages related to the compiler setup. It's normal.
{% endhint %}

&#x20;

