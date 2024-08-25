As the previous level, `delegate` mentions, the use of `delegatecall` to call libraries can be risky.
<br>
This is particularly true for contract libraries that have their own state.
<br>
This example demonstrates why the `library` keyword should be used for building libraries, as it prevents the libraries from storing and accessing state variables.
