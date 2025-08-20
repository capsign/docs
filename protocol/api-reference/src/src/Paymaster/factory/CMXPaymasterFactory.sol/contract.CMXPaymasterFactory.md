# CMXPaymasterFactory
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Paymaster/factory/CMXPaymasterFactory.sol)

Factory for deploying CMX paymaster diamonds

*Creates fully configured paymaster diamonds with proper facets and initialization*


## State Variables
### PROTOCOL_ADMIN
Address of the protocol admin


```solidity
address public immutable PROTOCOL_ADMIN;
```


### paymasterTemplate
Template diamond for paymaster deployment


```solidity
address public paymasterTemplate;
```


### deployedPaymasters
Mapping of deployed paymasters


```solidity
mapping(address => bool) public deployedPaymasters;
```


### allPaymasters
Array of all deployed paymasters


```solidity
address[] public allPaymasters;
```


### authorizedDeployers
Authorized deployers


```solidity
mapping(address => bool) public authorizedDeployers;
```


## Functions
### constructor

Initialize the CMX paymaster factory


```solidity
constructor(address protocolAdmin, address initialTemplate);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`protocolAdmin`|`address`|Address of the protocol admin|
|`initialTemplate`|`address`|Initial paymaster template address|


### onlyProtocolAdmin


```solidity
modifier onlyProtocolAdmin();
```

### onlyAuthorizedDeployer


```solidity
modifier onlyAuthorizedDeployer();
```

### deployCMXPaymaster

Deploy a new CMX paymaster diamond


```solidity
function deployCMXPaymaster(
    address cmxToken,
    address entryPoint,
    address treasury,
    uint256 initialExchangeRate,
    address[] calldata supportedFactories
) external onlyAuthorizedDeployer returns (address paymaster);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`cmxToken`|`address`|CMX token address|
|`entryPoint`|`address`|EntryPoint address|
|`treasury`|`address`|Treasury address for fee collection|
|`initialExchangeRate`|`uint256`|Initial CMX/ETH exchange rate (CMX per ETH, 18 decimals)|
|`supportedFactories`|`address[]`|Initial list of supported factories|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`paymaster`|`address`|Address of the deployed paymaster|


### deployMinimalCMXPaymaster

Deploy a minimal CMX paymaster with default settings


```solidity
function deployMinimalCMXPaymaster(address cmxToken, address entryPoint, address treasury)
    external
    onlyAuthorizedDeployer
    returns (address paymaster);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`cmxToken`|`address`|CMX token address|
|`entryPoint`|`address`|EntryPoint address|
|`treasury`|`address`|Treasury address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`paymaster`|`address`|Address of the deployed paymaster|


### updatePaymasterTemplate

Update paymaster template


```solidity
function updatePaymasterTemplate(address newTemplate) external onlyProtocolAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newTemplate`|`address`|New template address|


### authorizeDeployer

Authorize a deployer


```solidity
function authorizeDeployer(address deployer) external onlyProtocolAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`deployer`|`address`|Address to authorize|


### revokeDeployerAuthorization

Revoke deployer authorization


```solidity
function revokeDeployerAuthorization(address deployer) external onlyProtocolAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`deployer`|`address`|Address to revoke|


### getAllPaymasters

Get all deployed paymasters


```solidity
function getAllPaymasters() external view returns (address[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of paymaster addresses|


### getPaymasterCount

Get number of deployed paymasters


```solidity
function getPaymasterCount() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Number of deployed paymasters|


### isDeployedPaymaster

Check if an address is a deployed paymaster


```solidity
function isDeployedPaymaster(address paymaster) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paymaster`|`address`|Address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if it's a deployed paymaster|


### _validateDeploymentParams

Validate deployment parameters


```solidity
function _validateDeploymentParams(address cmxToken, address entryPoint, address treasury, uint256 exchangeRate)
    internal
    view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`cmxToken`|`address`|CMX token address|
|`entryPoint`|`address`|EntryPoint address|
|`treasury`|`address`|Treasury address|
|`exchangeRate`|`uint256`|Exchange rate|


### _deployPaymasterDiamond

Deploy a new paymaster diamond


```solidity
function _deployPaymasterDiamond() internal returns (address paymaster);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`paymaster`|`address`|Address of the deployed diamond|


### _createPaymasterFacetCuts

Create facet cuts for paymaster diamond


```solidity
function _createPaymasterFacetCuts() internal returns (IDiamond.FacetCut[] memory facetCuts);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`facetCuts`|`IDiamond.FacetCut[]`|Array of facet cuts|


### _initializePaymaster

Initialize the deployed paymaster with basic configuration


```solidity
function _initializePaymaster(
    address paymaster,
    address cmxToken,
    address entryPoint,
    address treasury,
    uint256 exchangeRate,
    address[] calldata supportedFactories
) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paymaster`|`address`|Paymaster address|
|`cmxToken`|`address`|CMX token address|
|`entryPoint`|`address`|EntryPoint address|
|`treasury`|`address`|Treasury address|
|`exchangeRate`|`uint256`|Initial exchange rate|
|`supportedFactories`|`address[]`|Initial supported factories|


### _getAccessControlSelectors

Get access control function selectors


```solidity
function _getAccessControlSelectors() internal pure returns (bytes4[] memory selectors);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`selectors`|`bytes4[]`|Array of function selectors|


### _getPaymasterValidationSelectors

Get paymaster validation function selectors


```solidity
function _getPaymasterValidationSelectors() internal pure returns (bytes4[] memory selectors);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`selectors`|`bytes4[]`|Array of function selectors|


### _getPaymasterManagementSelectors

Get paymaster management function selectors


```solidity
function _getPaymasterManagementSelectors() internal pure returns (bytes4[] memory selectors);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`selectors`|`bytes4[]`|Array of function selectors|


## Events
### CMXPaymasterDeployed
Emitted when a new CMX paymaster is deployed


```solidity
event CMXPaymasterDeployed(
    address indexed paymaster, address indexed deployer, address indexed cmxToken, address entryPoint, address treasury
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paymaster`|`address`|Address of the deployed paymaster|
|`deployer`|`address`|Address that deployed the paymaster|
|`cmxToken`|`address`|CMX token address|
|`entryPoint`|`address`|EntryPoint address|
|`treasury`|`address`|Treasury address for fee collection|

### PaymasterTemplateUpdated
Emitted when paymaster template is updated


```solidity
event PaymasterTemplateUpdated(address indexed oldTemplate, address indexed newTemplate);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oldTemplate`|`address`|Previous template address|
|`newTemplate`|`address`|New template address|

## Errors
### InvalidCMXToken

```solidity
error InvalidCMXToken(address token);
```

### InvalidEntryPoint

```solidity
error InvalidEntryPoint(address entryPoint);
```

### InvalidTreasury

```solidity
error InvalidTreasury(address treasury);
```

### InvalidExchangeRate

```solidity
error InvalidExchangeRate(uint256 rate);
```

### UnauthorizedDeployer

```solidity
error UnauthorizedDeployer(address deployer);
```

### PaymasterDeploymentFailed

```solidity
error PaymasterDeploymentFailed(string reason);
```

