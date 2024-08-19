### References
- [EVM opcode docs](https://www.ethervm.io/)
- [EVM opcode playground](https://www.evm.codes/playground)
- [EVM bytecode blog-post](https://medium.com/coinmonks/learn-evm-in-depth-2-executing-the-bytecode-step-by-step-in-the-deployment-of-a-contract-270a6335df75)

## 1. Building the bytecode

```assembly
// Contract creation bytecode
// 0x600a600c600039600a6000f3
[00] PUSH1 0x0a                          // The length of the deployed bytecode: 10 bytes    (2 bytes)
[02] PUSH1 0x0c                          // The offset, start of the deployed bytecode       (2 bytes)
[04] PUSH1 0x00                          // The offset in memory to store the code           (2 bytes)
[06] CODECOPY                            // Copy the bytecode to memory. Used by RETURN      (1 bytes)       memory[destOffset:destOffset+length] = address(this).code[offset:offset+length]
[07] PUSH1 0x0a                          // The length of returned bytecode: 10 bytes        (2 bytes)
[09] PUSH1 0x00                          // The offset in memory of the stored code          (2 bytes)
[0b] RETURN                              // Return the bytecode to deploy                    (1 bytes)       return memory[offset:offset+length]

// Deployed bytecode    (total bytes: 2+2+1+2+2+1 = 10)
// 0x602a60005260206000f3
[0c] PUSH1 0x2a                          // The value 42                                     (2 bytes)       currently at byte 12=0x0c)
[0e] PUSH1 0x00                          // The offset in memory where to start storing      (2 bytes)
[10] MSTORE                              // Store the value 42 in memory. Used by RETURN     (1 bytes)       memory[offset:offset+32] = value
[11] PUSH1 0x20                          // The length of returned data: 32 bytes            (2 bytes)
[13] PUSH1 0x00                          // The offset in memory of the stored data          (2 bytes)
[15] RETURN                              // Return the value 42 to the caller contract       (1 bytes)       return memory[offset:offset+length]
```

## 2. Translating to hex

The bytecode to use to create our contract:
```
0x600a600c600039600a6000f3602a60005260206000f3
```

## 3. Creating the contract

Sending raw tx:

```js
await sendTransaction({ from: player, data: '0x600a600c600039600a6000f3602a60005260206000f3' })
```

Alternatively we can use the following helper contract:

```solidity
contract DeployBytecode {
    function deployFromBytecode(bytes memory bytecode) public returns (address) {
        address child;
        assembly{
            mstore(0x0, bytecode)
            child := create(0, 0xa0, calldatasize())
        }
        return child;
   }
}
```
