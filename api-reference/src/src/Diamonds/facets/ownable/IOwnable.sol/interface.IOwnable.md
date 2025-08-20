# IOwnable
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/facets/ownable/IOwnable.sol)

**Inherits:**
[IOwnableBase](/src/Diamonds/facets/ownable/IOwnable.sol/interface.IOwnableBase.md)

Interface of the ERC173 contract. See [EIP-173](https://eips.ethereum.org/EIPS/eip-173).


## Functions
### owner

Returns the owner of the contract.


```solidity
function owner() external view returns (address);
```

### transferOwnership

Transfers the ownership of the contract to a new account (`newOwner`).

*OwnershipTransferred event is emitted here.*


```solidity
function transferOwnership(address newOwner) external;
```

