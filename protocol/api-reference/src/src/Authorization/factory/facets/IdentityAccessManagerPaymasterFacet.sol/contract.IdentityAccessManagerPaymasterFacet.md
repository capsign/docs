# IdentityAccessManagerPaymasterFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Authorization/factory/facets/IdentityAccessManagerPaymasterFacet.sol)

Paymaster integration facet for IdentityAccessManagerFactory diamond

*Handles paymaster settings for sponsored access manager creation*


## State Variables
### PAYMASTER_STORAGE_SLOT

```solidity
bytes32 private constant PAYMASTER_STORAGE_SLOT = keccak256("entity.access.manager.paymaster.storage");
```


## Functions
### _getPaymasterStorage

Get the storage struct for this facet


```solidity
function _getPaymasterStorage() internal pure returns (PaymasterStorage storage ps);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`ps`|`PaymasterStorage`|The storage struct|


### setPaymaster

Set the paymaster contract address


```solidity
function setPaymaster(address paymaster) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paymaster`|`address`|Address of the paymaster contract|


### setPaymasterEnabled

Enable or disable paymaster functionality


```solidity
function setPaymasterEnabled(bool enabled) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`enabled`|`bool`|Whether paymaster should be enabled|


### setEntitySponsorship

Set sponsorship for a specific entity


```solidity
function setEntitySponsorship(address entity, bool sponsored) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Entity address|
|`sponsored`|`bool`|Whether entity should have sponsored creation|


### setTierSponsorship

Set sponsorship eligibility for a tier


```solidity
function setTierSponsorship(uint8 tier, bool enabled) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`uint8`|Subscription tier|
|`enabled`|`bool`|Whether tier is eligible for sponsorship|


### setSponsorshipLimit

Set maximum sponsored creations per period


```solidity
function setSponsorshipLimit(uint256 maxCreations) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`maxCreations`|`uint256`|Maximum number of sponsored creations|


### setSponsorshipPeriod

Set sponsorship period duration


```solidity
function setSponsorshipPeriod(uint256 duration) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`duration`|`uint256`|Duration in seconds (e.g., 30 days)|


### isEligibleForSponsorship

Check if an entity is eligible for sponsored creation


```solidity
function isEligibleForSponsorship(address entity) external view returns (bool eligible);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Entity address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`eligible`|`bool`|Whether entity is eligible for sponsorship|


### _isEligibleForSponsorshipInternal

Internal logic to check sponsorship eligibility


```solidity
function _isEligibleForSponsorshipInternal(address entity) internal view returns (bool eligible);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Entity address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`eligible`|`bool`|Whether entity is eligible for sponsorship|


### recordSponsoredCreation

Record a sponsored access manager creation


```solidity
function recordSponsoredCreation(address entity, address accessManager) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Entity address|
|`accessManager`|`address`|Created access manager address|


### getPaymasterConfig

Get current paymaster configuration


```solidity
function getPaymasterConfig() external view returns (address paymaster, bool enabled);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`paymaster`|`address`|Current paymaster address|
|`enabled`|`bool`|Whether paymaster is enabled|


### isEntitySponsored

Check if an entity has sponsorship


```solidity
function isEntitySponsored(address entity) external view returns (bool sponsored);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Entity address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`sponsored`|`bool`|Whether entity is sponsored|


### isTierSponsorshipEnabled

Check if a tier has sponsorship enabled


```solidity
function isTierSponsorshipEnabled(uint8 tier) external view returns (bool enabled);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`uint8`|Subscription tier|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`enabled`|`bool`|Whether tier sponsorship is enabled|


### getSponsorshipStats

Get sponsorship statistics


```solidity
function getSponsorshipStats()
    external
    view
    returns (
        uint256 maxCreations,
        uint256 currentCount,
        uint256 periodStart,
        uint256 periodDuration,
        uint256 timeRemaining
    );
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`maxCreations`|`uint256`|Maximum sponsored creations per period|
|`currentCount`|`uint256`|Current sponsored count in period|
|`periodStart`|`uint256`|Start of current period|
|`periodDuration`|`uint256`|Duration of sponsorship period|
|`timeRemaining`|`uint256`|Time remaining in current period|


### getRemainingSponsorship

Get remaining sponsored creations in current period


```solidity
function getRemainingSponsorship() external view returns (uint256 remaining);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`remaining`|`uint256`|Number of remaining sponsored creations|


### _checkSponsorshipLimits

Check if sponsorship limits allow creation


```solidity
function _checkSponsorshipLimits() internal view returns (bool allowed);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`allowed`|`bool`|Whether creation is allowed within limits|


### _shouldResetPeriod

Check if sponsorship period should be reset


```solidity
function _shouldResetPeriod() internal view returns (bool shouldReset);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`shouldReset`|`bool`|Whether period should be reset|


### _resetPeriodIfNeeded

Reset sponsorship period if needed


```solidity
function _resetPeriodIfNeeded() internal;
```

### setMultipleEntitySponsorship

Set sponsorship for multiple entitys


```solidity
function setMultipleEntitySponsorship(address[] calldata entitys, bool[] calldata sponsored) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entitys`|`address[]`|Array of entity addresses|
|`sponsored`|`bool[]`|Array of sponsorship flags|


### setMultipleTierSponsorship

Set sponsorship for multiple tiers


```solidity
function setMultipleTierSponsorship(uint8[] calldata tiers, bool[] calldata enabled) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tiers`|`uint8[]`|Array of tier numbers|
|`enabled`|`bool[]`|Array of enabled flags|


### restricted

Modifier to check admin permissions


```solidity
modifier restricted();
```

## Events
### PaymasterUpdated

```solidity
event PaymasterUpdated(address indexed oldPaymaster, address indexed newPaymaster);
```

### PaymasterStatusChanged

```solidity
event PaymasterStatusChanged(bool enabled);
```

### EntitySponsorshipUpdated

```solidity
event EntitySponsorshipUpdated(address indexed entity, bool sponsored);
```

### TierSponsorshipUpdated

```solidity
event TierSponsorshipUpdated(uint8 tier, bool enabled);
```

### SponsoredAccessManagerCreated

```solidity
event SponsoredAccessManagerCreated(address indexed entity, address indexed accessManager, address paymaster);
```

### SponsorshipLimitUpdated

```solidity
event SponsorshipLimitUpdated(uint256 oldLimit, uint256 newLimit);
```

### SponsorshipPeriodUpdated

```solidity
event SponsorshipPeriodUpdated(uint256 duration);
```

## Errors
### PaymasterNotSet

```solidity
error PaymasterNotSet();
```

### PaymasterNotEnabled

```solidity
error PaymasterNotEnabled();
```

### NotSponsoredEntity

```solidity
error NotSponsoredEntity(address entity);
```

### SponsorshipLimitExceeded

```solidity
error SponsorshipLimitExceeded(uint256 current, uint256 max);
```

### InvalidPaymaster

```solidity
error InvalidPaymaster(address paymaster);
```

### InvalidSponsorshipConfig

```solidity
error InvalidSponsorshipConfig();
```

## Structs
### PaymasterStorage

```solidity
struct PaymasterStorage {
    address paymaster;
    bool paymasterEnabled;
    mapping(address => bool) sponsoredEntities;
    mapping(uint8 => bool) tierSponsorshipEnabled;
    uint256 maxSponsoredCreations;
    uint256 currentSponsoredCount;
    uint256 sponsorshipPeriodStart;
    uint256 sponsorshipPeriodDuration;
}
```

