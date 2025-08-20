# Facet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/facets/Facet.sol)

**Inherits:**
Initializable, ReentrancyGuardUpgradeable, [DelegateContext](/src/Diamonds/utils/DelegateContext.sol/abstract.DelegateContext.md), [DiamondLoupeBase](/src/Diamonds/facets/loupe/DiamondLoupeBase.sol/abstract.DiamondLoupeBase.md)


## Functions
### constructor


```solidity
constructor();
```

### protected

*Reverts if the caller is not the owner.*


```solidity
modifier protected();
```

### onlyDiamondOwner


```solidity
modifier onlyDiamondOwner();
```

## Errors
### CallerIsNotOwner

```solidity
error CallerIsNotOwner();
```

### CallerIsNotAuthorized

```solidity
error CallerIsNotAuthorized();
```

