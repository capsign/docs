# DocumentTemplateFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Documents/registry/facets/DocumentTemplateFacet.sol)

Handles template management and instance creation

*Template facet for DocumentRegistry diamond*


## Functions
### registerTemplate

*Register a new template and approve it*


```solidity
function registerTemplate(bytes32 hash, string calldata uri) external returns (bytes32 templateHash);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`hash`|`bytes32`|The hash of the template|
|`uri`|`string`|The URI pointing to the template location|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`templateHash`|`bytes32`|The hash of the registered template|


### createInstance

*Create an instance from a template*


```solidity
function createInstance(bytes32 templateHash, string calldata uri) external returns (bytes32 instanceHash);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`templateHash`|`bytes32`|The hash of the template to use|
|`uri`|`string`|The URI for the instance document|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`instanceHash`|`bytes32`|The hash of the created instance|


### registerInstanceAndSign

*Register instance and sign it in one transaction*


```solidity
function registerInstanceAndSign(bytes32 templateHash, string calldata uri) external returns (bytes32 instanceHash);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`templateHash`|`bytes32`|The hash of the template to use|
|`uri`|`string`|The URI for the instance document|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`instanceHash`|`bytes32`|The hash of the created and signed instance|


### getAllTemplates

*Get all approved templates*


```solidity
function getAllTemplates() external view returns (bytes32[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32[]`|Array of template hashes|


### isTemplateApproved

*Check if a template is approved*


```solidity
function isTemplateApproved(bytes32 templateHash) external view returns (bool approved);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`templateHash`|`bytes32`|The template hash to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`approved`|`bool`|Whether the template is approved|


### getTemplateOwner

*Get template owner*


```solidity
function getTemplateOwner(bytes32 templateHash) external view returns (address owner);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`templateHash`|`bytes32`|The template hash|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The template owner address|


### getTemplateInstances

*Get all instances created from a template*


```solidity
function getTemplateInstances(bytes32 templateHash) external view returns (bytes32[] memory instances);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`templateHash`|`bytes32`|The template hash|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`instances`|`bytes32[]`|Array of instance hashes|


### revokeTemplate

*Revoke a template (only template owner)*


```solidity
function revokeTemplate(bytes32 templateHash) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`templateHash`|`bytes32`|The template hash to revoke|


## Events
### TemplateApproved

```solidity
event TemplateApproved(bytes32 indexed templateHash, address indexed owner);
```

### TemplateRevoked

```solidity
event TemplateRevoked(bytes32 indexed templateHash, address indexed owner);
```

### InstanceCreated

```solidity
event InstanceCreated(bytes32 indexed instanceHash, bytes32 indexed templateHash, address indexed owner);
```

### InstanceSignedAndRegistered

```solidity
event InstanceSignedAndRegistered(bytes32 indexed instanceHash, bytes32 indexed templateHash, address indexed signer);
```

