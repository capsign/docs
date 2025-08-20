# TokenSale
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/TokenSale.sol)

**Inherits:**
[AccessManaged](/src/Authorization/AccessManaged.sol/abstract.AccessManaged.md), ReentrancyGuard

Allows users to purchase CMX tokens at a fixed $1 rate using stablecoins

*Supports multiple stablecoins with different decimal configurations*


## State Variables
### cmxToken

```solidity
IERC20 public immutable cmxToken;
```


### treasury

```solidity
address public treasury;
```


### acceptedStablecoins

```solidity
mapping(address => StablecoinConfig) public acceptedStablecoins;
```


### stablecoinList

```solidity
address[] public stablecoinList;
```


### saleActive

```solidity
bool public saleActive;
```


### maxPurchasePerTx

```solidity
uint256 public maxPurchasePerTx;
```


### totalSold

```solidity
uint256 public totalSold;
```


### maxSupplyForSale

```solidity
uint256 public maxSupplyForSale;
```


## Functions
### constructor

*Constructor*


```solidity
constructor(address _globalAccessManager, address _cmxToken, address _treasury, uint256 _maxSupplyForSale)
    AccessManaged(_globalAccessManager);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_globalAccessManager`|`address`|Address of the GlobalAccessManager|
|`_cmxToken`|`address`|Address of the CMX token contract|
|`_treasury`|`address`|Address to receive stablecoin payments|
|`_maxSupplyForSale`|`uint256`|Maximum CMX tokens available for sale|


### purchaseCMX

*Purchase CMX tokens with stablecoins at $1 per CMX*


```solidity
function purchaseCMX(address stablecoin, uint256 cmxAmount) external nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`stablecoin`|`address`|Address of the stablecoin to pay with|
|`cmxAmount`|`uint256`|Amount of CMX tokens to purchase (18 decimals)|


### _calculateStablecoinAmount

*Calculate the stablecoin amount needed for a given CMX amount*


```solidity
function _calculateStablecoinAmount(uint256 cmxAmount, uint8 stablecoinDecimals) internal pure returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`cmxAmount`|`uint256`|Amount of CMX tokens (18 decimals)|
|`stablecoinDecimals`|`uint8`|Decimals of the stablecoin|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|stablecoinAmount Amount of stablecoin needed|


### getStablecoinAmount

*Get the stablecoin amount needed for a given CMX amount (public view)*


```solidity
function getStablecoinAmount(address stablecoin, uint256 cmxAmount) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`stablecoin`|`address`|Address of the stablecoin|
|`cmxAmount`|`uint256`|Amount of CMX tokens (18 decimals)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|stablecoinAmount Amount of stablecoin needed|


### getCMXAmount

*Get the maximum CMX amount that can be purchased with a given stablecoin amount*


```solidity
function getCMXAmount(address stablecoin, uint256 stablecoinAmount) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`stablecoin`|`address`|Address of the stablecoin|
|`stablecoinAmount`|`uint256`|Amount of stablecoin available|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|cmxAmount Maximum CMX tokens that can be purchased|


### addStablecoin

*Add a new accepted stablecoin*


```solidity
function addStablecoin(address stablecoin, uint8 decimals, uint256 minPurchase) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`stablecoin`|`address`|Address of the stablecoin|
|`decimals`|`uint8`|Decimals of the stablecoin|
|`minPurchase`|`uint256`|Minimum purchase amount in CMX tokens (18 decimals)|


### removeStablecoin

*Remove an accepted stablecoin*


```solidity
function removeStablecoin(address stablecoin) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`stablecoin`|`address`|Address of the stablecoin to remove|


### updateStablecoinConfig

*Update stablecoin configuration*


```solidity
function updateStablecoinConfig(address stablecoin, uint8 decimals, uint256 minPurchase) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`stablecoin`|`address`|Address of the stablecoin|
|`decimals`|`uint8`|New decimals value|
|`minPurchase`|`uint256`|New minimum purchase amount|


### setSaleStatus

*Set sale status*


```solidity
function setSaleStatus(bool active) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`active`|`bool`|Whether the sale is active|


### setTreasury

*Set treasury address*


```solidity
function setTreasury(address newTreasury) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newTreasury`|`address`|New treasury address|


### setMaxPurchasePerTx

*Set maximum purchase per transaction*


```solidity
function setMaxPurchasePerTx(uint256 newMax) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newMax`|`uint256`|New maximum purchase amount in CMX tokens|


### setMaxSupplyForSale

*Set maximum supply for sale*


```solidity
function setMaxSupplyForSale(uint256 newMax) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newMax`|`uint256`|New maximum supply for sale|


### emergencyWithdrawCMX

*Emergency withdraw CMX tokens*


```solidity
function emergencyWithdrawCMX(uint256 amount) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|Amount of CMX tokens to withdraw|


### emergencyWithdrawToken

*Emergency withdraw any ERC20 token (except CMX)*


```solidity
function emergencyWithdrawToken(address token, uint256 amount) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|Address of the token to withdraw|
|`amount`|`uint256`|Amount to withdraw|


### getAcceptedStablecoins

*Get all accepted stablecoins*


```solidity
function getAcceptedStablecoins() external view returns (address[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of accepted stablecoin addresses|


### getSaleInfo

*Get sale information*


```solidity
function getSaleInfo()
    external
    view
    returns (bool active, uint256 sold, uint256 maxSupply, uint256 maxPerTx, uint256 remaining);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`active`|`bool`|Whether sale is active|
|`sold`|`uint256`|Total CMX tokens sold|
|`maxSupply`|`uint256`|Maximum CMX tokens for sale|
|`maxPerTx`|`uint256`|Maximum purchase per transaction|
|`remaining`|`uint256`|Remaining CMX tokens available|


### isStablecoinAccepted

*Check if a stablecoin is accepted*


```solidity
function isStablecoinAccepted(address stablecoin) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`stablecoin`|`address`|Address of the stablecoin|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|accepted Whether the stablecoin is accepted|


## Events
### CMXPurchased

```solidity
event CMXPurchased(address indexed buyer, address indexed stablecoin, uint256 stablecoinAmount, uint256 cmxAmount);
```

### StablecoinAdded

```solidity
event StablecoinAdded(address indexed stablecoin, uint8 decimals, uint256 minPurchase);
```

### StablecoinRemoved

```solidity
event StablecoinRemoved(address indexed stablecoin);
```

### StablecoinConfigUpdated

```solidity
event StablecoinConfigUpdated(address indexed stablecoin, uint8 decimals, uint256 minPurchase);
```

### SaleStatusUpdated

```solidity
event SaleStatusUpdated(bool active);
```

### TreasuryUpdated

```solidity
event TreasuryUpdated(address indexed oldTreasury, address indexed newTreasury);
```

### MaxPurchaseUpdated

```solidity
event MaxPurchaseUpdated(uint256 oldMax, uint256 newMax);
```

### MaxSupplyUpdated

```solidity
event MaxSupplyUpdated(uint256 oldMax, uint256 newMax);
```

## Structs
### StablecoinConfig

```solidity
struct StablecoinConfig {
    bool accepted;
    uint8 decimals;
    uint256 minPurchase;
}
```

