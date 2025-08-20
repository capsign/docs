# DebtUsageExample
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/examples/DebtUsageExample.sol)

Example contract showing how to use the debt instrument system

*This demonstrates various debt scenarios and best practices*


## State Variables
### assetDiamond

```solidity
address public assetDiamond;
```


## Functions
### constructor


```solidity
constructor(address _assetDiamond);
```

### issueSimpleBond

Example 1: Issue a Simple Bond
- Fixed rate, bullet payment at maturity
- Interest accrues but is paid at maturity with principal


```solidity
function issueSimpleBond(
    address creditor,
    address debtor,
    uint256 principal,
    uint256 annualRateBps,
    uint256 termMonths,
    address paymentToken
) external returns (uint256 debtId, bytes32 assetLotId);
```

### issueAmortizingLoan

Example 2: Issue an Amortizing Loan
- Monthly payments of principal + interest
- Principal paid down over time


```solidity
function issueAmortizingLoan(
    address creditor,
    address debtor,
    uint256 principal,
    uint256 annualRateBps,
    uint256 termYears,
    address paymentToken
) external returns (uint256 debtId, bytes32 assetLotId);
```

### issueCorporateBond

Example 3: Issue a Corporate Bond with Quarterly Interest
- Quarterly interest payments
- Principal repaid at maturity (balloon)


```solidity
function issueCorporateBond(
    address creditor,
    address debtor,
    uint256 principal,
    uint256 annualRateBps,
    uint256 termYears,
    address paymentToken
) external returns (uint256 debtId, bytes32 assetLotId);
```

### makeDebtPayment

Example 4: Make a payment on debt


```solidity
function makeDebtPayment(uint256 debtId, uint256 amount) external;
```

### checkAmountDue

Example 5: Check what's due


```solidity
function checkAmountDue(uint256 debtId)
    external
    view
    returns (uint256 principalDue, uint256 interestDue, uint256 totalDue, bool isOverdue);
```

### getPayoffAmount

Example 6: Get full payoff amount


```solidity
function getPayoffAmount(uint256 debtId) external view returns (uint256);
```

### viewPaymentSchedule

Example 7: View payment schedule


```solidity
function viewPaymentSchedule(uint256 debtId) external view returns (IDebtInstrument.PaymentScheduleItem[] memory);
```

### transferDebtToNewCreditor

Example 8: Transfer debt to new creditor
Note: This happens automatically when the asset lot is transferred


```solidity
function transferDebtToNewCreditor(bytes32 assetLotId, address newCreditor, uint256 price) external;
```

### getDebtorObligations

Example 9: Check debtor's obligations


```solidity
function getDebtorObligations(address debtor) external view returns (uint256[] memory debtIds);
```

