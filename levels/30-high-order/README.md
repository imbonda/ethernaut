# [HighOrder, Level-30](https://ethernaut.openzeppelin.com/level/0xd459773f02e53F6e91b0f766e42E495aEf26088F) ●●●●○

![high-order](https://ethernaut.openzeppelin.com/imgs/BigLevel30.svg)

Imagine a world where the rules are meant to be broken, and only the cunning and the bold can rise to power.
<br>
Welcome to the Higher Order, a group shrouded in mystery, where a treasure awaits and a commander rules supreme.

Your objective is to become the Commander of the Higher Order! Good luck!

<br>

Things that might help:
- Sometimes, `calldata` cannot be trusted.
- Compilers are constantly evolving into better spaceships.

##

```solidity
// SPDX-License-Identifier: MIT

pragma solidity 0.6.12;

contract HigherOrder {
    address public commander;

    uint256 public treasury;

    function registerTreasury(uint8) public {
        assembly {
            sstore(treasury_slot, calldataload(4))
        }
    }

    function claimLeadership() public {
        if (treasury > 255) commander = msg.sender;
        else revert("Only members of the Higher Order can become Commander");
    }
}
```
