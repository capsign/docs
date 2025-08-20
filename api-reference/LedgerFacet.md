# LedgerFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Wallets/wallet/facets/LedgerFacet.sol)

**Inherits:**
[MultiOwnable](/src/Wallets/wallet/MultiOwnable.sol/contract.MultiOwnable.md)

*Handles automatic ledger integration for wallet transactions*


## Functions
### onlyOwner


```solidity
modifier onlyOwner() override;
```

### configureLedger

*Configure ledger integration for this wallet*


```solidity
function configureLedger(
    address ledgerAddress,
    string calldata defaultDebitAccount,
    string calldata defaultCreditAccount,
    bool autoEnabled
) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ledgerAddress`|`address`|The ledger contract address|
|`defaultDebitAccount`|`string`|Default debit account code (e.g., "1000" for cash)|
|`defaultCreditAccount`|`string`|Default credit account code (e.g., "6000" for expenses)|
|`autoEnabled`|`bool`|Whether to automatically create ledger entries|


### configurePayrollAccounts

*Configure payroll withholding rates and accounts*


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
) external onlyOwner;
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

*Map a token to a specific ledger account*


```solidity
function mapTokenToAccount(address token, string calldata accountCode) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Token contract address|
|`accountCode`|`string`|Ledger account code for this token|


### setTransactionDescription

*Set transaction description template for a function selector*


```solidity
function setTransactionDescription(bytes4 selector, string calldata description) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`selector`|`bytes4`|Function selector|
|`description`|`string`|Description template|


### createLedgerEntry

*Manually create a ledger entry*


```solidity
function createLedgerEntry(string[] calldata accountCodes, int256[] calldata amounts, string calldata description)
    external
    onlyOwner
    returns (uint256 txId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`accountCodes`|`string[]`|Array of account codes|
|`amounts`|`int256[]`|Array of amounts (must sum to zero)|
|`description`|`string`|Transaction description|


### onTransactionExecuted

*Hook called after transaction execution for auto-ledgering*


```solidity
function onTransactionExecuted(
    address target,
    uint256 value,
    bytes calldata data,
    bool success,
    uint256 gasUsed,
    uint256 gasPrice
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`target`|`address`|Transaction target|
|`value`|`uint256`|ETH value sent|
|`data`|`bytes`|Transaction data|
|`success`|`bool`|Whether transaction succeeded|
|`gasUsed`|`uint256`|Amount of gas used for the transaction|
|`gasPrice`|`uint256`|Gas price in wei|


### getLedgerConfig

*Get ledger configuration*


```solidity
function getLedgerConfig()
    external
    view
    returns (
        address ledgerAddress,
        string memory defaultDebitAccount,
        string memory defaultCreditAccount,
        bool autoLedgerEnabled
    );
```

### getTokenAccount

*Get token to account mapping*


```solidity
function getTokenAccount(address token) external view returns (string memory);
```

### setAutoLedgerEnabled

*Toggle auto-ledgering on/off*


```solidity
function setAutoLedgerEnabled(bool enabled) external onlyOwner;
```

### _recordEthTransaction

*Record ETH transaction in ledger*


```solidity
function _recordEthTransaction(address target, uint256 value, WalletStorage.Layout storage l) internal;
```

### _recordTokenTransaction

*Record token transaction in ledger*


```solidity
function _recordTokenTransaction(address token, bytes calldata data, WalletStorage.Layout storage l) internal;
```

### _recordCustomTransaction

*Record custom transaction based on selector mapping*


```solidity
function _recordCustomTransaction(
    address target,
    bytes calldata data,
    string memory description,
    WalletStorage.Layout storage l
) internal;
```

### _recordGasFees

*Record gas fees as operating expense*


```solidity
function _recordGasFees(uint256 gasCost, WalletStorage.Layout storage l) internal;
```

### _recordPayrollTransaction

*Record complex payroll transaction with withholdings*


```solidity
function _recordPayrollTransaction(address target, bytes calldata data, WalletStorage.Layout storage l) internal;
```

### _calculatePayrollBreakdown

*Calculate payroll breakdown with withholdings*


```solidity
function _calculatePayrollBreakdown(uint256 netPay, WalletStorage.PayrollConfig memory config)
    internal
    pure
    returns (PayrollBreakdown memory);
```

### _sumAmounts

*Helper function to sum amounts array*


```solidity
function _sumAmounts(int256[] calldata amounts) internal pure returns (int256 sum);
```

### _addressToString

*Helper function to convert address to string*


```solidity
function _addressToString(address _addr) internal pure returns (string memory);
```

### _uint256ToString

*Helper function to convert uint256 to string*


```solidity
function _uint256ToString(uint256 value) internal pure returns (string memory);
```

## Events
### LedgerConfigured

```solidity
event LedgerConfigured(address indexed ledger, bool autoEnabled);
```

### AutoLedgerEntryCreated

```solidity
event AutoLedgerEntryCreated(uint256 indexed ledgerTxId, string description, int256 amount);
```

### TokenAccountMapped

```solidity
event TokenAccountMapped(address indexed token, string accountCode);
```

### TransactionCategorized

```solidity
event TransactionCategorized(bytes4 indexed selector, string description);
```

## Errors
### LedgerNotConfigured

```solidity
error LedgerNotConfigured();
```

### InvalidAccountCode

```solidity
error InvalidAccountCode();
```

### AutoLedgerDisabled

```solidity
error AutoLedgerDisabled();
```

## Structs
### PayrollBreakdown
*Payroll breakdown structure*


```solidity
struct PayrollBreakdown {
    uint256 grossPay;
    uint256 netPay;
    uint256 federalTax;
    uint256 stateTax;
    uint256 ficaTax;
    uint256 benefits;
}
```

