# CompensationFactory
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Compensation/factory/CompensationFactory.sol)

**Inherits:**
[FactoryBase](/src/Diamonds/factory/FactoryBase.sol/abstract.FactoryBase.md), [ICompensationFactory](/src/Compensation/factory/interfaces/ICompensationFactory.sol/interface.ICompensationFactory.md)

Factory for deploying compensation plan diamonds


## State Variables
### planTypes

```solidity
mapping(address => string) public planTypes;
```


### entityPlans

```solidity
mapping(address => address[]) public entityPlans;
```


### planTemplates

```solidity
mapping(string => address[]) public planTemplates;
```


## Functions
### constructor


```solidity
constructor(address _facetRegistry, address _authority) FactoryBase(_facetRegistry, _authority);
```

### deployRule701Plan

Deploy a Rule 701 compensation plan


```solidity
function deployRule701Plan(string calldata name, address entity, uint256 annualLimit, bytes calldata additionalConfig)
    external
    returns (address);
```

### deployCompensationPlan

Deploy a generic compensation plan


```solidity
function deployCompensationPlan(string calldata planType, string calldata name, address entity, bytes calldata config)
    external
    returns (address);
```

### addPlanTemplate

Add a new plan template


```solidity
function addPlanTemplate(string calldata planType, address[] calldata facets) external restricted;
```

### updatePlanTemplate

Update an existing plan template


```solidity
function updatePlanTemplate(string calldata planType, address[] calldata facets) external restricted;
```

### removePlanTemplate

Remove a plan template


```solidity
function removePlanTemplate(string calldata planType) external restricted;
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

### deployDiamond

Deploy a diamond with custom facet cuts (required by IFactoryBase)


```solidity
function deployDiamond(IDiamond.FacetCut[] memory facetCuts, bytes memory constructorArgs, bytes32 salt)
    external
    override
    returns (address diamond);
```

### _deployCompensationDiamond


```solidity
function _deployCompensationDiamond(
    string memory planType,
    string memory name,
    address entity,
    address[] memory facets,
    bytes memory initData
) internal returns (address);
```

### _buildFacetCuts


```solidity
function _buildFacetCuts(address[] memory facets) internal pure returns (IDiamond.FacetCut[] memory);
```

