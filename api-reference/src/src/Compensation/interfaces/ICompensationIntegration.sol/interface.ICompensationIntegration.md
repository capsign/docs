# ICompensationIntegration
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Compensation/interfaces/ICompensationIntegration.sol)

Interface for integrating assets with the compensation tracking system


## Functions
### recordCompensationGrant

Record a new compensation grant in the Rule701 tracker


```solidity
function recordCompensationGrant(
    address rule701Diamond,
    address employee,
    address assetAddress,
    uint256 amount,
    uint256 vestingStart,
    uint256 vestingDuration,
    uint256 exercisePrice
) external returns (bytes32 grantId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`rule701Diamond`|`address`|Address of the Rule701 diamond tracking compliance|
|`employee`|`address`|Address of the employee receiving the grant|
|`assetAddress`|`address`|Address of the asset being granted (ESO, RSA, SAR, etc.)|
|`amount`|`uint256`|Number of units being granted|
|`vestingStart`|`uint256`|Timestamp when vesting begins|
|`vestingDuration`|`uint256`|Duration of vesting in seconds|
|`exercisePrice`|`uint256`|Exercise price per unit (0 for RSAs)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`grantId`|`bytes32`|Unique identifier for the grant|


### getRule701Diamond

Get the appropriate Rule701 diamond for an entity


```solidity
function getRule701Diamond(address entity) external view returns (address rule701Diamond);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|The entity address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`rule701Diamond`|`address`|Address of the Rule701 diamond, or address(0) if none|


### checkRule701Limits

Check if a grant would exceed Rule 701 limits


```solidity
function checkRule701Limits(address rule701Diamond, address employee, uint256 amount)
    external
    view
    returns (bool withinLimits);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`rule701Diamond`|`address`|Address of the Rule701 diamond|
|`employee`|`address`|Address of the employee|
|`amount`|`uint256`|Amount to be granted|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`withinLimits`|`bool`|True if the grant is within limits|


