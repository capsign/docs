# ListingRegistry
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Trading/Settlement/ListingRegistry.sol)

Tracks active listings across various marketplace mechanisms.


## State Variables
### listings
*mapping of listingId to Listing*


```solidity
mapping(bytes32 => Listing) public listings;
```


### listingTokens

```solidity
mapping(bytes32 => TokenReference[]) public listingTokens;
```


## Functions
### addListing

*Adds a new listing to the registry with one or more lot references.*


```solidity
function addListing(
    bytes32 listingId,
    address seller,
    address asset,
    TokenReference[] calldata lotsData,
    uint256 price,
    string calldata marketType
) external;
```

### removeListing

*Removes a listing from the registry.*


```solidity
function removeListing(bytes32 listingId) external;
```

### getListing


```solidity
function getListing(bytes32 listingId) external view returns (Listing memory);
```

### getListingTokens


```solidity
function getListingTokens(bytes32 listingId) external view returns (TokenReference[] memory);
```

## Events
### ListingAdded

```solidity
event ListingAdded(
    bytes32 indexed listingId,
    address indexed seller,
    address indexed asset,
    uint96 totalQuantity,
    uint256 price,
    string marketType
);
```

### ListingRemoved

```solidity
event ListingRemoved(bytes32 indexed listingId);
```

## Structs
### Listing

```solidity
struct Listing {
    bytes32 listingId;
    address seller;
    address asset;
    uint96 totalQuantity;
    uint256 price;
    uint256 timestamp;
    string marketType;
}
```

### TokenReference

```solidity
struct TokenReference {
    bytes32 tokenId;
    uint96 quantity;
}
```

