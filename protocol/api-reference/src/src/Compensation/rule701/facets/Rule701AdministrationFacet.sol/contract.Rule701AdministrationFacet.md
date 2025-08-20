# Rule701AdministrationFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Compensation/rule701/facets/Rule701AdministrationFacet.sol)

**Inherits:**
[IRule701Events](/src/Compensation/rule701/interfaces/IRule701Events.sol/interface.IRule701Events.md)

Administrative functions for Rule 701 management


## Functions
### onlyEntity


```solidity
modifier onlyEntity();
```

### setAnnualLimit

Set the annual grant limit


```solidity
function setAnnualLimit(uint256 newLimit) external onlyEntity;
```

### setCompanyLimit

Set the company-wide grant limit


```solidity
function setCompanyLimit(uint256 newLimit) external onlyEntity;
```

### setRule701Status

Enable or disable Rule 701 plan


```solidity
function setRule701Status(bool isActive) external onlyEntity;
```

### addGrantAdmin

Add a grant administrator


```solidity
function addGrantAdmin(address admin) external onlyEntity;
```

### removeGrantAdmin

Remove a grant administrator


```solidity
function removeGrantAdmin(address admin) external onlyEntity;
```

### addApprovedAssetType

Add an approved asset type


```solidity
function addApprovedAssetType(address assetType) external onlyEntity;
```

### removeApprovedAssetType

Remove an approved asset type


```solidity
function removeApprovedAssetType(address assetType) external onlyEntity;
```

### getApprovedAssetTypes

Get approved asset types


```solidity
function getApprovedAssetTypes() external view returns (address[] memory);
```

### isApprovedAssetType

Check if an asset type is approved


```solidity
function isApprovedAssetType(address assetType) external view returns (bool);
```

### getPlanConfig

Get plan configuration


```solidity
function getPlanConfig()
    external
    view
    returns (address entity, uint256 annualLimit, uint256 companyLimit, uint256 totalGranted, bool isActive);
```

