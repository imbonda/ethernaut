```solidity
// SPDX-License-Identifier: MIT

pragma solidity >0.8.0;

interface INaughtCoin {
    function transferFrom(address from, address to, uint256 value) external returns (bool);
}

contract NaughtCoinSolution {
    function selfTransfer(address _token, address _from, uint256 _amount) external {
        INaughtCoin(_token).transferFrom(_from, address(this), _amount);
    }
}
```
