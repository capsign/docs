# IdentityAccessManagerGovernanceFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Authorization/iam/facets/IdentityAccessManagerGovernanceFacet.sol)

Integrates with existing governance infrastructure to provide full governance capabilities

*Orchestrates deployment and management of actual governance contracts for IAM wallets*


## State Variables
### _walletGovernance

```solidity
mapping(address => mapping(address => GovernanceDeployment)) private _walletGovernance;
```


### _governedWallets

```solidity
mapping(address => address[]) private _governedWallets;
```


### _governanceToIAM

```solidity
mapping(address => address) private _governanceToIAM;
```


### _eamGovernanceFactory

```solidity
mapping(address => address) private _eamGovernanceFactory;
```


## Functions
### constructor


```solidity
constructor();
```

### setGovernanceFactory

*Set the governance factory for this IAM*


```solidity
function setGovernanceFactory(address factory) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`factory`|`address`|Address of the GovernanceFactory contract|


### getGovernanceFactory

*Get the governance factory for this IAM*


```solidity
function getGovernanceFactory() external view returns (address);
```

### deploySimpleApproval

*Deploy Simple Approval governance for a wallet*


```solidity
function deploySimpleApproval(address wallet, ApprovalWorkflowConfig calldata config)
    external
    returns (address governanceContract);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`wallet`|`address`|The wallet address to govern|
|`config`|`ApprovalWorkflowConfig`|Approval workflow configuration|


### deployMultiSig

*Deploy Multi-Signature governance for a wallet*


```solidity
function deployMultiSig(address wallet, MultiSigConfig calldata config) external returns (address governanceContract);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`wallet`|`address`|The wallet address to govern|
|`config`|`MultiSigConfig`|Multi-signature configuration|


### deployBoardGovernance

*Deploy Board Governance for a wallet*


```solidity
function deployBoardGovernance(address wallet, BoardGovernanceConfig calldata config)
    external
    returns (address governanceContract);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`wallet`|`address`|The wallet address to govern|
|`config`|`BoardGovernanceConfig`|Board governance configuration|


### deployTokenholderGovernance

*Deploy Tokenholder Governance for a wallet*


```solidity
function deployTokenholderGovernance(address wallet, TokenholderGovernanceConfig calldata config)
    external
    returns (address governanceContract);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`wallet`|`address`|The wallet address to govern|
|`config`|`TokenholderGovernanceConfig`|Tokenholder governance configuration|


### activateGovernance

*Activate governance for a wallet*


```solidity
function activateGovernance(address wallet) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`wallet`|`address`|The wallet address|


### deactivateGovernance

*Deactivate governance for a wallet (emergency use)*


```solidity
function deactivateGovernance(address wallet) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`wallet`|`address`|The wallet address|


### updateGovernance

*Update governance configuration (deploys new governance contract)*


```solidity
function updateGovernance(address wallet, GovernanceType newGovernanceType, bytes calldata deploymentData)
    external
    returns (address newGovernanceContract);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`wallet`|`address`|The wallet address|
|`newGovernanceType`|`GovernanceType`|New governance type|
|`deploymentData`|`bytes`|Configuration data for new governance|


### requestApproval

*Request approval for a wallet operation*


```solidity
function requestApproval(address wallet, string calldata operation, bytes calldata callData)
    external
    returns (uint256 proposalId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`wallet`|`address`|The wallet address|
|`operation`|`string`|Description of the operation|
|`callData`|`bytes`|The call data to execute|


### executeApproval

*Execute an approved operation*


```solidity
function executeApproval(address wallet, uint256 proposalId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`wallet`|`address`|The wallet address|
|`proposalId`|`uint256`|The proposal ID|


### getGovernanceDeployment

*Get governance deployment for a wallet*


```solidity
function getGovernanceDeployment(address wallet) external view returns (GovernanceDeployment memory);
```

### getGovernedWallets

*Get all wallets governed by this IAM*


```solidity
function getGovernedWallets() external view returns (address[] memory);
```

### isGovernanceActive

*Check if governance is active for a wallet*


```solidity
function isGovernanceActive(address wallet) external view returns (bool);
```

### getGovernanceType

*Get governance type for a wallet*


```solidity
function getGovernanceType(address wallet) external view returns (GovernanceType);
```

### getGovernanceContract

*Get governance contract address for a wallet*


```solidity
function getGovernanceContract(address wallet) external view returns (address);
```

### _storeGovernanceDeployment


```solidity
function _storeGovernanceDeployment(
    address wallet,
    address governanceContract,
    GovernanceType governanceType,
    bytes32 configHash
) internal;
```

### _ensureNoExistingGovernance


```solidity
function _ensureNoExistingGovernance(address wallet) internal view;
```

### _ensureWalletOwnership


```solidity
function _ensureWalletOwnership(address wallet) internal view;
```

### _ensureIdentityAccess


```solidity
function _ensureIdentityAccess() internal view;
```

### _getTemplateName


```solidity
function _getTemplateName(GovernanceType governanceType) internal pure returns (string memory);
```

## Events
### GovernanceDeployed

```solidity
event GovernanceDeployed(
    address indexed iam,
    address indexed wallet,
    address indexed governanceContract,
    GovernanceType governanceType,
    bytes32 configHash
);
```

### GovernanceUpdated

```solidity
event GovernanceUpdated(
    address indexed iam, address indexed wallet, address indexed newGovernanceContract, address oldGovernanceContract
);
```

### GovernanceActivated

```solidity
event GovernanceActivated(address indexed iam, address indexed wallet, address indexed governanceContract);
```

### GovernanceDeactivated

```solidity
event GovernanceDeactivated(address indexed iam, address indexed wallet, address indexed governanceContract);
```

### ApprovalRequested

```solidity
event ApprovalRequested(
    address indexed iam,
    address indexed wallet,
    address indexed governance,
    uint256 proposalId,
    string operation,
    bytes callData
);
```

### ApprovalGranted

```solidity
event ApprovalGranted(address indexed iam, address indexed wallet, uint256 proposalId, address approver);
```

### GovernanceFactoryUpdated

```solidity
event GovernanceFactoryUpdated(address indexed iam, address indexed factory);
```

## Errors
### GovernanceFactoryNotSet

```solidity
error GovernanceFactoryNotSet();
```

### WalletNotDeployed

```solidity
error WalletNotDeployed();
```

### GovernanceAlreadyExists

```solidity
error GovernanceAlreadyExists();
```

### GovernanceNotFound

```solidity
error GovernanceNotFound();
```

### InvalidGovernanceType

```solidity
error InvalidGovernanceType();
```

### InvalidConfiguration

```solidity
error InvalidConfiguration();
```

### UnauthorizedGovernanceAccess

```solidity
error UnauthorizedGovernanceAccess();
```

### GovernanceNotActive

```solidity
error GovernanceNotActive();
```

### ApprovalTimeout

```solidity
error ApprovalTimeout();
```

### InsufficientSignatures

```solidity
error InsufficientSignatures();
```

## Structs
### GovernanceDeployment

```solidity
struct GovernanceDeployment {
    GovernanceType governanceType;
    address governanceContract;
    address wallet;
    uint256 deployedAt;
    bool active;
    bytes32 configHash;
}
```

### ApprovalWorkflowConfig

```solidity
struct ApprovalWorkflowConfig {
    address approver;
    uint256 approvalTimeout;
    bool requiresConfirmation;
    string[] approvedOperations;
}
```

### MultiSigConfig

```solidity
struct MultiSigConfig {
    address[] signers;
    uint256 requiredSignatures;
    uint256 timeout;
    bool allowOwnerExecution;
}
```

### BoardGovernanceConfig

```solidity
struct BoardGovernanceConfig {
    string name;
    address[] boardMembers;
    string[] boardRoles;
    uint256 boardVotingPeriod;
    uint256 boardQuorum;
    address[] committees;
    bool requiresUnanimous;
}
```

### TokenholderGovernanceConfig

```solidity
struct TokenholderGovernanceConfig {
    address[] governors;
    uint256 votingPeriod;
    uint256 quorumPercentage;
    uint256 proposalThreshold;
    address timelockController;
    bool useTokenWeighting;
}
```

## Enums
### GovernanceType

```solidity
enum GovernanceType {
    None,
    SimpleApproval,
    MultiSig,
    BoardGovernance,
    TokenholderGovernance
}
```

