# OfferingCoreFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Offerings/offering/facets/OfferingCoreFacet.sol)

**Inherits:**
[IOfferingEvents](/src/Offerings/offering/interfaces/IOfferingEvents.sol/interface.IOfferingEvents.md)

*Core facet for offering diamonds that handles basic offering operations*

*Now includes event bubbling to diamond level for subgraph indexing*


## Functions
### initialize

*Initialize the offering diamond*


```solidity
function initialize(
    address _entity,
    address _asset,
    address _paymentCurrency,
    uint256 _pricePerToken,
    uint256 _minInvestment,
    uint256 _investmentDeadline,
    uint256 _targetAmount,
    uint256 _maxAmount,
    string memory _baseURI
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_entity`|`address`|The entity address|
|`_asset`|`address`|The asset being offered|
|`_paymentCurrency`|`address`|The payment currency|
|`_pricePerToken`|`uint256`|Price per token|
|`_minInvestment`|`uint256`|Minimum investment amount|
|`_investmentDeadline`|`uint256`|Investment deadline|
|`_targetAmount`|`uint256`|Target amount to raise|
|`_maxAmount`|`uint256`|Maximum amount to raise|
|`_baseURI`|`string`|Base URI for offering metadata|


### closeOffering

*Closes the offering, preventing further investments.*


```solidity
function closeOffering() external;
```

### setPricePerToken

Set the price per token for this offering.


```solidity
function setPricePerToken(uint256 newPrice) external;
```

### setMinInvestment

Set the minimum investment per investor.


```solidity
function setMinInvestment(uint256 newMin) external;
```

### setInvestmentDeadline

Set an investment deadline (UTC timestamp).
Setting this to 0 effectively removes the deadline.


```solidity
function setInvestmentDeadline(uint256 newDeadline) external;
```

### extendInvestmentDeadline

Extend the current investment deadline to newDeadline (which must be later).


```solidity
function extendInvestmentDeadline(uint256 newDeadline) external;
```

### setTargetAmount

Set a target amount for this offering (informational/tracking only).


```solidity
function setTargetAmount(uint256 newTarget) external;
```

### setMaxAmount

Set a maximum total investment allowed in this offering.
Setting this to 0 removes any max limit.


```solidity
function setMaxAmount(uint256 newMax) external;
```

### setBaseURI

The entity can update the URI pointing to any relevant data.


```solidity
function setBaseURI(string calldata newURI) external;
```

### setDocumentRegistry

Sets the document registry contract address


```solidity
function setDocumentRegistry(address _documentRegistry) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_documentRegistry`|`address`|Address of the document registry contract|


### getOfferingInfo

Get offering basic information


```solidity
function getOfferingInfo()
    external
    view
    returns (
        IOffering.Status status,
        address entity,
        address asset,
        address paymentCurrency,
        uint256 pricePerToken,
        uint256 totalInvested
    );
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`status`|`IOffering.Status`|Current offering status|
|`entity`|`address`|Entity address|
|`asset`|`address`|Asset being offered|
|`paymentCurrency`|`address`|Payment currency|
|`pricePerToken`|`uint256`|Current price per token|
|`totalInvested`|`uint256`|Total amount invested so far|


### getInvestmentParams

Get offering investment parameters


```solidity
function getInvestmentParams()
    external
    view
    returns (uint256 minInvestment, uint256 investmentDeadline, uint256 targetAmount, uint256 maxAmount);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`minInvestment`|`uint256`|Minimum investment amount|
|`investmentDeadline`|`uint256`|Investment deadline|
|`targetAmount`|`uint256`|Target amount to raise|
|`maxAmount`|`uint256`|Maximum amount to raise|


### _requireEntityOrAgent

*Internal function to check if caller is entity or authorized agent*


```solidity
function _requireEntityOrAgent(OfferingStorage.Layout storage s) internal view;
```

### _requestOfferingOperatorRole

*Request OFFERING_OPERATOR_ROLE from the asset's IAM*


```solidity
function _requestOfferingOperatorRole(address _asset) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_asset`|`address`|The asset contract address|


