# IGovernanceFactory
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/factory/interfaces/IGovernanceFactory.sol)

Interface for deploying governance contracts


## Functions
### deployGovernance

*Deploy a governance contract from a template*


```solidity
function deployGovernance(string memory templateName, bytes memory deploymentData)
    external
    returns (address governanceContract);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`templateName`|`string`|Name of the governance template to deploy|
|`deploymentData`|`bytes`|Encoded deployment configuration|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`governanceContract`|`address`|Address of the deployed governance contract|


### getAvailableTemplates

*Get available governance templates*


```solidity
function getAvailableTemplates() external view returns (string[] memory templates);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`templates`|`string[]`|Array of available template names|


### isTemplateAvailable

*Check if a template is available*


```solidity
function isTemplateAvailable(string memory templateName) external view returns (bool available);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`templateName`|`string`|Name of the template|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`available`|`bool`|True if template exists and is deployable|


### getDeploymentCost

*Get deployment cost for a template*


```solidity
function getDeploymentCost(string memory templateName) external view returns (uint256 cost);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`templateName`|`string`|Name of the template|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`cost`|`uint256`|Deployment cost in wei|


