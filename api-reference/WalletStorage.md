# WalletStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Wallets/wallet/storage/WalletStorage.sol)

Diamond storage for wallet data and state management

*Uses diamond storage pattern to avoid storage conflicts*


## State Variables
### WALLET_STORAGE_POSITION

```solidity
bytes32 constant WALLET_STORAGE_POSITION = keccak256("capsign.storage.wallet");
```


## Functions
### layout


```solidity
function layout() internal pure returns (Layout storage l);
```

### isValidWalletType


```solidity
function isValidWalletType(string memory walletType) internal pure returns (bool);
```

### setWalletType


```solidity
function setWalletType(Layout storage l, string memory walletType) internal;
```

### getWalletType


```solidity
function getWalletType(Layout storage l) internal view returns (string memory);
```

### requiresApproval

*Check if a transaction needs approval*


```solidity
function requiresApproval(bytes4 selector, uint256 value) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`selector`|`bytes4`|Function selector|
|`value`|`uint256`|Transaction value|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether approval is required|


### getRequiredApprovals

*Get required approval count for a function*


```solidity
function getRequiredApprovals(bytes4 selector) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`selector`|`bytes4`|Function selector|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Number of required approvals|


### canDelegate

*Check delegation permissions*


```solidity
function canDelegate(address delegatedWallet, bytes4 selector, uint256 value) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegatedWallet`|`address`|The wallet trying to execute|
|`selector`|`bytes4`|Function selector|
|`value`|`uint256`|Transaction value|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether delegation is allowed|


### updateDailySpending

*Update daily spending for delegation*


```solidity
function updateDailySpending(address delegatedWallet, uint256 amount) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegatedWallet`|`address`|The delegated wallet|
|`amount`|`uint256`|Amount spent|


## Structs
### DelegationConfig

```solidity
struct DelegationConfig {
    address delegatedWallet;
    mapping(bytes4 => bool) allowedSelectors;
    uint256 maxValue;
    uint256 dailyLimit;
    uint256 lastResetTime;
    uint256 dailySpent;
    bool active;
}
```

### PayrollConfig

```solidity
struct PayrollConfig {
    uint256 federalTaxRate;
    uint256 stateTaxRate;
    uint256 ficaTaxRate;
    uint256 benefitsRate;
    string federalTaxAccount;
    string stateTaxAccount;
    string ficaTaxAccount;
    string benefitsAccount;
    string salaryExpenseAccount;
    string gasFeesAccount;
}
```

### LedgerConfig

```solidity
struct LedgerConfig {
    address ledgerAddress;
    bool autoLedgerEnabled;
    string defaultDebitAccount;
    string defaultCreditAccount;
    mapping(address => string) tokenToAccount;
    mapping(bytes4 => string) selectorToDescription;
    PayrollConfig payrollConfig;
}
```

### ApprovalWorkflow

```solidity
struct ApprovalWorkflow {
    uint256 requiredApprovals;
    uint256 timelockDuration;
    mapping(bytes4 => bool) requiresApproval;
    mapping(bytes4 => uint256) customRequiredApprovals;
    bool sequentialApproval;
    bytes[] approvalOrder;
    mapping(bytes32 => uint256) approvalCount;
    mapping(bytes32 => mapping(address => bool)) hasApproved;
    mapping(bytes32 => bool) executed;
}
```

### Layout

```solidity
struct Layout {
    bool initialized;
    string walletType;
    address entryPoint;
    bool emergencyMode;
    uint256 emergencyActivatedAt;
    ApprovalWorkflow approvalWorkflow;
    mapping(bytes32 => TransactionStatus) transactionStatus;
    uint256 nonce;
    mapping(address => DelegationConfig) delegations;
    address[] delegatedWallets;
    LedgerConfig ledgerConfig;
    mapping(address => mapping(bytes4 => bool)) functionPermissions;
    uint256 gasAllowance;
    uint256 gasUsed;
    uint256 lastGasReset;
    address governanceContract;
    mapping(bytes32 => bool) governanceApprovals;
    mapping(string => string) metadata;
}
```

## Enums
### TransactionStatus

```solidity
enum TransactionStatus {
    PENDING,
    EXECUTED,
    FAILED,
    CANCELLED
}
```

