# IdentityAccessManagerConfigFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Authorization/factory/facets/IdentityAccessManagerConfigFacet.sol)

Configuration management facet for IdentityAccessManagerFactory diamond

*Handles factory settings, tier management, and feature configuration*


## Functions
### configureAccessManagerCreation

Configure access manager creation feature requirements


```solidity
function configureAccessManagerCreation(uint8 requiredTier, bool requiresSubscription) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requiredTier`|`uint8`|Minimum tier required to create access managers|
|`requiresSubscription`|`bool`|Whether active subscription is required|


### configureTemplateAccessManagerCreation

Configure template-based access manager creation


```solidity
function configureTemplateAccessManagerCreation(uint8 requiredTier, bool requiresSubscription) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requiredTier`|`uint8`|Minimum tier required to use custom templates|
|`requiresSubscription`|`bool`|Whether active subscription is required|


### setMaxAccessManagersPerEntity

Set maximum access managers per entity by tier


```solidity
function setMaxAccessManagersPerEntity(uint8 tier, uint256 maxAccessManagers) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`uint8`|User tier|
|`maxAccessManagers`|`uint256`|Maximum number of access managers (0 = unlimited)|


### setMaxTotalAccessManagers

Set maximum total access managers per tier


```solidity
function setMaxTotalAccessManagers(uint8 tier, uint256 maxTotal) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`uint8`|User tier|
|`maxTotal`|`uint256`|Maximum total access managers (0 = unlimited)|


### setAccessManagerCreationFee

Set access manager creation fee by tier


```solidity
function setAccessManagerCreationFee(uint8 tier, uint256 fee) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`uint8`|User tier|
|`fee`|`uint256`|Creation fee in wei (0 = free)|


### setKYCRequirement

Configure KYC requirement for access manager creation


```solidity
function setKYCRequirement(bool required) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`required`|`bool`|Whether KYC is required|


### setKYCExemption

Grant KYC exemption to specific entity


```solidity
function setKYCExemption(address entity, bool exempt) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Entity address|
|`exempt`|`bool`|Whether to exempt from KYC|


### _getUserTier

Helper to get user tier


```solidity
function _getUserTier(address user) internal view returns (uint8 tier);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`uint8`|User's subscription tier|


### setEmergencyStop

Emergency stop functionality (placeholder)


```solidity
function setEmergencyStop(bool stopped) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`stopped`|`bool`|Whether to stop operations|


### getAccessManagerCreationConfig

Get access manager creation configuration


```solidity
function getAccessManagerCreationConfig() external view returns (uint8 requiredTier, bool requiresSubscription);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`requiredTier`|`uint8`|Minimum tier required|
|`requiresSubscription`|`bool`|Whether subscription is required|


### getTemplateCreationConfig

Get template creation configuration


```solidity
function getTemplateCreationConfig() external view returns (uint8 requiredTier, bool requiresSubscription);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`requiredTier`|`uint8`|Minimum tier required|
|`requiresSubscription`|`bool`|Whether subscription is required|


### getMaxAccessManagersForEntity

Get maximum access managers for user


```solidity
function getMaxAccessManagersForEntity(address entity) external view returns (uint256 maxAccessManagers);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Entity address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`maxAccessManagers`|`uint256`|Maximum allowed access managers|


### getCreationFeeForEntity

Get creation fee for user


```solidity
function getCreationFeeForEntity(address entity) external view returns (uint256 fee);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Entity address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`fee`|`uint256`|Creation fee in wei|


### isAccessManagerCreationAvailable

Check if access manager creation is available for user


```solidity
function isAccessManagerCreationAvailable(address entity) external view returns (bool available);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Entity address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`available`|`bool`|Whether feature is available|


### isTemplateCreationAvailable

Check if template creation is available for user


```solidity
function isTemplateCreationAvailable(address entity) external view returns (bool available);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Entity address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`available`|`bool`|Whether feature is available|


### isKYCExempt

Check if user is KYC exempt


```solidity
function isKYCExempt(address entity) external view returns (bool exempt);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Entity address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`exempt`|`bool`|Whether user is KYC exempt|


### isKYCRequired

Check if KYC is required globally


```solidity
function isKYCRequired() external view returns (bool required);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`required`|`bool`|Whether KYC is required|


### configureAccessManagerSettings

Configure all access manager creation settings at once


```solidity
function configureAccessManagerSettings(
    uint8 basicTier,
    uint8 templateTier,
    bool requiresSubscription,
    uint256[] calldata creationFees,
    uint256[] calldata maxPerEntity
) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`basicTier`|`uint8`|Tier required for basic creation|
|`templateTier`|`uint8`|Tier required for template creation|
|`requiresSubscription`|`bool`|Whether subscription is required|
|`creationFees`|`uint256[]`|Array of fees by tier|
|`maxPerEntity`|`uint256[]`|Array of max access managers per entity by tier|


### _validateTier

Validate tier value


```solidity
function _validateTier(uint8 tier) internal pure;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`uint8`|Tier to validate|


### _validateFee

Validate fee value


```solidity
function _validateFee(uint256 fee) internal pure;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`fee`|`uint256`|Fee to validate|


### setIdentityAccessManagerBytecode

*Set the bytecode for IdentityAccessManager deployment*


```solidity
function setIdentityAccessManagerBytecode(bytes calldata bytecode) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`bytecode`|`bytes`|The creation bytecode of the IdentityAccessManager contract|


### getIdentityAccessManagerBytecode

*Get the bytecode for IdentityAccessManager deployment*


```solidity
function getIdentityAccessManagerBytecode() external pure returns (bytes memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes`|The creation bytecode|


### restricted

Modifier to check admin permissions


```solidity
modifier restricted();
```

## Events
### AccessManagerCreationConfigUpdated

```solidity
event AccessManagerCreationConfigUpdated(string feature, uint8 requiredTier, bool requiresSubscription);
```

### AccessManagerLimitConfigUpdated

```solidity
event AccessManagerLimitConfigUpdated(string limitType, uint8 tier, uint256 limit);
```

### AccessManagerFeeConfigUpdated

```solidity
event AccessManagerFeeConfigUpdated(uint8 tier, uint256 fee);
```

### AccessManagerKYCRequirementUpdated

```solidity
event AccessManagerKYCRequirementUpdated(bool required);
```

## Errors
### InvalidTier

```solidity
error InvalidTier(uint8 tier);
```

### InvalidFee

```solidity
error InvalidFee(uint256 fee);
```

### FeatureNotFound

```solidity
error FeatureNotFound(string feature);
```

### LimitNotFound

```solidity
error LimitNotFound(string limitType);
```

