## Bug Description

Calling `CryptoVault.sweepToken(legacyToken)` would result in the following code flow:
- `delegate.delegateTransfer(to, value, msg.sender)`, where `delegate == underlying`
- passing the `onlyDelegateFrom` modifier checks
- `_transfer(origSender, to, value)` resulting in draining all `DET` tokens from `CryptoVault` balance

## Forta Detection Bot

To prevent this bug we register a forta detection bot that would raise an alert whenever the method is executed, thereby causing the transaction to `revert`.

```js
await web3.eth.call({
    to: await contract.address,
    data: web3.utils.sha3('forta()').substring(0, 10),
})
// '0x000000000000000000000000237e86b526d3ef0cf5e91ba9802622616bd980ec'

// After deploying "DoubleEntryPointSolution" to address "688BCdc4ad805083D670DD790C9814F020B95845",
// register it as a forta detection bot:
await sendTransaction({
    from: player,
    to: await '0x237e86b526d3ef0cf5e91ba9802622616bd980ec',
    data: web3.utils.sha3('setDetectionBot(address)').substring(0, 10) + '688BCdc4ad805083D670DD790C9814F020B95845'.padStart(64, '0'),
})
```

##

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface IDetectionBot {
    function handleTransaction(address user, bytes calldata msgData) external;
}

interface IForta {
    function setDetectionBot(address detectionBotAddress) external;
    function notify(address user, bytes calldata msgData) external;
    function raiseAlert(address user) external;
}

contract DoubleEntryPointSolution is IDetectionBot {
    IForta private forta;

    constructor(address _forta) {
        forta = IForta(_forta);
    }

    function handleTransaction(address user, bytes calldata _msgData) external {
        forta.raiseAlert(user);
    }
}
```
