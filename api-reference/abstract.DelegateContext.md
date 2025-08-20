# DelegateContext
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/utils/DelegateContext.sol)

*In a delegate call, address(this) will return diamond's address.*


## State Variables
### _self
*Stores the contract's address at the moment of deployment.
Useful for detecting if a contract is being delegated to.*


```solidity
address private immutable _self = address(this);
```


## Functions
### onlyDelegateCall


```solidity
modifier onlyDelegateCall();
```

### noDelegateCall


```solidity
modifier noDelegateCall();
```

## Errors
### DelegateNotAllowed

```solidity
error DelegateNotAllowed();
```

### OnlyDelegate

```solidity
error OnlyDelegate();
```

