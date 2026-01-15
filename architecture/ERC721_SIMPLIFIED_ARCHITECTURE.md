# ERC-721 Real Estate: Simplified Architecture

## Your Architecture is Better

### The Clean Flow

```
Physical Property
    ↓
ERC-721 Token (1 NFT = 1 property)
    ↓ (optional fractionalization)
SPV Diamond holds the ERC-721
    ↓
SPV issues ERC-7752 tokens (SPVUnits) to investors
```

## Why This Works Better

### 1. Simple TokenERC721Facet (like TokenERC20Facet)

```solidity
protocol/packages/tokens/src/token/facets/
├── TokenERC20Facet.sol      // Fungible tokens
├── TokenERC7752Facet.sol    // Lot accounting
└── TokenERC721Facet.sol     // NFTs ← Simple ~400 lines
```

**No complex property-specific facets needed!**

### 2. Metadata in baseURI (Standard NFT Pattern)

```json
// baseURI: ipfs://QmXxx/
// tokenURI(1) returns: ipfs://QmXxx/1

// Metadata at ipfs://QmXxx/1:
{
  "name": "123 Main Street, Anytown WY",
  "description": "Single family residential property",
  "image": "ipfs://QmYyy/property.jpg",
  "properties": {
    "address": "123 Main Street, Anytown, WY 82001",
    "type": "residential_single_family",
    "sqft": 2400,
    "bedrooms": 4,
    "bathrooms": 3,
    "yearBuilt": 2015,
    "legalDescription": "Lot 5, Block 2, Subdivision XYZ",
    "deedHash": "0xabcd...",
    "titleDocuments": "ipfs://QmZzz/"
  }
}
```

### 3. Valuations via EAS (Much Cleaner!)

```solidity
// EAS Schema: PropertyValuation
{
    tokenAddress: address,      // ERC-721 contract
    tokenId: uint256,           // Property token ID
    valuationAmount: uint256,   // $500,000 (in USDC decimals)
    valuationDate: uint256,     // block.timestamp
    appraiser: address,         // Licensed appraiser's wallet
    appraisalURI: string       // ipfs:// link to full appraisal
}

// Query valuations:
// 1. Get all attestations for schema
// 2. Filter by tokenAddress + tokenId
// 3. Sort by valuationDate DESC
// 4. Display valuation history timeline
```

**Benefits**:
- No on-chain storage costs
- Immutable valuation history
- Appraiser reputation via EAS
- Can revoke/dispute valuations
- Rich attestation ecosystem

### 4. Rental Income Naturally Tracked

If individual owns ERC-721:
```solidity
// Just monitor Transfer events to owner's wallet
// Filter: to == ownerOf(tokenId)
// Assumes rental payments sent to property owner
```

If SPV owns ERC-721:
```solidity
// SPV already has distribution logic!
// Rental payments → SPV wallet
// SPV.distributeIncome() → SPVUnit holders
// All on-chain, all tracked
```

### 5. Fractionalization via Existing SPV

```solidity
// Step 1: User mints ERC-721 property token
ERC721Token.mint(alice, tokenId: 1);

// Step 2: Alice creates SPV
SPV memory spvConfig = SPV({
    name: "123 Main St SPV",
    underlyingAsset: address(ERC721Token), // ← The ERC-721!
    // ...
});
address spvDiamond = VehicleFactory.createSPV(spvConfig);

// Step 3: Alice transfers ERC-721 to SPV
ERC721Token.transferFrom(alice, spvDiamond, tokenId: 1);

// Step 4: SPV issues ERC-7752 tokens
SPV(spvDiamond).issueUnits(investors, amounts);

// Now:
// - SPV owns the property (ERC-721)
// - Investors own SPVUnits (ERC-7752)
// - Rental income distributed by SPV
// - Governance via SPV voting
// - Compliance via SPV modules
```

## Implementation Checklist

### Phase 1: Core ERC-721 Facet (1 week)
- [x] TokenERC721Facet.sol (~400 lines, done!)
- [x] TokenERC721Storage in TokenStorage.sol (done!)
- [ ] Unit tests for ERC-721 operations
- [ ] Register facet in FacetRegistry

### Phase 2: Token Factory Integration (1 week)
- [ ] Add `TokenStandard` enum (ERC20_WITH_LOTS, ERC721)
- [ ] Add RealEstateProperty to TokenAssetType
- [ ] Update TokenFactory to route based on standard
- [ ] Add ERC-721 preset configuration
- [ ] Integration tests

### Phase 3: SPV Integration (1 week)
- [ ] Update SPV to accept ERC-721 as underlyingAsset
- [ ] Add ERC-721 ownership validation
- [ ] Test: Create ERC-721 → Transfer to SPV → Issue SPVUnits
- [ ] Document fractionalization flow

### Phase 4: Interface Layer (2 weeks)
- [ ] ERC-721 creation flow
- [ ] Property NFT card/detail components
- [ ] IPFS metadata upload UI
- [ ] EAS valuation display
- [ ] SPV fractionalization wizard
- [ ] Property transfer interface

### Phase 5: Pilot (2 weeks)
- [ ] Deploy to testnet
- [ ] Mint 1-2 test properties
- [ ] Test fractionalization via SPV
- [ ] Gather feedback
- [ ] Security review

**Total: ~7 weeks**

## Comparison: Old Architecture vs New

| Aspect | Original (Complex) | New (Simple) |
|--------|-------------------|--------------|
| **Facets** | PropertyNFTCoreFacet, PropertyNFTMetadataFacet, PropertyNFTValuationFacet, PropertyNFTRevenueFacet, PropertyNFTFractionalFacet | TokenERC721Facet (one facet) |
| **Metadata** | On-chain struct | baseURI + IPFS (standard) |
| **Valuations** | On-chain storage | EAS attestations |
| **Rental income** | Custom tracking | Wallet transfers / SPV distributions |
| **Fractionalization** | Custom vault contract | Existing SPV infrastructure |
| **Lines of code** | ~2000+ | ~400 |
| **Complexity** | High | Low |
| **Maintainability** | Multiple facets to maintain | One facet, reuses existing |

## ERC-4626 Question

You asked about ERC-4626 (tokenized vaults). That's the Ethereum standard for yield-bearing vaults. It could be used for:

```solidity
// ERC-4626 Vault
PropertyVault (ERC-4626)
├── Holds: ERC-721 property
├── Issues: Vault shares (ERC-20)
├── Yield: Rental income
└── Redeem: Burn all shares → get NFT back
```

**Comparison with SPV approach**:

| Feature | ERC-4626 Vault | SPV Diamond |
|---------|---------------|-------------|
| **Standardization** | Standard interface | Custom (but proven) |
| **DeFi integration** | Excellent (Yearn, etc) | Limited |
| **Governance** | Limited | Rich (voting, proposals) |
| **Compliance** | None built-in | Robust modules |
| **Distributions** | Automatic yield | Manual distributions |
| **Complexity** | Simple | More features |

**Recommendation**: 
- **SPV approach is better for CapSign** because:
  - Already built and tested
  - Compliance infrastructure
  - Governance built-in
  - Fits CapSign's existing ecosystem

- **ERC-4626 could be future addition** for:
  - DeFi composability
  - Automated yield strategies
  - Cross-protocol integrations

But start with SPV - it's working code that does what you need!

## Next Steps

1. **Finish TokenERC721Facet**
   - Unit tests
   - Register in FacetRegistry
   - Add to TokenTypePresets

2. **Update TokenFactory**
   - Add TokenStandard enum
   - Route ERC-721 creation
   - Test deployment

3. **SPV Integration**
   - Accept ERC-721 as underlyingAsset
   - Document fractionalization flow

4. **Pilot**
   - Deploy testnet
   - Mint test property
   - Fractionalize via SPV
   - Gather feedback

**Want me to continue with any of these?**

