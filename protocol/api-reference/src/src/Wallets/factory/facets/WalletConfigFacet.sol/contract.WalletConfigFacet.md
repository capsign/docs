# WalletConfigFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Wallets/factory/facets/WalletConfigFacet.sol)

**Inherits:**
[IFactoryConfig](/src/Diamonds/factory/IFactoryConfig.sol/interface.IFactoryConfig.md)

Tiered configuration facet for wallet factory with subscription-based access control

*Standalone diamond facet implementing IFactoryConfig with wallet-specific tier checking and limits*


## Functions
### initializeWalletFactory

Initialize wallet factory with tiered configuration


```solidity
function initializeWalletFactory(address subscriptionManager) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriptionManager`|`address`|Address of the subscription manager|


### canCreateWalletType

Check if a user can create a specific wallet type


```solidity
function canCreateWalletType(address user, string calldata walletType)
    external
    view
    returns (bool canCreate, string memory reason);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The user address|
|`walletType`|`string`|The wallet type to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`canCreate`|`bool`|Whether the user can create this wallet type|
|`reason`|`string`|Reason if cannot create|


### wouldExceedWalletLimit

Check if a user would exceed their wallet creation limit


```solidity
function wouldExceedWalletLimit(address user, uint256 currentWalletCount)
    external
    view
    returns (bool wouldExceed, uint256 currentLimit);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The user address|
|`currentWalletCount`|`uint256`|Current number of wallets owned by user|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`wouldExceed`|`bool`|Whether creating a wallet would exceed the limit|
|`currentLimit`|`uint256`|The user's current wallet limit|


### getWalletCreationSummary

Get wallet creation summary for a user


```solidity
function getWalletCreationSummary(address user)
    external
    view
    returns (uint8 tier, uint256 maxWallets, string[] memory availableWalletTypes, bool hasActiveSubscription);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The user address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`uint8`|User's subscription tier|
|`maxWallets`|`uint256`|Maximum wallets allowed for this tier|
|`availableWalletTypes`|`string[]`|Wallet types available for this tier|
|`hasActiveSubscription`|`bool`|Whether user has active subscription|


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


### _setupDefaultWalletTiers

Set up default wallet tier configuration


```solidity
function _setupDefaultWalletTiers() internal;
```

### initializeConfig


```solidity
function initializeConfig() external virtual override;
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

### _getAllWalletTypes

Get all wallet types supported by this factory


```solidity
function _getAllWalletTypes() internal pure returns (string[] memory walletTypes);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`walletTypes`|`string[]`|Array of wallet type names|


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

### getTierInfo


```solidity
function getTierInfo(uint8 tier) external pure returns (string memory name, uint256 monthlyFeeUSD, bool isActive);
```

### wouldExceedLimit


```solidity
function wouldExceedLimit(string calldata limitType, address user, uint256 currentValue, uint256 increment)
    external
    view
    returns (bool wouldExceed, uint256 currentLimit);
```

### getLimitForUser


```solidity
function getLimitForUser(string calldata limitType, address user) external view returns (uint256);
```

### isFeatureAvailableForUser


```solidity
function isFeatureAvailableForUser(string calldata feature, address user) external view returns (bool);
```

### setSubscriptionManager


```solidity
function setSubscriptionManager(address subscriptionManager) external override;
```

### getSubscriptionManager


```solidity
function getSubscriptionManager() external view override returns (address);
```

### setTieredFeature


```solidity
function setTieredFeature(string calldata feature, uint8 minimumTier, bool enabled) external override;
```

### isTieredFeatureAvailable


```solidity
function isTieredFeatureAvailable(string calldata feature, uint8 tier) external view override returns (bool);
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

### getAllTiers


```solidity
function getAllTiers()
    external
    pure
    override
    returns (uint8[] memory tiers, string[] memory names, uint256[] memory fees);
```

### _requireAdmin

Check if caller is admin


```solidity
function _requireAdmin() internal view;
```

### getUpgradePrompt

Get upgrade prompt for a user trying to access a restricted feature


```solidity
function getUpgradePrompt(address user, string calldata walletType)
    external
    view
    returns (bool shouldUpgrade, uint8 currentTier, uint8 requiredTier, string memory upgradeMessage);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The user address|
|`walletType`|`string`|The wallet type they're trying to create|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`shouldUpgrade`|`bool`|Whether user should upgrade|
|`currentTier`|`uint8`|User's current tier|
|`requiredTier`|`uint8`|Minimum tier required for the feature|
|`upgradeMessage`|`string`|Message explaining the upgrade|


## Events
### WalletCreationValidated

```solidity
event WalletCreationValidated(address indexed user, string walletType, uint8 userTier);
```

### WalletLimitChecked

```solidity
event WalletLimitChecked(address indexed user, uint256 currentCount, uint256 limit, uint8 tier);
```

### TierUpgradePrompted

```solidity
event TierUpgradePrompted(address indexed user, uint8 currentTier, uint8 requiredTier, string feature);
```

### FeeRecipientUpdated

```solidity
event FeeRecipientUpdated(address indexed feeRecipient);
```

### EmergencyFeesWithdrawn

```solidity
event EmergencyFeesWithdrawn(address indexed recipient, uint256 amount);
```

