# OwnableFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/facets/ownable/OwnableFacet.sol)

**Inherits:**
[IOwnable](/src/Diamonds/facets/ownable/IOwnable.sol/interface.IOwnable.md), [OwnableBase](/src/Diamonds/facets/ownable/OwnableBase.sol/abstract.OwnableBase.md), [Facet](/src/Diamonds/facets/Facet.sol/abstract.Facet.md)


## Functions
### Ownable_init


```solidity
function Ownable_init(address owner_) external onlyInitializing;
```

### owner

Returns the owner of the contract.


```solidity
function owner() external view returns (address);
```

### transferOwnership

Transfers the ownership of the contract to a new account (`newOwner`).

*OwnershipTransferred event is emitted here.*


```solidity
function transferOwnership(address newOwner) external onlyOwner;
```

### renounceOwnership

Renounces ownership of the contract

*Leaves the contract without an owner, disabling any functionality that is only available to the owner*


```solidity
function renounceOwnership() external onlyOwner;
```

