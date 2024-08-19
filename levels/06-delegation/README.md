# [Delegation, Level-6](https://ethernaut.openzeppelin.com/level/0x73379d8B82Fda494ee59555f333DF7D44483fD58) ●●○○○

![delegation](https://ethernaut.openzeppelin.com/imgs/BigLevel6.svg)

The goal of this level is for you to claim ownership of the instance you are given.

<br>

Things that might help:
- Look into Solidity's documentation on the `delegatecall` low level function, how it works,
  how it can be used to delegate operations to on-chain libraries, and what implications it has on execution scope.
- Fallback methods
- Method ids

##

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract Delegate {
    address public owner;

    constructor(address _owner) {
        owner = _owner;
    }

    function pwn() public {
        owner = msg.sender;
    }
}

contract Delegation {
    address public owner;
    Delegate delegate;

    constructor(address _delegateAddress) {
        delegate = Delegate(_delegateAddress);
        owner = msg.sender;
    }

    fallback() external {
        (bool result,) = address(delegate).delegatecall(msg.data);
        if (result) {
            this;
        }
    }
}
```
