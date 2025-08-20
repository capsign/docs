# ICompensationFactory
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Compensation/factory/interfaces/ICompensationFactory.sol)

Interface for compensation plan factory


## Functions
### deployRule701Plan


```solidity
function deployRule701Plan(string calldata name, address entity, uint256 annualLimit, bytes calldata additionalConfig)
    external
    returns (address);
```

### deployCompensationPlan


```solidity
function deployCompensationPlan(string calldata planType, string calldata name, address entity, bytes calldata config)
    external
    returns (address);
```

### getPlansByEntity


```solidity
function getPlansByEntity(address entity) external view returns (address[] memory);
```

### getPlanType


```solidity
function getPlanType(address plan) external view returns (string memory);
```

### isPlan


```solidity
function isPlan(address plan) external view returns (bool);
```

### addPlanTemplate


```solidity
function addPlanTemplate(string calldata planType, address[] calldata facets) external;
```

### updatePlanTemplate


```solidity
function updatePlanTemplate(string calldata planType, address[] calldata facets) external;
```

### removePlanTemplate


```solidity
function removePlanTemplate(string calldata planType) external;
```

## Events
### CompensationPlanCreated

```solidity
event CompensationPlanCreated(address indexed plan, address indexed entity, string planType, string name);
```

### PlanTemplateAdded

```solidity
event PlanTemplateAdded(string planType, address[] facets);
```

### PlanTemplateUpdated

```solidity
event PlanTemplateUpdated(string planType, address[] facets);
```

### PlanTemplateRemoved

```solidity
event PlanTemplateRemoved(string planType);
```

## Errors
### InvalidPlanType

```solidity
error InvalidPlanType();
```

### PlanTypeExists

```solidity
error PlanTypeExists();
```

### UnauthorizedDeployment

```solidity
error UnauthorizedDeployment();
```

### InvalidConfiguration

```solidity
error InvalidConfiguration();
```

