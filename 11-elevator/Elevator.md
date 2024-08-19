# [Elevator, Level-11](https://ethernaut.openzeppelin.com/level/0x6DcE47e94Fa22F8E2d8A7FDf538602B1F86aBFd2)

![elevator](https://ethernaut.openzeppelin.com/imgs/BigLevel11.svg)

This elevator won't let you reach the top of your building. Right?

<br>

Things that might help:
- Sometimes solidity is not good at keeping promises.
- This `Elevator` expects to be used from a `Building`.

##

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface Building {
    function isLastFloor(uint256) external returns (bool);
}

contract Elevator {
    bool public top;
    uint256 public floor;

    function goTo(uint256 _floor) public {
        Building building = Building(msg.sender);

        if (!building.isLastFloor(_floor)) {
            floor = _floor;
            top = building.isLastFloor(floor);
        }
    }
}
```
