# GovernanceConfigFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/factory/facets/GovernanceConfigFacet.sol)

**Inherits:**
[IFactoryConfig](/src/Diamonds/factory/IFactoryConfig.sol/interface.IFactoryConfig.md)

Handles governance type permissions and factory configuration through tiered interface

*Standalone diamond facet for configuring governance factory settings*

*Access control is handled by AccessControlFacet*


## Functions
### initializeGovernanceFactory

Initialize governance factory with tiered configuration


```solidity
function initializeGovernanceFactory(address subscriptionManager) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriptionManager`|`address`|Address of the subscription manager|


### _setupDefaultGovernanceTiers

Set up default governance tier configuration


```solidity
function _setupDefaultGovernanceTiers() internal;
```

### canCreateGovernanceType

Check if a user can create a specific governance type


```solidity
function canCreateGovernanceType(address user, string calldata governanceType)
    external
    view
    returns (bool canCreate, string memory reason);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The user address|
|`governanceType`|`string`|The governance type to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`canCreate`|`bool`|Whether the user can create this governance type|
|`reason`|`string`|Reason if cannot create|


### wouldExceedGovernanceLimit

Check if a user would exceed their governance creation limit


```solidity
function wouldExceedGovernanceLimit(address user, uint256 currentGovernanceCount)
    external
    view
    returns (bool wouldExceed, uint256 currentLimit);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The user address|
|`currentGovernanceCount`|`uint256`|Current number of governances owned by user|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`wouldExceed`|`bool`|Whether creating a governance would exceed the limit|
|`currentLimit`|`uint256`|The user's current governance limit|


### getGovernanceCreationSummary

Get governance creation summary for a user


```solidity
function getGovernanceCreationSummary(address user)
    external
    view
    returns (uint8 tier, uint256 maxGovernances, string[] memory availableGovernanceTypes, bool hasSubscription);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The user address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`uint8`|User's subscription tier|
|`maxGovernances`|`uint256`|Maximum governances allowed for this tier|
|`availableGovernanceTypes`|`string[]`|Governance types available for this tier|
|`hasSubscription`|`bool`|Whether user has active subscription|


### setFeeRecipient

Set the fee recipient address (admin only)


```solidity
function setFeeRecipient(address feeRecipient) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`feeRecipient`|`address`|New fee recipient address|


### getFeeRecipient

Get the current fee recipient address


```solidity
function getFeeRecipient() external view returns (address feeRecipient);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`feeRecipient`|`address`|Current fee recipient address|


### emergencyWithdrawFees

Emergency withdrawal of any fees stuck in the factory (admin only)


```solidity
function emergencyWithdrawFees(address payable recipient) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`recipient`|`address payable`|Address to receive the withdrawn funds|


### initializeConfig


```solidity
function initializeConfig() external override;
```

### isConfigInitialized


```solidity
function isConfigInitialized() external view override returns (bool);
```

### setFeatureEnabled


```solidity
function setFeatureEnabled(string calldata feature, bool enabled) external override;
```

### isFeatureEnabled


```solidity
function isFeatureEnabled(string calldata feature) external view override returns (bool);
```

### getEnabledFeatures


```solidity
function getEnabledFeatures() external view override returns (string[] memory);
```

### setLimit


```solidity
function setLimit(string calldata limitType, uint256 value) external override;
```

### getLimit


```solidity
function getLimit(string calldata limitType) external view override returns (uint256);
```

### setPermission


```solidity
function setPermission(string calldata permissionType, bool granted) external override;
```

### hasPermission


```solidity
function hasPermission(string calldata permissionType) external view override returns (bool);
```

### getFactoryConfiguration


```solidity
function getFactoryConfiguration()
    external
    view
    override
    returns (
        string[] memory features,
        string[] memory limits,
        uint256[] memory limitValues,
        string[] memory permissions,
        bool[] memory permissionStates
    );
```

### getConfigurationStats


```solidity
function getConfigurationStats()
    external
    view
    override
    returns (uint256 totalFeatures, uint256 enabledFeatures, uint256 totalLimits, uint256 totalPermissions);
```

### setSubscriptionManager


```solidity
function setSubscriptionManager(address subscriptionManager) external override;
```

### getSubscriptionManager


```solidity
function getSubscriptionManager() external view override returns (address);
```

### getUserTier


```solidity
function getUserTier(address user) external view override returns (uint8);
```

### hasActiveSubscription


```solidity
function hasActiveSubscription(address user) external view override returns (bool);
```

### _getUserTier


```solidity
function _getUserTier(address user) internal view returns (uint8);
```

### _hasActiveSubscription


```solidity
function _hasActiveSubscription(address user) internal view returns (bool);
```

### setTieredFeature


```solidity
function setTieredFeature(string calldata feature, uint8 minimumTier, bool enabled) external override;
```

### isTieredFeatureAvailable


```solidity
function isTieredFeatureAvailable(string calldata feature, uint8 tier) external view override returns (bool);
```

### isFeatureAvailableForUser


```solidity
function isFeatureAvailableForUser(string calldata feature, address user) external view override returns (bool);
```

### getAvailableFeaturesForTier


```solidity
function getAvailableFeaturesForTier(uint8 tier) external view override returns (string[] memory);
```

### getTieredFeatureInfo


```solidity
function getTieredFeatureInfo(string calldata feature)
    external
    view
    override
    returns (uint8 minimumTier, bool enabled);
```

### setTieredLimit


```solidity
function setTieredLimit(string calldata limitType, uint8 tier, uint256 value) external override;
```

### getTieredLimit


```solidity
function getTieredLimit(string calldata limitType, uint8 tier) external view override returns (uint256);
```

### getLimitForUser


```solidity
function getLimitForUser(string calldata limitType, address user) external view override returns (uint256);
```

### wouldExceedLimit


```solidity
function wouldExceedLimit(string calldata limitType, address user, uint256 currentUsage, uint256 additionalUsage)
    external
    view
    override
    returns (bool wouldExceed, uint256 currentLimit);
```

### setTieredPermission


```solidity
function setTieredPermission(string calldata permissionType, uint8 tier, bool granted) external override;
```

### isTieredPermissionGranted


```solidity
function isTieredPermissionGranted(string calldata permissionType, uint8 tier) external view override returns (bool);
```

### isPermissionGrantedForUser


```solidity
function isPermissionGrantedForUser(string calldata permissionType, address user)
    external
    view
    override
    returns (bool);
```

### setTieredFeaturesBatch


```solidity
function setTieredFeaturesBatch(string[] calldata features, uint8[] calldata minimumTiers, bool[] calldata enabled)
    external
    override;
```

### setTieredLimitsBatch


```solidity
function setTieredLimitsBatch(string[] calldata limitTypes, uint8 tier, uint256[] calldata values) external override;
```

### getUserConfigSummary


```solidity
function getUserConfigSummary(address user)
    external
    view
    override
    returns (uint8 tier, bool hasSubscription, string[] memory availableFeatures, uint256[] memory limits);
```

### getTierInfo


```solidity
function getTierInfo(uint8 tier)
    external
    pure
    override
    returns (string memory name, uint256 monthlyFeeUSD, bool isActive);
```

### getAllTiers


```solidity
function getAllTiers()
    external
    pure
    override
    returns (uint8[] memory tiers, string[] memory names, uint256[] memory fees);
```

### _getAllGovernanceTypes

Get all governance types supported by this factory


```solidity
function _getAllGovernanceTypes() internal pure returns (string[] memory governanceTypes);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`governanceTypes`|`string[]`|Array of governance type names|


### _requireAdmin


```solidity
function _requireAdmin() internal view;
```

### _getAuthority

*Get the authority (access manager) for this facet*


```solidity
function _getAuthority() internal view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The address of the access manager|


## Events
### GovernanceCreationValidated

```solidity
event GovernanceCreationValidated(address indexed user, string governanceType, uint8 userTier);
```

### GovernanceLimitChecked

```solidity
event GovernanceLimitChecked(address indexed user, uint256 currentCount, uint256 limit, uint8 tier);
```

### FeeRecipientUpdated

```solidity
event FeeRecipientUpdated(address indexed feeRecipient);
```

### EmergencyFeesWithdrawn

```solidity
event EmergencyFeesWithdrawn(address indexed recipient, uint256 amount);
```

