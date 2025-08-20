# GlobalAccessManagerStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Authorization/gam/GlobalAccessManagerStorage.sol)

Contains all storage variables for the global access manager

*Storage contract for GlobalAccessManager diamond*


## State Variables
### STORAGE_SLOT

```solidity
bytes32 internal constant STORAGE_SLOT = keccak256("capsign.storage.gam");
```


## Functions
### layout


```solidity
function layout() internal pure returns (Layout storage l);
```

## Structs
### Layout

```solidity
struct Layout {
    mapping(bytes4 => bool) emergencyFunctions;
    mapping(address => uint256) emergencyActionCount;
    uint256 proposalThreshold;
    uint256 votingDelay;
    uint256 votingPeriod;
    uint256 executionDelay;
    mapping(string => address) protocolContracts;
    mapping(address => bool) authorizedFactories;
    address subscriptionManager;
    mapping(address => uint8) entitySubscriptionTiers;
    bool emergencyMode;
    uint256 emergencyModeActivatedAt;
    uint256 totalEntities;
    uint256 totalFactories;
    mapping(address => uint256) roleGrantCount;
}
```

