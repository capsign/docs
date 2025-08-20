# UniswapV4CMXOracle
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Utilities/oracle/UniswapV4CMXOracle.sol)

Oracle for CMX/ETH exchange rates using Uniswap V4 pools

*Provides real-time and time-weighted average prices for CMX token*


## State Variables
### PROTOCOL_ADMIN
Address of the protocol admin


```solidity
address public immutable PROTOCOL_ADMIN;
```


### CMX_TOKEN
CMX token address


```solidity
address public immutable CMX_TOKEN;
```


### WETH_TOKEN
WETH token address


```solidity
address public immutable WETH_TOKEN;
```


### poolManager
Uniswap V4 Pool Manager


```solidity
address public poolManager;
```


### cmxPool
CMX/ETH pool configuration


```solidity
PoolConfig public cmxPool;
```


### maxPriceAge
Maximum age for price data (in seconds)


```solidity
uint256 public maxPriceAge = 300;
```


### authorizedUpdaters
Authorized price updaters


```solidity
mapping(address => bool) public authorizedUpdaters;
```


### priceObservations
Price observations for TWAP


```solidity
PriceObservation[] public priceObservations;
```


### MAX_OBSERVATIONS
Maximum number of observations to store


```solidity
uint256 public constant MAX_OBSERVATIONS = 100;
```


### MIN_TWAP_PERIOD
Minimum TWAP period (in seconds)


```solidity
uint32 public constant MIN_TWAP_PERIOD = 60;
```


### MAX_TWAP_PERIOD
Maximum TWAP period (in seconds)


```solidity
uint32 public constant MAX_TWAP_PERIOD = 3600;
```


## Functions
### constructor

Initialize the CMX oracle


```solidity
constructor(address protocolAdmin, address cmxToken, address wethToken, address initialPoolManager);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`protocolAdmin`|`address`|Address of the protocol admin|
|`cmxToken`|`address`|CMX token address|
|`wethToken`|`address`|WETH token address|
|`initialPoolManager`|`address`|Initial pool manager address|


### onlyProtocolAdmin


```solidity
modifier onlyProtocolAdmin();
```

### onlyAuthorizedUpdater


```solidity
modifier onlyAuthorizedUpdater();
```

### getCurrentPrice

Get current CMX/ETH exchange rate from Uniswap V4 pool


```solidity
function getCurrentPrice() external view returns (uint256 price, uint256 timestamp);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`price`|`uint256`|Current price (CMX per ETH, 18 decimals)|
|`timestamp`|`uint256`|Block timestamp of price|


### getTWAP

Get time-weighted average price for specified period


```solidity
function getTWAP(uint32 period) external view returns (uint256 twapPrice, uint256 observationCount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`period`|`uint32`|TWAP period in seconds|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`twapPrice`|`uint256`|Time-weighted average price|
|`observationCount`|`uint256`|Number of observations used|


### getPriceWithFallback

Get price with fallback to TWAP if spot price is stale


```solidity
function getPriceWithFallback(uint32 twapPeriod) external view returns (uint256 price, string memory source);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`twapPeriod`|`uint32`|TWAP period to use as fallback|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`price`|`uint256`|Best available price|
|`source`|`string`|Price source used ("spot" or "twap")|


### updatePoolManager

Update pool manager address


```solidity
function updatePoolManager(address newManager) external onlyProtocolAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newManager`|`address`|New pool manager address|


### configureCMXPool

Configure CMX/ETH pool


```solidity
function configureCMXPool(bytes32 poolKey, uint24 fee, int24 tickSpacing) external onlyProtocolAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`poolKey`|`bytes32`|Pool key identifier|
|`fee`|`uint24`|Pool fee tier|
|`tickSpacing`|`int24`|Tick spacing|


### updatePriceObservation

Update price observation (called by authorized updaters)


```solidity
function updatePriceObservation(uint256 price) external onlyAuthorizedUpdater;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`price`|`uint256`|New price observation|


### setMaxPriceAge

Set maximum price age


```solidity
function setMaxPriceAge(uint256 newMaxAge) external onlyProtocolAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newMaxAge`|`uint256`|New maximum age in seconds|


### authorizeUpdater

Authorize a price updater


```solidity
function authorizeUpdater(address updater) external onlyProtocolAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`updater`|`address`|Address to authorize|


### revokeUpdaterAuthorization

Revoke updater authorization


```solidity
function revokeUpdaterAuthorization(address updater) external onlyProtocolAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`updater`|`address`|Address to revoke|


### getConfiguration

Get oracle configuration


```solidity
function getConfiguration()
    external
    view
    returns (
        address cmxToken,
        address wethToken,
        address poolManagerAddr,
        PoolConfig memory poolConfig,
        uint256 maxAge,
        uint256 observationCount
    );
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`cmxToken`|`address`|CMX token address|
|`wethToken`|`address`|WETH token address|
|`poolManagerAddr`|`address`|Pool manager address|
|`poolConfig`|`PoolConfig`|Pool configuration|
|`maxAge`|`uint256`|Maximum price age|
|`observationCount`|`uint256`|Number of observations|


### getRecentObservations

Get recent price observations


```solidity
function getRecentObservations(uint256 count) external view returns (PriceObservation[] memory observations);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`count`|`uint256`|Number of recent observations to return|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`observations`|`PriceObservation[]`|Array of recent price observations|


### _fetchSpotPrice

Fetch spot price from Uniswap V4 pool


```solidity
function _fetchSpotPrice() internal view returns (uint256 price);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`price`|`uint256`|Current spot price|


## Events
### PoolManagerUpdated
Emitted when pool manager is updated


```solidity
event PoolManagerUpdated(address indexed oldManager, address indexed newManager);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oldManager`|`address`|Previous pool manager|
|`newManager`|`address`|New pool manager|

### CMXPoolUpdated
Emitted when CMX pool is updated


```solidity
event CMXPoolUpdated(bytes32 indexed oldPool, bytes32 indexed newPool);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oldPool`|`bytes32`|Previous pool key|
|`newPool`|`bytes32`|New pool key|

### PriceFetched
Emitted when price is fetched


```solidity
event PriceFetched(uint256 indexed price, uint256 timestamp, string source);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`price`|`uint256`|Current price (CMX per ETH)|
|`timestamp`|`uint256`|Block timestamp|
|`source`|`string`|Price source (spot or TWAP)|

## Errors
### InvalidPoolManager

```solidity
error InvalidPoolManager(address manager);
```

### InvalidCMXToken

```solidity
error InvalidCMXToken(address token);
```

### InvalidPoolKey

```solidity
error InvalidPoolKey(bytes32 poolKey);
```

### PriceNotAvailable

```solidity
error PriceNotAvailable(string reason);
```

### InvalidTWAPPeriod

```solidity
error InvalidTWAPPeriod(uint32 period);
```

### UnauthorizedCaller

```solidity
error UnauthorizedCaller(address caller);
```

### StalePrice

```solidity
error StalePrice(uint256 age, uint256 maxAge);
```

## Structs
### PoolConfig
Pool configuration for CMX/ETH pair


```solidity
struct PoolConfig {
    bytes32 poolKey;
    uint24 fee;
    int24 tickSpacing;
    bool isActive;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`poolKey`|`bytes32`|Unique identifier for the pool|
|`fee`|`uint24`|Pool fee tier|
|`tickSpacing`|`int24`|Tick spacing for the pool|
|`isActive`|`bool`|Whether the pool is active|

### PriceObservation
Price observation for TWAP calculations


```solidity
struct PriceObservation {
    uint256 price;
    uint256 timestamp;
    uint256 blockNumber;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`price`|`uint256`|Price at observation time|
|`timestamp`|`uint256`|Block timestamp of observation|
|`blockNumber`|`uint256`|Block number of observation|

