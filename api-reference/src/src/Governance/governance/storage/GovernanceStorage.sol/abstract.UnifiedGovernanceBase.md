# UnifiedGovernanceBase
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/governance/storage/GovernanceStorage.sol)

Base contract with common governance functionality

*Provides upgrade-safe foundation for all governance facets*


## Functions
### onlyValidStockClass


```solidity
modifier onlyValidStockClass(string memory classId);
```

### onlyAuthorizedSyncer


```solidity
modifier onlyAuthorizedSyncer();
```

### configureOCFStockClasses


```solidity
function configureOCFStockClasses(GovernanceStorage.OCFStockClass[] calldata stockClasses) external virtual;
```

### configureOCFProposalTypes


```solidity
function configureOCFProposalTypes(GovernanceStorage.OCFProposalTypeConfig[] calldata proposalTypes) external virtual;
```

### updateStakeholderShares


```solidity
function updateStakeholderShares(address stakeholder, string calldata classId, uint256 shares) external virtual;
```

### calculateVotingPower


```solidity
function calculateVotingPower(address stakeholder, string memory proposalType) public view virtual returns (uint256);
```

### canVetoProposal


```solidity
function canVetoProposal(address stakeholder, string memory proposalType) public view virtual returns (bool);
```

### syncFromCapTableProvider


```solidity
function syncFromCapTableProvider(string calldata provider, bytes calldata ocfData) external virtual returns (bool);
```

### approveLegalDocument


```solidity
function approveLegalDocument(bytes32 documentHash, string calldata approverRole) external virtual returns (bool);
```

### triggerAntiDilution


```solidity
function triggerAntiDilution(string calldata classId, uint256 newRoundPrice, uint256 newSharesIssued)
    external
    virtual
    returns (uint256);
```

### executeLiquidationWaterfall


```solidity
function executeLiquidationWaterfall(uint256 totalProceeds) external virtual returns (uint256[] memory);
```

### exercisePreemptiveRights


```solidity
function exercisePreemptiveRights(uint256 roundId, uint256 sharesToPurchase) external payable virtual returns (bool);
```

### _addStakeholderClass


```solidity
function _addStakeholderClass(address stakeholder, string memory classId) internal;
```

### _removeStakeholderClass


```solidity
function _removeStakeholderClass(address stakeholder, string memory classId) internal;
```

### getOCFStockClass


```solidity
function getOCFStockClass(string calldata classId) external view returns (GovernanceStorage.OCFStockClass memory);
```

### getStakeholderShares


```solidity
function getStakeholderShares(address stakeholder)
    external
    view
    returns (string[] memory classIds, uint256[] memory shares);
```

### getStorageVersion


```solidity
function getStorageVersion() external pure returns (string memory);
```

## Events
### OCFStockClassConfigured

```solidity
event OCFStockClassConfigured(string indexed classId, uint256 votesPerShare, bool canVoteOnBoard);
```

### StakeholderSharesUpdated

```solidity
event StakeholderSharesUpdated(address indexed stakeholder, string indexed classId, uint256 shares);
```

### OCFProposalTypeConfigured

```solidity
event OCFProposalTypeConfigured(string indexed proposalType, uint256 quorumThreshold, uint256 approvalThreshold);
```

### ProposalVetoed

```solidity
event ProposalVetoed(uint256 indexed proposalId, string indexed classId, address indexed vetoer);
```

### CapTableSynced

```solidity
event CapTableSynced(string indexed provider, bytes32 indexed syncHash);
```

### LegalDocumentApproved

```solidity
event LegalDocumentApproved(bytes32 indexed documentHash, string indexed approver);
```

### AntiDilutionTriggered

```solidity
event AntiDilutionTriggered(string indexed classId, uint256 newConversionRatio);
```

### TransferRestricted

```solidity
event TransferRestricted(address indexed from, address indexed to, string reason);
```

### LiquidationWaterfallExecuted

```solidity
event LiquidationWaterfallExecuted(uint256 totalProceeds, uint256[] distributions);
```

