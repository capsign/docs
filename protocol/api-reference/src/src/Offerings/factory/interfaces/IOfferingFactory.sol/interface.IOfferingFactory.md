# IOfferingFactory
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Offerings/factory/interfaces/IOfferingFactory.sol)

**Inherits:**
[IFactory](/src/Diamonds/factory/IFactory.sol/interface.IFactory.md)

Defines all functions available in the offering factory

*Interface for OfferingFactory diamond using unified FacetRegistry and IFactory*

*Template management moved to unified FacetRegistry*

*Combines FactoryBase functionality with offering-specific functions and unified access control*


## Functions
### createOffering


```solidity
function createOffering(string calldata templateName, address identityAccessManager, bytes calldata initData)
    external
    returns (address offering);
```

### createOfferingWithFee


```solidity
function createOfferingWithFee(string calldata templateName, address identityAccessManager, bytes calldata initData)
    external
    payable
    returns (address offering);
```

### getAllOfferings


```solidity
function getAllOfferings() external view returns (address[] memory offerings);
```

### getOfferingsByType


```solidity
function getOfferingsByType(string calldata offeringType) external view returns (address[] memory offerings);
```

### getOfferingsByEntity


```solidity
function getOfferingsByEntity(address entity) external view returns (address[] memory offerings);
```

### getOfferingType


```solidity
function getOfferingType(address offering) external view returns (string memory offeringType);
```

### getOfferingEntity


```solidity
function getOfferingEntity(address offering) external view returns (address entity);
```

### setMaxOfferingsPerEntity


```solidity
function setMaxOfferingsPerEntity(uint256 maxOfferings) external;
```

### setCreationFee


```solidity
function setCreationFee(uint256 fee) external;
```

### setFeeRecipient


```solidity
function setFeeRecipient(address recipient) external;
```

### setTypeSpecificFee


```solidity
function setTypeSpecificFee(string calldata offeringType, uint256 fee) external;
```

### setOfferingTypeAllowed


```solidity
function setOfferingTypeAllowed(string calldata offeringType, bool allowed) external;
```

### initializeFactory


```solidity
function initializeFactory(address feeRecipient, uint256 creationFee, uint256 maxOfferingsPerEntity) external;
```

### getTotalOfferingsCreated


```solidity
function getTotalOfferingsCreated() external view returns (uint256 total);
```

### getOfferingCountByType


```solidity
function getOfferingCountByType(string calldata offeringType) external view returns (uint256 count);
```

### getEntityOfferingCount


```solidity
function getEntityOfferingCount(address entity) external view returns (uint256 count);
```

### getCreationFee


```solidity
function getCreationFee(string calldata offeringType) external view returns (uint256 fee);
```

### isOfferingTypeAllowed


```solidity
function isOfferingTypeAllowed(string calldata offeringType) external view returns (bool allowed);
```

### getFeeRecipient


```solidity
function getFeeRecipient() external view returns (address recipient);
```

### getMaxOfferingsPerEntity


```solidity
function getMaxOfferingsPerEntity() external view returns (uint256 maxOfferings);
```

### getFactoryStats


```solidity
function getFactoryStats()
    external
    view
    returns (uint256 totalTemplates, uint256 totalOfferings, uint256 totalActiveTemplates);
```

### getFactoryConfiguration


```solidity
function getFactoryConfiguration()
    external
    view
    returns (uint256 maxOfferingsPerEntity, uint256 creationFee, address feeRecipient);
```

## Events
### OfferingCreated

```solidity
event OfferingCreated(address indexed offering, address indexed entity, string offeringType, string templateName);
```

### OfferingFactoryCreated

```solidity
event OfferingFactoryCreated(address indexed factory);
```

### FactoryConfigurationUpdated

```solidity
event FactoryConfigurationUpdated(bool paused, uint256 maxOfferingsPerEntity, uint256 creationFee);
```

### OfferingTypeAllowedUpdated

```solidity
event OfferingTypeAllowedUpdated(string offeringType, bool allowed);
```

