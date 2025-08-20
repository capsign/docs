# DebtInstrumentStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/storage/DebtInstrumentStorage.sol)

Storage layout for debt instrument functionality

*Uses diamond storage pattern to avoid storage collisions*


## State Variables
### STORAGE_SLOT

```solidity
bytes32 internal constant STORAGE_SLOT = keccak256("capsign.storage.DebtInstrument");
```


## Functions
### layout


```solidity
function layout() internal pure returns (Layout storage l);
```

### getNextDebtId

Gets the next debt ID and increments counter


```solidity
function getNextDebtId() internal returns (uint256);
```

### addDebtToDebtor

Adds debt ID to debtor's list


```solidity
function addDebtToDebtor(address debtor, uint256 debtId) internal;
```

### linkAssetLotToDebt

Links asset lot to debt ID


```solidity
function linkAssetLotToDebt(bytes32 assetLotId, uint256 debtId) internal;
```

### updateAssetLotLink

Updates asset lot link (for transfers)


```solidity
function updateAssetLotLink(bytes32 oldLotId, bytes32 newLotId, uint256 debtId) internal;
```

### isAuthorizedIssuer

Checks if address is authorized issuer


```solidity
function isAuthorizedIssuer(address issuer) internal view returns (bool);
```

### setAuthorizedIssuer

Sets authorized issuer status


```solidity
function setAuthorizedIssuer(address issuer, bool authorized) internal;
```

### recordPayment

Records a payment


```solidity
function recordPayment(uint256 debtId, uint256 principalPaid, uint256 interestPaid) internal;
```

### getTotalPayments

Gets total payments made


```solidity
function getTotalPayments(uint256 debtId) internal view returns (uint256 principal, uint256 interest);
```

### calculateAccruedInterest

Calculates accrued interest

*Simple interest calculation, can be overridden for compound interest*


```solidity
function calculateAccruedInterest(uint256 principal, uint256 annualRateBps, uint256 fromTimestamp, uint256 toTimestamp)
    internal
    pure
    returns (uint256);
```

### calculateNextPaymentDue

Calculates next payment due date based on frequency


```solidity
function calculateNextPaymentDue(uint256 lastPaymentDate, IDebtInstrument.PaymentFrequency frequency)
    internal
    pure
    returns (uint256);
```

### generatePaymentSchedule

Generates payment schedule for a debt


```solidity
function generatePaymentSchedule(IDebtInstrument.DebtTerms memory terms, uint256 debtId) internal;
```

### calculateNumberOfPayments


```solidity
function calculateNumberOfPayments(IDebtInstrument.DebtTerms memory terms) internal pure returns (uint256);
```

### getPaymentInterval


```solidity
function getPaymentInterval(IDebtInstrument.PaymentFrequency frequency) internal pure returns (uint256);
```

## Structs
### Layout

```solidity
struct Layout {
    uint256 nextDebtId;
    mapping(uint256 => IDebtInstrument.DebtRecord) debtRecords;
    mapping(address => uint256[]) debtorToDebtIds;
    mapping(bytes32 => uint256) assetLotToDebtId;
    mapping(uint256 => IDebtInstrument.PaymentScheduleItem[]) paymentSchedules;
    mapping(uint256 => uint256) totalInterestPaid;
    mapping(uint256 => uint256) totalPrincipalPaid;
    mapping(uint256 => mapping(address => bool)) restructureConsents;
    uint256 defaultGracePeriodDays;
    mapping(address => bool) authorizedIssuers;
    mapping(uint256 => address) paymentReceivers;
}
```

