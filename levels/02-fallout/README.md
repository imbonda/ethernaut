# [Fallout, Level-2](https://ethernaut.openzeppelin.com/level/0x676e57FdBbd8e5fE1A7A3f4Bb1296dAC880aa639) ●○○○○

![fallout](https://ethernaut.openzeppelin.com/imgs/BigLevel2.svg)

Claim ownership of the contract below to complete this level.

<br>

Things that might help:
- Solidity Remix IDE

##

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.6.0;

import "openzeppelin-contracts-06/math/SafeMath.sol";

contract Fallout {
    using SafeMath for uint256;

    mapping(address => uint256) allocations;
    address payable public owner;

    /* constructor */
    function Fal1out() public payable {
        owner = msg.sender;
        allocations[owner] = msg.value;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "caller is not the owner");
        _;
    }

    function allocate() public payable {
        allocations[msg.sender] = allocations[msg.sender].add(msg.value);
    }

    function sendAllocation(address payable allocator) public {
        require(allocations[allocator] > 0);
        allocator.transfer(allocations[allocator]);
    }

    function collectAllocations() public onlyOwner {
        msg.sender.transfer(address(this).balance);
    }

    function allocatorBalance(address allocator) public view returns (uint256) {
        return allocations[allocator];
    }
}
```
