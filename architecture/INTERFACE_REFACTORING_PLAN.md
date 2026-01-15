# Interface Refactoring Plan: Diamond Configuration & Token Type System

**Status**: Ready to Implement  
**Priority**: High  
**Estimated Time**: 1 day with AI assistance  
**Last Updated**: 2025-10-24

---

## Overview

This document outlines a comprehensive refactoring of the interface layer to:
1. Reorganize diamond configuration files for better structure
2. Remove unused/duplicate code
3. Implement multi-token-type support
4. Prepare for SPV, MembershipUnit, and FundUnit tokens

---

## Current State Analysis

### Files Audit

#### ✅ `lib/wagmi/diamond-config.ts` (604 lines)
**Status**: Used (but needs refactoring)

**What it does**:
- Wallet facet configuration
- Offering facet configuration  
- Token facet configuration (ShareClass only)
- Uses generated selectors ✅

**Problems**:
- ❌ Wrong location (`wagmi` is for wallet connection, not diamond deployment)
- ❌ Monolithic file (hard to find specific configs)
- ❌ Only supports ShareClass tokens (hardcoded)
- ❌ No support for MembershipUnit, SPVUnit, FundUnit

**Used by**:
- `interface/src/app/offerings/create/page.tsx`
- `interface/src/app/tokens/create/page.tsx`

---

#### ❌ `lib/factories/offering-helpers.ts` (167 lines)
**Status**: UNUSED - DELETE

**Problems**:
- Hardcodes selectors (doesn't use generated ones)
- Duplicates functionality from `diamond-config.ts`
- Zero imports found in codebase

**Action**: **DELETE** ✅

---

#### ❌ `lib/factories/token-helpers.ts` (190 lines)
**Status**: UNUSED - DELETE

**Problems**:
- Hardcodes selectors (doesn't use generated ones)
- Duplicates functionality from `diamond-config.ts`
- Zero imports found in codebase

**Action**: **DELETE** ✅

---

#### ⚠️ `lib/factories/wallet-helpers.ts` (120 lines)
**Status**: Used indirectly

**What it does**:
- `OwnerType` enum
- `WalletConfig` interface
- Helper functions for wallet configuration

**Action**: **MOVE** to new structure

---

## New Structure

### Directory Layout

```
interface/src/lib/diamonds/              # NEW! (was in lib/wagmi)
├── wallets/
│   ├── wallet-config.ts                 # Facet cuts & multi-init
│   └── wallet-types.ts                  # OwnerType enum, WalletConfig
│
├── tokens/
│   ├── base-token-config.ts             # Base facets (all tokens)
│   ├── share-class-config.ts            # ShareClass-specific
│   ├── membership-unit-config.ts        # MembershipUnit-specific (NEW)
│   ├── spv-unit-config.ts              # SPVUnit-specific (NEW) 🔥
│   ├── fund-unit-config.ts             # FundUnit-specific (NEW)
│   ├── token-types.ts                   # TokenAssetType enum
│   └── index.ts                         # buildTokenFacetCuts(tokenType)
│
├── offerings/
│   ├── offering-config.ts               # Facet cuts & multi-init
│   └── offering-types.ts                # Offering types
│
├── vehicles/                             # For Fund/SPV factories (future)
│   └── .gitkeep
│
└── index.ts                              # Re-export public API
```

---

## Detailed File Specifications

### `lib/diamonds/tokens/base-token-config.ts`

```typescript
import { getFacetSelectors, getFacetAddress } from "@/lib/generated/facet-selectors";
import type { FacetCut } from "@/types";

/**
 * Base facets that ALL token types require
 * These facets provide core ERC-20 functionality and lot-based accounting
 */
export function getBaseTokenFacets(): FacetCut[] {
  return [
    // Upgradeability
    {
      facet: getFacetAddress('DiamondCutFacet'),
      action: 0,
      selectors: getFacetSelectors('DiamondCutFacet'),
    },
    // Introspection
    {
      facet: getFacetAddress('DiamondLoupeFacet'),
      action: 0,
      selectors: getFacetSelectors('DiamondLoupeFacet'),
    },
    // Access Control
    {
      facet: getFacetAddress('AccessControlFacet'),
      action: 0,
      selectors: getFacetSelectors('AccessControlFacet'),
    },
    // Token Identity
    {
      facet: getFacetAddress('TokenMetadataFacet'),
      action: 0,
      selectors: getFacetSelectors('TokenMetadataFacet'),
    },
    // Balance Management
    {
      facet: getFacetAddress('TokenBalancesFacet'),
      action: 0,
      selectors: getFacetSelectors('TokenBalancesFacet'),
    },
    // Lot-Based Accounting
    {
      facet: getFacetAddress('TokenLotsFacet'),
      action: 0,
      selectors: getFacetSelectors('TokenLotsFacet'),
    },
    // Admin Functions
    {
      facet: getFacetAddress('TokenAdminFacet'),
      action: 0,
      selectors: getFacetSelectors('TokenAdminFacet'),
    },
    // ERC-20 Interface
    {
      facet: getFacetAddress('TokenERC20Facet'),
      action: 0,
      selectors: getFacetSelectors('TokenERC20Facet'),
    },
    // Transfer Logic
    {
      facet: getFacetAddress('TokenTransferFacet'),
      action: 0,
      selectors: getFacetSelectors('TokenTransferFacet'),
    },
  ];
}

/**
 * Get base multi-init calls for all token types
 */
export function getBaseTokenInitCalls(params: {
  admin: Address;
  name: string;
  symbol: string;
  decimals: number;
  baseURI: string;
}): MultiInitCall[] {
  return [
    // 1. DiamondLoupe
    {
      init: getFacetAddress('DiamondLoupeFacet'),
      initData: encodeFunctionData({
        abi: [{ name: "DiamondLoupe_init", type: "function", inputs: [], outputs: [] }],
        functionName: "DiamondLoupe_init",
        args: [],
      }),
    },
    // 2. AccessControl
    {
      init: getFacetAddress('AccessControlFacet'),
      initData: encodeFunctionData({
        abi: [{ name: "AccessControl_init", type: "function", inputs: [{ name: "admin", type: "address" }], outputs: [] }],
        functionName: "AccessControl_init",
        args: [params.admin],
      }),
    },
    // 3. TokenMetadata
    {
      init: getFacetAddress('TokenMetadataFacet'),
      initData: encodeFunctionData({
        abi: [{ 
          name: "TokenMetadata_init", 
          type: "function", 
          inputs: [
            { name: "name", type: "string" },
            { name: "symbol", type: "string" },
            { name: "decimals", type: "uint8" },
            { name: "baseURI", type: "string" },
          ], 
          outputs: [] 
        }],
        functionName: "TokenMetadata_init",
        args: [params.name, params.symbol, params.decimals, params.baseURI],
      }),
    },
    // 4. TokenBalances
    {
      init: getFacetAddress('TokenBalancesFacet'),
      initData: encodeFunctionData({
        abi: [{ name: "TokenBalances_init", type: "function", inputs: [], outputs: [] }],
        functionName: "TokenBalances_init",
        args: [],
      }),
    },
  ];
}
```

---

### `lib/diamonds/tokens/share-class-config.ts`

```typescript
import { getFacetSelectors, getFacetAddress } from "@/lib/generated/facet-selectors";
import type { FacetCut, MultiInitCall } from "@/types";

/**
 * ShareClass-specific facets
 * Adds corporate actions (splits, dividends) and voting
 */
export function getShareClassSpecificFacets(): FacetCut[] {
  return [
    {
      facet: getFacetAddress('TokenCorporateActionsFacet'),
      action: 0,
      selectors: getFacetSelectors('TokenCorporateActionsFacet'),
    },
    // Future: TokenVotingFacet, TokenCustodyFacet
  ];
}

/**
 * ShareClass-specific initialization calls
 */
export function getShareClassInitCalls(): MultiInitCall[] {
  return [
    {
      init: getFacetAddress('TokenCorporateActionsFacet'),
      initData: encodeFunctionData({
        abi: [{ name: "TokenCorporateActions_init", type: "function", inputs: [], outputs: [] }],
        functionName: "TokenCorporateActions_init",
        args: [],
      }),
    },
  ];
}
```

---

### `lib/diamonds/tokens/spv-unit-config.ts` 🔥

```typescript
import { getFacetSelectors, getFacetAddress } from "@/lib/generated/facet-selectors";
import type { FacetCut, MultiInitCall } from "@/types";

/**
 * SPVUnit-specific facets
 * For single-purpose investment vehicles
 */
export function getSPVUnitSpecificFacets(): FacetCut[] {
  return [
    // FUTURE: When SPV facets are deployed
    // {
    //   facet: getFacetAddress('TokenSPVUnitFacet'),
    //   action: 0,
    //   selectors: getFacetSelectors('TokenSPVUnitFacet'),
    // },
    // {
    //   facet: getFacetAddress('TokenSPVInvestmentFacet'),
    //   action: 0,
    //   selectors: getFacetSelectors('TokenSPVInvestmentFacet'),
    // },
    // {
    //   facet: getFacetAddress('TokenSPVWaterfallFacet'),
    //   action: 0,
    //   selectors: getFacetSelectors('TokenSPVWaterfallFacet'),
    // },
  ];
  
  return []; // Empty until SPV facets are deployed
}

/**
 * SPVUnit-specific initialization calls
 */
export function getSPVUnitInitCalls(params: {
  vehicle: Address;           // SPV diamond address
  underlyingAsset: Address;   // What the SPV invested in
  investmentAmount: bigint;   // Initial investment
  carriedInterestBps: number; // Carry percentage (basis points)
  carryRecipient: Address;    // Who receives carry
}): MultiInitCall[] {
  // FUTURE: When SPV facets are deployed
  return [];
}
```

---

### `lib/diamonds/tokens/membership-unit-config.ts`

```typescript
import { getFacetSelectors, getFacetAddress } from "@/lib/generated/facet-selectors";
import type { FacetCut, MultiInitCall } from "@/types";

/**
 * MembershipUnit-specific facets
 * For LLC/Partnership units (operating businesses)
 */
export function getMembershipUnitSpecificFacets(): FacetCut[] {
  // FUTURE: When membership facets are deployed
  // return [
  //   {
  //     facet: getFacetAddress('TokenDistributionsFacet'),
  //     action: 0,
  //     selectors: getFacetSelectors('TokenDistributionsFacet'),
  //   },
  //   {
  //     facet: getFacetAddress('TokenCapitalAccountFacet'),
  //     action: 0,
  //     selectors: getFacetSelectors('TokenCapitalAccountFacet'),
  //   },
  //   {
  //     facet: getFacetAddress('TokenAllocationsFacet'),
  //     action: 0,
  //     selectors: getFacetSelectors('TokenAllocationsFacet'),
  //   },
  // ];
  
  return []; // Empty until membership facets are deployed
}

/**
 * MembershipUnit-specific initialization calls
 */
export function getMembershipUnitInitCalls(): MultiInitCall[] {
  // FUTURE: When membership facets are deployed
  return [];
}
```

---

### `lib/diamonds/tokens/fund-unit-config.ts`

```typescript
import { getFacetSelectors, getFacetAddress } from "@/lib/generated/facet-selectors";
import type { FacetCut, MultiInitCall } from "@/types";

/**
 * FundUnit-specific facets
 * For multi-asset investment funds
 */
export function getFundUnitSpecificFacets(): FacetCut[] {
  // FUTURE: When fund facets are deployed
  // return [
  //   {
  //     facet: getFacetAddress('TokenFundUnitFacet'),
  //     action: 0,
  //     selectors: getFacetSelectors('TokenFundUnitFacet'),
  //   },
  //   {
  //     facet: getFacetAddress('TokenRedemptionFacet'),
  //     action: 0,
  //     selectors: getFacetSelectors('TokenRedemptionFacet'),
  //   },
  //   {
  //     facet: getFacetAddress('TokenNAVFacet'),
  //     action: 0,
  //     selectors: getFacetSelectors('TokenNAVFacet'),
  //   },
  // ];
  
  return []; // Empty until fund facets are deployed
}

/**
 * FundUnit-specific initialization calls
 */
export function getFundUnitInitCalls(params: {
  vehicle: Address;          // Fund diamond address
  managementFeeBps: number;  // Management fee (basis points)
  performanceFeeBps: number; // Performance fee (basis points)
}): MultiInitCall[] {
  // FUTURE: When fund facets are deployed
  return [];
}
```

---

### `lib/diamonds/tokens/token-types.ts`

```typescript
/**
 * All supported token asset types
 */
export type TokenAssetType = 
  | 'ShareClass'
  | 'MembershipUnit'
  | 'SPVUnit'
  | 'FundUnit'
  | 'EmployeeStockOption'
  | 'ConvertibleNote'
  | 'Bond'
  | 'PromissoryNote'
  | 'Warrant'
  | 'SAFE'
  | 'StockAppreciationRight';

/**
 * Token type metadata
 */
export interface TokenTypeMetadata {
  key: TokenAssetType;
  label: string;
  description: string;
  supportsConversion: boolean;
  requiresUnderlyingAsset: boolean;
  category: 'equity' | 'debt' | 'derivative' | 'vehicle';
}

/**
 * Token type registry
 */
export const TOKEN_TYPE_METADATA: Record<TokenAssetType, TokenTypeMetadata> = {
  ShareClass: {
    key: 'ShareClass',
    label: 'Share Class',
    description: 'Corporate stock (C-Corp, S-Corp)',
    supportsConversion: true,
    requiresUnderlyingAsset: false,
    category: 'equity',
  },
  MembershipUnit: {
    key: 'MembershipUnit',
    label: 'Membership Unit',
    description: 'LLC/Partnership units (operating businesses)',
    supportsConversion: false,
    requiresUnderlyingAsset: false,
    category: 'equity',
  },
  SPVUnit: {
    key: 'SPVUnit',
    label: 'SPV Unit',
    description: 'Special purpose vehicle interests',
    supportsConversion: false,
    requiresUnderlyingAsset: false,
    category: 'vehicle',
  },
  FundUnit: {
    key: 'FundUnit',
    label: 'Fund Unit',
    description: 'Multi-asset investment fund interests',
    supportsConversion: false,
    requiresUnderlyingAsset: false,
    category: 'vehicle',
  },
  // ... other types
};
```

---

### `lib/diamonds/tokens/index.ts`

```typescript
import { getBaseTokenFacets, getBaseTokenInitCalls } from './base-token-config';
import { getShareClassSpecificFacets, getShareClassInitCalls } from './share-class-config';
import { getMembershipUnitSpecificFacets, getMembershipUnitInitCalls } from './membership-unit-config';
import { getSPVUnitSpecificFacets, getSPVUnitInitCalls } from './spv-unit-config';
import { getFundUnitSpecificFacets, getFundUnitInitCalls } from './fund-unit-config';
import type { TokenAssetType } from './token-types';
import type { FacetCut, MultiInitCall } from '@/types';
import { encodeAbiParameters } from 'viem';
import CONTRACT_ADDRESSES from '@/constants/contracts';

export * from './token-types';

/**
 * Build complete facet cuts for a token type
 */
export function buildTokenFacetCuts(tokenType: TokenAssetType): FacetCut[] {
  const baseFacets = getBaseTokenFacets();
  
  let specificFacets: FacetCut[] = [];
  
  switch (tokenType) {
    case 'ShareClass':
      specificFacets = getShareClassSpecificFacets();
      break;
    case 'MembershipUnit':
      specificFacets = getMembershipUnitSpecificFacets();
      break;
    case 'SPVUnit':
      specificFacets = getSPVUnitSpecificFacets();
      break;
    case 'FundUnit':
      specificFacets = getFundUnitSpecificFacets();
      break;
    // ... other types
    default:
      throw new Error(`Unsupported token type: ${tokenType}`);
  }
  
  return [...baseFacets, ...specificFacets];
}

/**
 * Build multi-init data for token (type-aware)
 */
export function buildTokenMultiInitData(
  tokenType: TokenAssetType,
  params: {
    name: string;
    symbol: string;
    decimals: number;
    baseURI: string;
    admin: Address;
    // Type-specific params (use discriminated union in production)
    [key: string]: any;
  }
): `0x${string}` {
  // Base init calls
  const multiInitCalls: MultiInitCall[] = getBaseTokenInitCalls({
    admin: params.admin,
    name: params.name,
    symbol: params.symbol,
    decimals: params.decimals,
    baseURI: params.baseURI,
  });
  
  // Add type-specific init calls
  switch (tokenType) {
    case 'ShareClass':
      multiInitCalls.push(...getShareClassInitCalls());
      break;
    case 'MembershipUnit':
      multiInitCalls.push(...getMembershipUnitInitCalls());
      break;
    case 'SPVUnit':
      multiInitCalls.push(...getSPVUnitInitCalls(params as any));
      break;
    case 'FundUnit':
      multiInitCalls.push(...getFundUnitInitCalls(params as any));
      break;
    // ... other types
  }
  
  // Encode multi-init data
  const multiInitData = encodeAbiParameters(
    [
      {
        name: "calls",
        type: "tuple[]",
        components: [
          { name: "init", type: "address" },
          { name: "initData", type: "bytes" },
        ],
      },
    ],
    [multiInitCalls]
  );
  
  return multiInitData;
}

/**
 * Get token diamond deployment configuration
 */
export function getTokenDeploymentConfig(
  tokenType: TokenAssetType,
  params: {
    name: string;
    symbol: string;
    decimals: number;
    baseURI: string;
    admin: Address;
    [key: string]: any;
  }
) {
  return {
    baseFacets: buildTokenFacetCuts(tokenType),
    init: CONTRACT_ADDRESSES.MULTI_INIT_FLAG,
    initData: buildTokenMultiInitData(tokenType, params),
  };
}
```

---

## Implementation Steps

### Phase 1: Setup (30 min)

1. **Create directory structure**
   ```bash
   cd interface/src/lib
   mkdir -p diamonds/tokens
   mkdir -p diamonds/wallets
   mkdir -p diamonds/offerings
   mkdir -p diamonds/vehicles
   ```

2. **Create .gitkeep files**
   ```bash
   touch diamonds/vehicles/.gitkeep
   ```

---

### Phase 2: Token Configuration (1 hour)

3. **Extract base token config**
   - Create `diamonds/tokens/base-token-config.ts`
   - Extract from `wagmi/diamond-config.ts` lines 377-442
   - Refactor into `getBaseTokenFacets()` and `getBaseTokenInitCalls()`

4. **Extract ShareClass config**
   - Create `diamonds/tokens/share-class-config.ts`
   - Extract ShareClass-specific facets (TokenCorporateActionsFacet)
   - Extract ShareClass init calls

5. **Create new token type configs**
   - Create `diamonds/tokens/spv-unit-config.ts` (stub for now)
   - Create `diamonds/tokens/membership-unit-config.ts` (stub for now)
   - Create `diamonds/tokens/fund-unit-config.ts` (stub for now)

6. **Create token type system**
   - Create `diamonds/tokens/token-types.ts`
   - Define `TokenAssetType` union type
   - Define metadata for each type

7. **Create main token index**
   - Create `diamonds/tokens/index.ts`
   - Implement `buildTokenFacetCuts(tokenType)`
   - Implement `buildTokenMultiInitData(tokenType, params)`
   - Implement `getTokenDeploymentConfig(tokenType, params)`

---

### Phase 3: Wallet & Offering Migration (30 min)

8. **Migrate wallet configuration**
   - Create `diamonds/wallets/wallet-config.ts`
   - Extract from `wagmi/diamond-config.ts` lines 16-171
   - Create `diamonds/wallets/wallet-types.ts`
   - Move content from `factories/wallet-helpers.ts`

9. **Migrate offering configuration**
   - Create `diamonds/offerings/offering-config.ts`
   - Extract from `wagmi/diamond-config.ts` lines 179-586
   - Create `diamonds/offerings/offering-types.ts`
   - Add any offering-specific types

---

### Phase 4: Update Imports (30 min)

10. **Update token creation page**
    ```typescript
    // interface/src/app/tokens/create/page.tsx
    // OLD:
    import { buildTokenFacetCuts, buildTokenMultiInitData } from '@/lib/wagmi/diamond-config';
    
    // NEW:
    import { buildTokenFacetCuts, buildTokenMultiInitData } from '@/lib/diamonds/tokens';
    import type { TokenAssetType } from '@/lib/diamonds/tokens';
    
    // Add token type selection to form
    const [tokenType, setTokenType] = useState<TokenAssetType>('ShareClass');
    
    // Pass tokenType to builder
    const facetCuts = buildTokenFacetCuts(tokenType);
    const initData = buildTokenMultiInitData(tokenType, params);
    ```

11. **Update offering creation page**
    ```typescript
    // interface/src/app/offerings/create/page.tsx
    // OLD:
    import { buildOfferingFacetCuts, buildOfferingMultiInitData } from '@/lib/wagmi/diamond-config';
    
    // NEW:
    import { buildOfferingFacetCuts, buildOfferingMultiInitData } from '@/lib/diamonds/offerings';
    ```

12. **Create main diamonds index**
    ```typescript
    // interface/src/lib/diamonds/index.ts
    export * from './tokens';
    export * from './wallets';
    export * from './offerings';
    ```

---

### Phase 5: Cleanup (15 min)

13. **Delete unused files**
    ```bash
    rm interface/src/lib/factories/offering-helpers.ts
    rm interface/src/lib/factories/token-helpers.ts
    rm interface/src/lib/factories/wallet-helpers.ts
    rmdir interface/src/lib/factories  # If empty
    ```

14. **Delete old diamond-config.ts**
    ```bash
    rm interface/src/lib/wagmi/diamond-config.ts
    ```

15. **Test everything still works**
    - Test wallet creation
    - Test token creation (ShareClass)
    - Test offering creation
    - Verify no import errors

---

## Testing Checklist

After refactoring, verify:

- [ ] Wallet creation works (EOA)
- [ ] Wallet creation works (Passkey)
- [ ] Token creation works (ShareClass)
- [ ] Offering creation works
- [ ] All imports resolve correctly
- [ ] No runtime errors
- [ ] Build succeeds (`pnpm build`)
- [ ] TypeScript checks pass (`pnpm type-check`)

---

## Future Enhancements (After SPV Protocol)

Once SPV facets are deployed to protocol:

1. **Update `spv-unit-config.ts`**
   - Uncomment facet cuts
   - Implement init calls
   - Add SPV-specific params

2. **Update token creation UI**
   - Add SPVUnit to token type selector
   - Add SPV-specific form fields
   - Link to Vehicle diamond

3. **Test SPV token creation**
   - Create test SPV
   - Deploy SPVUnit token
   - Verify facets are installed correctly

---

## Benefits of This Refactor

✅ **Clearer Organization**: Diamond configs separated from wagmi  
✅ **Type Safety**: Token type drives facet selection  
✅ **Scalability**: Easy to add new token types  
✅ **No Duplication**: Deleted unused factory-helpers  
✅ **Generated Selectors**: Maintains current best practice  
✅ **SPV-Ready**: Structure ready for SPV implementation  
✅ **Future-Proof**: Ready for MembershipUnit, FundUnit, etc.

---

## Related Documents

- [Token Diamond Architecture](./TOKEN_DIAMOND_ARCHITECTURE.md) - Protocol layer architecture
- [Interface Token Types](./INTERFACE_TOKEN_TYPES.md) - Frontend type system
- [Current diamond-config.ts](/interface/src/lib/wagmi/diamond-config.ts) - File to be refactored

---

## Notes

- Keep backward compatibility during migration
- Test after each phase
- Update this document as implementation progresses
- Once complete, this becomes the reference for adding new token types



