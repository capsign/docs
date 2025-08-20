# DebtInstrumentFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/facets/DebtInstrumentFacet.sol)

**Inherits:**
[IDebtInstrument](/src/Tokenization/assets/interfaces/IDebtInstrument.sol/interface.IDebtInstrument.md), ReentrancyGuard

Implements debt instrument functionality for tokenized debt assets

*Registry-based approach where only creditor positions are tokenized*


## Functions
### onlyAuthorizedIssuer


```solidity
modifier onlyAuthorizedIssuer();
```

### onlyDebtor


```solidity
modifier onlyDebtor(uint256 debtId);
```

### debtExists


```solidity
modifier debtExists(uint256 debtId);
```

### issueDebt

Issues a new debt instrument


```solidity
function issueDebt(address creditor, address debtor, DebtTerms calldata terms, uint256 customId)
    external
    onlyAuthorizedIssuer
    nonReentrant
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
function makePayment(uint256 debtId, uint256 amount) external payable debtExists(debtId) nonReentrant;
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
    public
    view
    debtExists(debtId)
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
function getSettlementAmount(uint256 debtId) external view debtExists(debtId) returns (uint256 totalAmount);
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
function getPaymentSchedule(uint256 debtId)
    external
    view
    debtExists(debtId)
    returns (PaymentScheduleItem[] memory schedule);
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
function markAsDefaulted(uint256 debtId) external debtExists(debtId) onlyAuthorizedIssuer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`debtId`|`uint256`|The debt to mark as defaulted|


### restructureDebt

Restructures debt terms

*Requires consent from both debtor and creditor*


```solidity
function restructureDebt(uint256 debtId, DebtTerms calldata newTerms) external debtExists(debtId) nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`debtId`|`uint256`|The debt to restructure|
|`newTerms`|`DebtTerms`|The new terms|


### getDebtRecord

Gets debt record


```solidity
function getDebtRecord(uint256 debtId) external view debtExists(debtId) returns (DebtRecord memory record);
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


### _createDebtAssetLot

Creates a debt asset lot for the creditor


```solidity
function _createDebtAssetLot(
    address creditor,
    uint256 principal,
    address paymentCurrency,
    uint256 debtId,
    uint256 customId
) internal returns (bytes32 lotId);
```

### _getCreditorFromLot

Gets the current creditor from a lot


```solidity
function _getCreditorFromLot(bytes32 lotId) internal view returns (address);
```

### _updatePaymentSchedule

Updates payment schedule after a payment


```solidity
function _updatePaymentSchedule(uint256 debtId, uint256 paymentAmount) internal;
```

### onDebtAssetTransferred

Handles debt asset transfer notification

*Called by TransferFacet when a debt asset lot is transferred*


```solidity
function onDebtAssetTransferred(bytes32 oldLotId, bytes32 newLotId, address from, address to) external;
```

### setAuthorizedIssuer

Sets authorized issuer status


```solidity
function setAuthorizedIssuer(address issuer, bool authorized) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`issuer`|`address`|Address to authorize/unauthorize|
|`authorized`|`bool`|Whether the address should be authorized|


### consentToRestructure

Provides consent for debt restructuring


```solidity
function consentToRestructure(uint256 debtId) external debtExists(debtId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`debtId`|`uint256`|The debt to consent for|


