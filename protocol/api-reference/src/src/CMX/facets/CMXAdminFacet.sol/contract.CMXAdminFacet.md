# CMXAdminFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/CMX/facets/CMXAdminFacet.sol)

Administrative functionality for CMX diamond

*Handles initialization, minting, and admin functions*


## Functions
### onlyOwner


```solidity
modifier onlyOwner();
```

### onlyMinter


```solidity
modifier onlyMinter();
```

### onlyInitialized


```solidity
modifier onlyInitialized();
```

### onlySourceChain


```solidity
modifier onlySourceChain();
```

### initialize

Initialize the CMX diamond


```solidity
function initialize(
    string memory _name,
    string memory _symbol,
    uint8 _decimals,
    address _lzEndpoint,
    address _owner,
    bool _isSourceChain,
    uint256 _initialSupply
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_name`|`string`|Token name|
|`_symbol`|`string`|Token symbol|
|`_decimals`|`uint8`|Token decimals|
|`_lzEndpoint`|`address`|LayerZero endpoint address|
|`_owner`|`address`|Initial owner|
|`_isSourceChain`|`bool`|Whether this chain is a source chain|
|`_initialSupply`|`uint256`|Initial supply to mint (only on source chains)|


### owner

Returns the current owner


```solidity
function owner() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The owner address|


### transferOwnership

Transfers ownership of the contract to a new account


```solidity
function transferOwnership(address newOwner) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newOwner`|`address`|The new owner address|


### renounceOwnership

Renounces ownership of the contract

*Leaves the contract without an owner, disabling owner-only functions*


```solidity
function renounceOwnership() external onlyOwner;
```

### setSourceChain

Update source chain status

*Can only be called before initialization is complete*


```solidity
function setSourceChain(bool _isSourceChain) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_isSourceChain`|`bool`|New source chain status|


### isSourceChain

Check if this is a source chain


```solidity
function isSourceChain() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if this is a source chain|


### mint

Mint tokens to a specific address

*Only callable by authorized minters on source chain*


```solidity
function mint(address to, uint256 amount) external onlyInitialized onlySourceChain onlyMinter;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|The address to mint tokens to|
|`amount`|`uint256`|The amount of tokens to mint|


### getMaxSupply

Get the maximum supply


```solidity
function getMaxSupply() external pure returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The maximum supply|


### setMinter

Set minter authorization for an address


```solidity
function setMinter(address minter, bool authorized) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`minter`|`address`|The address to set minter status for|
|`authorized`|`bool`|Whether the address should be authorized to mint|


### isMinter

Check if an address is an authorized minter


```solidity
function isMinter(address account) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the address is an authorized minter|


### setPaused

Set pause status for minting and transfers


```solidity
function setPaused(bool _mintingPaused, bool _transfersPaused) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_mintingPaused`|`bool`|Whether minting should be paused|
|`_transfersPaused`|`bool`|Whether transfers should be paused|


### mintingPaused

Check if minting is paused


```solidity
function mintingPaused() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if minting is paused|


### transfersPaused

Check if transfers are paused


```solidity
function transfersPaused() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if transfers are paused|


### initialized

Check if the contract has been initialized


```solidity
function initialized() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


### lzEndpoint

Get the LayerZero endpoint address


```solidity
function lzEndpoint() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The LayerZero endpoint address|


### setTransferStrategy

Set the transfer strategy for ERC-20 transfers

*Only owner can change the transfer strategy*


```solidity
function setTransferStrategy(CMXStorage.TransferStrategy strategy) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`strategy`|`CMXStorage.TransferStrategy`|The new transfer strategy to use|


### getTransferStrategy

Get the current transfer strategy


```solidity
function getTransferStrategy() external view returns (CMXStorage.TransferStrategy);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`CMXStorage.TransferStrategy`|The current transfer strategy|


## Events
### Initialized

```solidity
event Initialized(bool isSourceChain, uint256 initialSupply);
```

### SourceChainUpdated

```solidity
event SourceChainUpdated(bool isSourceChain);
```

### MinterUpdated

```solidity
event MinterUpdated(address indexed minter, bool authorized);
```

### PauseUpdated

```solidity
event PauseUpdated(bool mintingPaused, bool transfersPaused);
```

### OwnershipTransferred

```solidity
event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
```

### TransferStrategyChanged

```solidity
event TransferStrategyChanged(
    CMXStorage.TransferStrategy indexed oldStrategy, CMXStorage.TransferStrategy indexed newStrategy
);
```

### Transfer

```solidity
event Transfer(address indexed from, address indexed to, uint256 value);
```

## Errors
### AlreadyInitialized

```solidity
error AlreadyInitialized();
```

### NotInitialized

```solidity
error NotInitialized();
```

### OnlyOwner

```solidity
error OnlyOwner();
```

### OnlySourceChain

```solidity
error OnlySourceChain();
```

### OnlyMinter

```solidity
error OnlyMinter();
```

### ExceedsMaxSupply

```solidity
error ExceedsMaxSupply();
```

### ZeroAddress

```solidity
error ZeroAddress();
```

