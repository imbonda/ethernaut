It's important to remember that marking a variable as private only prevents other contracts from accessing it.
State variables marked as private and local variables are still publicly accessible.

To ensure that data is private, it needs to be encrypted before being put onto the blockchain.
In this scenario, the decryption key should never be sent on-chain, as it will then be visible to anyone who looks for it.
[zk-SNARKs](https://blog.ethereum.org/2016/12/05/zksnarks-in-a-nutshell/) provide a way to determine whether someone possesses a secret parameter, without ever having to reveal the parameter.
