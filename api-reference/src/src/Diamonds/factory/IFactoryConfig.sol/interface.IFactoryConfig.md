# IFactoryConfig
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/factory/IFactoryConfig.sol)

Interface for factory configuration with subscription-based access control

*Provides tier-based functionality for subscription management*


## Functions
### setSubscriptionManager

Set the subscription manager contract address


```solidity
function setSubscriptionManager(address subscriptionManager) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriptionManager`|`address`|Address of the subscription manager|


### getSubscriptionManager

Get the subscription manager contract address


```solidity
function getSubscriptionManager() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|Address of the subscription manager|


### getUserTier

Get the subscription tier for a user


```solidity
function getUserTier(address user) external view returns (uint8 tier);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The user address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`uint8`|The user's current subscription tier (0-255)|


### hasActiveSubscription

Check if a user has an active subscription


```solidity
function hasActiveSubscription(address user) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The user address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if user has active subscription|


### setTieredFeature

Set a tiered feature with minimum required tier


```solidity
function setTieredFeature(string calldata feature, uint8 minimumTier, bool enabled) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`feature`|`string`|The feature identifier|
|`minimumTier`|`uint8`|Minimum tier required to access this feature|
|`enabled`|`bool`|Whether the feature is globally enabled|


### isTieredFeatureAvailable

Check if a feature is available for a specific tier


```solidity
function isTieredFeatureAvailable(string calldata feature, uint8 tier) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`feature`|`string`|The feature identifier|
|`tier`|`uint8`|The tier to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if feature is available for the tier|


### isFeatureAvailableForUser

Check if a feature is available for a user based on their subscription


```solidity
function isFeatureAvailableForUser(string calldata feature, address user) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`feature`|`string`|The feature identifier|
|`user`|`address`|The user address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if feature is available for the user|


### getAvailableFeaturesForTier

Get all features available for a specific tier


```solidity
function getAvailableFeaturesForTier(uint8 tier) external view returns (string[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`uint8`|The tier to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string[]`|Array of available feature names|


### getTieredFeatureInfo

Get the minimum tier required for a feature


```solidity
function getTieredFeatureInfo(string calldata feature) external view returns (uint8 minimumTier, bool enabled);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`feature`|`string`|The feature identifier|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`minimumTier`|`uint8`|The minimum tier required|
|`enabled`|`bool`|Whether the feature is globally enabled|


### setTieredLimit

Set a limit for a specific tier


```solidity
function setTieredLimit(string calldata limitType, uint8 tier, uint256 value) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`limitType`|`string`|The limit identifier|
|`tier`|`uint8`|The tier to set the limit for|
|`value`|`uint256`|The limit value (0 = unlimited)|


### getTieredLimit

Get the limit value for a specific tier


```solidity
function getTieredLimit(string calldata limitType, uint8 tier) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`limitType`|`string`|The limit identifier|
|`tier`|`uint8`|The tier to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The limit value (0 = unlimited)|


### getLimitForUser

Get the limit value for a user based on their subscription


```solidity
function getLimitForUser(string calldata limitType, address user) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`limitType`|`string`|The limit identifier|
|`user`|`address`|The user address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The limit value for the user|


### wouldExceedLimit

Check if a user would exceed their tier limit


```solidity
function wouldExceedLimit(string calldata limitType, address user, uint256 currentUsage, uint256 additionalUsage)
    external
    view
    returns (bool wouldExceed, uint256 currentLimit);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`limitType`|`string`|The limit identifier|
|`user`|`address`|The user address|
|`currentUsage`|`uint256`|Current usage amount|
|`additionalUsage`|`uint256`|Additional usage to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`wouldExceed`|`bool`|True if the additional usage would exceed the limit|
|`currentLimit`|`uint256`|The user's current limit|


### setTieredPermission

Set a permission for a specific tier


```solidity
function setTieredPermission(string calldata permissionType, uint8 tier, bool granted) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`permissionType`|`string`|The permission identifier|
|`tier`|`uint8`|The tier to set the permission for|
|`granted`|`bool`|Whether the permission is granted|


### isTieredPermissionGranted

Check if a permission is granted for a specific tier


```solidity
function isTieredPermissionGranted(string calldata permissionType, uint8 tier) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`permissionType`|`string`|The permission identifier|
|`tier`|`uint8`|The tier to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if permission is granted for the tier|


### isPermissionGrantedForUser

Check if a permission is granted for a user based on their subscription


```solidity
function isPermissionGrantedForUser(string calldata permissionType, address user) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`permissionType`|`string`|The permission identifier|
|`user`|`address`|The user address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if permission is granted for the user|


### setTieredFeaturesBatch

Set multiple tiered features at once


```solidity
function setTieredFeaturesBatch(string[] calldata features, uint8[] calldata minimumTiers, bool[] calldata enabled)
    external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`features`|`string[]`|Array of feature identifiers|
|`minimumTiers`|`uint8[]`|Array of minimum tiers for each feature|
|`enabled`|`bool[]`|Array of enabled flags for each feature|


### setTieredLimitsBatch

Set multiple tiered limits for a specific tier


```solidity
function setTieredLimitsBatch(string[] calldata limitTypes, uint8 tier, uint256[] calldata values) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`limitTypes`|`string[]`|Array of limit identifiers|
|`tier`|`uint8`|The tier to set limits for|
|`values`|`uint256[]`|Array of limit values|


### getUserConfigSummary

Get configuration summary for a user


```solidity
function getUserConfigSummary(address user)
    external
    view
    returns (uint8 tier, bool hasSubscription, string[] memory availableFeatures, uint256[] memory limits);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The user address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`uint8`|User's current tier|
|`hasSubscription`|`bool`|Whether user has active subscription|
|`availableFeatures`|`string[]`|Features available to the user|
|`limits`|`uint256[]`|Key limits for the user|


### getTierInfo

Get tier information from subscription manager


```solidity
function getTierInfo(uint8 tier) external view returns (string memory name, uint256 monthlyFeeUSD, bool isActive);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`uint8`|The tier to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Tier name|
|`monthlyFeeUSD`|`uint256`|Monthly fee in USD (6 decimals)|
|`isActive`|`bool`|Whether the tier is active|


### getAllTiers

Get all available tiers


```solidity
function getAllTiers() external view returns (uint8[] memory tiers, string[] memory names, uint256[] memory fees);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tiers`|`uint8[]`|Array of tier numbers|
|`names`|`string[]`|Array of tier names|
|`fees`|`uint256[]`|Array of monthly fees|


### initializeConfig

Initialize the factory configuration


```solidity
function initializeConfig() external;
```

### isConfigInitialized

Check if configuration is initialized


```solidity
function isConfigInitialized() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


### setFeatureEnabled

Enable or disable a factory feature


```solidity
function setFeatureEnabled(string calldata feature, bool enabled) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`feature`|`string`|The feature identifier|
|`enabled`|`bool`|Whether the feature should be enabled|


### isFeatureEnabled

Check if a feature is enabled


```solidity
function isFeatureEnabled(string calldata feature) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`feature`|`string`|The feature identifier|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if feature is enabled|


### getEnabledFeatures

Get all enabled features (convenience function)


```solidity
function getEnabledFeatures() external view returns (string[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string[]`|Array of enabled feature names|


### setLimit

Set a numeric limit for factory operations


```solidity
function setLimit(string calldata limitType, uint256 value) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`limitType`|`string`|The limit identifier|
|`value`|`uint256`|The limit value|


### getLimit

Get a numeric limit value


```solidity
function getLimit(string calldata limitType) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`limitType`|`string`|The limit identifier|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The limit value|


### setPermission

Set permission for a specific operation type


```solidity
function setPermission(string calldata permissionType, bool granted) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`permissionType`|`string`|The permission identifier|
|`granted`|`bool`|Whether the permission is granted|


### hasPermission

Check if a permission is granted


```solidity
function hasPermission(string calldata permissionType) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`permissionType`|`string`|The permission identifier|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if permission is granted|


### getFactoryConfiguration

Get comprehensive factory configuration


```solidity
function getFactoryConfiguration()
    external
    view
    returns (
        string[] memory features,
        string[] memory limits,
        uint256[] memory limitValues,
        string[] memory permissions,
        bool[] memory permissionStates
    );
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`features`|`string[]`|Array of enabled features|
|`limits`|`string[]`|Array of limit types|
|`limitValues`|`uint256[]`|Array of limit values|
|`permissions`|`string[]`|Array of permission types|
|`permissionStates`|`bool[]`|Array of permission states|


### getConfigurationStats

Get factory configuration statistics


```solidity
function getConfigurationStats()
    external
    view
    returns (uint256 totalFeatures, uint256 enabledFeatures, uint256 totalLimits, uint256 totalPermissions);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalFeatures`|`uint256`|Total number of features|
|`enabledFeatures`|`uint256`|Number of enabled features|
|`totalLimits`|`uint256`|Total number of limits|
|`totalPermissions`|`uint256`|Total number of permissions|


## Events
### SubscriptionManagerUpdated

```solidity
event SubscriptionManagerUpdated(address indexed oldManager, address indexed newManager);
```

### TieredFeatureSet

```solidity
event TieredFeatureSet(string indexed feature, uint8 minimumTier, bool enabled);
```

### TieredLimitSet

```solidity
event TieredLimitSet(string indexed limitType, uint8 tier, uint256 value);
```

### TieredPermissionSet

```solidity
event TieredPermissionSet(string indexed permissionType, uint8 tier, bool granted);
```

### TierUpgradeRequired

```solidity
event TierUpgradeRequired(address indexed user, uint8 currentTier, uint8 requiredTier, string reason);
```

### ConfigurationInitialized

```solidity
event ConfigurationInitialized(address indexed factory);
```

### FeatureToggled

```solidity
event FeatureToggled(string indexed feature, bool enabled);
```

### LimitUpdated

```solidity
event LimitUpdated(string indexed limitType, uint256 oldValue, uint256 newValue);
```

### PermissionUpdated

```solidity
event PermissionUpdated(string indexed permissionType, bool granted);
```

## Errors
### TierUpgradeRequiredError

```solidity
error TierUpgradeRequiredError(string message, uint8 requiredTier, uint8 currentTier);
```

### InvalidTier

```solidity
error InvalidTier(uint8 tier);
```

### TieredModeNotEnabled

```solidity
error TieredModeNotEnabled();
```

### SubscriptionManagerNotSet

```solidity
error SubscriptionManagerNotSet();
```

### FeatureNotAvailableForTier

```solidity
error FeatureNotAvailableForTier(string feature, uint8 tier);
```

### LimitExceededForTier

```solidity
error LimitExceededForTier(string limitType, uint256 current, uint256 max, uint8 tier);
```

