# OfferingInvestmentFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Offerings/offering/facets/OfferingInvestmentFacet.sol)

*Investment management facet for offering diamonds*


## Functions
### invest

*Allows an investor to invest in this offering.
Implementation differs for public vs exempt offerings.*


```solidity
function invest(uint256 _amount, uint256 _pricePerToken, bytes32 instanceHash) external;
```

### recordOffChainInvestment

*Records an off-chain investment (payment received outside the blockchain)*


```solidity
function recordOffChainInvestment(
    address investor,
    uint256 amount,
    uint256 investmentDate,
    uint256 _pricePerToken,
    bytes32 instanceHash
) external returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The address of the investor|
|`amount`|`uint256`|The investment amount in payment currency|
|`investmentDate`|`uint256`|The timestamp when the investment was made|
|`_pricePerToken`|`uint256`|The price per token at the time of investment|
|`instanceHash`|`bytes32`|The hash of the agreement (can be 0 for off-chain)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|investmentId The ID of the recorded investment|


### countersign

*Countersign the investment for a specific investor*


```solidity
function countersign(uint256 investmentId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investmentId`|`uint256`|The ID of the investment to countersign|


### rejectInvestment

Allows the entity to reject a specific investment and refund the investor


```solidity
function rejectInvestment(uint256 investmentId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investmentId`|`uint256`|The ID of the investment to reject|


### getInvestment

Get investment details


```solidity
function getInvestment(uint256 investmentId) external view returns (IOffering.Investment memory investment);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investmentId`|`uint256`|The investment ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`investment`|`IOffering.Investment`|The investment details|


### getInvestmentInvestor

Get investor for investment ID


```solidity
function getInvestmentInvestor(uint256 investmentId) external view returns (address investor);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investmentId`|`uint256`|The investment ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|


### getNextInvestmentId

Get next investment ID


```solidity
function getNextInvestmentId() external view returns (uint256 nextId);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`nextId`|`uint256`|The next investment ID|


### _preInvestmentChecks

*Internal function to validate investment before processing*


```solidity
function _preInvestmentChecks(OfferingStorage.Layout storage s, uint256 _amount, uint256 _pricePerToken, bytes32)
    internal
    view;
```

### _createLotTokensToInvestor

*Internal function to create tokens for investor*


```solidity
function _createLotTokensToInvestor(OfferingStorage.Layout storage s, address investor, uint256 amount) internal;
```

### _createLotTokensToInvestorWithPrice

*Internal function to create tokens for investor with specific price*


```solidity
function _createLotTokensToInvestorWithPrice(
    OfferingStorage.Layout storage s,
    address investor,
    uint256 amount,
    uint256 pricePerToken
) internal;
```

### _completeOfferingInternal

*Internal function to complete offering*


```solidity
function _completeOfferingInternal(OfferingStorage.Layout storage s) internal;
```

### _validateOffChainInvestment

*Validates an off-chain investment according to compliance rules.
To be implemented by specialized facets.*


```solidity
function _validateOffChainInvestment(address investor, uint256 amount, uint256 investmentDate) internal virtual;
```

### _beforeCountersign

*Hook called before countersigning*


```solidity
function _beforeCountersign(address investor) internal virtual;
```

### _afterCountersign

*Hook called after countersigning*


```solidity
function _afterCountersign(address investor) internal virtual;
```

### _requireEntityOrAgent

*Internal function to check if caller is entity or authorized agent*


```solidity
function _requireEntityOrAgent(OfferingStorage.Layout storage s) internal view;
```

## Events
### OfferingInvested

```solidity
event OfferingInvested(
    address indexed investor, uint256 indexed investmentId, uint256 amount, uint256 pricePerToken, bytes32 instanceHash
);
```

### OfferingInvestmentCountersigned

```solidity
event OfferingInvestmentCountersigned(address indexed investor, uint256 indexed investmentId, uint256 amount);
```

### OffChainInvestmentRecorded

```solidity
event OffChainInvestmentRecorded(
    uint256 indexed investmentId, address indexed investor, uint256 amount, uint256 investmentDate, bytes32 instanceHash
);
```

### InvestmentRejected

```solidity
event InvestmentRejected(uint256 indexed investmentId, address indexed investor, uint256 amount);
```

### InvestmentRefunded

```solidity
event InvestmentRefunded(uint256 indexed investmentId, address indexed investor, uint256 amount);
```

