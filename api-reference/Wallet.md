# Wallet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Wallets/wallet/Wallet.sol)

**Inherits:**
[Diamond](/src/Diamonds/Diamond.sol/contract.Diamond.md)

Provides a flexible, upgradeable smart wallet with pluggable facets

*Diamond implementation for modular smart wallet functionality*


## Functions
### constructor

The actual wallet initialization (owners, type, etc.) is handled by CoreWalletFacet.initialize

*Constructor for Wallet - uses standard Diamond constructor*


```solidity
constructor(Diamond.InitParams memory initParams) Diamond(initParams);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`initParams`|`Diamond.InitParams`|Diamond initialization parameters (facets and initialization)|


## Events
### WalletCreated

```solidity
event WalletCreated(address indexed wallet, address indexed creator);
```

