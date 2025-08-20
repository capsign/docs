# GovernanceRegistryFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/factory/facets/GovernanceRegistryFacet.sol)

*Registry for tracking deployed governance instances*


## Functions
### getTokenholderGovernance

*Legacy functions removed - use new tiered governance system
No backwards compatibility maintained*


```solidity
function getTokenholderGovernance(address) external pure returns (address);
```

### getBoardGovernance


```solidity
function getBoardGovernance(address) external pure returns (address);
```

### getAllGovernanceInstances

*Get all governance instances*


```solidity
function getAllGovernanceInstances() external view returns (address[] memory);
```

### isGovernanceInstance

*Check if address is a governance instance*


```solidity
function isGovernanceInstance(address governance) external view returns (bool);
```

### getGovernanceType

*Get governance type*


```solidity
function getGovernanceType(address governance) external view returns (string memory);
```

### getGovernanceOwner

*Get governance owner*


```solidity
function getGovernanceOwner(address governance) external view returns (address);
```

### getLinkedGovernance

*Legacy linking removed - use new governance system*


```solidity
function getLinkedGovernance(address) external pure returns (address);
```

### getDeploymentStats

*Get deployment statistics*


```solidity
function getDeploymentStats()
    external
    view
    returns (uint256 totalDeployments, uint256 tokenholderCount, uint256 boardCount, uint256 integratedCount);
```

