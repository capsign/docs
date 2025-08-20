# IMultiOwnableWallet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Attestations/registry/facets/WebAuthnProxyFacet.sol)

*Interface for smart accounts that support WebAuthn signatures
Based on the MultiOwnable pattern from Wallet.sol*


## Functions
### ownerAtIndex

*Get owner at specific index (from MultiOwnable pattern)*


```solidity
function ownerAtIndex(uint256 index) external view returns (bytes memory ownerBytes);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`index`|`uint256`|The owner index|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`ownerBytes`|`bytes`|The owner data (32 bytes for address, 64 bytes for WebAuthn pubkey)|


### nextOwnerIndex

*Get next owner index (from MultiOwnable pattern)*


```solidity
function nextOwnerIndex() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The next available owner index|


