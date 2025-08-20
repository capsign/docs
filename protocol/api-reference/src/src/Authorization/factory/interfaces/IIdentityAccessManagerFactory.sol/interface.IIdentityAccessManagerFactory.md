# IIdentityAccessManagerFactory
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Authorization/factory/interfaces/IIdentityAccessManagerFactory.sol)

Interface for IdentityAccessManagerFactory


## Functions
### pause


```solidity
function pause() external;
```

### unpause


```solidity
function unpause() external;
```

### paused


```solidity
function paused() external view returns (bool);
```

### createAccessManager


```solidity
function createAccessManager(address entity, string calldata entityName) external returns (address);
```

### getAccessManager


```solidity
function getAccessManager(address entity) external view returns (address);
```

### getEntity


```solidity
function getEntity(address accessManager) external view returns (address);
```

### hasAccessManager


```solidity
function hasAccessManager(address entity) external view returns (bool);
```

### hasRole


```solidity
function hasRole(bytes32 role, address account) external view returns (bool);
```

### grantRole


```solidity
function grantRole(bytes32 role, address account) external;
```

### revokeRole


```solidity
function revokeRole(bytes32 role, address account) external;
```

