# GovernanceDeploymentFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/factory/facets/GovernanceDeploymentFacet.sol)

Integrates with the modern factory pattern and fee collection system

*Handles deployment of governance instances using CREATE2 pattern with tiered configuration*


## Functions
### deployTokenholderGovernance

*Deploy a tokenholder governance instance using CREATE2*


```solidity
function deployTokenholderGovernance(
    string memory name,
    uint256 votingDelay,
    uint256 votingPeriod,
    uint256 proposalThreshold,
    uint256 quorumNumerator
) external payable returns (address governance);
```

### deployBoardGovernance

*Deploy a board governance instance using CREATE2*


```solidity
function deployBoardGovernance(string memory name, uint256 boardVotingPeriod, uint256 boardQuorum)
    external
    payable
    returns (address governance);
```

### deployIntegratedGovernance

*Deploy integrated governance (both tokenholder and board) using CREATE2*


```solidity
function deployIntegratedGovernance(
    string memory name,
    uint256 votingDelay,
    uint256 votingPeriod,
    uint256 proposalThreshold,
    uint256 quorumNumerator,
    uint256 boardVotingPeriod,
    uint256 boardQuorum
) external payable returns (address tokenholderGovernance, address boardGovernance);
```

### _deployTokenholderGovernanceInternal

*Internal function to deploy tokenholder governance without fee collection*


```solidity
function _deployTokenholderGovernanceInternal(
    string memory name,
    uint256 votingDelay,
    uint256 votingPeriod,
    uint256 proposalThreshold,
    uint256 quorumNumerator
) internal returns (address governance);
```

### _deployBoardGovernanceInternal

*Internal function to deploy board governance without fee collection*


```solidity
function _deployBoardGovernanceInternal(string memory name, uint256 boardVotingPeriod, uint256 boardQuorum)
    internal
    returns (address governance);
```

### _validateGovernanceCreation

*Validate that a user can create a specific governance type*


```solidity
function _validateGovernanceCreation(address user, string memory governanceType) internal view;
```

### _checkAndCollectFee

*Check and collect deployment fee*


```solidity
function _checkAndCollectFee(address user, string memory governanceType) internal returns (uint256 fee);
```

### _registerGovernance

*Register governance in the new tiered system*


```solidity
function _registerGovernance(address governanceInstance, string memory governanceType, address owner) internal;
```

### setGovernanceBytecode

*Set governance bytecode for deployment (admin only)*


```solidity
function setGovernanceBytecode(string memory templateName, bytes memory bytecode) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`templateName`|`string`|The name of the template (e.g., "TokenholderGovernance")|
|`bytecode`|`bytes`|The creation bytecode of the contract|


### getGovernanceBytecode

*Get governance bytecode*


```solidity
function getGovernanceBytecode(string memory templateName) external view returns (bytes memory);
```

### predictGovernanceAddress

*Predict the address of a contract deployed with CREATE2*


```solidity
function predictGovernanceAddress(string memory templateName, bytes memory constructorArgs, bytes32 salt)
    external
    view
    returns (address);
```

### getDeploymentCost

*Get deployment cost for a governance type*


```solidity
function getDeploymentCost(string memory governanceType) external pure returns (uint256);
```

### _getDeploymentCost

*Internal function to get deployment cost*


```solidity
function _getDeploymentCost(string memory governanceType) internal pure returns (uint256);
```

### _requireAdmin


```solidity
function _requireAdmin() internal view;
```

### _getAuthority

*Get the authority (access manager) for this facet*


```solidity
function _getAuthority() internal view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The address of the access manager|


## Events
### TokenholderGovernanceDeployed

```solidity
event TokenholderGovernanceDeployed(
    address indexed governance,
    address indexed owner,
    string name,
    uint256 votingDelay,
    uint256 votingPeriod,
    uint256 proposalThreshold,
    uint256 quorumNumerator,
    uint256 fee
);
```

### BoardGovernanceDeployed

```solidity
event BoardGovernanceDeployed(
    address indexed governance,
    address indexed owner,
    string name,
    uint256 boardVotingPeriod,
    uint256 boardQuorum,
    uint256 fee
);
```

### IntegratedGovernanceDeployed

```solidity
event IntegratedGovernanceDeployed(
    address indexed tokenholderGovernance,
    address indexed boardGovernance,
    address indexed owner,
    string name,
    uint256 fee
);
```

### FeeCollected

```solidity
event FeeCollected(address indexed payer, uint256 amount, address indexed recipient);
```

## Errors
### BytecodeNotSet

```solidity
error BytecodeNotSet();
```

### DeploymentFailed

```solidity
error DeploymentFailed();
```

### GovernanceTypeNotAvailable

```solidity
error GovernanceTypeNotAvailable(string governanceType, uint8 requiredTier);
```

### GovernanceLimitExceeded

```solidity
error GovernanceLimitExceeded(uint256 current, uint256 limit);
```

### InsufficientFee

```solidity
error InsufficientFee(uint256 required, uint256 provided);
```

