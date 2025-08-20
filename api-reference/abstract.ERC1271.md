# ERC1271
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Wallets/wallet/ERC1271.sol)

**Author:**
Adapted from Solady (https://github.com/vectorized/solady/blob/main/src/accounts/ERC1271.sol)

Abstract ERC-1271 implementation with guards to handle the same
signer being used on multiple accounts.

*To prevent the same signature from being validated on different accounts owned by the samer signer,
we introduce an anti cross-account-replay layer: the original hash is input into a new EIP-712 compliant
hash. The domain separator of this outer hash contains the chain id and address of this contract, so that
it cannot be used on two accounts (see `replaySafeHash()` for the implementation details).*


## State Variables
### _WALLET_MESSAGE_TYPEHASH
*Precomputed `typeHash` used to produce EIP-712 compliant hash when applying the anti
cross-account-replay layer.*


```solidity
bytes32 private constant _WALLET_MESSAGE_TYPEHASH = keccak256("SmartWalletMessage(bytes32 hash)");
```


## Functions
### eip712Domain


```solidity
function eip712Domain()
    external
    view
    virtual
    returns (
        bytes1 fields,
        string memory name,
        string memory version,
        uint256 chainId,
        address verifyingContract,
        bytes32 salt,
        uint256[] memory extensions
    );
```

### isValidSignature


```solidity
function isValidSignature(bytes32 hash, bytes calldata signature) public view virtual returns (bytes4 result);
```

### replaySafeHash


```solidity
function replaySafeHash(bytes32 hash) public view virtual returns (bytes32);
```

### domainSeparator


```solidity
function domainSeparator() public view virtual returns (bytes32);
```

### _eip712Hash


```solidity
function _eip712Hash(bytes32 hash) internal view virtual returns (bytes32);
```

### _hashStruct


```solidity
function _hashStruct(bytes32 hash) internal view virtual returns (bytes32);
```

### _domainNameAndVersion


```solidity
function _domainNameAndVersion() internal view virtual returns (string memory name, string memory version);
```

### _isValidSignature


```solidity
function _isValidSignature(bytes32 hash, bytes calldata signature) internal view virtual returns (bool);
```

