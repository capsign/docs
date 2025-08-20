# OfferingUtils
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Offerings/offering/OfferingUtils.sol)

Utility library for common offering validation and calculation patterns

*Consolidates repeated code patterns to reduce contract sizes*


## Functions
### validateTokenCreation

*Validates price and amount for token creation*


```solidity
function validateTokenCreation(uint256 amount, uint256 pricePerToken) internal pure returns (uint256 sharesToMint);
```

### validateInvestmentParams

*Validates basic investment parameters*


```solidity
function validateInvestmentParams(uint256 amount, uint256 pricePerToken, uint256 minInvestment) internal pure;
```

### requireEntity

*Checks entity authorization*


```solidity
function requireEntity(address msgSender, address entity) internal pure;
```

