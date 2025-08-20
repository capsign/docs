# AttestationRegistry
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Attestations/registry/AttestationRegistry.sol)

**Inherits:**
[Diamond](/src/Diamonds/Diamond.sol/contract.Diamond.md), [AccessManaged](/src/Authorization/AccessManaged.sol/abstract.AccessManaged.md)

Lightweight diamond contract for AttestationRegistry

*Much smaller than the original AttestationRegistry*


## Functions
### constructor

*Constructor*


```solidity
constructor(Diamond.InitParams memory initParams, address accessManager)
    Diamond(initParams)
    AccessManaged(accessManager);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`initParams`|`Diamond.InitParams`|Diamond initialization parameters|
|`accessManager`|`address`|Address of the access manager (GlobalAccessManager or IdentityAccessManager)|


### receive

*Allow diamond to receive ETH for attestation payments*


```solidity
receive() external payable;
```

