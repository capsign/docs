# IFundUnit
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Funds/fund/interfaces/IFundUnit.sol)

**Inherits:**
IERC20, IERC20Metadata

Interface for fund unit tokens representing LP interests

*Extends ERC20 with fund-specific functionality for managing LP interests*


## Functions
### initialize

Initialize the fund unit token


```solidity
function initialize(string memory name, string memory symbol, address fund, uint256 initialNAV) external;
```

### mint

Mint units for investor contributions


```solidity
function mint(address to, uint256 amount) external;
```

### burn

Burn units for redemptions


```solidity
function burn(address from, uint256 amount) external;
```

### updateNAV

Update NAV per unit


```solidity
function updateNAV(uint256 newTotalNAV) external;
```

### requestRedemption

Request redemption of units


```solidity
function requestRedemption(uint256 amount) external;
```

### processRedemptions

Process pending redemptions


```solidity
function processRedemptions(address[] calldata investors) external;
```

### cancelRedemption

Cancel redemption request


```solidity
function cancelRedemption(uint256 amount) external;
```

### recordCommitment

Record commitment for an investor


```solidity
function recordCommitment(address investor, uint256 amount) external;
```

### createLot

Create a lot for tracking investor positions


```solidity
function createLot(
    address investor,
    uint256 amount,
    address denominationAsset,
    uint256 costBasis,
    uint256 timestamp,
    string calldata documentURI,
    string calldata documentHash,
    uint256 lotType
) external returns (bytes32 lotId);
```

### recordCapitalContribution

Record capital contribution


```solidity
function recordCapitalContribution(address investor, uint256 amount) external;
```

### navPerUnit

Get current NAV per unit


```solidity
function navPerUnit() external view returns (uint256);
```

### totalNAV

Get total NAV of the fund


```solidity
function totalNAV() external view returns (uint256);
```

### fund

Get fund address


```solidity
function fund() external view returns (address);
```

### pendingRedemption

Get pending redemption amount for an investor


```solidity
function pendingRedemption(address investor) external view returns (uint256);
```

### redemptionRequestDate

Get redemption request date


```solidity
function redemptionRequestDate(address investor) external view returns (uint256);
```

### calculateRedemptionValue

Calculate redemption value for amount of units


```solidity
function calculateRedemptionValue(uint256 unitAmount) external view returns (uint256);
```

### calculateUnitsToMint

Calculate units to mint for contribution amount


```solidity
function calculateUnitsToMint(uint256 contributionAmount) external view returns (uint256);
```

### redemptionsAllowed

Check if redemptions are currently allowed


```solidity
function redemptionsAllowed() external view returns (bool);
```

### redemptionNoticePeriod

Get minimum redemption notice period


```solidity
function redemptionNoticePeriod() external view returns (uint256);
```

### canRedeem

Check if investor can redeem (notice period satisfied)


```solidity
function canRedeem(address investor) external view returns (bool);
```

## Events
### UnitsMinted
Emitted when units are minted for contributions


```solidity
event UnitsMinted(address indexed investor, uint256 amount, uint256 contribution);
```

### UnitsBurned
Emitted when units are burned for redemptions


```solidity
event UnitsBurned(address indexed investor, uint256 amount, uint256 redemptionValue);
```

### NAVUpdated
Emitted when NAV per unit is updated


```solidity
event NAVUpdated(uint256 newNAV, uint256 navPerUnit);
```

### RedemptionRequested
Emitted when redemption is requested


```solidity
event RedemptionRequested(address indexed investor, uint256 amount, uint256 requestDate);
```

### RedemptionProcessed
Emitted when redemption is processed


```solidity
event RedemptionProcessed(address indexed investor, uint256 amount, uint256 redemptionValue);
```

