# DocumentRegistry
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Documents/registry/DocumentRegistry.sol)

**Inherits:**
[Diamond](/src/Diamonds/Diamond.sol/contract.Diamond.md), [AccessManaged](/src/Authorization/AccessManaged.sol/abstract.AccessManaged.md)

Manages document registration, signing workflows, and template management

*Lightweight diamond implementation for document registry functionality*


## Functions
### constructor


```solidity
constructor(Diamond.InitParams memory _initParams, address _accessManager)
    Diamond(_initParams)
    AccessManaged(_accessManager);
```

### receive

*Allow diamond to receive ETH for document payments*


```solidity
receive() external payable;
```

## Events
### DocumentRegistryCreated

```solidity
event DocumentRegistryCreated(address indexed registry);
```

