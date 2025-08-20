# WhitelistFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Offerings/offering/facets/WhitelistFacet.sol)

Provides whitelisting capabilities for private offerings (especially 506(b))

*Facet for whitelist functionality in offering diamonds*


## Functions
### whitelistInvestor

Whitelists a single investor.

*Only accounts with ADMIN_ROLE can call this function*


```solidity
function whitelistInvestor(address investor) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The address to whitelist.|


### bulkWhitelistInvestors

Whitelists a batch of investor addresses.

*Only accounts with ADMIN_ROLE can call this function*


```solidity
function bulkWhitelistInvestors(address[] calldata investors) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investors`|`address[]`|Array of addresses to whitelist.|


### removeFromWhitelist

Removes an investor from the whitelist.

*Only accounts with ADMIN_ROLE can call this function*


```solidity
function removeFromWhitelist(address investor) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The address to remove from whitelist.|


### isWhitelisted

Returns true if an investor is whitelisted.


```solidity
function isWhitelisted(address investor) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The address to check.|


### getWhitelistedInvestors

Get all whitelisted investors.


```solidity
function getWhitelistedInvestors() external view returns (address[] memory investors);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`investors`|`address[]`|Array of whitelisted investor addresses|


### getWhitelistCount

Get the number of whitelisted investors.


```solidity
function getWhitelistCount() external view returns (uint256 count);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`count`|`uint256`|Number of whitelisted investors|


### _requireAuthorized

*Internal function to check authorization*

*Uses diamond's authority() function through delegatecall*


```solidity
function _requireAuthorized() internal view;
```

### _getAuthority

*Get the authority address from the diamond*

*This will be implemented by the diamond's AccessManaged inheritance*


```solidity
function _getAuthority() internal view returns (address);
```

## Events
### InvestorWhitelisted
Events


```solidity
event InvestorWhitelisted(address indexed investor);
```

### InvestorBatchWhitelisted

```solidity
event InvestorBatchWhitelisted(address[] investors);
```

### InvestorRemovedFromWhitelist

```solidity
event InvestorRemovedFromWhitelist(address indexed investor);
```

