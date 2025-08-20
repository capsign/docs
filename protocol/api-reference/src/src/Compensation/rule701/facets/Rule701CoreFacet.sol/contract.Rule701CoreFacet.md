# Rule701CoreFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Compensation/rule701/facets/Rule701CoreFacet.sol)

**Inherits:**
[IRule701Events](/src/Compensation/rule701/interfaces/IRule701Events.sol/interface.IRule701Events.md)

Core functionality for Rule 701 compliance management


## Functions
### onlyActive


```solidity
modifier onlyActive();
```

### onlyAdmin


```solidity
modifier onlyAdmin();
```

### initialize

Initialize the Rule 701 plan


```solidity
function initialize(address _entity, uint256 _annualLimit, uint256 _companyLimit) external;
```

### createGrant

Create a new grant for an employee


```solidity
function createGrant(
    address employee,
    address assetType,
    uint256 amount,
    uint256 vestingStart,
    uint256 vestingDuration,
    uint256 exercisePrice
) external onlyActive onlyAdmin returns (bytes32 grantId);
```

### exerciseGrant

Exercise a grant


```solidity
function exerciseGrant(bytes32 grantId, uint256 amount) external onlyActive;
```

### cancelGrant

Cancel a grant


```solidity
function cancelGrant(bytes32 grantId, string calldata reason) external onlyAdmin;
```

### getGrant


```solidity
function getGrant(bytes32 grantId) external view returns (IRule701.Grant memory);
```

### getEmployeeRecord


```solidity
function getEmployeeRecord(address employee) external view returns (IRule701.EmployeeRecord memory);
```

### isWithinLimits


```solidity
function isWithinLimits(address employee, uint256 amount) external view returns (bool);
```

### _checkLimits


```solidity
function _checkLimits(Rule701Storage.Layout storage s, address employee, uint256 amount) internal view returns (bool);
```

### _updateEmployeeRecord


```solidity
function _updateEmployeeRecord(Rule701Storage.Layout storage s, address employee, uint256 amount, bytes32 grantId)
    internal;
```

### _calculateVested


```solidity
function _calculateVested(IRule701.Grant storage grant) internal view returns (uint256);
```

### _isAdmin


```solidity
function _isAdmin(address account, Rule701Storage.Layout storage s) internal view returns (bool);
```

### _authority


```solidity
function _authority() internal view returns (address);
```

