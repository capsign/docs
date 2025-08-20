# FactoryConfigStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/factory/FactoryConfigStorage.sol)

Storage library for factory configuration that bridges subscription tiers with factory features

*Provides subscription-tier-based access control*


## State Variables
### BASIC_TIER

```solidity
uint8 constant BASIC_TIER = 0;
```


### PROFESSIONAL_TIER

```solidity
uint8 constant PROFESSIONAL_TIER = 1;
```


### ENTERPRISE_TIER

```solidity
uint8 constant ENTERPRISE_TIER = 2;
```


### MAX_TIER

```solidity
uint8 constant MAX_TIER = 255;
```


### TIERED_FACTORY_CONFIG_STORAGE_SLOT

```solidity
bytes32 private constant TIERED_FACTORY_CONFIG_STORAGE_SLOT = keccak256("capsign.storage.tiered_factory_config");
```


## Functions
### layout

Get the storage layout for tiered factory configuration


```solidity
function layout() internal pure returns (Layout storage l);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`l`|`Layout`|The storage layout|


### initialize

Initialize the tiered factory configuration


```solidity
function initialize() internal;
```

### isInitialized

Check if the tiered factory configuration is initialized


```solidity
function isInitialized() internal view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


### setSubscriptionManager

Set the subscription manager address


```solidity
function setSubscriptionManager(address subscriptionManager) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriptionManager`|`address`|Address of the subscription manager|


### getSubscriptionManager

Get the subscription manager address


```solidity
function getSubscriptionManager() internal view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|Address of the subscription manager|


### setTieredFeature

Set a tiered feature


```solidity
function setTieredFeature(string memory feature, uint8 minimumTier, bool enabled) internal;
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
function isTieredFeatureAvailable(string memory feature, uint8 tier) internal view returns (bool);
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


### getTieredFeatureInfo

Get tiered feature info


```solidity
function getTieredFeatureInfo(string memory feature) internal view returns (uint8 minimumTier, bool enabled);
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


### getAvailableFeaturesForTier

Get all available features for a tier


```solidity
function getAvailableFeaturesForTier(uint8 tier) internal view returns (string[] memory features);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`uint8`|The tier to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`features`|`string[]`|Array of available feature names|


### setTieredLimit

Set a tiered limit


```solidity
function setTieredLimit(string memory limitType, uint8 tier, uint256 value) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`limitType`|`string`|The limit identifier|
|`tier`|`uint8`|The tier to set the limit for|
|`value`|`uint256`|The limit value (0 = unlimited)|


### setDefaultTieredLimit

Set default limit for a limit type


```solidity
function setDefaultTieredLimit(string memory limitType, uint256 defaultValue) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`limitType`|`string`|The limit identifier|
|`defaultValue`|`uint256`|The default limit value|


### getTieredLimit

Get tiered limit for a specific tier


```solidity
function getTieredLimit(string memory limitType, uint8 tier) internal view returns (uint256);
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


### setTieredPermission

Set a tiered permission


```solidity
function setTieredPermission(string memory permissionType, uint8 tier, bool granted) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`permissionType`|`string`|The permission identifier|
|`tier`|`uint8`|The tier to set the permission for|
|`granted`|`bool`|Whether the permission is granted|


### setDefaultTieredPermission

Set default permission for a permission type


```solidity
function setDefaultTieredPermission(string memory permissionType, bool defaultValue) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`permissionType`|`string`|The permission identifier|
|`defaultValue`|`bool`|The default permission value|


### isTieredPermissionGranted

Check if a permission is granted for a specific tier


```solidity
function isTieredPermissionGranted(string memory permissionType, uint8 tier) internal view returns (bool);
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


### _isInTieredFeatureList

Check if a feature is in the tiered feature list


```solidity
function _isInTieredFeatureList(string memory feature) private view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`feature`|`string`|The feature to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if feature is in the list|


### _isInTieredLimitList

Check if a limit type is in the tiered limit list


```solidity
function _isInTieredLimitList(string memory limitType) private view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`limitType`|`string`|The limit type to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if limit type is in the list|


### _isInTieredPermissionList

Check if a permission type is in the tiered permission list


```solidity
function _isInTieredPermissionList(string memory permissionType) private view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`permissionType`|`string`|The permission type to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if permission type is in the list|


### getTieredFeatureList

Get all tiered feature names


```solidity
function getTieredFeatureList() internal view returns (string[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string[]`|Array of tiered feature names|


### getTieredLimitList

Get all tiered limit names


```solidity
function getTieredLimitList() internal view returns (string[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string[]`|Array of tiered limit names|


### getTieredPermissionList

Get all tiered permission names


```solidity
function getTieredPermissionList() internal view returns (string[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string[]`|Array of tiered permission names|


## Structs
### TieredFeature

```solidity
struct TieredFeature {
    uint8 minimumTier;
    bool enabled;
}
```

### TieredLimit

```solidity
struct TieredLimit {
    mapping(uint8 => uint256) tierLimits;
    uint256 defaultLimit;
    bool hasDefaultLimit;
}
```

### TieredPermission

```solidity
struct TieredPermission {
    mapping(uint8 => bool) tierPermissions;
    bool defaultPermission;
    bool hasDefaultPermission;
}
```

### Layout

```solidity
struct Layout {
    bool initialized;
    address subscriptionManager;
    mapping(string => TieredFeature) tieredFeatures;
    string[] tieredFeatureList;
    mapping(string => TieredLimit) tieredLimits;
    string[] tieredLimitList;
    mapping(string => TieredPermission) tieredPermissions;
    string[] tieredPermissionList;
    uint256 lastTieredUpdate;
    address lastTieredUpdatedBy;
    uint256[50] __gap;
}
```

