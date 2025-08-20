# GovernanceFactoryStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/factory/storage/GovernanceFactoryStorage.sol)

Storage library for governance factory specific data

*Uses diamond storage pattern for upgradeable contracts*


## State Variables
### STORAGE_SLOT

```solidity
bytes32 internal constant STORAGE_SLOT = keccak256("capsign.storage.governance_factory");
```


## Functions
### layout


```solidity
function layout() internal pure returns (Layout storage l);
```

### setFeeRecipient


```solidity
function setFeeRecipient(address feeRecipient) internal;
```

### getFeeRecipient


```solidity
function getFeeRecipient() internal view returns (address);
```

### incrementGovernanceCount


```solidity
function incrementGovernanceCount(address user, string memory governanceType) internal;
```

### decrementGovernanceCount


```solidity
function decrementGovernanceCount(address user, string memory governanceType) internal;
```

### getGovernanceCount


```solidity
function getGovernanceCount(address user, string memory governanceType) internal view returns (uint256);
```

### getTotalGovernanceCount


```solidity
function getTotalGovernanceCount(address user) internal view returns (uint256);
```

### registerGovernance


```solidity
function registerGovernance(address governanceInstance, string memory governanceType, address owner) internal;
```

### unregisterGovernance


```solidity
function unregisterGovernance(address governanceInstance) internal;
```

### isGovernance


```solidity
function isGovernance(address governanceInstance) internal view returns (bool);
```

### getGovernanceType


```solidity
function getGovernanceType(address governanceInstance) internal view returns (string memory);
```

### getGovernanceOwner


```solidity
function getGovernanceOwner(address governanceInstance) internal view returns (address);
```

## Structs
### Layout

```solidity
struct Layout {
    address feeRecipient;
    mapping(address => mapping(string => uint256)) userGovernanceCount;
    mapping(address => uint256) totalGovernanceCount;
    mapping(address => bool) isGovernanceInstance;
    mapping(address => string) governanceType;
    mapping(address => address) governanceOwner;
    mapping(string => bytes) governanceBytecode;
    address[] allGovernances;
    uint256 deploymentCount;
    uint256[40] __gap;
}
```

