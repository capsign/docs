# ILedgerFactory
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Ledgers/factory/interfaces/ILedgerFactory.sol)

**Inherits:**
[IFactory](/src/Diamonds/factory/IFactory.sol/interface.IFactory.md)

Interface for LedgerFactory with unified access control and factory base functionality

*Combines FactoryBase functionality with ledger-specific functions and unified access control*


## Functions
### createLedger


```solidity
function createLedger(address entity, string memory name, bytes memory initData) external returns (address);
```

### createLedger


```solidity
function createLedger() external returns (address);
```

### createLedger


```solidity
function createLedger(uint8 _decimals, string memory _currencyCode) external returns (address);
```

### getTotalLedgerCount


```solidity
function getTotalLedgerCount() external view returns (uint256);
```

### getLedgerByUser


```solidity
function getLedgerByUser(address user) external view returns (address);
```

### getUserLedger


```solidity
function getUserLedger() external view returns (address);
```

### setLedgerBytecode


```solidity
function setLedgerBytecode(bytes calldata bytecode) external;
```

### getLedgerBytecode


```solidity
function getLedgerBytecode() external view returns (bytes memory bytecode);
```

