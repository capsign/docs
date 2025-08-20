# Rule701Storage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Compensation/rule701/storage/Rule701Storage.sol)

Storage layout for Rule 701 compensation management


## State Variables
### STORAGE_SLOT

```solidity
bytes32 internal constant STORAGE_SLOT = keccak256("capsign.contracts.storage.Rule701");
```


## Functions
### layout


```solidity
function layout() internal pure returns (Layout storage s);
```

## Structs
### Layout

```solidity
struct Layout {
    address entity;
    uint256 annualLimit;
    uint256 companyLimit;
    bool isActive;
    mapping(bytes32 => IRule701.Grant) grants;
    mapping(address => IRule701.EmployeeRecord) employees;
    uint256 totalGranted;
    uint256 nextGrantNonce;
    mapping(address => bool) admins;
    mapping(address => bool) approvedAssetTypes;
    address[] assetTypeList;
}
```

