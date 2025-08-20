# IWarrant
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/interfaces/IWarrant.sol)

**Inherits:**
[IIssuedAsset](/src/Tokenization/assets/interfaces/IIssuedAsset.sol/interface.IIssuedAsset.md)

*Interface for the Warrant contract*


## Functions
### createWarrantToken


```solidity
function createWarrantToken(address holder, uint96 quantity, address paymentCurrency, uint96 strikePrice)
    external
    returns (bytes32);
```

### getWarrantToken


```solidity
function getWarrantToken(bytes32 tokenId) external view returns (WarrantToken memory);
```

### warrantTokens


```solidity
function warrantTokens(bytes32 tokenId)
    external
    view
    returns (bool isValid, address owner, uint96 quantity, address paymentCurrency, uint96 strikePrice);
```

## Structs
### WarrantToken

```solidity
struct WarrantToken {
    bytes32 tokenId;
    uint96 quantity;
    address paymentCurrency;
    uint96 strikePrice;
    bool isValid;
}
```

