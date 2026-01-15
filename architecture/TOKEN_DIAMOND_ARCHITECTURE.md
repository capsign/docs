# Token & Investment Vehicle Diamond Architecture

## Overview

This document outlines the protocol-layer architecture for supporting multiple token types and investment vehicle structures (funds, SPVs, etc.) using the diamond pattern.

**Last Updated**: 2025-10-24  
**Status**: Design Phase

**Key Change**: Renamed from "Fund Diamond Architecture" to "Investment Vehicle Architecture" to reflect the unified approach for funds, SPVs, and other investment vehicles via `VehicleFactory`.

---

## Token Type Taxonomy

### Complete Asset Type Enumeration

```solidity
enum TokenAssetType {
    // Operating Entity Ownership (Equity-like)
    ShareClass,              // Corporate stock (C-Corp, S-Corp)
    MembershipUnit,          // LLC/Partnership units (operating businesses)
    
    // Investment Vehicle Interests (Passive Investment)
    FundUnit,               // Multi-asset investment funds (any legal structure)
    SPVUnit,                // Single-purpose vehicles (any legal structure) 🔥 HIGH PRIORITY
    
    // Debt Instruments (Fixed Income)
    ConvertibleNote,        // Debt convertible to equity
    Bond,                   // Traditional bonds
    PromissoryNote,         // Simple debt instruments
    
    // Derivative Instruments (Rights to Equity)
    EmployeeStockOption,    // Employee compensation
    Warrant,                // Investor purchase rights
    SAFE,                   // Simple Agreement for Future Equity
    StockAppreciationRight, // Phantom equity
    
    // Future: Real Assets
    // RealEstateToken,
    // RevenueSharingToken,
}
```

---

## Token Type Configuration Matrix

| Type | Conversion | Underlying Asset | Redeemable | Maturity | Voting | Special Facets |
|------|-----------|------------------|------------|----------|--------|----------------|
| **ShareClass** | ✅ Own ratios | ❌ | ❌ | ❌ | ✅ | Corporate actions, voting |
| **MembershipUnit** | ❌ | ❌ | ❌ | ❌ | ✅* | Distributions, capital accounts |
| **FundUnit** | ❌ | Vehicle (Fund) | ✅** | ❌ | ❌ | NAV, redemptions, performance |
| **SPVUnit** 🔥 | ❌ | Vehicle (SPV) | ❌ | ✅*** | ❌ | Single investment, waterfall |
| **ConvertibleNote** | ❌ → ✅ | ShareClass | ❌ | ✅ | ❌ | Interest, conversion |
| **Bond** | ❌ | ❌ | ❌ | ✅ | ❌ | Coupons, maturity |
| **PromissoryNote** | ❌ | ❌ | ❌ | ✅ | ❌ | Amortization |
| **EmployeeStockOption** | ✅ Inherited | ShareClass | ❌ | ✅ | ❌ | Vesting, exercise |
| **Warrant** | ✅ Inherited | ShareClass | ❌ | ✅ | ❌ | Exercise |
| **SAFE** | ❌ → ✅ | ShareClass | ❌ | ❌ | ❌ | Valuation cap, conversion |
| **SAR** | Varies | ShareClass | ❌ | ✅ | ❌ | Settlement |

*Voting varies by operating agreement  
**Redeemable only for liquid funds (hedge funds, mutual funds)  
***SPV liquidates when underlying investment exits

---

## Diamond Facet Architecture

### Common Base Facets (All Token Types)

```
protocol/src/tokens/facets/
├── metadata/              # Name, symbol, decimals
│   └── TokenMetadataFacet.sol
├── balances/             # Balance tracking
│   └── TokenBalancesFacet.sol
├── lots/                 # ERC-7752 lot accounting
│   └── TokenLotsFacet.sol
├── erc20/                # ERC-20 interface
│   └── TokenERC20Facet.sol
├── erc7752/              # ERC-7752 interface
│   └── TokenERC7752Facet.sol
├── admin/                # Pause, freeze, emergency
│   └── TokenAdminFacet.sol
├── compliance/           # Transfer restrictions
│   └── TokenComplianceFacet.sol
└── transfer/             # Transfer logic
    └── TokenTransferFacet.sol
```

### Type-Specific Facets

```
protocol/src/tokens/facets/
├── equity/                          # ShareClass-specific
│   ├── corporate-actions/
│   │   ├── TokenCorporateActionsFacet.sol
│   │   └── TokenCorporateActionsStorage.sol
│   ├── voting/
│   │   ├── TokenVotingFacet.sol
│   │   └── TokenVotingStorage.sol
│   └── custody/
│       ├── TokenCustodyFacet.sol
│       └── TokenCustodyStorage.sol
│
├── membership/                      # MembershipUnit-specific
│   ├── distributions/
│   │   ├── TokenDistributionsFacet.sol
│   │   └── TokenDistributionsStorage.sol
│   ├── capital-accounts/
│   │   ├── TokenCapitalAccountFacet.sol
│   │   └── TokenCapitalAccountStorage.sol
│   └── allocations/
│       ├── TokenAllocationsFacet.sol
│       └── TokenAllocationsStorage.sol
│
├── vehicles/                        # Investment vehicle tokens (FundUnit, SPVUnit)
│   ├── fund/
│   │   ├── TokenFundUnitFacet.sol
│   │   ├── TokenRedemptionFacet.sol
│   │   └── TokenNAVFacet.sol
│   └── spv/
│       ├── TokenSPVUnitFacet.sol         # 🔥 HIGH PRIORITY
│       ├── TokenSPVInvestmentFacet.sol   # Track single investment
│       └── TokenSPVWaterfallFacet.sol    # Simplified carry distribution
│
├── debt/                           # Debt instruments (shared)
│   ├── TokenDebtFacet.sol          # Principal, interest
│   ├── TokenMaturityFacet.sol      # Maturity date
│   ├── TokenCouponFacet.sol        # Bond coupons
│   └── TokenAmortizationFacet.sol  # Payment schedules
│
├── convertibles/                   # Convertible instruments
│   ├── TokenConversionFacet.sol
│   └── TokenSAFEFacet.sol
│
└── derivatives/                    # Options, warrants, SARs
    ├── options/
    │   ├── TokenESOFacet.sol
    │   ├── TokenVestingFacet.sol
    │   └── TokenExerciseFacet.sol
    ├── warrants/
    │   └── TokenWarrantFacet.sol
    └── sars/
        ├── TokenSARFacet.sol
        └── TokenSettlementFacet.sol
```

---

## Token Type Preset System

**File**: `protocol/src/tokens/factory/TokenTypePresets.sol`

```solidity
library TokenTypePresets {
    /**
     * @notice Get facet cuts for a specific token type
     * @param tokenType The type of token to create
     * @return Facet cuts array for diamond deployment
     */
    function getFacetCutsForType(TokenAssetType tokenType) 
        internal 
        pure 
        returns (IDiamond.FacetCut[] memory) 
    {
        if (tokenType == TokenAssetType.ShareClass) {
            return _getShareClassFacets();
        } else if (tokenType == TokenAssetType.MembershipUnit) {
            return _getMembershipUnitFacets();
        } else if (tokenType == TokenAssetType.FundUnit) {
            return _getFundUnitFacets();
        } else if (tokenType == TokenAssetType.EmployeeStockOption) {
            return _getEmployeeStockOptionFacets();
        } else if (tokenType == TokenAssetType.ConvertibleNote) {
            return _getConvertibleNoteFacets();
        } else if (tokenType == TokenAssetType.SAFE) {
            return _getSAFEFacets();
        }
        // ... other types
        
        revert("Unsupported token type");
    }
    
    function _getBaseFacets() private pure returns (IDiamond.FacetCut[] memory) {
        IDiamond.FacetCut[] memory cuts = new IDiamond.FacetCut[](8);
        
        cuts[0] = _createCut(TOKEN_METADATA_FACET, FacetCutAction.Add, getMetadataSelectors());
        cuts[1] = _createCut(TOKEN_BALANCES_FACET, FacetCutAction.Add, getBalancesSelectors());
        cuts[2] = _createCut(TOKEN_LOTS_FACET, FacetCutAction.Add, getLotsSelectors());
        cuts[3] = _createCut(TOKEN_ERC20_FACET, FacetCutAction.Add, getERC20Selectors());
        cuts[4] = _createCut(TOKEN_ERC7752_FACET, FacetCutAction.Add, getERC7752Selectors());
        cuts[5] = _createCut(TOKEN_ADMIN_FACET, FacetCutAction.Add, getAdminSelectors());
        cuts[6] = _createCut(TOKEN_COMPLIANCE_FACET, FacetCutAction.Add, getComplianceSelectors());
        cuts[7] = _createCut(TOKEN_TRANSFER_FACET, FacetCutAction.Add, getTransferSelectors());
        
        return cuts;
    }
    
    function _getShareClassFacets() private pure returns (IDiamond.FacetCut[] memory) {
        IDiamond.FacetCut[] memory baseCuts = _getBaseFacets();
        IDiamond.FacetCut[] memory specializedCuts = new IDiamond.FacetCut[](3);
        
        specializedCuts[0] = _createCut(
            TOKEN_CORPORATE_ACTIONS_FACET, 
            FacetCutAction.Add, 
            getCorporateActionsSelectors()
        );
        specializedCuts[1] = _createCut(
            TOKEN_VOTING_FACET, 
            FacetCutAction.Add, 
            getVotingSelectors()
        );
        specializedCuts[2] = _createCut(
            TOKEN_CUSTODY_FACET, 
            FacetCutAction.Add, 
            getCustodySelectors()
        );
        
        return _mergeCuts(baseCuts, specializedCuts);
    }
    
    function _getMembershipUnitFacets() private pure returns (IDiamond.FacetCut[] memory) {
        IDiamond.FacetCut[] memory baseCuts = _getBaseFacets();
        IDiamond.FacetCut[] memory specializedCuts = new IDiamond.FacetCut[](3);
        
        specializedCuts[0] = _createCut(
            TOKEN_DISTRIBUTIONS_FACET, 
            FacetCutAction.Add, 
            getDistributionsSelectors()
        );
        specializedCuts[1] = _createCut(
            TOKEN_CAPITAL_ACCOUNT_FACET, 
            FacetCutAction.Add, 
            getCapitalAccountSelectors()
        );
        specializedCuts[2] = _createCut(
            TOKEN_ALLOCATIONS_FACET, 
            FacetCutAction.Add, 
            getAllocationsSelectors()
        );
        
        return _mergeCuts(baseCuts, specializedCuts);
    }
    
    function _getFundUnitFacets() private pure returns (IDiamond.FacetCut[] memory) {
        IDiamond.FacetCut[] memory baseCuts = _getBaseFacets();
        IDiamond.FacetCut[] memory specializedCuts = new IDiamond.FacetCut[](3);
        
        specializedCuts[0] = _createCut(
            TOKEN_FUND_UNIT_FACET, 
            FacetCutAction.Add, 
            getFundUnitSelectors()
        );
        specializedCuts[1] = _createCut(
            TOKEN_REDEMPTION_FACET, 
            FacetCutAction.Add, 
            getRedemptionSelectors()
        );
        specializedCuts[2] = _createCut(
            TOKEN_NAV_FACET, 
            FacetCutAction.Add, 
            getNAVSelectors()
        );
        
        return _mergeCuts(baseCuts, specializedCuts);
    }
    
    function _getEmployeeStockOptionFacets() private pure returns (IDiamond.FacetCut[] memory) {
        IDiamond.FacetCut[] memory baseCuts = _getBaseFacets();
        IDiamond.FacetCut[] memory specializedCuts = new IDiamond.FacetCut[](3);
        
        specializedCuts[0] = _createCut(
            TOKEN_ESO_FACET, 
            FacetCutAction.Add, 
            getESOSelectors()
        );
        specializedCuts[1] = _createCut(
            TOKEN_VESTING_FACET, 
            FacetCutAction.Add, 
            getVestingSelectors()
        );
        specializedCuts[2] = _createCut(
            TOKEN_EXERCISE_FACET, 
            FacetCutAction.Add, 
            getExerciseSelectors()
        );
        
        return _mergeCuts(baseCuts, specializedCuts);
    }
    
    function _getConvertibleNoteFacets() private pure returns (IDiamond.FacetCut[] memory) {
        IDiamond.FacetCut[] memory baseCuts = _getBaseFacets();
        IDiamond.FacetCut[] memory specializedCuts = new IDiamond.FacetCut[](3);
        
        specializedCuts[0] = _createCut(
            TOKEN_DEBT_FACET, 
            FacetCutAction.Add, 
            getDebtSelectors()
        );
        specializedCuts[1] = _createCut(
            TOKEN_MATURITY_FACET, 
            FacetCutAction.Add, 
            getMaturitySelectors()
        );
        specializedCuts[2] = _createCut(
            TOKEN_CONVERSION_FACET, 
            FacetCutAction.Add, 
            getConversionSelectors()
        );
        
        return _mergeCuts(baseCuts, specializedCuts);
    }
    
    // Helper functions
    function _createCut(
        address facet,
        FacetCutAction action,
        bytes4[] memory selectors
    ) private pure returns (IDiamond.FacetCut memory) {
        return IDiamond.FacetCut({
            facet: facet,
            action: action,
            selectors: selectors
        });
    }
    
    function _mergeCuts(
        IDiamond.FacetCut[] memory a,
        IDiamond.FacetCut[] memory b
    ) private pure returns (IDiamond.FacetCut[] memory) {
        IDiamond.FacetCut[] memory merged = new IDiamond.FacetCut[](a.length + b.length);
        
        for (uint256 i = 0; i < a.length; i++) {
            merged[i] = a[i];
        }
        
        for (uint256 i = 0; i < b.length; i++) {
            merged[a.length + i] = b[i];
        }
        
        return merged;
    }
}
```

---

## Updated Token Factory

**File**: `protocol/src/tokens/factory/ITokenFactory.sol`

```solidity
interface ITokenFactory {
    struct TokenConfig {
        TokenAssetType tokenType;  // ← New field
        string name;
        string symbol;
        uint8 decimals;
        string baseURI;
        address admin;
        
        // Type-specific configs (optional based on type)
        address underlyingAsset;    // For ESO, Warrant, SAR
        uint256 strikePrice;        // For options/warrants
        uint256 principalAmount;    // For debt instruments
        uint256 interestRate;       // For debt instruments
        uint256 maturityDate;       // For debt instruments
    }
    
    function createToken(
        TokenConfig memory config,
        IDiamond.FacetCut[] memory additionalFacets,  // Optional extra facets
        address init,
        bytes memory initData,
        ComplianceConditionConfig[] memory complianceConditions
    ) external returns (DeploymentResult memory result);
}
```

**Implementation**: `protocol/src/tokens/factory/facets/core/TokenFactoryCoreBase.sol`

```solidity
function _createToken(
    ITokenFactory.TokenConfig memory config,
    IDiamond.FacetCut[] memory additionalFacets,
    address init,
    bytes memory initData,
    ITokenFactory.ComplianceConditionConfig[] memory complianceConditions
) internal returns (ITokenFactory.DeploymentResult memory result) {
    
    // Get type-specific facets
    IDiamond.FacetCut[] memory typedFacets = TokenTypePresets.getFacetCutsForType(
        config.tokenType
    );
    
    // Merge with any additional facets provided
    IDiamond.FacetCut[] memory allFacets = _mergeFacetCuts(typedFacets, additionalFacets);
    
    // Deploy token diamond
    Diamond.InitParams memory initParams = Diamond.InitParams({
        baseFacets: allFacets,
        init: init,
        initData: initData
    });
    
    DiamondFactory factory = DiamondFactory(s.diamondFactory);
    address tokenDiamond = factory.createDiamond(initParams);
    
    // Deploy and configure compliance conditions
    // ... (existing compliance logic)
    
    emit TokenCreated(
        tokenDiamond,
        config.admin,
        config.tokenType,  // ← Emit token type
        config.name,
        config.symbol,
        result.complianceConditions
    );
    
    return result;
}
```

---

## Investment Vehicle Diamond Architecture (VehicleFactory)

### Vehicle Types and Configuration

```solidity
// protocol/src/vehicles/IVehicle.sol

enum VehicleType {
    HEDGE_FUND,         // Liquid hedge fund
    PE_FUND,            // Private equity fund (committed capital)
    VC_FUND,            // Venture capital fund (committed capital)
    REAL_ESTATE_FUND,   // Real estate fund
    SPV,                // Single-purpose vehicle 🔥 HIGH PRIORITY
    SYNDICATE           // Rolling SPVs / deal-by-deal
}

enum VehicleStructure {
    LIQUID,        // Hedge fund, mutual fund (continuous NAV, redemptions)
    COMMITTED,     // PE/VC fund (capital calls, distributions)
    SINGLE_ASSET,  // SPV - one investment only 🔥 HIGH PRIORITY
    CLOSED_END,    // Fixed capital, no redemptions
    INTERVAL,      // Periodic redemption windows
    PASS_THROUGH   // No active management (SPV variant)
}

struct VehicleConfig {
    string name;
    string symbol;
    VehicleType vehicleType;
    VehicleStructure structure;
    address unitToken;              // Token diamond (FundUnit or SPVUnit)
    address manager;
    
    // Fee structure
    uint256 managementFeeBps;
    uint256 carriedInterestBps;     // Performance fee / carry
    uint256 hurdleRateBps;
    bool highWaterMark;             // For funds only
    
    // Investment parameters
    uint256 minInvestment;
    address denominationAsset;
    
    // Structure-specific parameters
    uint256 targetSize;             // For funds
    address underlyingAsset;        // For SPVs (optional, can be set later)
    uint256 liquidationDate;        // For SPVs (optional)
    
    // Redemption settings (for liquid funds only)
    uint256 redemptionNoticeDays;
    uint256 redemptionFeeBps;
    uint256 lockupPeriod;
    
    // Commitment settings (for PE/VC funds)
    uint256 commitmentPeriod;
    uint256 investmentPeriod;
}
```

### Vehicle Facet Architecture

```
protocol/src/vehicles/facets/
├── core/                        # Base (all vehicles)
│   ├── VehicleCoreFacet.sol
│   └── VehicleCoreStorage.sol
│
├── investment/                  # Investment tracking (all vehicles)
│   ├── VehicleInvestmentFacet.sol
│   └── VehicleInvestmentStorage.sol
│
├── fund/                        # Multi-asset fund operations
│   ├── portfolio/
│   │   ├── PortfolioManagementFacet.sol
│   │   ├── PortfolioRebalancingFacet.sol
│   │   └── PortfolioValuationFacet.sol
│   ├── nav/
│   │   └── NAVCalculationFacet.sol
│   ├── oracle/
│   │   └── PriceFeedFacet.sol
│   └── liquidity/
│       ├── RedemptionFacet.sol
│       └── LiquidityWindowFacet.sol
│
├── spv/                         # 🔥 SPV-specific (HIGH PRIORITY)
│   ├── SPVCoreFacet.sol         # SPV lifecycle management
│   ├── SPVInvestmentFacet.sol   # Single investment tracking
│   ├── SPVValuationFacet.sol    # Pass-through valuation
│   └── SPVWaterfallFacet.sol    # Simplified carry distribution
│
├── committed/                   # PE/VC style (both funds & SPVs can use)
│   ├── CapitalCallFacet.sol
│   ├── CommitmentFacet.sol
│   ├── DistributionFacet.sol
│   └── WaterfallFacet.sol
│
└── performance/                 # Performance tracking (funds)
    ├── PerformanceFacet.sol
    └── HighWaterMarkFacet.sol
```

### Vehicle Factory with Type Presets

```solidity
// protocol/src/vehicles/factory/VehicleTypePresets.sol

library VehicleTypePresets {
    function getFacetCutsForVehicle(
        VehicleType vehicleType,
        VehicleStructure structure
    ) 
        internal 
        pure 
        returns (IDiamond.FacetCut[] memory) 
    {
        if (vehicleType == VehicleType.SPV) {
            return _getSPVFacets(structure);  // 🔥 HIGH PRIORITY
        } else if (vehicleType == VehicleType.HEDGE_FUND) {
            return _getHedgeFundFacets();
        } else if (vehicleType == VehicleType.PE_FUND || vehicleType == VehicleType.VC_FUND) {
            return _getCommittedFundFacets();
        }
        // ... other vehicle types
    }
    
    function _getBaseVehicleFacets() private pure returns (IDiamond.FacetCut[] memory) {
        // Core, Investment, Distribution (common to all vehicles)
    }
    
    // 🔥 HIGH PRIORITY: SPV Facets
    function _getSPVFacets(VehicleStructure structure) 
        private pure returns (IDiamond.FacetCut[] memory) 
    {
        IDiamond.FacetCut[] memory baseCuts = _getBaseVehicleFacets();
        IDiamond.FacetCut[] memory spvCuts = new IDiamond.FacetCut[](4);
        
        spvCuts[0] = _createCut(SPV_CORE_FACET, ...);
        spvCuts[1] = _createCut(SPV_INVESTMENT_FACET, ...);
        spvCuts[2] = _createCut(SPV_VALUATION_FACET, ...);
        spvCuts[3] = _createCut(SPV_WATERFALL_FACET, ...);
        
        // Note: NO portfolio management, NO NAV calculation
        // SPV is simpler - just tracks one investment
        
        return _mergeCuts(baseCuts, spvCuts);
    }
    
    function _getHedgeFundFacets() private pure returns (IDiamond.FacetCut[] memory) {
        IDiamond.FacetCut[] memory baseCuts = _getBaseVehicleFacets();
        IDiamond.FacetCut[] memory fundCuts = new IDiamond.FacetCut[](5);
        
        fundCuts[0] = _createCut(PORTFOLIO_MANAGEMENT_FACET, ...);
        fundCuts[1] = _createCut(NAV_CALCULATION_FACET, ...);
        fundCuts[2] = _createCut(REDEMPTION_FACET, ...);
        fundCuts[3] = _createCut(PERFORMANCE_FACET, ...);
        fundCuts[4] = _createCut(HIGH_WATER_MARK_FACET, ...);
        
        return _mergeCuts(baseCuts, fundCuts);
    }
    
    function _getCommittedFundFacets() private pure returns (IDiamond.FacetCut[] memory) {
        IDiamond.FacetCut[] memory baseCuts = _getBaseVehicleFacets();
        IDiamond.FacetCut[] memory commitCuts = new IDiamond.FacetCut[](5);
        
        commitCuts[0] = _createCut(PORTFOLIO_MANAGEMENT_FACET, ...);
        commitCuts[1] = _createCut(COMMITMENT_FACET, ...);
        commitCuts[2] = _createCut(CAPITAL_CALL_FACET, ...);
        commitCuts[3] = _createCut(DISTRIBUTION_FACET, ...);
        commitCuts[4] = _createCut(WATERFALL_FACET, ...);
        
        return _mergeCuts(baseCuts, commitCuts);
    }
}
```

### Vehicle + Token Relationship

```
┌─────────────────────────────────────────┐
│      Vehicle Diamond (Fund or SPV)      │
│    (Investment Management)              │
│                                         │
│   For Fund:                             │
│   • VehicleCoreFacet                   │
│   • PortfolioManagementFacet           │
│   • NAVCalculationFacet                │
│   • RedemptionFacet                    │
│                                         │
│   For SPV: 🔥                           │
│   • VehicleCoreFacet                   │
│   • SPVInvestmentFacet                 │
│   • SPVWaterfallFacet                  │
│                                         │
│   config.unitToken: 0xABC...   ────────┼───┐
└─────────────────────────────────────────┘   │
                                              │
                                              ▼
                         ┌────────────────────────────────┐
                         │  Token Diamond                  │
                         │  (FundUnit or SPVUnit)         │
                         │                                │
                         │  For FundUnit:                 │
                         │  • TokenFundUnitFacet          │
                         │  • TokenRedemptionFacet        │
                         │  • TokenNAVFacet               │
                         │                                │
                         │  For SPVUnit: 🔥               │
                         │  • TokenSPVUnitFacet           │
                         │  • TokenSPVInvestmentFacet     │
                         │  • TokenSPVWaterfallFacet      │
                         │                                │
                         │  Investors hold these tokens   │
                         └────────────────────────────────┘
```

---

## Type-Specific Facet Examples

### 1. MembershipUnit: Distributions Facet

```solidity
// protocol/src/tokens/facets/membership/distributions/TokenDistributionsFacet.sol

contract TokenDistributionsFacet {
    event DistributionDeclared(
        uint256 indexed distributionId,
        uint256 totalAmount,
        uint256 perUnit,
        uint256 distributionDate,
        DistributionType dtype
    );
    
    enum DistributionType {
        OperatingIncome,
        CapitalGain,
        ReturnOfCapital
    }
    
    function declareDistribution(
        uint256 totalAmount,
        DistributionType dtype
    ) external onlyAdmin returns (uint256 distributionId) {
        // Calculate per-unit amount
        uint256 totalSupply = TokenBalancesStorage.layout().totalSupply;
        uint256 perUnit = (totalAmount * 1e18) / totalSupply;
        
        // Store distribution
        TokenDistributionsStorage.Layout storage l = TokenDistributionsStorage.layout();
        distributionId = l.nextDistributionId++;
        
        l.distributions[distributionId] = Distribution({
            totalAmount: totalAmount,
            perUnit: perUnit,
            distributionDate: block.timestamp,
            dtype: dtype,
            claimed: 0
        });
        
        emit DistributionDeclared(distributionId, totalAmount, perUnit, block.timestamp, dtype);
    }
    
    function claimDistribution(uint256 distributionId) external {
        // Member claims their share of distribution
        // ... implementation
    }
}
```

### 2. FundUnit: NAV Facet

```solidity
// protocol/src/tokens/facets/fund/TokenNAVFacet.sol

contract TokenNAVFacet {
    event NAVUpdated(uint256 navPerShare, uint256 totalAUM, uint256 timestamp);
    
    function updateNAV(uint256 totalAUM) external onlyFundManager {
        TokenNAVStorage.Layout storage l = TokenNAVStorage.layout();
        
        uint256 totalSupply = TokenBalancesStorage.layout().totalSupply;
        uint256 navPerShare = (totalAUM * 1e18) / totalSupply;
        
        l.currentNAV = navPerShare;
        l.totalAUM = totalAUM;
        l.lastUpdate = block.timestamp;
        
        // Update high-water mark if necessary
        if (navPerShare > l.highWaterMark) {
            l.highWaterMark = navPerShare;
        }
        
        emit NAVUpdated(navPerShare, totalAUM, block.timestamp);
    }
    
    function getCurrentNAV() external view returns (uint256 navPerShare, uint256 totalAUM) {
        TokenNAVStorage.Layout storage l = TokenNAVStorage.layout();
        return (l.currentNAV, l.totalAUM);
    }
}
```

### 3. ConvertibleNote: Conversion Facet

```solidity
// protocol/src/tokens/facets/convertibles/TokenConversionFacet.sol

contract TokenConversionFacet {
    event ConversionTriggered(
        bytes32 indexed noteLotId,
        address indexed targetShareClass,
        uint256 principalConverted,
        uint256 sharesReceived
    );
    
    function convertToEquity(
        bytes32 noteLotId,
        address targetShareClass,
        uint256 conversionPrice
    ) external returns (bytes32 shareLotId) {
        // Get note details
        TokenConversionStorage.Layout storage l = TokenConversionStorage.layout();
        ConversionTerms memory terms = l.conversionTerms;
        
        // Calculate conversion
        IToken.Lot memory noteLot = _getLot(noteLotId);
        uint256 principal = noteLot.quantity;
        
        // Apply valuation cap if present
        if (terms.valuationCap > 0 && conversionPrice > terms.valuationCap) {
            conversionPrice = terms.valuationCap;
        }
        
        // Apply discount
        if (terms.discountRate > 0) {
            conversionPrice = (conversionPrice * (10000 - terms.discountRate)) / 10000;
        }
        
        uint256 sharesReceived = (principal * 1e18) / conversionPrice;
        
        // Invalidate note lot
        _invalidateLot(noteLotId);
        
        // Create shares in target ShareClass
        (shareLotId,) = IToken(targetShareClass).createLot(
            noteLot.owner,
            sharesReceived,
            address(0),
            conversionPrice,
            block.timestamp,
            "Converted from note",
            abi.encode("Note conversion"),
            0
        );
        
        emit ConversionTriggered(noteLotId, targetShareClass, principal, sharesReceived);
        
        return shareLotId;
    }
}
```

### 4. EmployeeStockOption: Vesting Facet

```solidity
// protocol/src/tokens/facets/derivatives/options/TokenVestingFacet.sol

contract TokenVestingFacet {
    struct VestingSchedule {
        uint256 startDate;
        uint256 cliffDate;
        uint256 endDate;
        uint256 totalAmount;
        uint256 vestedAmount;
        bool revoked;
    }
    
    function createVestingSchedule(
        bytes32 lotId,
        uint256 startDate,
        uint256 cliffDuration,
        uint256 vestingDuration
    ) external onlyAdmin {
        IToken.Lot memory lot = _getLot(lotId);
        
        TokenVestingStorage.Layout storage l = TokenVestingStorage.layout();
        l.schedules[lotId] = VestingSchedule({
            startDate: startDate,
            cliffDate: startDate + cliffDuration,
            endDate: startDate + vestingDuration,
            totalAmount: lot.quantity,
            vestedAmount: 0,
            revoked: false
        });
    }
    
    function getVestedAmount(bytes32 lotId) public view returns (uint256) {
        TokenVestingStorage.Layout storage l = TokenVestingStorage.layout();
        VestingSchedule storage schedule = l.schedules[lotId];
        
        if (schedule.revoked) return schedule.vestedAmount;
        if (block.timestamp < schedule.cliffDate) return 0;
        if (block.timestamp >= schedule.endDate) return schedule.totalAmount;
        
        // Linear vesting
        uint256 elapsed = block.timestamp - schedule.startDate;
        uint256 duration = schedule.endDate - schedule.startDate;
        
        return (schedule.totalAmount * elapsed) / duration;
    }
}
```

---

## Subgraph Schema Updates

```graphql
# Updated enum
enum TokenAssetType {
  ShareClass
  MembershipUnit
  FundUnit
  ConvertibleNote
  Bond
  PromissoryNote
  EmployeeStockOption
  Warrant
  SAFE
  StockAppreciationRight
}

# Common interface
interface IToken {
  id: ID!
  name: String!
  symbol: String!
  decimals: Int!
  totalSupply: BigInt!
  admin: Bytes!
  createdAt: BigInt!
  assetType: TokenAssetType!
  supportsConversion: Boolean!
  
  # Relationships
  lots: [Lot!]!
  offerings: [Offering!]!
}

# MembershipUnit type
type MembershipUnit implements IToken @entity {
  # All IToken fields
  id: ID!
  name: String!
  symbol: String!
  decimals: Int!
  totalSupply: BigInt!
  admin: Bytes!
  createdAt: BigInt!
  assetType: TokenAssetType!
  supportsConversion: Boolean!  # Always false
  lots: [Lot!]! @derivedFrom(field: "token")
  offerings: [Offering!]! @derivedFrom(field: "token")
  
  # MembershipUnit-specific
  totalCapitalContributions: BigInt!
  totalDistributions: BigInt!
  distributions: [Distribution!]! @derivedFrom(field: "token")
  capitalAccounts: [CapitalAccount!]! @derivedFrom(field: "token")
  
  # Admin state
  paused: Boolean!
  frozenAccounts: [Bytes!]!
  transferController: Bytes
  hasTransferConditions: Boolean!
  complianceConditions: [Bytes!]!
}

type Distribution @entity {
  id: ID!
  token: MembershipUnit!
  distributionId: BigInt!
  totalAmount: BigInt!
  perUnit: BigInt!
  distributionDate: BigInt!
  dtype: DistributionType!
  claimed: BigInt!
  tx: Bytes!
}

enum DistributionType {
  OperatingIncome
  CapitalGain
  ReturnOfCapital
}

type CapitalAccount @entity {
  id: ID! # token-holder
  token: MembershipUnit!
  holder: Wallet!
  capitalContributed: BigInt!
  distributionsReceived: BigInt!
  allocatedIncome: BigInt!
  allocatedLoss: BigInt!
  currentBalance: BigInt!
}

# FundUnit type
type FundUnit implements IToken @entity {
  # All IToken fields
  id: ID!
  name: String!
  symbol: String!
  decimals: Int!
  totalSupply: BigInt!
  admin: Bytes!
  createdAt: BigInt!
  assetType: TokenAssetType!
  supportsConversion: Boolean!  # Always false
  lots: [Lot!]! @derivedFrom(field: "token")
  offerings: [Offering!]! @derivedFrom(field: "token")
  
  # FundUnit-specific
  vehicle: Bytes!                 # Vehicle diamond address (Fund)
  currentNAV: BigInt!
  totalAUM: BigInt!
  managementFeeBps: Int!
  performanceFeeBps: Int!
  highWaterMark: BigInt!
  
  # Redemptions
  redemptions: [FundRedemption!]! @derivedFrom(field: "fundUnit")
  
  # Admin state
  paused: Boolean!
  frozenAccounts: [Bytes!]!
  complianceConditions: [Bytes!]!
}

# 🔥 SPVUnit type (HIGH PRIORITY)
type SPVUnit implements IToken @entity {
  # All IToken fields
  id: ID!
  name: String!
  symbol: String!
  decimals: Int!
  totalSupply: BigInt!
  admin: Bytes!
  createdAt: BigInt!
  assetType: TokenAssetType!
  supportsConversion: Boolean!  # Always false
  lots: [Lot!]! @derivedFrom(field: "token")
  offerings: [Offering!]! @derivedFrom(field: "token")
  
  # SPVUnit-specific
  vehicle: Bytes!                 # Vehicle diamond address (SPV)
  underlyingAsset: Bytes          # What the SPV invested in
  investmentAmount: BigInt!       # How much was invested
  investmentDate: BigInt          # When investment was made
  liquidationDate: BigInt         # When SPV winds down
  liquidated: Boolean!
  
  # Valuation (pass-through from underlying)
  currentValuation: BigInt!
  lastValuationDate: BigInt!
  
  # Carry structure
  carriedInterestBps: Int!
  carryRecipient: Bytes!
  carryPaid: Boolean!
  
  # Distributions
  distributions: [SPVDistribution!]! @derivedFrom(field: "spvUnit")
  
  # Admin state
  paused: Boolean!
  frozenAccounts: [Bytes!]!
  complianceConditions: [Bytes!]!
}

type SPVDistribution @entity {
  id: ID!
  spvUnit: SPVUnit!
  totalAmount: BigInt!
  carryAmount: BigInt!
  lpAmount: BigInt!
  distributedAt: BigInt!
  dtype: SPVDistributionType!
  tx: Bytes!
}

enum SPVDistributionType {
  PartialExit
  FinalLiquidation
  DividendIncome
}

type FundRedemption @entity {
  id: ID!
  fundUnit: FundUnit!
  investor: Wallet!
  unitsRedeemed: BigInt!
  amountReceived: BigInt!
  navAtRedemption: BigInt!
  redeemedAt: BigInt!
  tx: Bytes!
}
  offerings: [Offering!]! @derivedFrom(field: "token")
  
  # FundUnit-specific
  fund: Bytes!                    # Fund diamond address
  currentNAV: BigInt!
  totalAUM: BigInt!
  managementFeeBps: Int!
  performanceFeeBps: Int!
  highWaterMark: BigInt!
  
  # Redemptions
  redemptions: [FundRedemption!]! @derivedFrom(field: "fundUnit")
  
  # Admin state
  paused: Boolean!
  frozenAccounts: [Bytes!]!
  complianceConditions: [Bytes!]!
}

type FundRedemption @entity {
  id: ID!
  fundUnit: FundUnit!
  investor: Wallet!
  unitsRedeemed: BigInt!
  amountReceived: BigInt!
  navAtRedemption: BigInt!
  redeemedAt: BigInt!
  tx: Bytes!
}

# Similar patterns for ConvertibleNote, EmployeeStockOption, etc.
```

---

## Implementation Phases

### Phase 1: MembershipUnit (Weeks 1-4)
**Protocol**:
- [ ] Create `TokenDistributionsFacet`
- [ ] Create `TokenCapitalAccountFacet`
- [ ] Create `TokenAllocationsFacet`
- [ ] Update `TokenTypePresets` with MembershipUnit
- [ ] Deploy and test facets

**Subgraph**:
- [ ] Add `MembershipUnit` entity
- [ ] Add `Distribution` and `CapitalAccount` entities
- [ ] Create membership-specific mappings
- [ ] Deploy updated subgraph

**Interface**:
- [ ] See `INTERFACE_TOKEN_TYPES.md` Phase 2

### Phase 2: SPVUnit & VehicleFactory 🔥 (Weeks 5-8) **HIGH PRIORITY**
**Why First**: SPVs are simpler than funds (single investment), highly demanded for deal syndication, and validate the VehicleFactory architecture.

**Protocol**:
- [ ] Rename `funds/` to `vehicles/`
- [ ] Create `IVehicle.sol` with `VehicleType` and `VehicleStructure` enums
- [ ] Create `VehicleFactory` (rename from FundFactory)
- [ ] Create `VehicleTypePresets.sol`
- [ ] Create SPV-specific facets:
  - [ ] `SPVCoreFacet` - lifecycle management
  - [ ] `SPVInvestmentFacet` - single investment tracking
  - [ ] `SPVValuationFacet` - pass-through valuation
  - [ ] `SPVWaterfallFacet` - carry distribution
- [ ] Create SPVUnit token facets:
  - [ ] `TokenSPVUnitFacet`
  - [ ] `TokenSPVInvestmentFacet`
  - [ ] `TokenSPVWaterfallFacet`

**Subgraph**:
- [ ] Rename Fund entities to Vehicle entities
- [ ] Add `SPVUnit` token entity
- [ ] Add `SPVDistribution` entity
- [ ] Add `Vehicle` entity (base for Fund/SPV diamonds)
- [ ] Create SPV-specific mappings

**Interface**:
- [ ] Add SPV creation flow
- [ ] Add SPV management UI
- [ ] Add investment tracking interface
- [ ] Add distribution/liquidation interface
- [ ] Update terminology system for SPVs

### Phase 3: Enhanced Fund Support (Weeks 9-12)
**Protocol**:
- [ ] Enhance existing Fund diamond with VehicleFactory integration
- [ ] Create `TokenFundUnitFacet`
- [ ] Create `TokenRedemptionFacet`
- [ ] Create `TokenNAVFacet`
- [ ] Add high-water mark tracking
- [ ] Add performance fee calculation

**Subgraph**:
- [ ] Add `FundUnit` entity (if not already present)
- [ ] Add `FundRedemption` entity
- [ ] Link Vehicle and token entities

**Interface**:
- [ ] Fund creation via VehicleFactory
- [ ] NAV display and management
- [ ] Redemption interface
- [ ] Performance tracking

### Phase 4: EmployeeStockOptions (Weeks 13-16)
**Protocol**:
- [ ] Create `TokenESOFacet`
- [ ] Create `TokenVestingFacet`
- [ ] Create `TokenExerciseFacet`
- [ ] Handle underlying asset reference

**Subgraph**:
- [ ] Add `EmployeeStockOption` entity
- [ ] Add `ESOGrant` entity
- [ ] Link to underlying ShareClass

**Interface**:
- [ ] See `INTERFACE_TOKEN_TYPES.md` Phase 4

### Phase 5: SAFE & Convertible Instruments (Weeks 17-20)
**Protocol**:
- [ ] Create shared `TokenDebtFacet`
- [ ] Create `TokenConversionFacet`
- [ ] Create `TokenSAFEFacet`
- [ ] Create `TokenMaturityFacet`

**Subgraph**:
- [ ] Add `ConvertibleNote` entity
- [ ] Add `SAFE` entity
- [ ] Add `NoteConversion` tracking

### Phase 6: PE/VC Fund Support (Weeks 21-24)
**Vehicle Diamond**:
- [ ] Create `CommitmentFacet`
- [ ] Create `CapitalCallFacet`
- [ ] Create `DistributionFacet`
- [ ] Create `WaterfallFacet` (complex version for PE/VC)

---

## Testing Strategy

### Unit Tests
- Each facet in isolation
- Storage layout compatibility
- Selector uniqueness
- Access control

### Integration Tests
- Diamond deployment with type presets
- Facet interaction within diamond
- Cross-diamond interactions (Fund → FundUnit)
- Conversion mechanics (SAFE → ShareClass)

### E2E Tests
- Complete token lifecycle per type
- Fund creation and operations
- Multi-token workflows

---

## References

- Interface architecture: See `INTERFACE_TOKEN_TYPES.md`
- Diamond pattern: See existing diamond implementation
- Corporate actions: See `TokenCorporateActionsBase.sol`
- Current fund implementation: See `protocol/src/funds/`

