Nothing in the ethereum blockchain is private.
The keyword private is merely an artificial construct of the Solidity language.
Web3's `getStorageAt(...)` can be used to read anything from storage.
It can be tricky to read what you want though, since several optimization rules and techniques are used to compact the storage as much as possible.

It can't get much more complicated than what was exposed in this level.
<br>

For more, check out this excellent article by "Darius": [How to read Ethereum contract storage](https://medium.com/aigang-network/how-to-read-ethereum-contract-storage-44252c8af925).
