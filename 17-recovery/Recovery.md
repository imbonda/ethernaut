# [Recovery, Level-17](https://ethernaut.openzeppelin.com/level/0xAF98ab8F2e2B24F42C661ed023237f5B7acAB048)

![recovery](https://ethernaut.openzeppelin.com/imgs/BigLevel17.svg)

A contract creator has built a very simple token factory contract.
<br>
Anyone can create new tokens with ease.
<br>
After deploying the first token contract, the creator sent `0.001` ether to obtain more tokens. They have since lost the contract address.
<br>
This level will be completed if you can recover (or remove) the 0.001 ether from the lost contract address.

##

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract Recovery {
    //generate tokens
    function generateToken(string memory _name, uint256 _initialSupply) public {
        new SimpleToken(_name, msg.sender, _initialSupply);
    }
}

contract SimpleToken {
    string public name;
    mapping(address => uint256) public balances;

    // constructor
    constructor(string memory _name, address _creator, uint256 _initialSupply) {
        name = _name;
        balances[_creator] = _initialSupply;
    }

    // collect ether in return for tokens
    receive() external payable {
        balances[msg.sender] = msg.value * 10;
    }

    // allow transfers of tokens
    function transfer(address _to, uint256 _amount) public {
        require(balances[msg.sender] >= _amount);
        balances[msg.sender] = balances[msg.sender] - _amount;
        balances[_to] = _amount;
    }

    // clean up after ourselves
    function destroy(address payable _to) public {
        selfdestruct(_to);
    }
}
```
