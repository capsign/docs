# MultiOwnable
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Wallets/wallet/MultiOwnable.sol)

Auth contract allowing multiple owners, each identified as bytes.
Supports EOA addresses, WebAuthn public keys, and smart contract owners.


## State Variables
### MULTI_OWNABLE_STORAGE_LOCATION
*Slot for the `MultiOwnableStorage` struct in storage.
Computed from
keccak256(abi.encode(uint256(keccak256("capsign.storage.MultiOwnable")) - 1)) & ~bytes32(uint256(0xff))
Follows ERC-7201 (see https://eips.ethereum.org/EIPS/eip-7201).*


```solidity
bytes32 private constant MULTI_OWNABLE_STORAGE_LOCATION =
    0x2433c1232f895d776590506d5dd701a63700303d9358e18771ba0e4c84d30200;
```


### EIP1271_MAGIC_VALUE

```solidity
bytes4 private constant EIP1271_MAGIC_VALUE = 0x1626ba7e;
```


## Functions
### onlyOwner


```solidity
modifier onlyOwner() virtual;
```

### addOwnerAddress


```solidity
function addOwnerAddress(address owner) external virtual onlyOwner;
```

### addOwnerPublicKey


```solidity
function addOwnerPublicKey(bytes32 x, bytes32 y) external virtual onlyOwner;
```

### addGovernanceOwner

Add a smart contract as an owner with governance delegation


```solidity
function addGovernanceOwner(address contractOwner) external virtual onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`contractOwner`|`address`|Address of the smart contract that will own this wallet|


### removeGovernanceOwner

Remove a governance owner


```solidity
function removeGovernanceOwner(uint256 index, bytes calldata owner) external virtual onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`index`|`uint256`|Index of the owner to remove|
|`owner`|`bytes`|Owner data to verify|


### removeOwnerAtIndex


```solidity
function removeOwnerAtIndex(uint256 index, bytes calldata owner) external virtual onlyOwner;
```

### removeLastOwner


```solidity
function removeLastOwner(uint256 index, bytes calldata owner) external virtual onlyOwner;
```

### isOwnerAddress


```solidity
function isOwnerAddress(address account) public view virtual returns (bool);
```

### isOwnerPublicKey


```solidity
function isOwnerPublicKey(bytes32 x, bytes32 y) public view virtual returns (bool);
```

### isOwnerBytes


```solidity
function isOwnerBytes(bytes memory account) public view virtual returns (bool);
```

### isGovernanceDelegate

Check if an address is a governance delegate owner


```solidity
function isGovernanceDelegate(address account) public view virtual returns (bool);
```

### ownerAtIndex


```solidity
function ownerAtIndex(uint256 index) public view virtual returns (bytes memory);
```

### nextOwnerIndex


```solidity
function nextOwnerIndex() public view virtual returns (uint256);
```

### ownerCount


```solidity
function ownerCount() public view virtual returns (uint256);
```

### removedOwnersCount


```solidity
function removedOwnersCount() public view virtual returns (uint256);
```

### _initializeOwners


```solidity
function _initializeOwners(bytes[] memory owners) internal virtual;
```

### _addOwnerAtIndex


```solidity
function _addOwnerAtIndex(bytes memory owner, uint256 index) internal virtual;
```

### _removeOwnerAtIndex


```solidity
function _removeOwnerAtIndex(uint256 index, bytes calldata owner) internal virtual;
```

### _checkOwner

Enhanced owner check supporting EOAs, self-calls, and smart contract governance


```solidity
function _checkOwner() internal view virtual;
```

### _isValidGovernanceOwner

Check if a smart contract is a valid governance owner and can execute


```solidity
function _isValidGovernanceOwner(address contractOwner) internal view returns (bool);
```

### _supportsGovernanceInterface

Verify a contract supports the governance delegation interface


```solidity
function _supportsGovernanceInterface(address contractAddr) internal view returns (bool);
```

### _getMultiOwnableStorage


```solidity
function _getMultiOwnableStorage() internal pure returns (MultiOwnableStorage storage $);
```

## Events
### AddOwner

```solidity
event AddOwner(uint256 indexed index, bytes owner);
```

### RemoveOwner

```solidity
event RemoveOwner(uint256 indexed index, bytes owner);
```

### GovernanceDelegateAdded

```solidity
event GovernanceDelegateAdded(address indexed delegate);
```

### GovernanceDelegateRemoved

```solidity
event GovernanceDelegateRemoved(address indexed delegate);
```

## Errors
### Unauthorized

```solidity
error Unauthorized();
```

### AlreadyOwner

```solidity
error AlreadyOwner(bytes owner);
```

### NoOwnerAtIndex

```solidity
error NoOwnerAtIndex(uint256 index);
```

### WrongOwnerAtIndex

```solidity
error WrongOwnerAtIndex(uint256 index, bytes expectedOwner, bytes actualOwner);
```

### InvalidOwnerBytesLength

```solidity
error InvalidOwnerBytesLength(bytes owner);
```

### InvalidEthereumAddressOwner

```solidity
error InvalidEthereumAddressOwner(bytes owner);
```

### LastOwner

```solidity
error LastOwner();
```

### NotLastOwner

```solidity
error NotLastOwner(uint256 ownersRemaining);
```

### InvalidGovernanceDelegate

```solidity
error InvalidGovernanceDelegate(address delegate);
```

