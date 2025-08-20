# IFundFactory
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Funds/factory/interfaces/IFundFactory.sol)

**Inherits:**
[IFactory](/src/Diamonds/factory/IFactory.sol/interface.IFactory.md)

Interface for FundFactory with unified access control and factory base functionality

*Combines FactoryBase functionality with fund-specific functions and unified access control*


## Functions
### createFund


```solidity
function createFund(string memory name, string memory symbol, address entity, bytes memory initData)
    external
    returns (address);
```

