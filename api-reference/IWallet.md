# IWallet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Wallets/wallet/interfaces/IWallet.sol)

**Inherits:**
IAccount, IERC721Receiver, IERC1155Receiver, IERC1271

Combines all wallet functionality including ERC-4337, multi-ownership, token receiving,
ledger integration, and delegation capabilities

*Interface for the modular wallet diamond implementation*


## Functions
### initialize

*Initialize the wallet*


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
    returns (bool success, bytes memory result);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`target`|`address`|Target contract address|
|`value`|`uint256`|ETH value to send|
|`data`|`bytes`|Transaction data|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`success`|`bool`|Whether the transaction succeeded|
|`result`|`bytes`|Return data from the transaction|


### executeBatch

*Execute multiple transactions in batch*


```solidity
function executeBatch(address[] calldata targets, uint256[] calldata values, bytes[] calldata datas) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`targets`|`address[]`|Array of target addresses|
|`values`|`uint256[]`|Array of ETH values|
|`datas`|`bytes[]`|Array of transaction data|


### getWalletInfo

*Get wallet information*


```solidity
function getWalletInfo()
    external
    view
    returns (
        string memory walletType,
        string memory name,
        uint256 ownerCount,
        uint256 requiredApprovals,
        bool emergencyMode
    );
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`walletType`|`string`|Type of wallet|
|`name`|`string`|Wallet name|
|`ownerCount`|`uint256`|Number of owners|
|`requiredApprovals`|`uint256`|Required approvals|
|`emergencyMode`|`bool`|Whether emergency mode is active|


### entryPoint

*Get EntryPoint address for ERC-4337*


```solidity
function entryPoint() external view returns (address);
```

### activateEmergencyMode

*Activate emergency mode*


```solidity
function activateEmergencyMode() external;
```

### deactivateEmergencyMode

*Deactivate emergency mode*


```solidity
function deactivateEmergencyMode() external;
```

### addOwnerAddress

*Add an owner by address*


```solidity
function addOwnerAddress(address owner) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|Address of the new owner|


### addOwnerPublicKey

*Add an owner by WebAuthn public key*


```solidity
function addOwnerPublicKey(bytes32 x, bytes32 y) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`x`|`bytes32`|X coordinate of the public key|
|`y`|`bytes32`|Y coordinate of the public key|


### addGovernanceOwner

*Add a smart contract as an owner with governance delegation*


```solidity
function addGovernanceOwner(address contractOwner) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`contractOwner`|`address`|Address of the smart contract that will own this wallet|


### removeGovernanceOwner

*Remove a governance owner*


```solidity
function removeGovernanceOwner(uint256 index, bytes calldata owner) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`index`|`uint256`|Index of the owner to remove|
|`owner`|`bytes`|Owner data to verify|


### removeOwnerAtIndex

*Remove an owner at a specific index*


```solidity
function removeOwnerAtIndex(uint256 index, bytes calldata owner) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`index`|`uint256`|Index of the owner to remove|
|`owner`|`bytes`|Owner data to verify|


### removeLastOwner

*Remove the last owner (special case)*


```solidity
function removeLastOwner(uint256 index, bytes calldata owner) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`index`|`uint256`|Index of the last owner|
|`owner`|`bytes`|Owner data to verify|


### isOwnerAddress

*Check if an address is an owner*


```solidity
function isOwnerAddress(address account) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|Address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether the address is an owner|


### isOwnerPublicKey

*Check if a WebAuthn public key is an owner*


```solidity
function isOwnerPublicKey(bytes32 x, bytes32 y) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`x`|`bytes32`|X coordinate of the public key|
|`y`|`bytes32`|Y coordinate of the public key|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether the public key is an owner|


### isOwnerBytes

*Check if owner data represents an owner*


```solidity
function isOwnerBytes(bytes memory account) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`bytes`|Owner data to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether the data represents an owner|


### isGovernanceDelegate

*Check if an address is a governance delegate owner*


```solidity
function isGovernanceDelegate(address account) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|Address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether the address is a governance delegate|


### setGovernanceContract

*Set the governance contract for this wallet*


```solidity
function setGovernanceContract(address governanceContract) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`governanceContract`|`address`|Address of the governance contract (0x0 to disable)|


### getGovernanceContract

*Get the governance contract for this wallet*


```solidity
function getGovernanceContract() external view returns (address governanceContract);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`governanceContract`|`address`|Address of the governance contract|


### requiresGovernanceApproval

*Check if an operation requires governance approval*


```solidity
function requiresGovernanceApproval(string calldata operation) external view returns (bool requiresApproval);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`operation`|`string`|The operation identifier|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`requiresApproval`|`bool`|True if governance approval is required|


### executeWithGovernance

*Execute a transaction with governance checks*


```solidity
function executeWithGovernance(address target, uint256 value, bytes calldata data, string calldata operation)
    external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`target`|`address`|The target address|
|`value`|`uint256`|The ether value|
|`data`|`bytes`|The call data|
|`operation`|`string`|The operation description for governance|


### ownerAtIndex

*Get owner data at a specific index*


```solidity
function ownerAtIndex(uint256 index) external view returns (bytes memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`index`|`uint256`|Index of the owner|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes`|Owner data|


### nextOwnerIndex

*Get the next owner index*


```solidity
function nextOwnerIndex() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Next index to be used|


### ownerCount

*Get the total number of active owners*


```solidity
function ownerCount() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Number of active owners|


### removedOwnersCount

*Get the number of removed owners*


```solidity
function removedOwnersCount() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Number of removed owners|


### configureDelegation

*Configure delegation for another wallet*


```solidity
function configureDelegation(
    address delegatedWallet,
    uint256 maxValue,
    uint256 dailyLimit,
    bytes4[] calldata allowedSelectors
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegatedWallet`|`address`|Wallet that can execute on behalf of this wallet|
|`maxValue`|`uint256`|Maximum value for delegated transactions|
|`dailyLimit`|`uint256`|Daily spending limit for delegated transactions|
|`allowedSelectors`|`bytes4[]`|Function selectors that can be delegated|


### revokeDelegation

*Revoke delegation for a wallet*


```solidity
function revokeDelegation(address delegatedWallet) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegatedWallet`|`address`|Wallet to revoke delegation from|


### setFunctionDelegation

*Set function delegation permission*


```solidity
function setFunctionDelegation(address delegatedWallet, bytes4 selector, bool allowed) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegatedWallet`|`address`|Wallet to set permission for|
|`selector`|`bytes4`|Function selector|
|`allowed`|`bool`|Whether the function is allowed|


### executeDelegated

*Execute a delegated transaction*


```solidity
function executeDelegated(address target, uint256 value, bytes calldata data, bytes calldata signature)
    external
    returns (bool success, bytes memory result);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`target`|`address`|Target contract address|
|`value`|`uint256`|ETH value to send|
|`data`|`bytes`|Transaction data|
|`signature`|`bytes`|Signature from the delegating wallet|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`success`|`bool`|Whether the transaction succeeded|
|`result`|`bytes`|Return data from the transaction|


### executeDelegatedBatch

*Execute multiple delegated transactions in batch*


```solidity
function executeDelegatedBatch(
    address[] calldata targets,
    uint256[] calldata values,
    bytes[] calldata datas,
    bytes calldata signature
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`targets`|`address[]`|Array of target addresses|
|`values`|`uint256[]`|Array of ETH values|
|`datas`|`bytes[]`|Array of transaction data|
|`signature`|`bytes`|Signature from the delegating wallet|


### getDelegationConfig

*Get delegation configuration for a wallet*


```solidity
function getDelegationConfig(address delegatedWallet)
    external
    view
    returns (uint256 maxValue, uint256 dailyLimit, uint256 dailySpent, uint256 lastResetTime, bool active);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegatedWallet`|`address`|Wallet to get configuration for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`maxValue`|`uint256`|Maximum value for delegated transactions|
|`dailyLimit`|`uint256`|Daily spending limit|
|`dailySpent`|`uint256`|Amount spent today|
|`lastResetTime`|`uint256`|Last time daily limit was reset|
|`active`|`bool`|Whether delegation is active|


### isFunctionDelegated

*Check if a function is delegated to a wallet*


```solidity
function isFunctionDelegated(address delegatedWallet, bytes4 selector) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegatedWallet`|`address`|Wallet to check|
|`selector`|`bytes4`|Function selector to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether the function is delegated|


### getDelegatedWallets

*Get all wallets that have delegation from this wallet*


```solidity
function getDelegatedWallets() external view returns (address[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of delegated wallet addresses|


### updateDailyLimit

*Update daily limit for a delegated wallet*


```solidity
function updateDailyLimit(address delegatedWallet, uint256 newDailyLimit) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegatedWallet`|`address`|Wallet to update limit for|
|`newDailyLimit`|`uint256`|New daily limit|


### canDelegate

*Check if a wallet can delegate a specific function with a value*


```solidity
function canDelegate(address delegatedWallet, bytes4 selector, uint256 value) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegatedWallet`|`address`|Wallet to check|
|`selector`|`bytes4`|Function selector|
|`value`|`uint256`|Transaction value|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether delegation is allowed|


### configureLedger

*Configure ledger integration*


```solidity
function configureLedger(
    address ledgerAddress,
    string calldata defaultDebitAccount,
    string calldata defaultCreditAccount,
    bool autoEnabled
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ledgerAddress`|`address`|Address of the ledger contract|
|`defaultDebitAccount`|`string`|Default debit account code|
|`defaultCreditAccount`|`string`|Default credit account code|
|`autoEnabled`|`bool`|Whether auto-ledgering is enabled|


### configurePayrollAccounts

*Configure payroll-specific accounts and rates*


```solidity
function configurePayrollAccounts(
    uint256 federalTaxRate,
    uint256 stateTaxRate,
    uint256 ficaTaxRate,
    uint256 benefitsRate,
    string calldata federalTaxAccount,
    string calldata stateTaxAccount,
    string calldata ficaTaxAccount,
    string calldata benefitsAccount
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`federalTaxRate`|`uint256`|Federal tax rate in basis points|
|`stateTaxRate`|`uint256`|State tax rate in basis points|
|`ficaTaxRate`|`uint256`|FICA tax rate in basis points|
|`benefitsRate`|`uint256`|Benefits rate in basis points|
|`federalTaxAccount`|`string`|Account code for federal tax withholding|
|`stateTaxAccount`|`string`|Account code for state tax withholding|
|`ficaTaxAccount`|`string`|Account code for FICA tax withholding|
|`benefitsAccount`|`string`|Account code for benefits withholding|


### mapTokenToAccount

*Map a token address to an account code*


```solidity
function mapTokenToAccount(address token, string calldata accountCode) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Token address|
|`accountCode`|`string`|Account code for the token|


### setTransactionDescription

*Set transaction description template for a function selector*


```solidity
function setTransactionDescription(bytes4 selector, string calldata description) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`selector`|`bytes4`|Function selector|
|`description`|`string`|Description template|


### createLedgerEntry

*Create a manual ledger entry*


```solidity
function createLedgerEntry(string[] calldata accounts, int256[] calldata amounts, string calldata description)
    external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`accounts`|`string[]`|Array of account codes|
|`amounts`|`int256[]`|Array of amounts (positive for debit, negative for credit)|
|`description`|`string`|Description of the entry|


### getLedgerConfig

*Get ledger configuration*


```solidity
function getLedgerConfig()
    external
    view
    returns (
        address ledgerAddress,
        bool autoEnabled,
        string memory defaultDebitAccount,
        string memory defaultCreditAccount
    );
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`ledgerAddress`|`address`|Address of the ledger contract|
|`autoEnabled`|`bool`|Whether auto-ledgering is enabled|
|`defaultDebitAccount`|`string`|Default debit account code|
|`defaultCreditAccount`|`string`|Default credit account code|


### getTokenAccount

*Get account code for a token*


```solidity
function getTokenAccount(address token) external view returns (string memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Token address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|Account code for the token|


### setAutoLedgerEnabled

*Enable or disable auto-ledgering*


```solidity
function setAutoLedgerEnabled(bool enabled) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`enabled`|`bool`|Whether auto-ledgering should be enabled|


### eip712Domain

*Get EIP-712 domain information*


```solidity
function eip712Domain()
    external
    view
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
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`fields`|`bytes1`|Bit mask of fields present|
|`name`|`string`|Domain name|
|`version`|`string`|Domain version|
|`chainId`|`uint256`|Chain ID|
|`verifyingContract`|`address`|Address of this contract|
|`salt`|`bytes32`|Domain salt|
|`extensions`|`uint256[]`|Extensions array|


### replaySafeHash

*Get replay-safe hash for ERC1271*


```solidity
function replaySafeHash(bytes32 hash) external view returns (bytes32);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`hash`|`bytes32`|Original hash|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|Replay-safe hash|


### domainSeparator

*Get domain separator*


```solidity
function domainSeparator() external view returns (bytes32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|Domain separator hash|


## Events
### WalletInitialized

```solidity
event WalletInitialized(address indexed wallet, string walletType);
```

### TransactionExecuted

```solidity
event TransactionExecuted(address indexed target, uint256 value, bytes data, bool success);
```

### OwnerAdded

```solidity
event OwnerAdded(bytes indexed ownerData);
```

### OwnerRemoved

```solidity
event OwnerRemoved(bytes indexed ownerData);
```

### AddOwner

```solidity
event AddOwner(uint256 indexed index, bytes owner);
```

### RemoveOwner

```solidity
event RemoveOwner(uint256 indexed index, bytes owner);
```

### DelegationConfigured

```solidity
event DelegationConfigured(address indexed delegatedWallet, uint256 maxValue, uint256 dailyLimit);
```

### DelegationRevoked

```solidity
event DelegationRevoked(address indexed delegatedWallet);
```

### DelegatedTransactionExecuted

```solidity
event DelegatedTransactionExecuted(address indexed delegatedWallet, address indexed target, uint256 value);
```

### LedgerConfigured

```solidity
event LedgerConfigured(address indexed ledgerAddress, bool autoEnabled);
```

### LedgerEntryCreated

```solidity
event LedgerEntryCreated(string[] accounts, int256[] amounts, string description);
```

### PayrollTransactionProcessed

```solidity
event PayrollTransactionProcessed(address indexed recipient, uint256 grossPay, uint256 netPay);
```

### GovernanceContractUpdated

```solidity
event GovernanceContractUpdated(address indexed oldGovernanceContract, address indexed newGovernanceContract);
```

### GovernanceApprovalRequired

```solidity
event GovernanceApprovalRequired(address indexed governance, string operation, bytes callData);
```

### GovernanceTransactionExecuted

```solidity
event GovernanceTransactionExecuted(
    address indexed governance, address indexed target, uint256 value, string operation
);
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

### NotInitialized

```solidity
error NotInitialized();
```

### NotOwner

```solidity
error NotOwner();
```

### InvalidOwner

```solidity
error InvalidOwner();
```

### GovernanceApprovalMissing

```solidity
error GovernanceApprovalMissing(address governance, string operation, bytes callData);
```

### InvalidGovernanceContract

```solidity
error InvalidGovernanceContract();
```

### GovernanceNotSet

```solidity
error GovernanceNotSet();
```

