# Interface Architecture: Multi-Token Type Support

## Overview

This document outlines the architecture for supporting multiple token types (ShareClass, MembershipUnit, FundUnit, SPVUnit, ESO, etc.) in the CapSign interface layer.

**Last Updated**: 2025-10-24  
**Status**: Design Phase

**Recent Updates**:
- Renamed `MembershipInterest` → `MembershipUnit` for clarity
- Added `SPVUnit` as high-priority token type (Phase 3)
- Added SPV-specific terminology and operations
- Updated to reflect VehicleFactory architecture (funds + SPVs)

---

## Core Principles

1. **Type-Agnostic Components**: Common components work for all token types
2. **Type-Specific Extensions**: Each token type has specialized components/operations
3. **Dynamic Routing**: Operations and UI determined by token type at runtime
4. **No Hardcoded Terminology**: Use terminology system for type-specific language
5. **Conversion Awareness**: Handle raw-to-real conversions only where needed

---

## Token Type Taxonomy

### Tokens Requiring Raw→Real Conversion
- **ShareClass**: Yes (corporate actions: splits, dividends)
- **EmployeeStockOption**: Yes (inherit from underlying ShareClass)
- **Warrant**: Yes (inherit from underlying ShareClass)
- **StockAppreciationRight**: Depends on settlement type

### Tokens Using Direct Quantities
- **MembershipUnit**: No conversion (operating LLCs don't have stock splits)
- **FundUnit**: No conversion (NAV-based pricing, investment vehicles)
- **SPVUnit**: No conversion (pass-through valuation) 🔥 **HIGH PRIORITY**
- **SAFE**: No conversion until conversion event
- **ConvertibleNote**: No conversion until conversion event
- **Bond**: No conversion
- **PromissoryNote**: No conversion

---

## File Structure

```
interface/src/
├── components/tokens/
│   ├── common/                          # Shared across all types
│   │   ├── TokenCard.tsx
│   │   ├── TokenLogo.tsx
│   │   ├── TokenHolders.tsx
│   │   ├── TokenQuantityDisplay.tsx    # Auto-handles conversions
│   │   ├── ComplianceStatus.tsx
│   │   └── TransactionHistory.tsx
│   │
│   ├── shareclass/                      # Equity tokens
│   │   ├── ShareTokenInfo.tsx
│   │   ├── CorporateActionsPanel.tsx
│   │   ├── StockSplitForm.tsx
│   │   ├── DividendsForm.tsx
│   │   └── VotingInterface.tsx
│   │
│   ├── membership/                      # LLC/Partnership (operating businesses)
│   │   ├── MembershipTokenInfo.tsx
│   │   ├── DistributionsPanel.tsx
│   │   ├── CapitalAccountsTable.tsx
│   │   ├── AllocationForm.tsx
│   │   └── K1TaxReporting.tsx
│   │
│   ├── fund/                            # Investment funds
│   │   ├── FundUnitInfo.tsx
│   │   ├── NAVDisplay.tsx
│   │   ├── RedemptionForm.tsx
│   │   ├── PerformanceChart.tsx
│   │   └── PortfolioBreakdown.tsx
│   │
│   ├── spv/                             # 🔥 SPVs (HIGH PRIORITY)
│   │   ├── SPVUnitInfo.tsx
│   │   ├── SPVInvestmentDisplay.tsx
│   │   ├── SPVValuationPanel.tsx
│   │   ├── SPVDistributionForm.tsx
│   │   └── SPVLiquidationInterface.tsx
│   │
│   ├── options/                         # Employee stock options
│   │   ├── ESOTokenInfo.tsx
│   │   ├── GrantsTable.tsx
│   │   ├── VestingSchedule.tsx
│   │   ├── ExerciseForm.tsx
│   │   └── TaxCalculator.tsx
│   │
│   ├── debt/                            # Debt instruments
│   │   ├── DebtTokenInfo.tsx
│   │   ├── InterestSchedule.tsx
│   │   ├── MaturityCountdown.tsx
│   │   └── ConversionInterface.tsx      # For convertibles
│   │
│   └── index.ts                         # Type routing factory
│
├── lib/
│   ├── tokens/
│   │   ├── token-config.ts              # Token type configurations
│   │   ├── operations-registry.ts       # Operation routing system
│   │   ├── terminology.ts               # Type-specific language
│   │   └── conversion-utils.ts          # Balance conversion logic
│   │
│   ├── corporate-actions.ts             # Enhanced with type awareness
│   └── aa/
│       └── token-operations.ts          # Type-aware AA operations
│
├── app/tokens/
│   ├── create/
│   │   └── page.tsx                     # Multi-step creation flow
│   │
│   ├── [id]/
│   │   ├── page.tsx                     # Dynamic token detail page
│   │   ├── (components)/
│   │   │   ├── TokenInfo.tsx            # Type-aware wrapper
│   │   │   ├── TokenOperations.tsx      # Dynamic operations grid
│   │   │   └── TokenHolders.tsx
│   │   │
│   │   ├── [operation]/
│   │   │   └── page.tsx                 # Dynamic operation routing
│   │   │
│   │   └── lots/
│   │       └── [lotId]/
│   │           ├── page.tsx
│   │           └── edit/page.tsx
│   │
│   └── page.tsx                         # Token list (all types)
│
├── queries/
│   ├── tokens.ts                        # Polymorphic GraphQL queries
│   └── index.ts
│
└── types/
    └── tokens.ts                        # TypeScript type definitions
```

---

## Core System Components

### 1. Token Type Configuration System

**File**: `interface/src/lib/tokens/token-config.ts`

```typescript
export interface TokenTypeConfig {
  key: string;
  label: string;
  supportsConversion: boolean;
  requiresUnderlyingAsset: boolean;
  legalEntities: string[];
  terminology: TerminologySet;
  facets: string[];
  defaultOperations: string[];
}

export const TOKEN_TYPE_CONFIGS: Record<string, TokenTypeConfig> = {
  ShareClass: {
    key: "ShareClass",
    label: "Share Class",
    supportsConversion: true,
    requiresUnderlyingAsset: false,
    legalEntities: ["Corporation", "C-Corp", "S-Corp"],
    terminology: SHARE_TERMINOLOGY,
    facets: [
      "TokenCorporateActionsFacet",
      "TokenVotingFacet",
      "TokenCustodyFacet",
    ],
    defaultOperations: [
      "issuance",
      "corporate-actions",
      "voting",
      "agents",
      "max-supply",
      "conditions",
      "pause",
      "freeze",
    ],
  },
  
  MembershipUnit: {
    key: "MembershipUnit",
    label: "Membership Unit",
    supportsConversion: false,
    requiresUnderlyingAsset: false,
    legalEntities: ["LLC", "LP", "LLP"],
    terminology: MEMBERSHIP_TERMINOLOGY,
    facets: [
      "TokenDistributionsFacet",
      "TokenCapitalAccountFacet",
      "TokenAllocationsFacet",
    ],
    defaultOperations: [
      "issuance",
      "distributions",
      "capital-accounts",
      "allocations",
      "conditions",
      "pause",
      "freeze",
    ],
  },
  
  FundUnit: {
    key: "FundUnit",
    label: "Fund Unit",
    supportsConversion: false,
    requiresUnderlyingAsset: false,
    legalEntities: ["Fund", "LP"], // Investment vehicles (any legal structure)
    terminology: FUND_TERMINOLOGY,
    facets: [
      "TokenFundUnitFacet",
      "TokenRedemptionFacet",
      "TokenNAVFacet",
    ],
    defaultOperations: [
      "nav",
      "redemptions",
      "performance",
      "portfolio",
      "capital-calls", // For PE/VC funds
      "distributions",
      "conditions",
    ],
  },
  
  SPVUnit: {
    key: "SPVUnit",
    label: "SPV Unit",
    supportsConversion: false,
    requiresUnderlyingAsset: false,
    legalEntities: ["SPV", "LP", "LLC"], // SPVs (any legal structure)
    terminology: SPV_TERMINOLOGY,
    facets: [
      "TokenSPVUnitFacet",
      "TokenSPVInvestmentFacet",
      "TokenSPVWaterfallFacet",
    ],
    defaultOperations: [
      "investment",
      "valuation",
      "distributions",
      "liquidation",
    ],
  },
  
  EmployeeStockOption: {
    key: "EmployeeStockOption",
    label: "Employee Stock Option",
    supportsConversion: true, // Inherits from underlying
    requiresUnderlyingAsset: true,
    legalEntities: ["Corporation"],
    terminology: ESO_TERMINOLOGY,
    facets: [
      "TokenESOFacet",
      "TokenVestingFacet",
      "TokenExerciseFacet",
    ],
    defaultOperations: [
      "grants",
      "vesting",
      "exercise",
      "cancel",
      "conditions",
    ],
  },
  
  ConvertibleNote: {
    key: "ConvertibleNote",
    label: "Convertible Note",
    supportsConversion: false,
    requiresUnderlyingAsset: false,
    legalEntities: ["Any"],
    terminology: DEBT_TERMINOLOGY,
    facets: [
      "TokenDebtFacet",
      "TokenMaturityFacet",
      "TokenConversionFacet",
    ],
    defaultOperations: [
      "interest",
      "conversion",
      "maturity",
      "repayment",
    ],
  },
  
  SAFE: {
    key: "SAFE",
    label: "SAFE",
    supportsConversion: false,
    requiresUnderlyingAsset: false,
    legalEntities: ["Any"],
    terminology: SAFE_TERMINOLOGY,
    facets: [
      "TokenSAFEFacet",
      "TokenSAFEConversionFacet",
    ],
    defaultOperations: [
      "conversion",
      "terms",
    ],
  },
};

// Helper functions
export function getTokenConfig(assetType: string): TokenTypeConfig {
  return TOKEN_TYPE_CONFIGS[assetType];
}

export function getAvailableTokenTypes(legalForm?: string): string[] {
  if (!legalForm) return [];
  
  return Object.values(TOKEN_TYPE_CONFIGS)
    .filter(config => 
      config.legalEntities.some(entity => 
        legalForm.toUpperCase().includes(entity.toUpperCase())
      )
    )
    .map(config => config.key);
}
```

### 2. Terminology System

**File**: `interface/src/lib/tokens/terminology.ts`

```typescript
export interface TerminologySet {
  instrument: string;        // "share", "unit", "option"
  instrumentPlural: string;  // "shares", "units", "options"
  holder: string;            // "shareholder", "member", "optionee"
  holderPlural: string;      // "shareholders", "members", "optionees"
  issuance: string;          // "share issuance", "unit issuance", "grant"
  transfer: string;          // "transfer", "assignment"
  action: string;            // "corporate action", "distribution", "exercise"
  actionPlural: string;      // "corporate actions", "distributions", "exercises"
}

export const SHARE_TERMINOLOGY: TerminologySet = {
  instrument: "share",
  instrumentPlural: "shares",
  holder: "shareholder",
  holderPlural: "shareholders",
  issuance: "share issuance",
  transfer: "transfer",
  action: "corporate action",
  actionPlural: "corporate actions",
};

export const MEMBERSHIP_TERMINOLOGY: TerminologySet = {
  instrument: "unit",
  instrumentPlural: "units",
  holder: "member",
  holderPlural: "members",
  issuance: "unit issuance",
  transfer: "transfer",
  action: "distribution",
  actionPlural: "distributions",
};

export const FUND_TERMINOLOGY: TerminologySet = {
  instrument: "unit",
  instrumentPlural: "units",
  holder: "investor",
  holderPlural: "investors",
  issuance: "subscription",
  transfer: "transfer",
  action: "transaction",
  actionPlural: "transactions",
};

export const SPV_TERMINOLOGY: TerminologySet = {
  instrument: "unit",
  instrumentPlural: "units",
  holder: "investor",
  holderPlural: "investors",
  issuance: "subscription",
  transfer: "transfer",
  action: "distribution",
  actionPlural: "distributions",
};

export const ESO_TERMINOLOGY: TerminologySet = {
  instrument: "option",
  instrumentPlural: "options",
  holder: "optionee",
  holderPlural: "optionees",
  issuance: "grant",
  transfer: "assignment",
  action: "exercise",
  actionPlural: "exercises",
};

export const DEBT_TERMINOLOGY: TerminologySet = {
  instrument: "note",
  instrumentPlural: "notes",
  holder: "noteholder",
  holderPlural: "noteholders",
  issuance: "issuance",
  transfer: "assignment",
  action: "payment",
  actionPlural: "payments",
};

export const SAFE_TERMINOLOGY: TerminologySet = {
  instrument: "SAFE",
  instrumentPlural: "SAFEs",
  holder: "investor",
  holderPlural: "investors",
  issuance: "issuance",
  transfer: "assignment",
  action: "conversion",
  actionPlural: "conversions",
};

// Helper function
export function getTerminology(assetType: string): TerminologySet {
  const config = getTokenConfig(assetType);
  return config?.terminology || SHARE_TERMINOLOGY;
}

export function formatWithTerminology(
  template: string,
  assetType: string,
  replacements?: Record<string, string>
): string {
  const terms = getTerminology(assetType);
  let result = template;
  
  // Replace terminology placeholders
  Object.entries(terms).forEach(([key, value]) => {
    result = result.replace(new RegExp(`{${key}}`, 'g'), value);
  });
  
  // Replace custom placeholders
  if (replacements) {
    Object.entries(replacements).forEach(([key, value]) => {
      result = result.replace(new RegExp(`{${key}}`, 'g'), value);
    });
  }
  
  return result;
}
```

### 3. Operations Registry System

**File**: `interface/src/lib/tokens/operations-registry.ts`

```typescript
import { ReactNode } from "react";

export interface TokenOperation {
  key: string;
  title: string;
  description: string;
  icon: ReactNode;
  component: React.ComponentType<any>;
  requiredFacets?: string[];
  requiresAdmin?: boolean;
}

export const OPERATIONS_REGISTRY: Record<string, TokenOperation[]> = {
  ShareClass: [
    {
      key: "issuance",
      title: "Share Issuance",
      description: "Issue new shares to shareholders",
      icon: <FontAwesomeIcon icon={faFileCertificate} />,
      component: ShareIssuanceForm,
      requiredFacets: ["TokenLotsFacet"],
      requiresAdmin: true,
    },
    {
      key: "corporate-actions",
      title: "Corporate Actions",
      description: "Manage splits, dividends, and other actions",
      icon: <FontAwesomeIcon icon={faGavel} />,
      component: CorporateActionsPanel,
      requiredFacets: ["TokenCorporateActionsFacet"],
      requiresAdmin: true,
    },
    {
      key: "voting",
      title: "Voting",
      description: "View voting power and history",
      icon: <FontAwesomeIcon icon={faVoteYea} />,
      component: VotingInterface,
      requiredFacets: ["TokenVotingFacet"],
      requiresAdmin: false,
    },
    // ... more operations
  ],
  
  MembershipUnit: [
    {
      key: "issuance",
      title: "Unit Issuance",
      description: "Issue new units to members",
      icon: <FontAwesomeIcon icon={faFileCertificate} />,
      component: UnitIssuanceForm,
      requiredFacets: ["TokenLotsFacet"],
      requiresAdmin: true,
    },
    {
      key: "distributions",
      title: "Distributions",
      description: "Distribute profits to members",
      icon: <FontAwesomeIcon icon={faMoneyBillWave} />,
      component: DistributionsForm,
      requiredFacets: ["TokenDistributionsFacet"],
      requiresAdmin: true,
    },
    {
      key: "capital-accounts",
      title: "Capital Accounts",
      description: "Manage member capital accounts",
      icon: <FontAwesomeIcon icon={faBookOpen} />,
      component: CapitalAccountsPanel,
      requiredFacets: ["TokenCapitalAccountFacet"],
      requiresAdmin: false,
    },
    // ... more operations
  ],
  
  FundUnit: [
    {
      key: "nav",
      title: "NAV",
      description: "View and manage Net Asset Value",
      icon: <FontAwesomeIcon icon={faChartLine} />,
      component: NAVPanel,
      requiredFacets: ["TokenNAVFacet"],
      requiresAdmin: false,
    },
    {
      key: "redemptions",
      title: "Redemptions",
      description: "Process redemption requests",
      icon: <FontAwesomeIcon icon={faArrowRightFromBracket} />,
      component: RedemptionsPanel,
      requiredFacets: ["TokenRedemptionFacet"],
      requiresAdmin: true,
    },
    // ... more operations
  ],
  
  SPVUnit: [
    {
      key: "investment",
      title: "Investment",
      description: "View and manage the SPV's investment",
      icon: <FontAwesomeIcon icon={faHandHoldingDollar} />,
      component: SPVInvestmentPanel,
      requiredFacets: ["TokenSPVInvestmentFacet"],
      requiresAdmin: false,
    },
    {
      key: "valuation",
      title: "Valuation",
      description: "Update SPV valuation",
      icon: <FontAwesomeIcon icon={faScaleBalanced} />,
      component: SPVValuationForm,
      requiredFacets: ["TokenSPVValuationFacet"],
      requiresAdmin: true,
    },
    {
      key: "distributions",
      title: "Distributions",
      description: "Distribute proceeds to investors",
      icon: <FontAwesomeIcon icon={faMoneyBillTransfer} />,
      component: SPVDistributionForm,
      requiredFacets: ["TokenSPVWaterfallFacet"],
      requiresAdmin: true,
    },
    {
      key: "liquidation",
      title: "Liquidation",
      description: "Liquidate SPV and distribute final proceeds",
      icon: <FontAwesomeIcon icon={faCircleXmark} />,
      component: SPVLiquidationInterface,
      requiredFacets: ["TokenSPVWaterfallFacet"],
      requiresAdmin: true,
    },
  ],
  
  // ... other token types
};

export function getOperationsForToken(token: Token): TokenOperation[] {
  const assetType = token.assetType;
  const operations = OPERATIONS_REGISTRY[assetType] || [];
  
  // Filter based on available facets (if we can detect them)
  // This could query the diamond loupe to check which facets are installed
  return operations;
}
```

### 4. Conversion Utilities (Enhanced)

**File**: `interface/src/lib/tokens/conversion-utils.ts`

```typescript
import { Token } from "@/types/tokens";
import { TOKEN_TYPE_CONFIGS } from "./token-config";

export interface ConversionContext {
  splitNum: string;
  splitDen: string;
  divNum: string;
  divDen: string;
}

export function tokenSupportsConversion(token: Token): boolean {
  const config = TOKEN_TYPE_CONFIGS[token.assetType];
  return config?.supportsConversion || false;
}

export function getConversionContext(token: Token): ConversionContext | null {
  if (!tokenSupportsConversion(token)) {
    return null;
  }
  
  // For ShareClass, use its own ratios
  if (token.assetType === "ShareClass") {
    return {
      splitNum: token.splitNum || "1",
      splitDen: token.splitDen || "1",
      divNum: token.divNum || "1",
      divDen: token.divDen || "1",
    };
  }
  
  // For ESO/Warrant, use underlying ShareClass ratios
  if (
    (token.assetType === "EmployeeStockOption" || 
     token.assetType === "Warrant") && 
    token.underlyingShareClass
  ) {
    return {
      splitNum: token.underlyingShareClass.splitNum || "1",
      splitDen: token.underlyingShareClass.splitDen || "1",
      divNum: token.underlyingShareClass.divNum || "1",
      divDen: token.underlyingShareClass.divDen || "1",
    };
  }
  
  return null;
}

export function calculateRealQuantity(
  rawQuantity: string,
  token: Token
): string {
  const context = getConversionContext(token);
  
  if (!context) {
    return rawQuantity; // No conversion needed
  }
  
  const raw = BigInt(rawQuantity);
  const splitNum = BigInt(context.splitNum);
  const splitDen = BigInt(context.splitDen);
  const divNum = BigInt(context.divNum);
  const divDen = BigInt(context.divDen);
  
  return ((raw * splitNum * divNum) / (splitDen * divDen)).toString();
}

export function displayQuantity(
  quantity: string,
  token: Token,
  decimals?: number
): string {
  const realQuantity = calculateRealQuantity(quantity, token);
  return formatTokenQuantity(realQuantity, decimals ?? token.decimals);
}

function formatTokenQuantity(quantity: string, decimals: number): string {
  const rawValue = Number(quantity);
  const divisor = Math.pow(10, decimals);
  const formattedValue = rawValue / divisor;
  
  return formattedValue.toLocaleString(undefined, {
    minimumFractionDigits: 0,
    maximumFractionDigits: decimals,
  });
}
```

---

## Key Components

### Dynamic Token Info Component

**File**: `interface/src/app/tokens/[id]/(components)/TokenInfo.tsx`

```typescript
export default function TokenInfo({ token }: { token: Token }) {
  const TokenInfoComponent = TOKEN_INFO_COMPONENTS[token.assetType] || DefaultTokenInfo;
  
  return <TokenInfoComponent token={token} />;
}

const TOKEN_INFO_COMPONENTS: Record<string, React.ComponentType<any>> = {
  ShareClass: ShareTokenInfo,
  MembershipUnit: MembershipTokenInfo,
  FundUnit: FundUnitInfo,
  SPVUnit: SPVUnitInfo,
  EmployeeStockOption: ESOTokenInfo,
  ConvertibleNote: DebtTokenInfo,
  SAFE: SAFETokenInfo,
};
```

### Dynamic Operations Grid

**File**: `interface/src/app/tokens/[id]/(components)/TokenOperations.tsx`

```typescript
export function TokenOperations({ token }: { token: Token }) {
  const operations = getOperationsForToken(token);
  const terminology = getTerminology(token.assetType);
  
  return (
    <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
      {operations.map((op) => (
        <Link
          key={op.key}
          href={`/tokens/${token.id}/${op.key}`}
          className="operation-card"
        >
          <div className="operation-icon">{op.icon}</div>
          <h3>{op.title}</h3>
          <p>{op.description}</p>
        </Link>
      ))}
    </div>
  );
}
```

### Token Quantity Display (Conversion-Aware)

**File**: `interface/src/components/tokens/common/TokenQuantityDisplay.tsx`

```typescript
export function TokenQuantityDisplay({ 
  quantity, 
  token, 
  label 
}: TokenQuantityDisplayProps) {
  const displayValue = displayQuantity(quantity, token);
  const needsConversion = tokenSupportsConversion(token);
  const terminology = getTerminology(token.assetType);
  
  return (
    <div>
      <div className="text-2xl font-bold">{displayValue}</div>
      <div className="text-sm text-gray-500">
        {label || terminology.instrumentPlural}
      </div>
      
      {needsConversion && (
        <Tooltip content="Adjusted for corporate actions">
          <InfoIcon className="ml-1" />
        </Tooltip>
      )}
    </div>
  );
}
```

---

## GraphQL Query Updates

### Polymorphic Token Query

**File**: `interface/src/queries/tokens.ts`

```typescript
export const GET_TOKEN_BY_ID = gql`
  query GetTokenById($id: ID!) {
    token(id: $id) {
      __typename
      
      ... on ShareClass {
        id
        name
        symbol
        totalSupply
        decimals
        admin
        splitNum
        splitDen
        divNum
        divDen
        maxSupply
        isPublic
      }
      
      ... on MembershipUnit {
        id
        name
        symbol
        totalSupply
        decimals
        admin
        totalCapitalContributions
        totalDistributions
      }
      
      ... on FundUnit {
        id
        name
        symbol
        totalSupply
        decimals
        admin
        vehicle
        currentNAV
        managementFeeBps
      }
      
      ... on SPVUnit {
        id
        name
        symbol
        totalSupply
        decimals
        admin
        vehicle
        underlyingAsset
        investmentAmount
        currentValuation
        carriedInterestBps
      }
      
      ... on EmployeeStockOption {
        id
        name
        symbol
        totalSupply
        decimals
        admin
        underlyingShareClass {
          id
          splitNum
          splitDen
          divNum
          divDen
        }
        poolSize
        optionsGranted
      }
      
      # ... other types
    }
  }
`;
```

---

## Implementation Checklist

### Phase 1: Foundation
- [ ] Create `token-config.ts` with type configurations
- [ ] Create `terminology.ts` with type-specific language
- [ ] Create `operations-registry.ts` for operation routing
- [ ] Enhance `conversion-utils.ts` with type awareness
- [ ] Refactor existing ShareClass code to use new patterns

### Phase 2: MembershipUnit
- [ ] Create `components/tokens/membership/` directory
- [ ] Build membership-specific components
- [ ] Add distribution operations
- [ ] Add capital account tracking
- [ ] Update GraphQL queries for MembershipUnit

### Phase 3: SPVUnit 🔥 **HIGH PRIORITY**
- [ ] Create `components/tokens/spv/` directory
- [ ] Build SPV-specific components:
  - [ ] `SPVUnitInfo.tsx` - SPV information display
  - [ ] `SPVInvestmentDisplay.tsx` - Show single investment
  - [ ] `SPVValuationPanel.tsx` - Valuation management
  - [ ] `SPVDistributionForm.tsx` - Distribution interface
  - [ ] `SPVLiquidationInterface.tsx` - Liquidation workflow
- [ ] Add SPV creation flow
- [ ] Add SPV operations:
  - [ ] Investment tracking
  - [ ] Valuation updates
  - [ ] Distribution management
  - [ ] Liquidation process
- [ ] Update GraphQL queries for SPVUnit
- [ ] Add SPV terminology to terminology system

### Phase 4: FundUnit
- [ ] Create `components/tokens/fund/` directory
- [ ] Build fund-specific components
- [ ] Add NAV display
- [ ] Add redemption interface
- [ ] Integrate with Vehicle diamond

### Phase 5: EmployeeStockOptions
- [ ] Create `components/tokens/options/` directory
- [ ] Build ESO-specific components
- [ ] Add grants table
- [ ] Add vesting visualization
- [ ] Add exercise interface

### Phase 6: Debt Instruments
- [ ] Create `components/tokens/debt/` directory
- [ ] Build debt-specific components
- [ ] Add interest schedule
- [ ] Add maturity tracking
- [ ] Add conversion interface (for convertibles)

---

## Testing Strategy

### Unit Tests
- Token type detection
- Conversion calculations
- Terminology replacement
- Operation filtering

### Integration Tests
- Token creation flow for each type
- Operation execution for each type
- Cross-type interactions (ESO → ShareClass)

### E2E Tests
- Complete token lifecycle per type
- User permissions per operation
- Type-specific workflows

---

## Migration Path

### Existing ShareClass Tokens
1. No database changes needed
2. Update UI to use new component system
3. Test conversion display
4. Verify all operations work

### New Token Types
1. Deploy new facets to protocol
2. Update subgraph schema
3. Deploy new UI components
4. Enable in token creation flow

---

## Future Considerations

- **Multi-chain support**: Token type configs per chain
- **Custom token types**: Allow users to define custom types
- **Token type conversion**: Migrate tokens between types (e.g., SAFE → ShareClass)
- **Advanced workflows**: Multi-step operations spanning token types
- **Analytics**: Type-specific dashboards and reporting

---

## References

- Main architecture discussion: (Current conversation)
- Protocol layer plan: See `TOKEN_DIAMOND_ARCHITECTURE.md`
- Subgraph updates: See subgraph schema comments

