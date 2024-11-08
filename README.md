## About

1. Create a basic AA on Ethereum
2. Create a basic AA on zksync
3. Deploy and send a userOp / tx through them
   1. No sending on ETH
   2. Sending on zksync

Using eth-infinitism/account-abstraction for the implementation.

struct PackedUserOperation {
address sender; --- our minimal account
uint256 nonce; --- nonce
bytes initCode; --- ignore (constructor basically)
bytes callData; --- what the Tx is supposed to do, eg: mint, transfer etc
bytes32 accountGasLimits;
uint256 preVerificationGas;
bytes32 gasFees;
bytes paymasterAndData; --- if we setup a paymaster we need this
bytes signature; --- valid signature, used to verify owner address
}

1.  ## Purpose of validateUserOp:

    The purpose of this function is to validate user operations by ensuring that the signature is valid. It also handles missing account funds.

2.  ## \_validateSignature:

    It verifies the signer's address by first converting the `userOpHash` into a signed message hash. It then recovers the signer's address using ECDSA.recover with the signed message hash and the signature from userOp. Finally, it compares the recovered address to the owner's address to determine if the signature is valid.

3.  ## Ownable:

    We need OpenZeppelin's Ownable contract to manage ownership of the contract, ensuring that only the owner can validate signatures.

4.  ## \_payPreFund:
    This function handles the payment of missing account funds owed to the EntryPoint. It checks if there are any missing funds and, if so, pays what is owed.
