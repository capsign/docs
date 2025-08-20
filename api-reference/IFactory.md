# IFactory
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/factory/IFactory.sol)

**Inherits:**
[IFactoryBase](/src/Diamonds/factory/IFactoryBase.sol/interface.IFactoryBase.md), [IAccessControl](/src/Diamonds/facets/access/IAccessControl.sol/interface.IAccessControl.md)

Unified interface for all CapSign factory diamonds

*Combines IFactoryBase functionality with unified access control*

*All CapSign factory interfaces should inherit from this interface*


## Events
### FactoryInitialized

```solidity
event FactoryInitialized(address indexed factory, string factoryType);
```

### FactoryConfigurationUpdated

```solidity
event FactoryConfigurationUpdated(address indexed factory, string configType);
```

