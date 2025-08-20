# WalletPaymasterFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Wallets/factory/facets/WalletPaymasterFacet.sol)

**Inherits:**
[IFactoryCMXPaymaster](/src/Diamonds/factory/IFactoryCMXPaymaster.sol/interface.IFactoryCMXPaymaster.md)

Paymaster integration facet for wallet factory

*Handles CMX token-based fee payments for wallet creation*


## Functions
### onlyFactoryAdmin


```solidity
modifier onlyFactoryAdmin();
```

### updatePaymasterSupport

Update paymaster support


```solidity
function updatePaymasterSupport(address paymaster, bool supported) external override;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paymaster`|`address`|Paymaster address|
|`supported`|`bool`|Whether to support this paymaster|


### isPaymasterSupported

Check if a paymaster is supported


```solidity
function isPaymasterSupported(address paymaster) external view override returns (bool supported);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paymaster`|`address`|Paymaster address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`supported`|`bool`|Whether the paymaster is supported|


### getSupportedPaymasters

Get all supported paymasters


```solidity
function getSupportedPaymasters() external view override returns (address[] memory paymasters);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`paymasters`|`address[]`|Array of supported paymaster addresses|


### preValidatePayment

Pre-validate payment before operation


```solidity
function preValidatePayment(address user, address paymaster, PackedUserOperation calldata userOp)
    external
    view
    override
    returns (bool validated);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User who will pay|
|`paymaster`|`address`|Paymaster address|
|`userOp`|`PackedUserOperation`|User operation (for gas estimation)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`validated`|`bool`|Whether the payment is pre-validated|


### updateCMXPaymasterSupport

Update CMX paymaster support


```solidity
function updateCMXPaymasterSupport(address paymaster, bool supported) external override onlyFactoryAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paymaster`|`address`|CMX paymaster address|
|`supported`|`bool`|Whether to support this paymaster|


### isCMXPaymasterSupported

Check if a CMX paymaster is supported


```solidity
function isCMXPaymasterSupported(address paymaster) external view override returns (bool supported);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paymaster`|`address`|CMX paymaster address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`supported`|`bool`|Whether the paymaster is supported|


### getSupportedCMXPaymasters

Get all supported CMX paymasters


```solidity
function getSupportedCMXPaymasters() external view override returns (address[] memory paymasters);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`paymasters`|`address[]`|Array of supported paymaster addresses|


### validateCMXPayment

Validate CMX paymaster can handle the fee for an operation


```solidity
function validateCMXPayment(address user, address paymaster, string calldata operationType)
    external
    view
    override
    returns (bool canPay, uint256 cmxRequired, uint256 ethEquivalent);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User who will pay the fee|
|`paymaster`|`address`|CMX paymaster address|
|`operationType`|`string`|Type of operation (for fee calculation)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`canPay`|`bool`|Whether the user can pay via CMX|
|`cmxRequired`|`uint256`|CMX amount required|
|`ethEquivalent`|`uint256`|ETH equivalent amount|


### preValidateCMXPayment

Pre-validate CMX payment before operation


```solidity
function preValidateCMXPayment(address user, address paymaster, PackedUserOperation calldata userOp)
    external
    view
    override
    returns (bool validated);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User who will pay|
|`paymaster`|`address`|CMX paymaster address|
|`userOp`|`PackedUserOperation`|User operation (for gas estimation)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`validated`|`bool`|Whether the payment is pre-validated|


### getBaseFee

Get the base creation fee for factory operations


```solidity
function getBaseFee(string calldata operationType) public view override returns (uint256 fee);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`operationType`|`string`|Type of operation|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`fee`|`uint256`|Base fee in ETH (wei)|


### calculateTotalFee

Calculate total fee including any factory-specific multipliers


```solidity
function calculateTotalFee(string calldata operationType, uint256 baseAmount)
    public
    view
    override
    returns (uint256 totalFee);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`operationType`|`string`|Type of operation|
|`baseAmount`|`uint256`|Base amount in ETH|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalFee`|`uint256`|Total fee including multipliers|


### validateWalletCreationPayment

Validate and prepare CMX payment for wallet creation


```solidity
function validateWalletCreationPayment(address user, address paymaster, string calldata walletType)
    external
    view
    returns (bool validated, uint256 cmxAmount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User creating the wallet|
|`paymaster`|`address`|CMX paymaster to use|
|`walletType`|`string`|Type of wallet being created|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`validated`|`bool`|Whether the payment is valid|
|`cmxAmount`|`uint256`|CMX amount that will be charged|


### emitCMXPaymasterUsed

Emit CMX paymaster usage event


```solidity
function emitCMXPaymasterUsed(address user, address paymaster, uint256 cmxAmount, uint256 ethEquivalent) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User who used the paymaster|
|`paymaster`|`address`|Paymaster address|
|`cmxAmount`|`uint256`|CMX amount charged|
|`ethEquivalent`|`uint256`|ETH equivalent value|


### _extractGasLimit

Extract gas limit from user operation


```solidity
function _extractGasLimit(PackedUserOperation calldata userOp) internal pure returns (uint256 gasLimit);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`userOp`|`PackedUserOperation`|User operation|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`gasLimit`|`uint256`|Total gas limit|


### _extractGasPrice

Extract gas price from user operation


```solidity
function _extractGasPrice(PackedUserOperation calldata userOp) internal pure returns (uint256 gasPrice);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`userOp`|`PackedUserOperation`|User operation|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`gasPrice`|`uint256`|Max fee per gas|


