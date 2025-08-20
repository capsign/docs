# CoreWalletFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Wallets/wallet/facets/CoreWalletFacet.sol)

**Inherits:**
IAccount, [MultiOwnable](/src/Wallets/wallet/MultiOwnable.sol/contract.MultiOwnable.md), [ERC1271](/src/Wallets/wallet/ERC1271.sol/abstract.ERC1271.md), Receiver

Inherits from MultiOwnable for standardized owner management and ERC1271 for signature validation

*Core wallet functionality including ERC-4337 compatibility and basic operations*


## State Variables
### REPLAYABLE_NONCE_KEY

```solidity
uint256 public constant REPLAYABLE_NONCE_KEY = 0xCA95;
```


## Functions
### onlyEntryPoint


```solidity
modifier onlyEntryPoint();
```

### notInEmergencyMode


```solidity
modifier notInEmergencyMode();
```

### initialize

*Initialize the wallet with owners and configuration*


```solidity
function initialize(bytes[] calldata owners, string calldata walletType, uint256 requiredApprovals) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owners`|`bytes[]`|Array of owner data (addresses or WebAuthn pubkeys)|
|`walletType`|`string`|Type of wallet being created (string-based)|
|`requiredApprovals`|`uint256`|Number of approvals required for transactions|


### execute

*Execute a transaction from the wallet*


```solidity
function execute(address target, uint256 value, bytes calldata data)
    external
    onlyOwner
    notInEmergencyMode
    returns (bool success, bytes memory result);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`target`|`address`|Target contract address|
|`value`|`uint256`|ETH value to send|
|`data`|`bytes`|Transaction data|


### executeBatch

*Execute multiple transactions in batch*


```solidity
function executeBatch(address[] calldata targets, uint256[] calldata values, bytes[] calldata datas)
    external
    onlyOwner
    notInEmergencyMode;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`targets`|`address[]`|Array of target addresses|
|`values`|`uint256[]`|Array of ETH values|
|`datas`|`bytes[]`|Array of transaction data|


### validateUserOp

*ERC-4337 user operation validation*


```solidity
function validateUserOp(PackedUserOperation calldata userOp, bytes32 userOpHash, uint256 missingAccountFunds)
    external
    override
    onlyEntryPoint
    returns (uint256 validationData);
```

### getWalletInfo

*Get wallet information*


```solidity
function getWalletInfo()
    external
    view
    returns (string memory walletType, uint256 ownerCount, uint256 requiredApprovals, bool emergencyMode);
```

### entryPoint

*Get EntryPoint address*


```solidity
function entryPoint() public view returns (address);
```

### activateEmergencyMode

*Activate emergency mode (can be called by any owner)*


```solidity
function activateEmergencyMode() external onlyOwner;
```

### deactivateEmergencyMode

*Deactivate emergency mode (requires multiple owners)*


```solidity
function deactivateEmergencyMode() external onlyOwner;
```

### _executeTransaction

*Internal function to execute a transaction*


```solidity
function _executeTransaction(address target, uint256 value, bytes memory data)
    internal
    returns (bool success, bytes memory result);
```

### _domainNameAndVersion

*ERC1271 domain name and version*


```solidity
function _domainNameAndVersion() internal pure override returns (string memory name, string memory version);
```

### _isValidSignature

*ERC1271 signature validation implementation*


```solidity
function _isValidSignature(bytes32 hash, bytes calldata signature) internal view override returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`hash`|`bytes32`|The hash that was signed|
|`signature`|`bytes`|The signature to validate|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether the signature is valid|


### executeWithoutChainIdValidation

Only callable by EntryPoint, requires REPLAYABLE_NONCE_KEY

*Execute calls without chain ID validation for cross-chain compatibility*


```solidity
function executeWithoutChainIdValidation(bytes[] calldata calls) external payable onlyEntryPoint;
```

### getUserOpHashWithoutChainId

*Get UserOp hash without chain ID for cross-chain operations*


```solidity
function getUserOpHashWithoutChainId(PackedUserOperation calldata userOp) public view returns (bytes32);
```

### canSkipChainIdValidation

*Check if a function selector can skip chain ID validation*


```solidity
function canSkipChainIdValidation(bytes4 functionSelector) public pure returns (bool);
```

### _call

*Internal function to execute a call*


```solidity
function _call(address target, uint256 value, bytes memory data) internal;
```

### receive

*Receive ETH*


```solidity
receive() external payable override;
```

## Events
### WalletInitialized

```solidity
event WalletInitialized(address indexed wallet, string walletType);
```

### TransactionExecuted

```solidity
event TransactionExecuted(address indexed target, uint256 value, bytes data, bool success);
```

### EmergencyModeActivated

```solidity
event EmergencyModeActivated(address indexed activator);
```

### EmergencyModeDeactivated

```solidity
event EmergencyModeDeactivated(address indexed deactivator);
```

## Errors
### AlreadyInitialized

```solidity
error AlreadyInitialized();
```

### EmergencyModeActive

```solidity
error EmergencyModeActive();
```

### InsufficientApprovals

```solidity
error InsufficientApprovals();
```

### SelectorNotAllowed

```solidity
error SelectorNotAllowed(bytes4 selector);
```

### InvalidNonceKey

```solidity
error InvalidNonceKey(uint256 key);
```

## Structs
### SignatureWrapper
A wrapper struct used for signature validation so that callers
can identify the owner that signed.


```solidity
struct SignatureWrapper {
    uint256 ownerIndex;
    bytes signatureData;
}
```

