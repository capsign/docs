# IAttestationCoreFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Attestations/registry/facets/WebAuthnProxyFacet.sol)

*Interface for AttestationCoreFacet to access shared functions*


## Functions
### decodeSignatureWrapper


```solidity
function decodeSignatureWrapper(bytes calldata signature)
    external
    pure
    returns (uint8 sigType, bytes memory signatureData, bytes memory ownerData);
```

