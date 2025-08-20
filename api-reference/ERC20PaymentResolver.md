# ERC20PaymentResolver
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Attestations/ERC20PaymentResolver.sol)

Resolver contract for EAS attestations that enforces payment in an ERC-20 token.


## State Variables
### paymentCurrency

```solidity
IERC20 public immutable paymentCurrency;
```


### attestationFee

```solidity
uint256 public immutable attestationFee;
```


### feeRecipient

```solidity
address public immutable feeRecipient;
```


## Functions
### constructor


```solidity
constructor(address _token, uint256 _fee, address _feeRecipient);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_token`|`address`|The ERC-20 token address used for payment.|
|`_fee`|`uint256`|The fee required per attestation (in token's smallest unit).|
|`_feeRecipient`|`address`|The address that will receive the fees.|


### resolve

Called by the EAS contract when processing an attestation.

*This function performs an ERC-20 transferFrom() to collect the fee.
The attester must have approved this contract for the required fee amount.*


```solidity
function resolve(address attester, bytes calldata) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`attester`|`address`|The address initiating the attestation.|
|`<none>`|`bytes`||


