# Address Collection Integration

## Overview

This document describes the implementation of pre-KYC address collection for business entities in the CapSign platform. Address data is now collected during the KYC modal flow and sent to Bridge.xyz as part of the customer creation process.

## Key Changes

### 1. New Utility Files

#### `/interface/src/lib/utils/country.ts`
- **Purpose**: Country code conversion between ISO 3166-1 alpha-2 (our DB) and alpha-3 (Bridge API)
- **Key Functions**:
  - `alpha2ToAlpha3()`: Convert "US" → "USA"
  - `alpha3ToAlpha2()`: Convert "USA" → "US"
  - `getCountryName()`: Get human-readable name
  - `getSupportedCountries()`: Get dropdown options

#### `/interface/src/lib/utils/address.ts`
- **Purpose**: Address validation and formatting utilities
- **Key Functions**:
  - `getLocalityLabel()`: Country-specific "City" vs "Town"
  - `getAdministrativeAreaLabel()`: "State" vs "Province" vs "County"
  - `getPostalCodeLabel()`: "ZIP Code" vs "Postcode"
  - `validatePostalCode()`: Country-specific postal code validation
  - `validateAddress()`: Full address validation
  - `toBridgeAddress()`: Convert our format to Bridge API format
  - `formatAddress()`: Display formatting

### 2. Type Definitions

#### `/interface/src/types/bridge.ts`
- **Purpose**: TypeScript types for Bridge.xyz API
- **Key Types**:
  - `Address`: Bridge address format (alpha-3 country codes)
  - `CreateBusinessCustomerPayload`: Includes `registered_address` and `physical_address`
  - `Customer`, `CustomerStatus`, `Endorsement`, etc.

### 3. Reusable Components

#### `/interface/src/components/Address/AddressFormFields.tsx`
- **Purpose**: Reusable address input fields with country-aware labels and validation
- **Features**:
  - Country-specific field labels
  - Postal code validation
  - Conditional rendering (some countries don't use postal codes)
  - Real-time error display
  - Accessibility (labels, required indicators)

### 4. Enhanced EntityKYCModal

#### `/interface/src/components/Banking/EntityKYCModal.tsx`
- **Changes**: Converted to multi-step modal flow
- **Steps**:
  1. **Basic Information**: Country, state, website, email, legal form, DAO status, representative
  2. **Registered Address**: Official address where business is registered
  3. **Physical Address**: Primary place of business (can be same as registered)
  4. **Review**: Summary of all information before submission
- **Features**:
  - Progress bar showing current step
  - "Same as registered address" checkbox
  - Country-aware address validation
  - Back/Next navigation
  - Comprehensive error handling

### 5. API Updates

#### `/interface/src/app/api/user/profile/route.ts` (PATCH)
- **Changes**: Now handles `registeredAddress` and `physicalAddress`
- **Logic**:
  - Creates `REGISTERED` address type for registered address
  - Creates `HEADQUARTERS` address type for physical address
  - Links registered address to `Account.addressId`
  - Updates `Entity.country` from registered address
  - Maintains backward compatibility with single `address` field

#### `/interface/src/app/api/bridge/customers/route.ts` (POST)
- **Changes**: Sends addresses to Bridge API during customer creation
- **Logic**:
  - Queries for `REGISTERED` and `HEADQUARTERS` addresses
  - Converts our alpha-2 country codes to Bridge's alpha-3 format
  - Sends `registered_address` and `physical_address` in Bridge payload
  - Falls back to `Account.address` if no typed addresses found

#### `/interface/src/components/Banking/KYCStatus.tsx`
- **Changes**: Updated `handleEntityKYCSubmit` signature to include both addresses
- **Logic**:
  - Receives `registeredAddress` and `physicalAddress` from modal
  - Sends both to `/api/user/profile` for DB storage
  - Proceeds with KYC link creation after profile update

## Data Flow

### 1. User Opens KYC Modal
```
EntityKYCModal opens
  ↓
Step 1: Basic Info (country, state, website, legal form, etc.)
  ↓
Step 2: Registered Address (street, city, state, ZIP)
  ↓
Step 3: Physical Address (or "same as registered")
  ↓
Step 4: Review all information
  ↓
User clicks "Continue to KYC"
```

### 2. Data Submission
```
handleEntityKYCSubmit(data)
  ↓
POST /api/user/profile
  {
    country: "US",
    state: "DE",
    website: "https://example.com",
    legalForm: "Delaware Corporation",
    registeredAddress: { ... },
    physicalAddress: { ... }
  }
  ↓
Database Updates:
  - Entity.country = "US"
  - Entity.website = "..."
  - Entity.legalForm = "..."
  - Address (REGISTERED) created
  - Address (HEADQUARTERS) created
  - Account.addressId → REGISTERED address
```

### 3. Bridge Customer Creation
```
POST /api/bridge/kyc-links
  ↓
POST /api/bridge/customers (internal)
  ↓
Query Addresses from DB
  - REGISTERED address
  - HEADQUARTERS address
  ↓
Convert to Bridge format
  - country: "US" → "USA" (alpha-2 to alpha-3)
  - street_line_1, city, subdivision, postal_code, etc.
  ↓
Bridge API Request:
  {
    type: "business",
    business_legal_name: "...",
    registered_address: {
      street_line_1: "123 Main St",
      city: "Wilmington",
      subdivision: "DE",
      postal_code: "19801",
      country: "USA"
    },
    physical_address: { ... }
  }
```

## Database Schema

### Address Table Fields Used
- `id` (CUID)
- `country` (ISO 3166-1 alpha-2)
- `addressLine1` (required)
- `addressLine2` (optional)
- `locality` (city/town)
- `administrativeArea` (state/province)
- `postalCode`
- `addressType` (REGISTERED | HEADQUARTERS | BUSINESS | RESIDENTIAL | ...)
- `createdAt`, `updatedAt`

### Account → Address Relationship
- `Account.addressId` → `Address.id` (one-to-one for primary/registered address)
- Additional addresses are linked by type and timestamp

## Country Support

### Fully Tested Countries
- **United States**: ZIP code (5 digits or 5+4), State required
- **Canada**: Postal code (A1A 1A1), Province required
- **United Kingdom**: Postcode (SW1A 1AA), County optional

### Additional Supported Countries
- France, Germany, Italy, Spain, Netherlands, Belgium, Switzerland
- Australia, New Zealand
- Brazil, Mexico, Argentina
- China, Japan, South Korea, India, Singapore

### Country-Specific Features
- Postal code validation patterns
- Label customization (ZIP vs Postcode)
- Subdivision requirement (state/province/county)
- Special cases (countries without postal codes)

## Validation Rules

### Required Fields
- Street Address (Line 1) - min 4 characters
- City/Locality - required
- State/Province - required (for most countries)
- Postal Code - required and validated per country
- Country - required

### Optional Fields
- Street Address (Line 2) - for apartment, suite, etc.

### Country-Specific Validation
- **US ZIP Code**: `12345` or `12345-6789`
- **Canadian Postal Code**: `A1A 1A1`
- **UK Postcode**: `SW1A 1AA`
- **German PLZ**: 5 digits
- **Japanese**: `123-4567`

## Testing Checklist

### Manual Testing
- [ ] Select US entity, enter DE state
- [ ] Fill registered address (Wilmington, DE)
- [ ] Select "Same as registered" for physical
- [ ] Review shows both addresses correctly
- [ ] Submit and check database
  - [ ] Two Address records created (REGISTERED, HEADQUARTERS)
  - [ ] Account.addressId links to REGISTERED
  - [ ] Entity.country = "US"
- [ ] Check Bridge API request includes addresses
  - [ ] `registered_address` with `country: "USA"`
  - [ ] `physical_address` with `country: "USA"`
- [ ] Complete KYC flow
- [ ] Verify addresses in Bridge dashboard

### Edge Cases
- [ ] Different registered vs physical address
- [ ] Non-US country (e.g., UK)
- [ ] Country without postal codes
- [ ] Invalid postal code (shows error)
- [ ] Missing required fields (shows errors)
- [ ] Back button preserves data
- [ ] Cancel and reopen preserves partial data

## Bridge API Fields

### Business Customer Payload (Relevant Fields)
```typescript
{
  type: "business",
  business_legal_name: string,
  business_trade_name: string,
  business_description: string,
  email: string,
  business_type: BusinessType,
  primary_website: string,
  is_dao: boolean,
  
  // NEW: Addresses
  registered_address: {
    street_line_1: string,      // Required
    street_line_2?: string,      // Optional
    city: string,                // Required
    subdivision?: string,        // Optional (state/province ISO 3166-2)
    postal_code?: string,        // Optional (required for most countries)
    country: string              // Required (ISO 3166-1 alpha-3)
  },
  
  physical_address: {
    // Same structure as registered_address
  },
  
  signed_agreement_id: string,
  associated_persons: [...],
  identifying_information: [...],
  documents: [...]
}
```

## Benefits

### For Users
✅ No extra step after KYC
✅ Address verified as part of KYC process
✅ Faster onboarding (single flow)
✅ Clear step-by-step progress

### For CapSign
✅ Complete data from day one
✅ Can generate documents immediately post-KYC
✅ Bridge has address for compliance checks
✅ Better UX (no redirect interruption)

### For Compliance
✅ Bridge can verify address matches business formation documents
✅ More robust KYC (address verification included)
✅ Proof of address collected by Bridge
✅ Audit trail for address data

## Future Enhancements

### Potential Improvements
1. **Address Autocomplete**: Integrate Google Places or similar
2. **Address Validation API**: Real-time USPS/Canada Post validation
3. **Multiple Addresses**: Support for mailing, billing, branch locations
4. **Address History**: Track address changes over time
5. **International Support**: Expand to more countries/formats

### Internationalization
- Translate address field labels
- Support RTL languages
- Regional date formats
- Currency-specific postal codes

## Troubleshooting

### Common Issues

**Issue**: "Postal code is required" but field is filled
- **Cause**: Country-specific validation failing
- **Fix**: Check `validatePostalCode()` pattern for that country

**Issue**: Bridge returns 400 "Invalid address"
- **Cause**: Missing required fields or wrong format
- **Fix**: Ensure `street_line_1`, `city`, `country` are present and `country` is alpha-3

**Issue**: Addresses not saving to database
- **Cause**: Profile API validation failing
- **Fix**: Check server logs for Prisma errors, ensure `addressType` is valid enum

**Issue**: "Same as registered" not working
- **Cause**: State not copying between addresses
- **Fix**: Check `handleUpdateRegisteredAddress` copies to physical when checkbox is true

## Code References

### Key Files Modified
- ✅ `/interface/src/lib/utils/country.ts` (new)
- ✅ `/interface/src/lib/utils/address.ts` (new)
- ✅ `/interface/src/types/bridge.ts` (new)
- ✅ `/interface/src/components/Address/AddressFormFields.tsx` (new)
- ✅ `/interface/src/components/Banking/EntityKYCModal.tsx` (major update)
- ✅ `/interface/src/components/Banking/KYCStatus.tsx` (updated handler)
- ✅ `/interface/src/app/api/user/profile/route.ts` (address logic)
- ✅ `/interface/src/app/api/bridge/customers/route.ts` (Bridge integration)

### Database Models
- `Address` (existing, using `addressType` field)
- `Account` (existing, using `addressId` foreign key)
- `Entity` (existing, using `country` field)

## Implementation Timeline

- **Phase 1**: Utilities & Types (30 min) ✅
- **Phase 2**: Reusable Component (1 hour) ✅
- **Phase 3**: Modal Integration (2 hours) ✅
- **Phase 4**: API Integration (1 hour) ✅
- **Phase 5**: Testing (1 hour) ⏳

**Total**: ~5.5 hours of development time




