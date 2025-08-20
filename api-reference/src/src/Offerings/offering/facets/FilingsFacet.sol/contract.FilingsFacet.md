# FilingsFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Offerings/offering/facets/FilingsFacet.sol)

Facet for tracking regulatory filings in offering diamonds

*Supports federal, state, and local filings with links to supporting documentation*


## Functions
### setFilingDocumentRegistry

Set the document registry for filings


```solidity
function setFilingDocumentRegistry(address _documentRegistry) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_documentRegistry`|`address`|Address of the document registry|


### addFiling

Add a new filing record


```solidity
function addFiling(
    string calldata filingType,
    string calldata filingId,
    string calldata jurisdiction,
    bytes32 documentHash
) external returns (bytes32 id);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`filingType`|`string`|The type of filing (e.g. "Form D", "Blue Sky")|
|`filingId`|`string`|The filing ID or reference number|
|`jurisdiction`|`string`|The jurisdiction where the filing is made|
|`documentHash`|`bytes32`|Hash of the filing document in DocumentRegistry (optional, can be bytes32(0))|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`id`|`bytes32`|The generated filing ID|


### amendFiling

Amend an existing filing


```solidity
function amendFiling(bytes32 filingId, string calldata newFilingRefId, bytes32 documentHash) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`filingId`|`bytes32`|The ID of the filing to amend|
|`newFilingRefId`|`string`|The new filing ID or reference|
|`documentHash`|`bytes32`|Hash of the amendment document (optional)|


### deactivateFiling

Deactivate a filing


```solidity
function deactivateFiling(bytes32 filingId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`filingId`|`bytes32`|The ID of the filing to deactivate|


### reactivateFiling

Reactivate a filing


```solidity
function reactivateFiling(bytes32 filingId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`filingId`|`bytes32`|The ID of the filing to reactivate|


### getAllFilingIds

Get all filing IDs


```solidity
function getAllFilingIds() external view returns (bytes32[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32[]`|Array of filing IDs|


### getFiling

Get a filing by ID


```solidity
function getFiling(bytes32 filingId)
    external
    view
    returns (
        string memory filingType,
        string memory id,
        string memory jurisdiction,
        uint256 filingDate,
        uint256 amendmentCount,
        bool isActive,
        bytes32 documentHash
    );
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`filingId`|`bytes32`|The ID of the filing|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`filingType`|`string`|The type of filing|
|`id`|`string`|The filing ID or reference number|
|`jurisdiction`|`string`|The jurisdiction where filed|
|`filingDate`|`uint256`|The date the filing was made|
|`amendmentCount`|`uint256`|The number of amendments|
|`isActive`|`bool`|Whether the filing is active|
|`documentHash`|`bytes32`|The hash of the associated document|


### getFilingsByJurisdiction

Get all filings for a jurisdiction


```solidity
function getFilingsByJurisdiction(string calldata jurisdiction) external view returns (bytes32[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`jurisdiction`|`string`|The jurisdiction to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32[]`|Array of filing IDs|


### getFilingsByType

Get all filings of a specific type


```solidity
function getFilingsByType(string calldata filingType) external view returns (bytes32[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`filingType`|`string`|The filing type to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32[]`|Array of filing IDs|


### isFilingActive

Check if a filing exists and is active


```solidity
function isFilingActive(bytes32 filingId) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`filingId`|`bytes32`|The ID of the filing|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|true if filing exists and is active|


### getFilingDocumentURI

Get the document URI for a filing if available


```solidity
function getFilingDocumentURI(bytes32 filingId) external view returns (string memory documentURI);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`filingId`|`bytes32`|The ID of the filing|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`documentURI`|`string`|The URI of the document or empty string if not available|


### getFilingDocumentRegistry

Get the filing document registry address


```solidity
function getFilingDocumentRegistry() external view returns (address registry);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`registry`|`address`|The address of the filing document registry|


### _requireEntity

*Internal function to check if caller is the entity*


```solidity
function _requireEntity(OfferingStorage.Layout storage s) internal view;
```

## Events
### FilingAdded

```solidity
event FilingAdded(
    bytes32 indexed filingId,
    string filingType,
    string filingReference,
    string jurisdiction,
    uint256 filingDate,
    bytes32 documentHash
);
```

### FilingAmended

```solidity
event FilingAmended(
    bytes32 indexed filingId,
    string filingType,
    string filingReference,
    uint256 filingDate,
    uint256 amendmentCount,
    bytes32 documentHash
);
```

### FilingDeactivated

```solidity
event FilingDeactivated(bytes32 indexed filingId);
```

### FilingReactivated

```solidity
event FilingReactivated(bytes32 indexed filingId);
```

### FilingDocumentRegistryUpdated

```solidity
event FilingDocumentRegistryUpdated(address indexed oldRegistry, address indexed newRegistry);
```

