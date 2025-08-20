# TokenholderVotingFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/governance/facets/TokenholderVotingFacet.sol)

**Inherits:**
[IGovernanceDelegate](/src/Wallets/wallet/MultiOwnable.sol/interface.IGovernanceDelegate.md), [UnifiedGovernanceBase](/src/Governance/governance/storage/GovernanceStorage.sol/abstract.UnifiedGovernanceBase.md)

Implements IGovernanceDelegate to own and control corporate wallets

*OCF-based tokenholder governance with lot-based voting*


## Functions
### onlyTokenholder


```solidity
modifier onlyTokenholder();
```

### onlyActiveGovernance


```solidity
modifier onlyActiveGovernance();
```

### authorizeWallet


```solidity
function authorizeWallet(address wallet) external onlyTokenholder;
```

### deauthorizeWallet


```solidity
function deauthorizeWallet(address wallet) external onlyTokenholder;
```

### canExecuteFor


```solidity
function canExecuteFor(address wallet, bytes4 selector) public view override returns (bool);
```

### executeOnWallet


```solidity
function executeOnWallet(address wallet, bytes calldata data) external override returns (bytes memory);
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

### castVoteWithLots


```solidity
function castVoteWithLots(
    uint256 proposalId,
    uint8 support,
    bytes32[] calldata lotIds,
    address assetContract,
    string calldata proposalType
) external;
```

### vetoProposal


```solidity
function vetoProposal(uint256 proposalId, string calldata proposalType) external;
```

### createTokenholderProposal


```solidity
function createTokenholderProposal(
    TokenholderProposalType proposalType,
    address[] memory targets,
    uint256[] memory values,
    bytes[] memory calldatas,
    string memory description,
    string memory proposalTypeName
) external onlyTokenholder returns (uint256 proposalId);
```

### _hasGovernanceRole


```solidity
function _hasGovernanceRole(address account) internal view returns (bool);
```

## Events
### TokenholderProposalCreated

```solidity
event TokenholderProposalCreated(uint256 indexed proposalId, address indexed proposer, string proposalType);
```

### LotVoteCast

```solidity
event LotVoteCast(
    address indexed voter, uint256 indexed proposalId, bytes32[] lotIds, uint8 support, uint256 totalWeight
);
```

### VotingPowerDelegated

```solidity
event VotingPowerDelegated(address indexed delegator, address indexed delegate, uint256 amount);
```

### VotingPowerUndelegated

```solidity
event VotingPowerUndelegated(address indexed delegator, address indexed delegate, uint256 amount);
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
### TokenholderProposalType

```solidity
enum TokenholderProposalType {
    ShareholderResolution,
    MajorTransaction,
    ChartAmendment,
    EquityRound,
    LiquidationEvent,
    DirectorElection
}
```

