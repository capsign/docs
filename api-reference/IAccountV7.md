# IAccountV7
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/interfaces/IAccountV7.sol)

ERC-4337 v0.7 IAccount interface with PackedUserOperation

*This interface ensures we use the correct function signature for validateUserOp*


## Functions
### validateUserOp

Validate user's signature and nonce
the entryPoint will make the call to the recipient only if this validation call returns successfully.
signature failure should be reported by returning SIG_VALIDATION_FAILED (1).
This allows making a "simulation call" without a valid signature
Other failures (e.g. nonce mismatch, or invalid signature format) should still revert to signal failure.

*Must validate caller is the entryPoint.
Must validate the signature and nonce*


```solidity
function validateUserOp(PackedUserOperation calldata userOp, bytes32 userOpHash, uint256 missingAccountFunds)
    external
    returns (uint256 validationData);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`userOp`|`PackedUserOperation`|             - The operation that is about to be executed.|
|`userOpHash`|`bytes32`|         - Hash of the user's request data. can be used as the basis for signature.|
|`missingAccountFunds`|`uint256`|- Missing funds on the account's deposit in the entrypoint. This is the minimum amount to transfer to the sender(entryPoint) to be able to make the call. The excess is left as a deposit in the entrypoint for future calls. Can be withdrawn anytime using "entryPoint.withdrawTo()". In case there is a paymaster in the request (or the current deposit is high enough), this value will be zero.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`validationData`|`uint256`|      - Packaged ValidationData structure. use `_packValidationData` and `_unpackValidationData` to encode and decode. <20-byte> sigAuthorizer - 0 for valid signature, 1 to mark signature failure, otherwise, an address of an "authorizer" contract. <6-byte> validUntil - Last timestamp this operation is valid. 0 for "indefinite" <6-byte> validAfter - First timestamp this operation is valid If an account doesn't use time-range, it is enough to return SIG_VALIDATION_FAILED value (1) for signature failure. Note that the validation code cannot use block.timestamp (or block.number) directly.|


