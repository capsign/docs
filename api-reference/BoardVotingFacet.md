# BoardVotingFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/governance/facets/BoardVotingFacet.sol)

**Inherits:**
[IGovernanceDelegate](/src/Wallets/wallet/MultiOwnable.sol/interface.IGovernanceDelegate.md), [UnifiedGovernanceBase](/src/Governance/governance/storage/GovernanceStorage.sol/abstract.UnifiedGovernanceBase.md)

Implements IGovernanceDelegate to own and control corporate wallets

*OCF-based board governance with multi-class voting and corporate wallet control*


## Functions
### onlyBoardMember


```solidity
modifier onlyBoardMember();
```

### onlyActiveGovernance


```solidity
modifier onlyActiveGovernance();
```

### authorizeWallet


```solidity
function authorizeWallet(address wallet) external onlyBoardMember;
```

### deauthorizeWallet


```solidity
function deauthorizeWallet(address wallet) external onlyBoardMember;
```

### canExecuteFor


```solidity
function canExecuteFor(address wallet, bytes4 selector) public view override returns (bool);
```

### executeOnWallet


```solidity
function executeOnWallet(address wallet, bytes calldata data) external override returns (bytes memory);
```

### grantExecutiveApproval


```solidity
function grantExecutiveApproval(
    address executive,
    address targetWallet,
    bytes4 selector,
    uint256 maxValue,
    uint256 validFor
) external onlyBoardMember;
```

### configureOCFStockClasses


```solidity
function configureOCFStockClasses(GovernanceStorage.OCFStockClass[] calldata stockClasses)
    external
    override
    onlyActiveGovernance;
```

### configureOCFProposalTypes


```solidity
function configureOCFProposalTypes(GovernanceStorage.OCFProposalTypeConfig[] calldata proposalTypes)
    external
    override
    onlyActiveGovernance;
```

### updateStakeholderShares


```solidity
function updateStakeholderShares(address stakeholder, string calldata classId, uint256 shares)
    external
    override
    onlyActiveGovernance;
```

### calculateVotingPower


```solidity
function calculateVotingPower(address stakeholder, string memory proposalType)
    public
    view
    override
    returns (uint256 totalVotingPower);
```

### canVetoProposal


```solidity
function canVetoProposal(address stakeholder, string memory proposalType) public view override returns (bool canVeto);
```

### castVoteOCF


```solidity
function castVoteOCF(uint256 proposalId, uint8 support, string calldata proposalType) external;
```

### vetoProposal


```solidity
function vetoProposal(uint256 proposalId, string calldata proposalType) external;
```

### addBoardMember


```solidity
function addBoardMember(address member, string calldata role) external onlyActiveGovernance;
```

### removeBoardMember


```solidity
function removeBoardMember(address member) external onlyActiveGovernance;
```

### _hasRole


```solidity
function _hasRole(address account) internal view returns (bool);
```

## Events
### BoardProposalCreated

```solidity
event BoardProposalCreated(uint256 indexed proposalId, address indexed proposer, string proposalType);
```

### BoardVoteCast

```solidity
event BoardVoteCast(address indexed voter, uint256 indexed proposalId, uint8 support, string role);
```

### ExecutiveApproval

```solidity
event ExecutiveApproval(uint256 indexed proposalId, address indexed executive, string approvalType);
```

### BoardMemberAdded

```solidity
event BoardMemberAdded(address indexed member, string role);
```

### BoardMemberRemoved

```solidity
event BoardMemberRemoved(address indexed member, string role);
```

### ExecutiveAuthorityGranted

```solidity
event ExecutiveAuthorityGranted(address indexed executive, string authority);
```

### WalletAuthorized

```solidity
event WalletAuthorized(address indexed wallet);
```

### WalletDeauthorized

```solidity
event WalletDeauthorized(address indexed wallet);
```

## Enums
### BoardRole

```solidity
enum BoardRole {
    Director,
    IndependentDirector,
    CEO,
    CFO,
    Chairman,
    AuditCommittee,
    CompensationCommittee
}
```

### BoardProposalType

```solidity
enum BoardProposalType {
    EquityGrant,
    ExecutiveCompensation,
    BoardAppointment,
    AuditApproval,
    BudgetApproval,
    StrategicDecision,
    PolicyChange
}
```

