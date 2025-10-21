# Offering Architecture

Investment offerings with built-in compliance modules.

## Overview

Features:
- Hybrid escrow (investor protection + issuer control)
- Compliance presets (506b, 506c, Reg S, Reg A+)
- Automated token issuance
- Modular compliance system
- Document management

## Diamond Structure

```
OfferingDiamond
├── DiamondCutFacet
├── DiamondLoupeFacet
├── AccessControlFacet
├── OfferingCoreFacet - Core offering logic
├── OfferingComplianceFacet - Compliance coordinator
└── OfferingDocumentsFacet - Document management
```

Plus separate compliance module contracts.

## Core Interface

```solidity
interface IOfferingCore {
  // Make investment
  function invest(uint256 amount, bytes32 attestationUID) 
    external payable returns (uint256 investmentId);
  
  // Countersign (accept) investment
  function countersignInvestment(uint256 investmentId) external;
  
  // Reject investment
  function rejectInvestment(uint256 investmentId) external;
  
  // Claim refund (investor protection)
  function claimRefund(uint256 investmentId) external;
  
  // Update status
  function updateOfferingStatus(OfferingStatus newStatus) external;
}
```

## Offering Configuration

```solidity
struct OfferingConfig {
  address issuer;           // Entity creating offering
  address tokenAddress;     // Token being offered
  address paymentToken;     // USDC, ETH, etc.
  address paymentRecipient; // Where funds go
  uint256 pricePerToken;    // Price per token
  uint256 minInvestment;    // Minimum investment
  uint256 maxAmount;        // Maximum total raise
  uint256 deadline;         // Offering deadline
  address admin;            // Admin address
}
```

## Investment Flow

### 1. Investor Invests

```solidity
// Investor submits investment
uint256 investmentId = offering.invest{value: investmentAmount}(
  amount,
  kycAttestationUID
);

// Funds held in offering contract
// Investment status: PENDING
```

### 2. Compliance Checks

Automatically checked:
- KYC verification
- Accreditation (if required)
- Document signing
- Investor limits
- Whitelist (if enabled)

### 3. Issuer Countersigns

```solidity
// Issuer reviews and accepts
offering.countersignInvestment(investmentId);

// Funds released to issuer
// Tokens auto-issued to investor
// Investment status: ACCEPTED
```

### 4. Or Issuer Rejects

```solidity
// Issuer rejects (investor not qualified)
offering.rejectInvestment(investmentId);

// Funds refunded to investor
// Investment status: REJECTED
```

## Compliance Modules

### Module Interface

```solidity
interface IComplianceModule {
  function initialize(bytes memory initData) external;
  
  function checkInvestment(
    address investor,
    uint256 amount,
    bytes32 attestationUID,
    bytes memory data
  ) external view returns (bool allowed, string memory reason);
}
```

### Built-in Modules

**KYCComplianceModule:**
- Checks investor has KYC attestation
- Verifies attestation not expired/revoked

**WhitelistComplianceModule:**
- Checks investor on approved list
- Issuer controls whitelist

**InvestorLimitsComplianceModule:**
- Enforces max investor count (e.g., 99 for 506b)
- Tracks total investors

**AccreditationComplianceModule:**
- Requires accreditation attestation
- For 506(c) offerings

**DocumentComplianceModule:**
- Requires signed subscription agreement
- Checks document signatures

### Example: KYC Module

```solidity
contract KYCComplianceModule is IComplianceModule {
  IEAS public eas;
  bytes32 public kycSchemaUID;
  address public admin;
  
  function checkInvestment(
    address investor,
    uint256,
    bytes32 attestationUID,
    bytes memory
  ) external view returns (bool, string memory) {
    // Check attestation exists and is valid
    Attestation memory attestation = eas.getAttestation(attestationUID);
    
    if (attestation.attester != admin) {
      return (false, "Invalid attester");
    }
    
    if (attestation.revocationTime != 0) {
      return (false, "KYC revoked");
    }
    
    if (attestation.expirationTime != 0 && attestation.expirationTime < block.timestamp) {
      return (false, "KYC expired");
    }
    
    return (true, "");
  }
}
```

## Hybrid Escrow

### Investor Protection

If issuer doesn't countersign within grace period:

```solidity
function claimRefund(uint256 investmentId) external {
  Investment memory inv = getInvestment(investmentId);
  
  require(inv.investor == msg.sender, "Not investor");
  require(!inv.countersigned, "Already countersigned");
  require(block.timestamp > offering.deadline + GRACE_PERIOD, "Grace period not expired");
  
  // Refund investor
  inv.status = InvestmentStatus.REFUNDED;
  paymentToken.transfer(inv.investor, inv.amount);
}
```

### Issuer Control

Issuer can reject unqualified investors:

```solidity
function rejectInvestment(uint256 investmentId) external onlyIssuer {
  Investment memory inv = getInvestment(investmentId);
  
  require(!inv.countersigned, "Already countersigned");
  require(inv.status == InvestmentStatus.PENDING, "Not pending");
  
  // Refund investor
  inv.status = InvestmentStatus.REJECTED;
  paymentToken.transfer(inv.investor, inv.amount);
  
  emit InvestmentRejected(investmentId, inv.investor);
}
```

## Deployment

### Via OfferingFactory

```solidity
IOffering offering = IOfferingFactory(OFFERING_FACTORY).createOffering({
  config: offeringConfig,
  facetCuts: facetCuts,
  init: MULTI_INIT_FLAG,
  initData: multiInitData,
  complianceModules: [
    {
      moduleName: "KYCCompliance",
      executionOrder: 1,
      enabled: true,
      initData: abi.encode(eas, kycSchemaUID, admin)
    },
    {
      moduleName: "AccreditationCompliance",
      executionOrder: 2,
      enabled: true,
      initData: abi.encode(eas, accreditationSchemaUID, admin)
    }
  ]
});
```

## Storage

### OfferingCoreStorage

```solidity
library OfferingCoreStorage {
  struct Layout {
    OfferingInfo info;
    mapping(uint256 => Investment) investments;
    uint256 investmentCount;
    uint256 totalInvested;
    uint256 investorCount;
    mapping(address => bool) hasInvested;
  }
  
  struct Investment {
    address investor;
    uint256 amount;
    uint256 timestamp;
    bool countersigned;
    InvestmentStatus status;
    bytes32 documentUID;
  }
}
```

## Events

```solidity
event InvestmentReceived(uint256 indexed investmentId, address indexed investor, uint256 amount);
event InvestmentCountersigned(uint256 indexed investmentId);
event InvestmentRejected(uint256 indexed investmentId, address indexed investor);
event RefundClaimed(uint256 indexed investmentId, address indexed investor);
event OfferingStatusUpdated(OfferingStatus newStatus);
```

## Security

- Access control on admin functions
- Reentrancy protection
- Pausable
- Compliance checks before accepting investment

## Gas Costs

On Base:
- Offering deployment: ~13M gas (~$0.20)
- Invest: ~500k gas (~$0.005)
- Countersign: ~300k gas (~$0.003)

## Testing

```solidity
function testInvest() public {
  vm.prank(investor);
  uint256 investmentId = offering.invest(1000e6, kycUID);
  
  assertEq(offering.totalInvested(), 1000e6);
}

function testCountersign() public {
  vm.prank(issuer);
  offering.countersignInvestment(investmentId);
  
  assertEq(token.balanceOf(investor), 1000);
}
```

## Resources

- [SEC Reg D](https://www.sec.gov/education/smallbusiness/exemptofferings/rule506b)
- [Contract Addresses](/reference/contract-addresses.md)
