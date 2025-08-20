# OfferingFactoryStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Offerings/factory/storage/OfferingFactoryStorage.sol)

Contains offering deployment tracking and factory-specific configuration

*Storage contract for OfferingFactory diamond using unified FacetRegistry*

*Template management moved to unified FacetRegistry*


## State Variables
### STORAGE_SLOT

```solidity
bytes32 internal constant STORAGE_SLOT = keccak256("capsign.storage.offering_factory");
```


## Functions
### layout


```solidity
function layout() internal pure returns (Layout storage l);
```

### setFeeRecipient

Set fee recipient address


```solidity
function setFeeRecipient(address feeRecipient) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`feeRecipient`|`address`|New fee recipient address|


### getFeeRecipient

Get fee recipient address


```solidity
function getFeeRecipient() internal view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|Fee recipient address|


## Structs
### Layout

```solidity
struct Layout {
    address[] allOfferings;
    mapping(address => string) offeringTypes;
    mapping(address => address) offeringEntities;
    mapping(string => address[]) offeringsByType;
    mapping(string => uint256) offeringCountsByType;
    mapping(address => uint256) entityOfferingCounts;
    uint256 totalOfferingsCreated;
    mapping(string => bool) allowedOfferingTypes;
    uint256 maxOfferingsPerEntity;
    uint256 creationFee;
    address feeRecipient;
    mapping(string => uint256) typeSpecificFees;
    uint256[50] __gap;
}
```

