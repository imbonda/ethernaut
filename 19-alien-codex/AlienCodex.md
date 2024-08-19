# [Alien Codex, Level-19](https://ethernaut.openzeppelin.com/level/0x0BC04aa6aaC163A6B3667636D798FA053D43BD11)

![alien-codex](https://ethernaut.openzeppelin.com/imgs/BigLevel19.svg)

You've uncovered an Alien contract. Claim ownership to complete the level.

<br>

Things that might help:
- Understanding how array storage works
- Understanding [ABI specifications](https://solidity.readthedocs.io/en/v0.4.21/abi-spec.html)
- Using a very `underhanded` approach

##

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.5.0;

import "../helpers/Ownable-05.sol";

contract AlienCodex is Ownable {
    bool public contact;
    bytes32[] public codex;

    modifier contacted() {
        assert(contact);
        _;
    }

    function makeContact() public {
        contact = true;
    }

    function record(bytes32 _content) public contacted {
        codex.push(_content);
    }

    function retract() public contacted {
        codex.length--;
    }

    function revise(uint256 i, bytes32 _content) public contacted {
        codex[i] = _content;
    }
}
```
