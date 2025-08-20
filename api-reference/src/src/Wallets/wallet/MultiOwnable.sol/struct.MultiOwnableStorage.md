# MultiOwnableStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Wallets/wallet/MultiOwnable.sol)

Storage layout used by this contract.

**Note:**
storage-location: erc7201:capsign.storage.MultiOwnable // Renamed slot


```solidity
struct MultiOwnableStorage {
    uint256 nextOwnerIndex;
    uint256 removedOwnersCount;
    mapping(uint256 index => bytes owner) ownerAtIndex;
    mapping(bytes bytes_ => bool isOwner_) isOwner;
    mapping(address smartContractOwner => bool isGovernanceDelegate) governanceDelegates;
}
```

