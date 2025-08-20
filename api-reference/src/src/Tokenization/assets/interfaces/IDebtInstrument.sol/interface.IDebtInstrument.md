# IDebtInstrument
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/interfaces/IDebtInstrument.sol)

Interface for debt instruments (bonds, loans, notes) in the tokenization system

*Implements a registry-based approach where only creditor assets are tokenized*


## Functions
### issueDebt

Issues a new debt instrument


```solidity
function issueDebt(address creditor, address debtor, DebtTerms calldata terms, uint256 customId)
    external
    returns (uint256 debtId, bytes32 assetLotId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`creditor`|`address`|The initial creditor (asset token holder)|
|`debtor`|`address`|The party owing the money|
|`terms`|`DebtTerms`|The terms of the debt|
|`customId`|`uint256`|Optional custom ID for the asset lot|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`debtId`|`uint256`|The unique debt identifier|
|`assetLotId`|`bytes32`|The lot ID for the creditor's asset token|


### makePayment

Makes a payment on a debt

*Payment allocation: interest first, then principal*


```solidity
function makePayment(uint256 debtId, uint256 amount) external payable;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`debtId`|`uint256`|The debt to pay|
|`amount`|`uint256`|The payment amount|


### getAmountDue

Gets the current amount due


```solidity
function getAmountDue(uint256 debtId)
    external
    view
    returns (uint256 principalDue, uint256 interestDue, bool isOverdue);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`debtId`|`uint256`|The debt to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`principalDue`|`uint256`|Amount of principal due|
|`interestDue`|`uint256`|Amount of interest due|
|`isOverdue`|`bool`|Whether payment is overdue|


### getSettlementAmount

Calculates total amount to settle debt today


```solidity
function getSettlementAmount(uint256 debtId) external view returns (uint256 totalAmount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`debtId`|`uint256`|The debt to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalAmount`|`uint256`|Principal + all accrued interest|


### getPaymentSchedule

Gets payment schedule for a debt


```solidity
function getPaymentSchedule(uint256 debtId) external view returns (PaymentScheduleItem[] memory schedule);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`debtId`|`uint256`|The debt to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`schedule`|`PaymentScheduleItem[]`|Array of payment schedule items|


### markAsDefaulted

Marks a debt as defaulted

*Only callable by authorized parties after grace period*


```solidity
function markAsDefaulted(uint256 debtId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`debtId`|`uint256`|The debt to mark as defaulted|


### restructureDebt

Restructures debt terms

*Requires consent from both debtor and creditor*


```solidity
function restructureDebt(uint256 debtId, DebtTerms calldata newTerms) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`debtId`|`uint256`|The debt to restructure|
|`newTerms`|`DebtTerms`|The new terms|


### getDebtRecord

Gets debt record


```solidity
function getDebtRecord(uint256 debtId) external view returns (DebtRecord memory record);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`debtId`|`uint256`|The debt ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`record`|`DebtRecord`|The complete debt record|


### getDebtorObligations

Gets all debt IDs for a debtor


```solidity
function getDebtorObligations(address debtor) external view returns (uint256[] memory debtIds);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`debtor`|`address`|The debtor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`debtIds`|`uint256[]`|Array of debt IDs|


### getDebtByAssetLot

Links a transferred asset lot to its debt record


```solidity
function getDebtByAssetLot(bytes32 assetLotId) external view returns (uint256 debtId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`assetLotId`|`bytes32`|The asset lot being transferred|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`debtId`|`uint256`|The associated debt ID|


## Events
### DebtIssued

```solidity
event DebtIssued(
    uint256 indexed debtId,
    address indexed debtor,
    address indexed creditor,
    uint256 principal,
    uint256 interestRate,
    uint256 maturityDate
);
```

### PaymentMade

```solidity
event PaymentMade(
    uint256 indexed debtId,
    address indexed debtor,
    uint256 principalPaid,
    uint256 interestPaid,
    uint256 remainingPrincipal
);
```

### DebtTransferred

```solidity
event DebtTransferred(
    uint256 indexed debtId, address indexed oldCreditor, address indexed newCreditor, bytes32 assetLotId
);
```

### DebtDefaulted

```solidity
event DebtDefaulted(uint256 indexed debtId, address indexed debtor, uint256 amountInDefault);
```

### DebtRestructured

```solidity
event DebtRestructured(uint256 indexed debtId, address indexed debtor, DebtTerms newTerms);
```

### DebtSettled

```solidity
event DebtSettled(uint256 indexed debtId, address indexed debtor, uint256 finalPayment);
```

## Structs
### DebtTerms

```solidity
struct DebtTerms {
    uint256 principal;
    uint256 interestRate;
    uint256 issuanceDate;
    uint256 maturityDate;
    PaymentFrequency paymentFrequency;
    address paymentCurrency;
    bool isAmortizing;
    uint256 gracePeriodDays;
}
```

### DebtRecord

```solidity
struct DebtRecord {
    uint256 debtId;
    address debtor;
    DebtTerms terms;
    uint256 outstandingPrincipal;
    uint256 accruedInterest;
    uint256 lastPaymentDate;
    uint256 nextPaymentDue;
    DebtStatus status;
    bytes32 assetLotId;
}
```

### PaymentScheduleItem

```solidity
struct PaymentScheduleItem {
    uint256 dueDate;
    uint256 principalDue;
    uint256 interestDue;
    bool isPaid;
    uint256 paidDate;
    uint256 paidAmount;
}
```

## Enums
### DebtStatus

```solidity
enum DebtStatus {
    ACTIVE,
    PAID,
    DEFAULTED,
    RESTRUCTURED
}
```

### PaymentFrequency

```solidity
enum PaymentFrequency {
    NONE,
    MONTHLY,
    QUARTERLY,
    SEMIANNUAL,
    ANNUAL
}
```

