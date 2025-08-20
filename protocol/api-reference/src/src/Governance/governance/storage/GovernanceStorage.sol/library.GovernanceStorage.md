# GovernanceStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/governance/storage/GovernanceStorage.sol)

Unified storage for OCF-based and traditional governance with upgrade safety

*Consolidates all governance storage with proper gaps for future phases*


## State Variables
### STORAGE_POSITION

```solidity
bytes32 constant STORAGE_POSITION = keccak256("capsign.storage.governance.v1");
```


## Functions
### layout


```solidity
function layout() internal pure returns (Layout storage l);
```

## Structs
### Proposal

```solidity
struct Proposal {
    uint256 id;
    address proposer;
    uint256 voteStart;
    uint256 voteEnd;
    uint256 eta;
    bool executed;
    bool canceled;
    address[] targets;
    uint256[] values;
    bytes[] calldatas;
    bytes32 descriptionHash;
    uint256 forVotes;
    uint256 againstVotes;
    uint256 abstainVotes;
    mapping(bytes32 => bool) lotVoted;
    mapping(bytes32 => uint8) lotVoteChoice;
    mapping(address => bool) addressVoted;
    mapping(address => uint8) addressVoteChoice;
}
```

### OCFStockClass

```solidity
struct OCFStockClass {
    string classId;
    uint256 votesPerShare;
    bool canVoteOnBoard;
    bool canVoteOnMajorDecisions;
    string[] vetoRights;
    uint256 boardSeatsDesignated;
    bool conversionVoting;
    uint256 liquidationPreference;
    uint256 antiDilutionMultiplier;
    bool hasDragAlongRights;
    bool hasTagAlongRights;
    bool hasPreemptiveRights;
    uint256 participationCap;
    uint256[5] __stockClassGap;
}
```

### OCFVotingRights

```solidity
struct OCFVotingRights {
    string[] eligibleClasses;
    uint256 quorumThreshold;
    uint256 approvalThreshold;
    bool requiresUnanimous;
    uint256[4] __votingRightsGap;
}
```

### OCFProposalTypeConfig

```solidity
struct OCFProposalTypeConfig {
    string proposalType;
    OCFVotingRights votingRights;
    string[] vetoClasses;
    uint256[3] __proposalTypeGap;
}
```

### BoardMember

```solidity
struct BoardMember {
    uint256[] committeeIds;
    bool active;
    uint256 appointedAt;
    address appointedBy;
    uint256 totalVotingWeight;
    string primaryRole;
    uint256[4] __boardMemberGap;
}
```

### ExecutiveApproval

```solidity
struct ExecutiveApproval {
    address executive;
    address targetWallet;
    bytes4 selector;
    uint256 maxValue;
    bool active;
    uint256 validUntil;
    uint256[2] __executiveGap;
}
```

### AntiDilutionTerms

```solidity
struct AntiDilutionTerms {
    string protectionType;
    uint256 conversionPrice;
    uint256 adjustmentMultiplier;
    uint256 lastUpdateRound;
    uint256[4] __antiDilutionGap;
}
```

### TransferRestrictions

```solidity
struct TransferRestrictions {
    bool hasRightOfFirstRefusal;
    bool hasDragAlongRights;
    bool hasTagAlongRights;
    address transferAgent;
    uint256 minimumHoldPeriod;
    uint256[3] __transferGap;
}
```

### LiquidationWaterfall

```solidity
struct LiquidationWaterfall {
    uint256[] preferences;
    uint256[] participationCaps;
    bool[] participationRights;
    uint256[5] __liquidationGap;
}
```

### Layout

```solidity
struct Layout {
    string name;
    uint256 votingDelay;
    uint256 votingPeriod;
    uint256 proposalThreshold;
    uint256 quorumNumerator;
    uint256 quorumDenominator;
    uint256 timelockDelay;
    uint256 proposalCount;
    mapping(uint256 => Proposal) proposals;
    mapping(bytes32 => uint256) proposalIds;
    mapping(string => OCFStockClass) ocfStockClasses;
    string[] ocfStockClassIds;
    mapping(address => mapping(string => uint256)) stakeholderShares;
    mapping(address => string[]) stakeholderClasses;
    mapping(string => OCFProposalTypeConfig) ocfProposalTypes;
    string[] ocfProposalTypeIds;
    mapping(uint256 => mapping(string => bool)) proposalVetoes;
    mapping(address => BoardMember) boardMembers;
    address[] activeBoardMembers;
    uint256 boardVotingPeriod;
    uint256 boardQuorum;
    mapping(address => bool) authorizedWallets;
    mapping(address => mapping(bytes4 => ExecutiveApproval)) executiveApprovals;
    string companyName;
    string ocfVersion;
    uint256 lastUpdated;
    address deployer;
    uint256[50] __phase1Gap;
    mapping(string => address) capTableProviders;
    mapping(bytes32 => uint256) syncEventNonces;
    mapping(address => bool) authorizedSyncers;
    mapping(bytes32 => string) legalDocumentHashes;
    mapping(string => bool) approvedLegalTerms;
    uint256[45] __phase2Gap;
    mapping(string => AntiDilutionTerms) antiDilutionTerms;
    mapping(uint256 => uint256) roundValuations;
    mapping(string => TransferRestrictions) transferRestrictions;
    mapping(address => mapping(address => uint256)) transferApprovals;
    LiquidationWaterfall liquidationWaterfall;
    mapping(uint256 => uint256) liquidationEvents;
    mapping(uint256 => mapping(address => uint256)) preemptiveAllocations;
    mapping(uint256 => mapping(address => bool)) preemptiveExercised;
    uint256[40] __phase3Gap;
    mapping(address => string) externalIntegrations;
    mapping(bytes32 => bytes) crossChainData;
    mapping(address => bytes32) complianceHashes;
    mapping(string => bool) regulatoryApprovals;
    uint256[46] __phase4Gap;
    uint256[100] __futureGap;
}
```

