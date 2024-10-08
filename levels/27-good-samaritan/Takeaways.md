Congratulations!

Custom errors in Solidity are identified by their 4-byte ‘selector’, the same as a function call.
<br>
They are bubbled up through the call chain until they are caught by a catch statement in a try-catch block, as seen in the GoodSamaritan's `requestDonation()` function.
<br>
For these reasons, it is not safe to assume that the error was thrown by the immediate target of the contract call (i.e., Wallet in this case).
<br>
Any other contract further down in the call chain can declare the same error and throw it at an unexpected location, such as in the `notify(uint256 amount)` function in your attacker contract.
