# AssetConfigFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/factory/facets/AssetConfigFacet.sol)

**Inherits:**
[IFactoryConfig](/src/Diamonds/factory/IFactoryConfig.sol/interface.IFactoryConfig.md)

Handles asset type permissions and factory configuration through tiered interface

*Standalone diamond facet for configuring asset factory settings*

*Access control is handled by AccessControlFacet*


## Functions
### initializeAssetFactory

Initialize asset factory with tiered configuration


```solidity
function initializeAssetFactory(address subscriptionManager) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriptionManager`|`address`|Address of the subscription manager|


### _setupDefaultAssetTiers

Set up default asset tier configuration


```solidity
function _setupDefaultAssetTiers() internal;
```

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

### _getAllAssetTypes

Get all asset types supported by this factory


```solidity
function _getAllAssetTypes() internal pure returns (string[] memory assetTypes);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`assetTypes`|`string[]`|Array of asset type names|


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
### AssetCreationValidated

```solidity
event AssetCreationValidated(address indexed user, string assetType, uint8 userTier);
```

### AssetLimitChecked

```solidity
event AssetLimitChecked(address indexed user, uint256 currentCount, uint256 limit, uint8 tier);
```

