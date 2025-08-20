# FactoryConfigFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/factory/FactoryConfigFacet.sol)

**Inherits:**
[IFactoryConfig](/src/Diamonds/factory/IFactoryConfig.sol/interface.IFactoryConfig.md)

Implementation of factory configuration with subscription integration

*Provides subscription-tier-based access control for factory features, limits, and permissions*


## Functions
### onlyAdmin


```solidity
modifier onlyAdmin();
```

### validTier


```solidity
modifier validTier(uint8 tier);
```

### setSubscriptionManager


```solidity
function setSubscriptionManager(address subscriptionManager) external override onlyAdmin;
```

### getSubscriptionManager


```solidity
function getSubscriptionManager() external view override returns (address);
```

### getUserTier


```solidity
function getUserTier(address user) external view override returns (uint8 tier);
```

### hasActiveSubscription


```solidity
function hasActiveSubscription(address user) external view override returns (bool);
```

### setTieredFeature


```solidity
function setTieredFeature(string calldata feature, uint8 minimumTier, bool enabled)
    external
    override
    onlyAdmin
    validTier(minimumTier);
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
function getAvailableFeaturesForTier(uint8 tier) external view override validTier(tier) returns (string[] memory);
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
function setTieredLimit(string calldata limitType, uint8 tier, uint256 value)
    external
    override
    onlyAdmin
    validTier(tier);
```

### getTieredLimit


```solidity
function getTieredLimit(string calldata limitType, uint8 tier)
    external
    view
    override
    validTier(tier)
    returns (uint256);
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
function setTieredPermission(string calldata permissionType, uint8 tier, bool granted)
    external
    override
    onlyAdmin
    validTier(tier);
```

### isTieredPermissionGranted


```solidity
function isTieredPermissionGranted(string calldata permissionType, uint8 tier)
    external
    view
    override
    validTier(tier)
    returns (bool);
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
    override
    onlyAdmin;
```

### setTieredLimitsBatch


```solidity
function setTieredLimitsBatch(string[] calldata limitTypes, uint8 tier, uint256[] calldata values)
    external
    override
    onlyAdmin
    validTier(tier);
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
    view
    override
    validTier(tier)
    returns (string memory name, uint256 monthlyFeeUSD, bool isActive);
```

### getAllTiers


```solidity
function getAllTiers()
    external
    view
    override
    returns (uint8[] memory tiers, string[] memory names, uint256[] memory fees);
```

### initializeConfig


```solidity
function initializeConfig() external virtual override onlyAdmin;
```

### isConfigInitialized


```solidity
function isConfigInitialized() external view override returns (bool);
```

### getEnabledFeatures


```solidity
function getEnabledFeatures() external view override returns (string[] memory);
```

### _getUserTier


```solidity
function _getUserTier(address user) internal view returns (uint8);
```

### _hasActiveSubscription


```solidity
function _hasActiveSubscription(address user) internal view returns (bool);
```

### _requireAdmin


```solidity
function _requireAdmin() internal view virtual;
```

