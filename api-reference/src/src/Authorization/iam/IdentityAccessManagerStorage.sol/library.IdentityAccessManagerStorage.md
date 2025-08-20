# IdentityAccessManagerStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Authorization/iam/IdentityAccessManagerStorage.sol)

Contains all storage variables for entity-specific access management

*Storage contract for IdentityAccessManager diamond*


## State Variables
### STORAGE_SLOT

```solidity
bytes32 internal constant STORAGE_SLOT = keccak256("capsign.storage.iam");
```


## Functions
### layout


```solidity
function layout() internal pure returns (Layout storage l);
```

## Structs
### CustomRoleDefinition

```solidity
struct CustomRoleDefinition {
    uint64 roleId;
    string name;
    string description;
    uint32 executionDelay;
    bool isActive;
    uint256 createdAt;
    address createdBy;
}
```

### Layout

```solidity
struct Layout {
    address entity;
    string entityName;
    string entityElfCode;
    mapping(uint64 => CustomRoleDefinition) customRoles;
    uint64[] customRolesList;
    uint64 nextRoleId;
    ISubscriptionVerifier subscriptionManager;
    uint8 requiredTier;
    bool subscriptionEnforced;
    address globalAccessManager;
    mapping(address => bool) managedAssets;
    address[] assetList;
    mapping(string => bool) complianceRules;
    mapping(address => mapping(string => bool)) assetComplianceOverrides;
    mapping(address => mapping(uint64 => address)) roleDelegates;
    mapping(address => uint256) delegationExpiry;
    mapping(address => uint256) lastActivity;
    mapping(uint64 => uint256) roleUsageCount;
    bool entityEmergencyMode;
    uint256 entityEmergencyActivatedAt;
    mapping(bytes4 => bool) entityEmergencyFunctions;
    bool paymasterEnabled;
    uint256 paymasterBalance;
    mapping(address => bool) sponsoredUsers;
    mapping(address => uint256) userDailySpent;
    mapping(address => uint256) userDailyLimit;
    mapping(address => uint256) userLastResetDay;
    mapping(bytes4 => bool) sponsoredOperations;
    mapping(bytes4 => uint256) operationCosts;
    uint256 totalDailyBudget;
    uint256 totalDailySpent;
    uint256 budgetLastResetDay;
    mapping(address => bool) supportedCMXPaymasters;
    address[] cmxPaymasterList;
    bool cmxPaymasterEnabled;
}
```

