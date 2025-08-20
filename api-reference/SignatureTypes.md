# SignatureTypes
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Attestations/registry/types/SignatureTypes.sol)

*Shared signature types for attestation facets*


## Structs
### SignatureWrapper
*Signature wrapper for flexible signature types*


```solidity
struct SignatureWrapper {
    uint8 signatureType;
    bytes signatureData;
    bytes ownerData;
}
```

