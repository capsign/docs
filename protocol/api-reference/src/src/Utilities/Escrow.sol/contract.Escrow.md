# Escrow
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Utilities/Escrow.sol)

**Inherits:**
ReentrancyGuard

*Implementation for ERC-20 tokens escrow with extended unlock periods
Example Usage for CMX Distribution:
1. Initial Setup:
- Total CMX Supply: 100,000,000,000 tokens
- Escrow Periods: Multiple 5-year locks with gradual releases
2. Recommended Escrow Schedule:
- Year 1-5:  20B tokens locked (5B per year release)
- Year 6-10: 30B tokens locked (6B per year release)
- Year 11-15: 30B tokens locked (6B per year release)
- Year 16-20: 20B tokens locked (4B per year release)
3. Implementation:
// Lock 20B tokens for 5 years
createEscrow(recipient, 20_000_000_000 * 1e18, 5 * 365 days);
// Lock 30B tokens for 10 years
createEscrow(recipient, 30_000_000_000 * 1e18, 10 * 365 days);
Note: Amounts should be adjusted for token decimals (multiply by 10^decimals)*

*A contract that implements Ripple-like escrow functionality for ERC-20 tokens*


## State Variables
### token

```solidity
IERC20 public immutable token;
```


### escrows

```solidity
mapping(uint256 => EscrowData) public escrows;
```


### _escrowIdCounter

```solidity
uint256 private _escrowIdCounter;
```


## Functions
### constructor

*Constructor that sets the token address*


```solidity
constructor(address _token);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_token`|`address`|Address of the ERC-20 token to be used|


### createEscrow

*Creates a new escrow*


```solidity
function createEscrow(address recipient, uint256 amount, uint256 lockDuration)
    external
    nonReentrant
    returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`recipient`|`address`|Address that will receive the tokens|
|`amount`|`uint256`|Amount of tokens to be escrowed|
|`lockDuration`|`uint256`|Duration in seconds for which the tokens will be locked|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|escrowId The ID of the created escrow|


### releaseEscrow

*Releases tokens from an escrow to the recipient*


```solidity
function releaseEscrow(uint256 escrowId) external nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`escrowId`|`uint256`|ID of the escrow to release|


### cancelEscrow

*Cancels an escrow and returns tokens to the owner*


```solidity
function cancelEscrow(uint256 escrowId) external nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`escrowId`|`uint256`|ID of the escrow to cancel|


### getEscrow

*Returns the details of an escrow*


```solidity
function getEscrow(uint256 escrowId)
    external
    view
    returns (address owner, address recipient, uint256 amount, uint256 releaseTime, bool released);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`escrowId`|`uint256`|ID of the escrow|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|Address of the escrow owner|
|`recipient`|`address`|Address of the recipient|
|`amount`|`uint256`|Amount of tokens in escrow|
|`releaseTime`|`uint256`|Timestamp when the escrow can be released|
|`released`|`bool`|Whether the escrow has been released|


## Events
### EscrowCreated

```solidity
event EscrowCreated(
    uint256 indexed escrowId, address indexed owner, address indexed recipient, uint256 amount, uint256 releaseTime
);
```

### EscrowReleased

```solidity
event EscrowReleased(uint256 indexed escrowId, uint256 amount);
```

### EscrowCancelled

```solidity
event EscrowCancelled(uint256 indexed escrowId, uint256 amount);
```

## Structs
### EscrowData

```solidity
struct EscrowData {
    address owner;
    address recipient;
    uint256 amount;
    uint256 releaseTime;
    bool released;
}
```

