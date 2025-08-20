# AssetPaymasterFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/factory/facets/AssetPaymasterFacet.sol)

**Inherits:**
[IFactoryCMXPaymaster](/src/Diamonds/factory/IFactoryCMXPaymaster.sol/interface.IFactoryCMXPaymaster.md)

Paymaster integration facet for asset factory

*Handles CMX token-based fee payments for asset creation*


## State Variables
### CMX_PAYMASTER_STORAGE_SLOT

```solidity
bytes32 private constant CMX_PAYMASTER_STORAGE_SLOT = keccak256("capsign.storage.asset_factory.cmx_paymaster");
```


## Functions
### _getCMXPaymasterStorage


```solidity
function _getCMXPaymasterStorage() internal pure returns (CMXPaymasterLayout storage l);
```

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
|`operationType`|`string`|Type of operation (asset type)|

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
|`operationType`|`string`|Type of operation (asset type)|
|`baseAmount`|`uint256`|Base amount in ETH|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalFee`|`uint256`|Total fee including multipliers|


### setCreationFee

Set the general creation fee


```solidity
function setCreationFee(uint256 fee) external onlyFactoryAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`fee`|`uint256`|New fee amount in ETH (wei)|


### setAssetTypeFee

Set fee for a specific asset type


```solidity
function setAssetTypeFee(string calldata assetType, uint256 fee) external onlyFactoryAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`assetType`|`string`|Asset type name|
|`fee`|`uint256`|Fee amount in ETH (wei)|


### setFeeRecipient

Set fee recipient address


```solidity
function setFeeRecipient(address feeRecipient) external onlyFactoryAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`feeRecipient`|`address`|New fee recipient address|


### getCreationFee

Get the general creation fee


```solidity
function getCreationFee() external view returns (uint256 fee);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`fee`|`uint256`|Current general creation fee|


### getAssetTypeFee

Get fee for a specific asset type


```solidity
function getAssetTypeFee(string calldata assetType) external view returns (uint256 fee);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`assetType`|`string`|Asset type name|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`fee`|`uint256`|Fee for the asset type|


### getFeeRecipient

Get fee recipient address


```solidity
function getFeeRecipient() external view returns (address feeRecipient);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`feeRecipient`|`address`|Current fee recipient address|


### validateAssetCreationPayment

Validate and prepare CMX payment for asset creation


```solidity
function validateAssetCreationPayment(address user, address paymaster, string calldata assetType)
    external
    view
    returns (bool validated, uint256 cmxAmount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User creating the asset|
|`paymaster`|`address`|CMX paymaster to use|
|`assetType`|`string`|Type of asset being created|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`validated`|`bool`|Whether the payment is valid|
|`cmxAmount`|`uint256`|CMX amount that will be charged|


### emitCMXPaymasterUsed

Emit CMX paymaster usage event


```solidity
function emitCMXPaymasterUsed(
    address user,
    address paymaster,
    uint256 cmxAmount,
    uint256 ethEquivalent,
    string calldata assetType
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User who used the paymaster|
|`paymaster`|`address`|Paymaster address|
|`cmxAmount`|`uint256`|CMX amount charged|
|`ethEquivalent`|`uint256`|ETH equivalent value|
|`assetType`|`string`|Type of asset created|


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


## Events
### AssetCreationFeeUpdated
Emitted when asset creation fee is updated


```solidity
event AssetCreationFeeUpdated(string indexed assetType, uint256 fee);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`assetType`|`string`|Asset type (empty string for general fee)|
|`fee`|`uint256`|New fee amount|

### FeeRecipientUpdated
Emitted when fee recipient is updated


```solidity
event FeeRecipientUpdated(address indexed feeRecipient);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`feeRecipient`|`address`|New fee recipient address|

## Structs
### CMXPaymasterLayout

```solidity
struct CMXPaymasterLayout {
    mapping(address => bool) supportedCMXPaymasters;
    address[] cmxPaymasterList;
    uint256 creationFee;
    address feeRecipient;
    mapping(string => uint256) typeSpecificFees;
}
```

