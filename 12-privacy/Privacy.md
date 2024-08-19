# [Privacy, Level-12](https://ethernaut.openzeppelin.com/level/0x131c3249e115491E83De375171767Af07906eA36)

![privacy](https://ethernaut.openzeppelin.com/imgs/BigLevel12.svg)

The creator of this contract was careful enough to protect the sensitive areas of its storage.

Unlock this contract to beat the level.

<br>

Things that might help:
- Understanding how storage works
- Understanding how parameter parsing works
- Understanding how casting works

<br>

Tips:
- Remember that metamask is just a commodity.
Use another tool if it is presenting problems.
Advanced gameplay could involve using remix, or your own web3 provider.

##

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract Privacy {
    bool public locked = true;
    uint256 public ID = block.timestamp;
    uint8 private flattening = 10;
    uint8 private denomination = 255;
    uint16 private awkwardness = uint16(block.timestamp);
    bytes32[3] private data;

    constructor(bytes32[3] memory _data) {
        data = _data;
    }

    function unlock(bytes16 _key) public {
        require(_key == bytes16(data[2]));
        locked = false;
    }

    /*
    A bunch of super advanced solidity algorithms...

      ,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`
      .,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,
      *.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^         ,---/V\
      `*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.    ~|__(o.o)
      ^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'  UU  UU
    */
}
```
