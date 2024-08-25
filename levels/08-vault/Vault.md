# [Vault, Level-8](https://ethernaut.openzeppelin.com/level/0xB7257D8Ba61BD1b3Fb7249DCd9330a023a5F3670) ●●○○○

![vault](https://ethernaut.openzeppelin.com/imgs/BigLevel8.svg)

Unlock the vault to pass the level!

##

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract Vault {
    bool public locked;
    bytes32 private password;

    constructor(bytes32 _password) {
        locked = true;
        password = _password;
    }

    function unlock(bytes32 _password) public {
        if (password == _password) {
            locked = false;
        }
    }
}
```
