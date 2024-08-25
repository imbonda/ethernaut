# [Coin Flip, Level-3](https://ethernaut.openzeppelin.com/level/0xA62fE5344FE62AdC1F356447B669E9E6D10abaaF) ●●○○○

![coin-flip](https://ethernaut.openzeppelin.com/imgs/BigLevel3.svg)

This is a coin flipping game where you need to build up your winning streak by guessing the outcome of a coin flip.
To complete this level you'll need to use your psychic abilities to guess the correct outcome 10 times in a row.

<br>

Things that might help:
- See the ["?"](https://ethernaut.openzeppelin.com/help) page above in the top right corner menu, section "Beyond the console"

##

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract CoinFlip {
    uint256 public consecutiveWins;
    uint256 lastHash;
    uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;

    constructor() {
        consecutiveWins = 0;
    }

    function flip(bool _guess) public returns (bool) {
        uint256 blockValue = uint256(blockhash(block.number - 1));

        if (lastHash == blockValue) {
            revert();
        }

        lastHash = blockValue;
        uint256 coinFlip = blockValue / FACTOR;
        bool side = coinFlip == 1 ? true : false;

        if (side == _guess) {
            consecutiveWins++;
            return true;
        } else {
            consecutiveWins = 0;
            return false;
        }
    }
}
```
