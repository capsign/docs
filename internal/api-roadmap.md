# API Roadmap

This document tracks planned API endpoints that should be added to CapSign.

## Priority Levels

- **P0**: Critical - Blocking core functionality
- **P1**: High - Important for production readiness
- **P2**: Medium - Nice to have, improves developer experience
- **P3**: Low - Future consideration

---

## Planned APIs

### User-Entity Relationships API

**Priority**: P1  
**Status**: Not Started  
**Use Case**: Manage investor relationships, employee relationships, board member access, etc.

**Current State**: Only entity-to-entity relationships have an API (`/api/entity-relations`). User-to-entity relationships are managed directly via Prisma.

**Proposed Endpoints**:

```
GET    /api/user-relations                    # List relationships for current user
GET    /api/user-relations/:id                # Get specific relationship
POST   /api/user-relations                    # Create relationship
PATCH  /api/user-relations/:id                # Update relationship
DELETE /api/user-relations/:id                # Remove relationship

# Entity-side management (for admins)
GET    /api/entities/:id/members              # List all users with access to entity
POST   /api/entities/:id/members              # Add user to entity
PATCH  /api/entities/:id/members/:userId      # Update user's role
DELETE /api/entities/:id/members/:userId      # Remove user from entity
```

**Request Body Example**:
```json
{
  "entityId": "clxxx...",
  "relationshipType": "INVESTOR",
  "contextType": "OFFERING",
  "contextId": "0x1234...",
  "canSeeFullName": true,
  "canSeeEmail": true
}
```

---

### Profile Management API

**Priority**: P1  
**Status**: Partial (exists for logged-in user profile updates)  
**Use Case**: Programmatic profile management, admin updates, bulk operations

**Current State**: `/api/user/profile` exists but only for the logged-in user. No API for admin management of other profiles.

**Proposed Endpoints**:

```
# Individual Profiles
GET    /api/profiles/individuals              # List (admin only)
GET    /api/profiles/individuals/:id          # Get profile
PATCH  /api/profiles/individuals/:id          # Update profile (admin)

# Entity Profiles  
GET    /api/profiles/entities                 # List (admin only)
GET    /api/profiles/entities/:id             # Get profile
PATCH  /api/profiles/entities/:id             # Update profile (admin)

# Unified search
GET    /api/profiles/search                   # Search across all profiles
```

**Features Needed**:
- Admin role verification
- Audit logging for profile changes
- Bulk update support

---

### Investment Requests API

**Priority**: P2  
**Status**: Not Started  
**Use Case**: Track investment interest, manage pipeline, CRM integration

**Current State**: `InvestmentRequest` model exists in Prisma but no API endpoints.

**Proposed Endpoints**:

```
GET    /api/investment-requests               # List requests (issuer)
GET    /api/investment-requests/:id           # Get specific request
POST   /api/investment-requests               # Create request (investor)
PATCH  /api/investment-requests/:id           # Update status
DELETE /api/investment-requests/:id           # Cancel/reject

# Issuer dashboard
GET    /api/offerings/:id/investment-requests # Requests for specific offering
```

**Request Body Example**:
```json
{
  "offeringId": "0x1234...",
  "amount": "50000000000",  // 50,000 USDC
  "message": "Interested in investing",
  "accreditedStatus": "VERIFIED"
}
```

---

### Data Room Management API

**Priority**: P2  
**Status**: Partial (upload exists, but no JSON-based creation)  
**Use Case**: Programmatic document management, seed scripts, migrations

**Current State**: 
- `POST /api/offerings/:id/data-room/upload` - Requires multipart form data (file upload)
- No endpoint for creating documents from URLs or metadata only

**Proposed Endpoints**:

```
# Existing
POST   /api/offerings/:id/data-room/upload    # Upload file

# New - JSON-based management
POST   /api/offerings/:id/data-room           # Create from URL/metadata
PATCH  /api/offerings/:id/data-room/:docId    # Update metadata
DELETE /api/offerings/:id/data-room/:docId    # Delete document

# Bulk operations
POST   /api/offerings/:id/data-room/bulk      # Bulk create
DELETE /api/offerings/:id/data-room/bulk      # Bulk delete
```

**Request Body Example** (URL-based creation):
```json
{
  "title": "Q4 2024 Financials",
  "description": "Quarterly financial statements",
  "category": "FINANCIALS",
  "accessLevel": "INVESTORS_ONLY",
  "fileUrl": "https://storage.example.com/file.pdf",
  "mimeType": "application/pdf",
  "fileSize": 1024000
}
```

---

### SAFE Offering API

**Priority**: P0  
**Status**: Not Started  
**Use Case**: Create and manage SAFE offerings with proper convertible structure

**Current State**: 
- Protocol has `ConvertibleCoreFacet`, `ConvertibleIssuanceFacet`, etc.
- No frontend or API support for creating SAFE offerings

**Proposed Endpoints**:

```
# SAFE Offering Creation
POST   /api/offerings/safe                    # Create SAFE offering

# SAFE Investment (creates individual TokenSAFE diamond)
POST   /api/offerings/:id/safe/invest         # Create SAFE for investor

# SAFE Conversion
POST   /api/offerings/:id/safe/trigger-conversion  # Trigger conversion event
POST   /api/offerings/:id/safe/convert-all         # Convert all SAFEs to equity
```

**Request Body Example** (SAFE Offering):
```json
{
  "name": "Seed Round",
  "valuationCap": "8000000000000",  // $8M
  "discountRate": 20,               // 20%
  "targetRaise": "1500000000000",   // $1.5M
  "proRataRight": true,
  "hasMFN": true
}
```

---

## Implementation Notes

### Authentication

All new APIs should:
1. Use `getServerSession(authOptions)` for authentication
2. Support entity context switching via `session.user.context`
3. Check permissions using `checkContextPermission()` utility

### Validation

Consider using Zod schemas for request validation:
```typescript
import { z } from 'zod';

const CreateRelationshipSchema = z.object({
  entityId: z.string().cuid(),
  relationshipType: z.enum(['INVESTOR', 'EMPLOYEE', 'ADVISOR', ...]),
  contextType: z.enum(['OFFERING', 'TOKEN', 'ENTITY']).optional(),
  contextId: z.string().optional(),
});
```

### Testing

APIs should be testable via seed script. Update `scripts/seed/api-client.ts` when adding new APIs.

---

## Changelog

| Date | Change |
|------|--------|
| 2026-01-15 | Initial roadmap created |
