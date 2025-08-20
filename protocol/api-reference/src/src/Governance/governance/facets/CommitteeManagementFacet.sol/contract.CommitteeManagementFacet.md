# CommitteeManagementFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/governance/facets/CommitteeManagementFacet.sol)

**Inherits:**
[UnifiedGovernanceBase](/src/Governance/governance/storage/GovernanceStorage.sol/abstract.UnifiedGovernanceBase.md)

Handles basic committee operations within the unified governance framework

*Simplified committee management for unified governance*


## Functions
### addBoardMember

Add a new board member


```solidity
function addBoardMember(address member, string calldata role) external;
```

### removeBoardMember

Remove a board member


```solidity
function removeBoardMember(address member) external;
```

### updateBoardMemberRole

Update a board member's role


```solidity
function updateBoardMemberRole(address member, string calldata newRole) external;
```

### isBoardMember

Check if an address is an active board member


```solidity
function isBoardMember(address account) external view returns (bool);
```

### getBoardMember

Get board member details


```solidity
function getBoardMember(address member)
    external
    view
    returns (
        bool active,
        uint256 appointedAt,
        address appointedBy,
        uint256 totalVotingWeight,
        string memory primaryRole
    );
```

### getActiveBoardMembers

Get all active board members


```solidity
function getActiveBoardMembers() external view returns (address[] memory);
```

### getBoardSize

Get the total number of active board members


```solidity
function getBoardSize() external view returns (uint256);
```

### configureOCFStockClasses


```solidity
function configureOCFStockClasses(GovernanceStorage.OCFStockClass[] calldata stockClasses) external pure override;
```

### configureOCFProposalTypes


```solidity
function configureOCFProposalTypes(GovernanceStorage.OCFProposalTypeConfig[] calldata proposalTypes)
    external
    pure
    override;
```

### updateStakeholderShares


```solidity
function updateStakeholderShares(address stakeholder, string calldata classId, uint256 shares) external pure override;
```

### calculateVotingPower


```solidity
function calculateVotingPower(address stakeholder, string memory proposalType) public pure override returns (uint256);
```

### canVetoProposal


```solidity
function canVetoProposal(address stakeholder, string memory proposalType) public pure override returns (bool);
```

## Events
### BoardMemberAdded

```solidity
event BoardMemberAdded(address indexed member, string role);
```

### BoardMemberRemoved

```solidity
event BoardMemberRemoved(address indexed member);
```

### BoardMemberRoleUpdated

```solidity
event BoardMemberRoleUpdated(address indexed member, string newRole);
```

## Enums
### CommitteeType

```solidity
enum CommitteeType {
    AUDIT,
    COMPENSATION,
    NOMINATING,
    RISK,
    TECHNOLOGY
}
```

