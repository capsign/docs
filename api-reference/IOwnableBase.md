# IOwnableBase
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/facets/ownable/IOwnable.sol)


## Events
### OwnershipTransferred
Emitted when the ownership of the contract is transferred.


```solidity
event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`previousOwner`|`address`|The previous owner of the contract.|
|`newOwner`|`address`|The new owner of the contract.|

## Errors
### Ownable_ZeroAddress
Thrown when setting the owner to the zero address.


```solidity
error Ownable_ZeroAddress();
```

### Ownable_CallerIsNotOwner
Thrown when a caller is not the owner.


```solidity
error Ownable_CallerIsNotOwner();
```

