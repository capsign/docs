# IRule701Events
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Compensation/rule701/interfaces/IRule701Events.sol)

Domain-specific events for Rule 701 compensation diamonds


## Events
### GrantCreated

```solidity
event GrantCreated(
    address indexed employee,
    address indexed assetType,
    uint256 amount,
    uint256 grantDate,
    uint256 vestingStart,
    uint256 vestingDuration
);
```

### GrantExercised

```solidity
event GrantExercised(address indexed employee, address indexed assetType, uint256 amount, uint256 exercisePrice);
```

### GrantCancelled

```solidity
event GrantCancelled(address indexed employee, address indexed assetType, uint256 amount, string reason);
```

### AnnualLimitUpdated

```solidity
event AnnualLimitUpdated(uint256 oldLimit, uint256 newLimit);
```

### EmployeeLimitReached

```solidity
event EmployeeLimitReached(address indexed employee, uint256 totalGranted);
```

### CompanyLimitReached

```solidity
event CompanyLimitReached(uint256 totalGranted);
```

### GrantAdminAdded

```solidity
event GrantAdminAdded(address indexed admin);
```

### GrantAdminRemoved

```solidity
event GrantAdminRemoved(address indexed admin);
```

### Rule701StatusChanged

```solidity
event Rule701StatusChanged(bool isActive);
```

### GrantConverted

```solidity
event GrantConverted(address indexed employee, address indexed fromAsset, address indexed toAsset, uint256 amount);
```

