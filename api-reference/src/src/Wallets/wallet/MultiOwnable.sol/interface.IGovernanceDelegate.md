# IGovernanceDelegate
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Wallets/wallet/MultiOwnable.sol)

Interface for governance contract delegation


## Functions
### canExecuteFor


```solidity
function canExecuteFor(address wallet, bytes4 selector) external view returns (bool);
```

### executeOnWallet


```solidity
function executeOnWallet(address wallet, bytes calldata data) external returns (bytes memory);
```

