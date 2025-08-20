# IEmployeeStockOption
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/interfaces/IEmployeeStockOption.sol)

**Inherits:**
[IIssuedAsset](/src/Tokenization/assets/interfaces/IIssuedAsset.sol/interface.IIssuedAsset.md)


## Functions
### createEmployeeStockOptionToken


```solidity
function createEmployeeStockOptionToken(
    address holder,
    uint96 quantity,
    address paymentCurrency,
    uint96 strikePrice,
    uint256 vestingStartTime,
    uint256 cliffTime,
    uint256 vestingEndTime
) external returns (bytes32);
```

### employeeStockOptionTokens


```solidity
function employeeStockOptionTokens(bytes32 tokenId)
    external
    view
    returns (
        bool isValid,
        address owner,
        uint96 quantity,
        address paymentCurrency,
        uint96 strikePrice,
        uint256 vestingStartTime,
        uint256 cliffTime,
        uint256 vestingEndTime
    );
```

