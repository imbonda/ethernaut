# [Telephone, Level-4](https://ethernaut.openzeppelin.com/level/0x2C2307bb8824a0AbBf2CC7D76d8e63374D2f8446) ●○○○○

![telephone](https://ethernaut.openzeppelin.com/imgs/BigLevel4.svg)

Claim ownership of the contract below to complete this level.

<br>

Things that might help:
- See the ["?"](https://ethernaut.openzeppelin.com/help) page above, section "Beyond the console"

##

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract Telephone {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    function changeOwner(address _owner) public {
        if (tx.origin != msg.sender) {
            owner = _owner;
        }
    }
}
```
