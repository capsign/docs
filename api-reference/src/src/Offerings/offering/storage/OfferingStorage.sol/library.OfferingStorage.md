# OfferingStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Offerings/offering/storage/OfferingStorage.sol)

Contains all storage variables for offering functionality

*Storage contract for Offering diamond*


## State Variables
### STORAGE_SLOT

```solidity
bytes32 internal constant STORAGE_SLOT = keccak256("capsign.storage.offering");
```


## Functions
### layout


```solidity
function layout() internal pure returns (Layout storage l);
```

## Structs
### Layout

```solidity
struct Layout {
    IOffering.Status status;
    address entity;
    IIssuedAsset asset;
    IERC20 paymentCurrency;
    uint256 totalInvested;
    uint256 pricePerToken;
    uint256 minInvestment;
    uint256 investmentDeadline;
    uint256 targetAmount;
    uint256 maxAmount;
    string baseURI;
    IDocumentRegistry documentRegistry;
    mapping(IOffering.DocumentType => bytes32[]) documentHashes;
    bytes32 signatureTemplate;
    uint256 nextInvestmentId;
    mapping(uint256 => IOffering.Investment) investments;
    mapping(uint256 => address) investmentInvestors;
    mapping(address => bool) whitelist;
    address[] whitelistedInvestors;
    mapping(address => bool) accreditedInvestors;
    mapping(address => uint256) investorLimits;
    mapping(string => bool) jurisdictionAllowed;
    mapping(bytes32 => bool) filedDocuments;
    mapping(string => bytes32) regulatoryFilings;
    uint256 maxInvestors;
    uint256 nonAccreditedInvestorCount;
    bool isPrivateOffering;
    bool allowGeneralSolicitation;
    bool requireAccreditation;
    string offeringType;
    mapping(address => bytes32) investorAgreements;
    mapping(bytes32 => bool) usedNonces;
    uint256 offeringClosingDate;
    bool allowPartialFills;
    IDocumentRegistry filingDocumentRegistry;
    mapping(bytes32 => Filing) filings;
    bytes32[] filingIds;
    mapping(string => bytes32[]) jurisdictionFilings;
    mapping(string => bytes32[]) filingTypeToFilings;
    uint256 exemptionLimit;
    mapping(address => uint256) investorAmountIssued;
    mapping(uint256 => uint256) monthlyInvestmentTotals;
    uint8 regSCategory;
    uint256 distributionComplianceStart;
    uint256 distributionComplianceEnd;
    address attestationRegistry;
    bytes32 basicComplianceSchemaUID;
    bytes32 foreignInvestorSchemaUID;
    bytes32 customerIdentificationSchemaUID;
    bytes32 basicBusinessSchemaUID;
    uint256 minHoldingPeriod;
    mapping(address => uint256) tokenAcquisitionDates;
}
```

### Filing

```solidity
struct Filing {
    string filingType;
    string filingId;
    string jurisdiction;
    uint256 filingDate;
    uint256 amendmentCount;
    bool isActive;
    bytes32 documentHash;
}
```

