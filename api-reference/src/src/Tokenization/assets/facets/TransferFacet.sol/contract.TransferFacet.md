# TransferFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/facets/TransferFacet.sol)

Facet for transferring lots and managing approvals in IssuedAsset

*Diamond facet providing transfer functionality*


## Functions
### transfer

Transfer a lot or partial lot from sender to recipient


```solidity
function transfer(
    bytes32 lotId,
    address to,
    uint256 quantity,
    IIssuedAsset.TransferType tType,
    uint256 newCostBasis,
    string memory uri,
    bytes memory data
) external returns (bytes32 newLotId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The ID of the lot to transfer from|
|`to`|`address`|The new owner|
|`quantity`|`uint256`|The quantity to transfer|
|`tType`|`IIssuedAsset.TransferType`|The type of transfer (SALE, GIFT, etc.)|
|`newCostBasis`|`uint256`|The new cost basis (used for SALE, INHERITANCE, REWARD)|
|`uri`|`string`|The URI of the lot|
|`data`|`bytes`|Additional data associated with the transfer|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`newLotId`|`bytes32`|The ID of the new lot created for the recipient|


### transferFrom

Transfer a lot or partial lot from 'from' to 'to'


```solidity
function transferFrom(
    bytes32 lotId,
    address from,
    address to,
    uint256 quantity,
    IIssuedAsset.TransferType tType,
    uint256 newCostBasis,
    string memory uri,
    bytes memory data
) external returns (bytes32 newLotId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The ID of the lot to transfer from|
|`from`|`address`|The current owner of the lot|
|`to`|`address`|The new owner|
|`quantity`|`uint256`|The quantity to transfer|
|`tType`|`IIssuedAsset.TransferType`|The type of transfer (SALE, GIFT, etc.)|
|`newCostBasis`|`uint256`|The new cost basis (used for SALE, INHERITANCE, REWARD)|
|`uri`|`string`|The URI of the lot|
|`data`|`bytes`|Additional data associated with the transfer|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`newLotId`|`bytes32`|The ID of the new lot created for the recipient|


### approve

Approve a spender to transfer a specific amount from a lot


```solidity
function approve(bytes32 lotId, address spender, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID to approve spending from|
|`spender`|`address`|The address to approve|
|`amount`|`uint256`|The amount to approve|


### setApprovalForAll

Set or unset approval for all lots owned by the caller


```solidity
function setApprovalForAll(address operator, bool approved) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`operator`|`address`|The operator to approve or revoke|
|`approved`|`bool`|Whether to approve or revoke the operator|


### getApproved

Get the approved amount for a spender on a specific lot


```solidity
function getApproved(bytes32 lotId, address spender) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID to check|
|`spender`|`address`|The spender to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The approved amount|


### isApprovedForAll

Check if an operator is approved for all lots of an owner


```solidity
function isApprovedForAll(address owner, address operator) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner to check|
|`operator`|`address`|The operator to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the operator is approved for all|


### _performTransfer

Internal function to perform the actual transfer logic


```solidity
function _performTransfer(
    bytes32 lotId,
    address from,
    address to,
    uint256 quantity,
    IIssuedAsset.TransferType tType,
    uint256 newCostBasis,
    string memory uri,
    bytes memory data
) internal returns (bytes32 newLotId);
```

### mergeLots

Merge multiple lots into a single new lot


```solidity
function mergeLots(bytes32[] calldata sourceLotIds, string memory uri, bytes memory data)
    external
    returns (bytes32 newLotId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`sourceLotIds`|`bytes32[]`|Array of lot IDs to merge|
|`uri`|`string`|URI for the new merged lot|
|`data`|`bytes`|Additional data for the new lot|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`newLotId`|`bytes32`|The ID of the new merged lot|


### _validateCompliance

Validate transfer compliance through ComplianceFacet


```solidity
function _validateCompliance(address from, address to, bytes32 lotId, uint256 quantity) internal view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address|
|`to`|`address`|The recipient address|
|`lotId`|`bytes32`|The lot ID being transferred|
|`quantity`|`uint256`|The quantity being transferred|


### _processCompliance

Process transfer through compliance conditions


```solidity
function _processCompliance(address from, address to, bytes32 lotId, uint256 quantity, bytes memory data) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address|
|`to`|`address`|The recipient address|
|`lotId`|`bytes32`|The lot ID being transferred|
|`quantity`|`uint256`|The quantity being transferred|
|`data`|`bytes`|Additional transfer data|


## Events
### LotTransferred

```solidity
event LotTransferred(
    bytes32 indexed lotId,
    address indexed from,
    address indexed to,
    uint256 quantity,
    string uri,
    bytes data,
    IIssuedAsset.TransferType tType,
    uint256 newCostBasis
);
```

### LotApproval

```solidity
event LotApproval(bytes32 indexed lotId, address indexed owner, address indexed spender, uint256 amount);
```

### ApprovalForAll

```solidity
event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
```

## Errors
### Transfer_NotCompliant

```solidity
error Transfer_NotCompliant();
```

### Transfer_ComplianceCallFailed

```solidity
error Transfer_ComplianceCallFailed();
```

