# RegAOfferingFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Offerings/offering/facets/RegAOfferingFacet.sol)

Implements Reg A+ specific compliance including:
- Tier 1 ($20M limit) and Tier 2 ($75M limit) offerings
- Ongoing reporting requirements

*Specialized facet for Regulation A+ offerings*

*Uses ExemptionLimitFacet for shared exemption tracking functionality*


## State Variables
### TIER_1_LIMIT

```solidity
uint256 public constant TIER_1_LIMIT = 20_000_000e6;
```


### TIER_2_LIMIT

```solidity
uint256 public constant TIER_2_LIMIT = 75_000_000e6;
```


## Functions
### initializeRegA

*Initialize Regulation A+ specific settings*


```solidity
function initializeRegA(uint8 tier, uint256 customLimit, bool requiresAudits) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`uint8`|The Regulation A+ tier (1 or 2)|
|`customLimit`|`uint256`|Custom exemption limit (0 to use tier default)|
|`requiresAudits`|`bool`|Whether audited financials are required|


### changeTier

*Change the Regulation A+ tier*


```solidity
function changeTier(uint8 newTier) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newTier`|`uint8`|The new tier (1 or 2)|


### validateRegAInvestment

*Validate investment for Regulation A+ compliance*


```solidity
function validateRegAInvestment(address investor, uint256 amount) external pure returns (bool valid);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`amount`|`uint256`|The investment amount|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`valid`|`bool`|Whether the investment is valid under Reg A+ rules|


### enforceRegACompliance

*Enforce Regulation A+ compliance and exemption limits*


```solidity
function enforceRegACompliance(address investor, uint256 amount) external view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`amount`|`uint256`|The investment amount|


### setExemptionLimit

*Set the exemption limit (must not exceed tier maximum)*


```solidity
function setExemptionLimit(uint256 newLimit) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newLimit`|`uint256`|The new exemption limit|


### getRegAConfig

*Get Regulation A+ configuration*


```solidity
function getRegAConfig() external view returns (uint8 tier, uint256 tierLimit, uint256 currentLimit);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`uint8`|The Reg A+ tier (1 or 2)|
|`tierLimit`|`uint256`|The tier-specific limit|
|`currentLimit`|`uint256`|The current exemption limit|


### _requireEntity

*Internal function to check if caller is the entity*


```solidity
function _requireEntity(OfferingStorage.Layout storage s) internal view;
```

## Events
### RegAOfferingConfigured

```solidity
event RegAOfferingConfigured(uint8 tier, uint256 tierLimit, bool requiresAudits);
```

### TierChanged

```solidity
event TierChanged(uint8 oldTier, uint8 newTier);
```

## Enums
### Tier

```solidity
enum Tier {
    Tier1,
    Tier2
}
```

