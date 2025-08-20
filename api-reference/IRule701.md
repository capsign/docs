# IRule701
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Compensation/rule701/interfaces/IRule701.sol)

Interface for Rule 701 compliance management


## Functions
### createGrant


```solidity
function createGrant(
    address employee,
    address assetType,
    uint256 amount,
    uint256 vestingStart,
    uint256 vestingDuration,
    uint256 exercisePrice
) external returns (bytes32 grantId);
```

### exerciseGrant


```solidity
function exerciseGrant(bytes32 grantId, uint256 amount) external;
```

### cancelGrant


```solidity
function cancelGrant(bytes32 grantId, string calldata reason) external;
```

### getGrant


```solidity
function getGrant(bytes32 grantId) external view returns (Grant memory);
```

### getEmployeeRecord


```solidity
function getEmployeeRecord(address employee) external view returns (EmployeeRecord memory);
```

### isWithinLimits


```solidity
function isWithinLimits(address employee, uint256 amount) external view returns (bool);
```

### setAnnualLimit


```solidity
function setAnnualLimit(uint256 newLimit) external;
```

### setRule701Status


```solidity
function setRule701Status(bool isActive) external;
```

### addGrantAdmin


```solidity
function addGrantAdmin(address admin) external;
```

### removeGrantAdmin


```solidity
function removeGrantAdmin(address admin) external;
```

## Errors
### Rule701Inactive

```solidity
error Rule701Inactive();
```

### ExceedsAnnualLimit

```solidity
error ExceedsAnnualLimit();
```

### ExceedsEmployeeLimit

```solidity
error ExceedsEmployeeLimit();
```

### ExceedsCompanyLimit

```solidity
error ExceedsCompanyLimit();
```

### InvalidGrant

```solidity
error InvalidGrant();
```

### GrantNotActive

```solidity
error GrantNotActive();
```

### UnauthorizedAccess

```solidity
error UnauthorizedAccess();
```

### InvalidAssetType

```solidity
error InvalidAssetType();
```

## Structs
### Grant

```solidity
struct Grant {
    address employee;
    address assetType;
    uint256 amount;
    uint256 grantDate;
    uint256 vestingStart;
    uint256 vestingDuration;
    uint256 exercisePrice;
    bool isActive;
    bool isExercised;
}
```

### EmployeeRecord

```solidity
struct EmployeeRecord {
    uint256 totalGranted;
    uint256 annualGranted;
    uint256 lastGrantYear;
    bytes32[] grantIds;
    bool isActive;
}
```

