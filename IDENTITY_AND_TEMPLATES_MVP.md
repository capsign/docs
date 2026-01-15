# Identity Resolution & Document Templates - MVP Design

## Executive Summary

This document outlines the **minimal viable product (MVP)** design for two critical features:
1. **Identity Resolution System**: Display human-readable names instead of wallet addresses
2. **Document Templates Engine**: Create, fill, and sign investment documents (e.g., SAFE, Series Seed)

Both systems are designed to be **simple, secure, and scalable** without overengineering.

### Key Design Decisions (Updated)

#### Identity System
✅ **Reuse Existing Tables**: Instead of creating `IdentityAlias`, we extend `BridgeCustomer` and `UserEntityRelationship`
- **Why**: Avoid data duplication, leverage existing Bridge KYC data
- **Benefit**: Simpler code, fewer tables, backward compatible

✅ **Include Email Lookup**: Add hashed email index for secure search
- **Why**: Critical for issuer workflows (finding investors by email)
- **Benefit**: Better UX without exposing actual emails in queries

✅ **Extend UserEntityRelationship**: Make it handle ALL connection types (not just entity-identity)
- **Why**: Reuse proven relationship system from old interface
- **Benefit**: Consistent with existing codebase, less learning curve

#### Templates System
✅ **HTML-Only for MVP**: Use ReactQuill WYSIWYG editor
- **Why**: Simpler than PDF/Word parsing, better control
- **Post-MVP**: Add PDF/Word upload if needed

✅ **Copy from Old Interface**: Proven 3-step workflow
- **Why**: Already works, saves development time
- **Benefit**: Fast implementation, known patterns

---

## Part 1: Identity Resolution System

### Problem Statement
- **Current**: Everywhere shows `0x742d35...` instead of "CapSign Inc" or "John Doe"
- **Impact**: Poor UX, difficult to search/find users, no relationship context
- **Risk**: Must not leak PII, must respect privacy

### MVP Design Principles
1. **Minimal Data**: Only store what's absolutely necessary
2. **Relationship-Based**: Show names only to connected parties
3. **Leverage Existing**: Use Bridge KYC data already in database
4. **No Over-Engineering**: Skip features like custom privacy settings, complex role systems, advanced search for now

---

### Database Schema Changes

**Key Decision**: Instead of creating a new `IdentityAlias` table, we'll **extend existing tables** to avoid data duplication:

1. **Use `BridgeCustomer` for name data** (already has `firstName`, `lastName`, `email`)
2. **Extend `UserEntityRelationship`** to handle both entity-identity AND wallet-to-wallet connections
3. **Add email lookup support** via hashed email index

```prisma
// ============ EXTEND EXISTING MODELS ============

// Update BridgeCustomer to add email hash for secure lookup
model BridgeCustomer {
  // ... existing fields ...
  
  // Add for secure email lookup (without exposing actual emails)
  emailHash         String?  @unique @map("email_hash")  // SHA256(lowercase(email))
  
  @@index([emailHash])  // Add index for fast lookup
}

// Extend UserEntityRelationship to support ALL types of connections
model UserEntityRelationship {
  id               String                   @id @default(cuid()) @map("id")
  identityId       String                   @map("identity_id")   // Account.id
  entityId         String                   @map("entity_id")      // Account.id (can be entity OR individual)
  relationshipType String                   @map("relationship_type")  // NEW: 'INVESTOR', 'ISSUER', 'COLLEAGUE', etc.
  status           EntityRelationshipStatus @default(PENDING_INVITE) @map("status")
  invitedBy        String?                  @map("invited_by")
  
  // NEW: Connection context (for offering-based relationships)
  contextType      String?                  @map("context_type")    // 'OFFERING', 'TOKEN', 'DIRECT'
  contextId        String?                  @map("context_id")      // Offering/token address
  
  // NEW: Privacy permissions (what they can see)
  canSeeFullName   Boolean                  @default(false) @map("can_see_full_name")
  canSeeEmail      Boolean                  @default(false) @map("can_see_email")
  
  createdAt        DateTime                 @default(now()) @map("created_at")
  updatedAt        DateTime                 @updatedAt @map("updated_at")
  
  identity         Account                  @relation("ManagedIdentity", fields: [identityId], references: [id], onDelete: Cascade)
  inviter          Account?                 @relation("SentInvitations", fields: [invitedBy], references: [id])
  entity           Account                  @relation("ManagedBy", fields: [entityId], references: [id], onDelete: Cascade)

  // Update unique constraint to include contextType/contextId
  @@unique([entityId, identityId, relationshipType, contextType, contextId])
  @@index([entityId])
  @@index([identityId])
  @@index([contextType, contextId])
  @@map("entity_relationships")
}

// Add new enum values for relationshipType
enum EntityRelationshipStatus {
  PENDING_INVITE
  ACTIVE
  INACTIVE
  BLOCKED  // NEW: For blocking unwanted connections
}
```

**Why this approach is better:**
- ✅ **No data duplication**: Reuse `BridgeCustomer.firstName/lastName/email`
- ✅ **Backward compatible**: Extends existing `UserEntityRelationship` model
- ✅ **Flexible**: `UserEntityRelationship` can now handle ANY type of connection
- ✅ **Simple**: Less code, fewer tables to maintain
- ✅ **Email lookup**: Secure hashed email search for better UX

---

### Comparison: Old vs New Relationship System

| Feature | interface-old | New MVP Design |
|---------|---------------|----------------|
| **Connection Model** | `UserEntityRelationship` | **Same!** `UserEntityRelationship` (extended) |
| **Scope** | Entity ↔ Identity only | **All connections** (entity ↔ entity, identity ↔ identity) |
| **Context Tracking** | ❌ No | ✅ Yes (`contextType`, `contextId`) |
| **Email Lookup** | ❌ No | ✅ Yes (via `emailHash`) |
| **Privacy Controls** | ❌ No | ✅ Yes (`canSeeFullName`, `canSeeEmail`) |
| **Auto-Creation** | Manual invitation | **Auto on investment** |
| **Bidirectional** | ✅ Yes | ✅ Yes |
| **Status Flow** | `PENDING_INVITE` → `ACTIVE` | **Same** (plus `BLOCKED` for blocking) |

**Key Improvements:**
1. ✅ **Context-Aware**: Track WHY connection exists (e.g., "via Offering 0x123")
2. ✅ **Granular Privacy**: Control what each party can see
3. ✅ **Email Lookup**: Find investors by email (hashed for security)
4. ✅ **Auto-Connection**: No manual invitation needed for investments

---

### Implementation Flow

#### 1. **Auto-Create Email Hash** (On KYC Completion)

```typescript
// /src/lib/identity/sync-bridge-customer.ts
import crypto from 'crypto';

export async function syncBridgeCustomerEmailHash(bridgeCustomerId: string) {
  const customer = await prisma.bridgeCustomer.findUnique({
    where: { id: bridgeCustomerId }
  });
  
  if (customer?.email) {
    const emailHash = crypto
      .createHash('sha256')
      .update(customer.email.toLowerCase())
      .digest('hex');
    
    await prisma.bridgeCustomer.update({
      where: { id: bridgeCustomerId },
      data: { emailHash }
    });
  }
}
```

#### 2. **Auto-Create Connections** (On Investment)

```typescript
// /src/lib/identity/create-connection.ts
export async function createInvestorConnection(
  investorAccountId: string,  // Account.id (not wallet address)
  offeringAddress: Address
) {
  const offering = await getOffering(offeringAddress);
  
  // Get issuer's account ID from their wallet
  const issuerAccount = await prisma.account.findFirst({
    where: {
      wallet: {
        address: offering.issuer.toLowerCase()
      }
    }
  });
  
  if (!issuerAccount) {
    throw new Error("Issuer account not found");
  }
  
  // Create bidirectional relationship using existing UserEntityRelationship model
  // Note: "entityId" and "identityId" are both Account IDs - naming is legacy but functional
  await prisma.userEntityRelationship.createMany({
    data: [
      {
        entityId: issuerAccount.id,      // Issuer (viewer)
        identityId: investorAccountId,   // Investor (who they can see)
        relationshipType: 'INVESTOR',
        status: 'ACTIVE',
        contextType: 'OFFERING',
        contextId: offeringAddress.toLowerCase(),
        canSeeFullName: true,  // Issuer can see investor details (compliance)
        canSeeEmail: true,     // Issuer can see investor email
      },
      {
        entityId: investorAccountId,     // Investor (viewer)
        identityId: issuerAccount.id,    // Issuer (who they can see)
        relationshipType: 'ISSUER',
        status: 'ACTIVE',
        contextType: 'OFFERING',
        contextId: offeringAddress.toLowerCase(),
        canSeeFullName: true,  // Investor can see issuer details
        canSeeEmail: false,    // Investor cannot see issuer email (optional)
      }
    ],
    skipDuplicates: true  // Prevent errors if relationship already exists
  });
}
```

#### 3. **Resolve Identity** (Display Component)

```tsx
// /src/components/Identity/IdentityDisplay.tsx
interface IdentityDisplayProps {
  address: Address;          // Wallet address to resolve
  viewerAddress?: Address;   // Who's viewing (optional)
  variant?: 'default' | 'issuer' | 'investor';
  showAddress?: boolean;
  showEmail?: boolean;
}

export function IdentityDisplay({ 
  address, 
  viewerAddress, 
  variant, 
  showAddress = false,
  showEmail = false 
}: IdentityDisplayProps) {
  const { data, loading } = useQuery(RESOLVE_IDENTITY, {
    variables: { address, viewerAddress }
  });
  
  if (loading) return <span className="animate-pulse">Loading...</span>;
  
  const identity = data?.resolveIdentity;
  
  return (
    <div className="flex items-center gap-2">
      {identity?.isVerified && (
        <FontAwesomeIcon 
          icon={faCheckCircle} 
          className="text-blue-500" 
          title="KYC Verified" 
        />
      )}
      <span className="font-medium">
        {identity?.displayName || `${address.slice(0, 6)}...${address.slice(-4)}`}
      </span>
      {showEmail && identity?.canSeeEmail && identity?.email && (
        <span className="text-xs text-gray-500">
          ({identity.email})
        </span>
      )}
      {showAddress && identity?.displayName && (
        <span className="text-xs text-gray-500 font-mono">
          ({address.slice(0, 6)}...{address.slice(-4)})
        </span>
      )}
    </div>
  );
}
```

#### 4. **API Endpoint** (Resolve Identity)

```typescript
// /src/app/api/identity/resolve/route.ts
export async function POST(request: Request) {
  const { address, viewerAddress } = await request.json();
  
  // 1. Get account by wallet address
  const account = await prisma.account.findFirst({
    where: {
      wallet: {
        address: address.toLowerCase()
      }
    },
    include: {
      wallet: true,
      individualProfile: true,
      entityProfile: true,
      bridgeCustomer: true,
    }
  });
  
  if (!account) {
    return NextResponse.json({
      address,
      displayName: `${address.slice(0, 6)}...${address.slice(-4)}`,
      displayType: 'UNKNOWN',
      isVerified: false,
      canSeeFullInfo: false,
      canSeeEmail: false,
    });
  }
  
  // 2. Determine display name
  let displayName: string;
  if (account.type === 'INDIVIDUAL' && account.individualProfile) {
    displayName = account.individualProfile.displayName || 
                  `${account.individualProfile.firstName} ${account.individualProfile.lastName?.[0]}.`;
  } else if (account.type === 'ENTITY' && account.entityProfile) {
    displayName = account.entityProfile.displayName || account.name;
  } else {
    displayName = account.name || `${address.slice(0, 6)}...${address.slice(-4)}`;
  }
  
  // 3. Check if viewer has permission to see this identity
  if (viewerAddress && viewerAddress.toLowerCase() !== address.toLowerCase()) {
    // Get viewer's account
    const viewerAccount = await prisma.account.findFirst({
      where: {
        wallet: {
          address: viewerAddress.toLowerCase()
        }
      }
    });
    
    if (!viewerAccount) {
      // No viewer account = show truncated address
      return NextResponse.json({
        address,
        displayName: `${address.slice(0, 6)}...${address.slice(-4)}`,
        displayType: account.type,
        isVerified: account.kycVerified,
        canSeeFullInfo: false,
        canSeeEmail: false,
      });
    }
    
    // Check for relationship (in either direction)
    const relationship = await prisma.userEntityRelationship.findFirst({
      where: {
        OR: [
          {
            entityId: viewerAccount.id,
            identityId: account.id,
            status: 'ACTIVE'
          },
          {
            entityId: account.id,
            identityId: viewerAccount.id,
            status: 'ACTIVE'
          }
        ]
      }
    });
    
    if (!relationship) {
      // No connection = show truncated address
      return NextResponse.json({
        address,
        displayName: `${address.slice(0, 6)}...${address.slice(-4)}`,
        displayType: account.type,
        isVerified: account.kycVerified,
        canSeeFullInfo: false,
        canSeeEmail: false,
      });
    }
    
    // Has connection = show name based on permissions
    return NextResponse.json({
      address,
      displayName: relationship.canSeeFullName ? displayName : `${address.slice(0, 6)}...${address.slice(-4)}`,
      displayType: account.type,
      isVerified: account.kycVerified,
      canSeeFullInfo: relationship.canSeeFullName,
      canSeeEmail: relationship.canSeeEmail,
      email: relationship.canSeeEmail ? account.bridgeCustomer?.email : undefined,
    });
  }
  
  // Viewer is the owner or no viewer specified = show everything
  return NextResponse.json({
    address,
    displayName,
    displayType: account.type,
    isVerified: account.kycVerified,
    canSeeFullInfo: true,
    canSeeEmail: true,
    email: account.bridgeCustomer?.email,
  });
}
```

#### 5. **Search/Autocomplete** (For Forms) - WITH EMAIL LOOKUP

```typescript
// /src/components/Identity/AddressInput.tsx
export function AddressInput({ value, onChange, connectionContext }: AddressInputProps) {
  const [query, setQuery] = useState('');
  const { data: session } = useSession();
  const viewerWallet = session?.user?.context?.wallet?.address;
  
  const { data: searchResults } = useQuery(SEARCH_IDENTITIES, {
    variables: { query, viewerWallet, connectionContext },
    skip: query.length < 2
  });
  
  return (
    <Combobox value={value} onChange={onChange}>
      <ComboboxInput
        placeholder="Search by name, email, or enter address..."
        onChange={(e) => setQuery(e.target.value)}
      />
      <ComboboxOptions>
        {searchResults?.searchIdentities.map((identity) => (
          <ComboboxOption key={identity.address} value={identity.address}>
            <IdentityDisplay address={identity.address} />
            <span className="text-xs text-gray-500">
              {identity.matchType === 'EMAIL' && '📧 Email match'}
              {identity.matchType === 'NAME' && '👤 Name match'}
              {identity.matchType === 'CONNECTION' && '🔗 Connected'}
            </span>
          </ComboboxOption>
        ))}
        {/* Allow direct address input */}
        {isAddress(query) && (
          <ComboboxOption value={query}>
            Use address: {query}
          </ComboboxOption>
        )}
      </ComboboxOptions>
    </Combobox>
  );
}
```

**Search API with Email Lookup:**

```typescript
// /src/app/api/identity/search/route.ts
export async function POST(request: Request) {
  const { query, viewerAddress } = await request.json();
  
  const viewerAccount = await prisma.account.findFirst({
    where: { wallet: { address: viewerAddress?.toLowerCase() } }
  });
  
  if (!viewerAccount) {
    return NextResponse.json({ results: [] });
  }
  
  // 1. Check if query is an email
  if (query.includes('@')) {
    const emailHash = crypto
      .createHash('sha256')
      .update(query.toLowerCase())
      .digest('hex');
    
    const matchByEmail = await prisma.bridgeCustomer.findUnique({
      where: { emailHash },
      include: {
        account: {
          include: {
            wallet: true,
            individualProfile: true,
            entityProfile: true,
          }
        }
      }
    });
    
    if (matchByEmail?.account?.wallet) {
      // Check if viewer has permission to see this person
      const relationship = await prisma.userEntityRelationship.findFirst({
        where: {
          OR: [
            { entityId: viewerAccount.id, identityId: matchByEmail.account.id, status: 'ACTIVE' },
            { entityId: matchByEmail.account.id, identityId: viewerAccount.id, status: 'ACTIVE' }
          ]
        }
      });
      
      if (relationship?.canSeeEmail) {
        return NextResponse.json({
          results: [{
            address: matchByEmail.account.wallet.address,
            displayName: getDisplayName(matchByEmail.account),
            matchType: 'EMAIL',
            isVerified: matchByEmail.account.kycVerified,
          }]
        });
      }
    }
    
    // Email lookup disabled or no permission
    return NextResponse.json({ results: [] });
  }
  
  // 2. Search by name (only among connections)
  const connections = await prisma.userEntityRelationship.findMany({
    where: {
      OR: [
        { entityId: viewerAccount.id, status: 'ACTIVE' },
        { identityId: viewerAccount.id, status: 'ACTIVE' }
      ]
    },
    include: {
      identity: {
        include: {
          wallet: true,
          individualProfile: true,
          entityProfile: true,
          bridgeCustomer: true,
        }
      },
      entity: {
        include: {
          wallet: true,
          individualProfile: true,
          entityProfile: true,
          bridgeCustomer: true,
        }
      }
    }
  });
  
  // Filter by name match
  const results = connections
    .flatMap(conn => {
      const connectedAccount = conn.entityId === viewerAccount.id ? conn.identity : conn.entity;
      const displayName = getDisplayName(connectedAccount);
      
      if (displayName.toLowerCase().includes(query.toLowerCase())) {
        return [{
          address: connectedAccount.wallet?.address,
          displayName,
          matchType: 'NAME',
          isVerified: connectedAccount.kycVerified,
        }];
      }
      return [];
    })
    .filter(r => r.address);
  
  return NextResponse.json({ results });
}
```

---

### Migration Strategy

**Phase 1**: Database schema + Email hash + Backfill (1-2 days)
- Add `emailHash`, `contextType`, `contextId`, `canSeeEmail` fields to schema
- Run migration script to:
  - Generate email hashes for all `BridgeCustomer` records
  - Backfill `UserEntityRelationship` for existing investments (query subgraph)
- Create utility functions for connection management

**Phase 2**: API endpoints (1 day)
- Create `resolveIdentity` API endpoint
- Create `searchIdentities` API endpoint with email lookup
- Add GraphQL queries

**Phase 3**: Display components (2-3 days)
- Create `<IdentityDisplay>` component
- Replace address displays in:
  - Offering list/detail pages
  - Investor lists
  - Token issuance pages
  - Compliance management

**Phase 4**: Search/Autocomplete (2-3 days)
- Create `<AddressInput>` component with email lookup
- Add to forms:
  - Compliance management
  - Offchain investment recording
  - Custom terms setting
  - Token lot creation

---

## Part 2: Document Templates Engine

### Problem Statement
- **Current**: No way to attach fillable documents to offerings
- **Need**: SAFE agreements, Series Seed docs, investor questionnaires
- **Must**: Support placeholders, signature blocks, PDF generation

### MVP Design Principles
1. **Leverage Old System**: Reuse working template engine from interface-old
2. **HTML-Only for MVP**: Use WYSIWYG editor (ReactQuill) for simplicity
3. **On-Chain Registry**: Hash templates on-chain for authenticity
4. **Generate PDFs**: Use `@react-pdf/renderer` or similar

**Future Enhancement** (Post-MVP): Allow PDF/Word upload with `{{placeholders}}`, but this requires:
- Complex parsing (PDFKit, docxtemplater)
- Layout preservation challenges
- More testing for edge cases
- For MVP, HTML templates are simpler and more reliable

---

### Database Schema (Reuse from old interface)

```prisma
// ============ ADD TO schema.prisma ============

model DocumentTemplate {
  id           String          @id @default(cuid())
  name         String          @map("name")
  description  String?         @map("description")
  htmlContent  String          @map("html_content")  // Template with {{placeholders}}
  hash         Bytes           @unique @map("hash")  // Keccak256 hash
  tags         String[]        @map("tags")
  signerRoles  String[]        @map("signer_roles")  // ["Investor", "Company Representative"]
  purpose      DocumentPurpose @map("purpose")       // Enum: SAFE_AGREEMENT, SUBSCRIPTION_AGREEMENT, etc.
  
  creatorId    String          @map("creator_id")
  creator      Account         @relation(fields: [creatorId], references: [id])
  
  isPublic     Boolean         @default(false) @map("is_public")  // Public templates can be reused
  isActive     Boolean         @default(true) @map("is_active")
  
  createdAt    DateTime        @default(now()) @map("created_at")
  updatedAt    DateTime        @updatedAt @map("updated_at")
  
  // Relationships
  documents    Document[]      // Filled instances of this template
  
  @@index([creatorId])
  @@index([purpose])
  @@index([isPublic, isActive])
  @@map("document_templates")
}

model Document {
  id                 String           @id @default(cuid())
  name               String           @map("name")
  description        String?          @map("description")
  
  // Content
  htmlContent        String           @map("html_content")      // Filled template
  pdfUrl             String?          @map("pdf_url")           // Generated PDF URL
  hash               Bytes            @unique @map("hash")      // Keccak256 hash
  
  // Template linkage
  templateId         String?          @map("template_id")
  template           DocumentTemplate? @relation(fields: [templateId], references: [id])
  placeholderData    Json?            @map("placeholder_data")  // Store filled values
  
  // Context
  purpose            DocumentPurpose  @map("purpose")
  relatedOfferingId  String?          @map("related_offering_id")  // Link to offering
  relatedTokenId     String?          @map("related_token_id")     // Link to token
  
  // Creator/Owner
  creatorId          String           @map("creator_id")
  creator            Account          @relation(fields: [creatorId], references: [id])
  
  // Status
  status             DocumentStatus   @default(DRAFT) @map("status")
  isTemplate         Boolean          @default(false) @map("is_template")
  
  // On-chain registration
  registryAddress    String?          @map("registry_address")
  registeredAt       DateTime?        @map("registered_at")
  registrationTx     String?          @map("registration_tx")
  
  createdAt          DateTime         @default(now()) @map("created_at")
  updatedAt          DateTime         @updatedAt @map("updated_at")
  
  // Relationships
  signatures         Signature[]
  
  @@index([creatorId])
  @@index([templateId])
  @@index([relatedOfferingId])
  @@index([purpose])
  @@index([status])
  @@map("documents")
}

model Signature {
  id                String         @id @default(cuid())
  documentId        String         @map("document_id")
  document          Document       @relation(fields: [documentId], references: [id], onDelete: Cascade)
  
  signerId          String         @map("signer_id")
  signer            Account        @relation(fields: [signerId], references: [id])
  
  signerRole        String         @map("signer_role")  // "Investor", "Company Representative"
  signatureData     Bytes?         @map("signature_data")  // WebAuthn signature
  signedAt          DateTime?      @map("signed_at")
  
  status            SignatureStatus @default(PENDING) @map("status")
  
  createdAt         DateTime       @default(now()) @map("created_at")
  updatedAt         DateTime       @updatedAt @map("updated_at")
  
  @@unique([documentId, signerId, signerRole])
  @@index([documentId])
  @@index([signerId])
  @@index([status])
  @@map("signatures")
}

// ============ ENUMS ============

enum DocumentPurpose {
  SAFE_AGREEMENT
  CONVERTIBLE_NOTE
  SUBSCRIPTION_AGREEMENT
  SOPHISTICATION_QUESTIONNAIRE
  RSPA_AGREEMENT
  RSA_AGREEMENT
  STOCK_OPTION_GRANT
  SAR_GRANT
  WARRANT_PURCHASE
  TERM_SHEET_EQUITY
  TERM_SHEET_CONVERTIBLE
  ADVISORY_AGREEMENT
  ELECTION_83B
  BOARD_CONSENT
  SHAREHOLDER_CONSENT
  INDEMNIFICATION_AGREEMENT
  CIIA_AGREEMENT
  OTHER
}

enum DocumentStatus {
  DRAFT
  PENDING_SIGNATURES
  PARTIALLY_SIGNED
  FULLY_SIGNED
  VOIDED
}

enum SignatureStatus {
  PENDING
  SIGNED
  DECLINED
}
```

---

### Implementation Flow

#### 1. **Template Creation** (Reuse from old interface)

Copy these files from `interface-old/src/app/documents/create/`:
- `page.tsx` - Main template creation UI
- `TemplateTypeSelect.tsx` - Category dropdown
- `TemplateInstructionsDisplay.tsx` - Help text
- `WorkflowStepper.tsx` - 3-step workflow

**File**: `/src/app/documents/templates/create/page.tsx`

Key features:
- **Step 1**: Basic info + rich text editor (ReactQuill)
- **Step 2**: Define signer roles + validate placeholders
- **Step 3**: Save to DB + register hash on-chain

#### 2. **Template Storage API**

**File**: `/src/app/api/documents/templates/route.ts`

```typescript
export async function POST(request: Request) {
  const session = await getServerSession(authOptions);
  if (!session?.user?.id) {
    return NextResponse.json({ error: "Unauthorized" }, { status: 401 });
  }
  
  const { title, description, templateType, htmlContent, signerRoles, tags } = await request.json();
  
  // Validate
  if (!title || !templateType || !htmlContent || signerRoles.length === 0) {
    return NextResponse.json({ error: "Missing required fields" }, { status: 400 });
  }
  
  // Calculate hash
  const htmlBuffer = Buffer.from(htmlContent, "utf-8");
  const templateHashHex = keccak256(htmlBuffer);
  const templateHashBuffer = Buffer.from(templateHashHex.slice(2), "hex");
  
  // Save template
  const template = await prisma.documentTemplate.create({
    data: {
      name: title,
      description,
      htmlContent,
      hash: templateHashBuffer,
      tags,
      signerRoles,
      purpose: templateType as DocumentPurpose,
      creatorId: session.user.id,
    }
  });
  
  return NextResponse.json({
    id: template.id,
    hash: toHex(template.hash),
    url: `capsign:template:${template.id}`,
    message: "Template created successfully"
  }, { status: 201 });
}
```

#### 3. **Fill Template** (Create Document from Template)

**File**: `/src/app/documents/[templateId]/fill/page.tsx`

```tsx
export default function FillTemplatePage() {
  const { templateId } = useParams();
  const [template, setTemplate] = useState(null);
  const [placeholderValues, setPlaceholderValues] = useState({});
  
  // Load template
  useEffect(() => {
    fetch(`/api/documents/templates/${templateId}`)
      .then(res => res.json())
      .then(setTemplate);
  }, [templateId]);
  
  // Extract placeholders from template HTML
  const placeholders = useMemo(() => {
    const regex = /{{\s*([^{}\s]+)\s*}}/g;
    const matches = template?.htmlContent?.matchAll(regex) || [];
    return [...new Set([...matches].map(m => m[1]))];
  }, [template]);
  
  const handleFillTemplate = async () => {
    // Replace placeholders in HTML
    let filledHtml = template.htmlContent;
    Object.entries(placeholderValues).forEach(([key, value]) => {
      filledHtml = filledHtml.replace(new RegExp(`{{\\s*${key}\\s*}}`, 'g'), value);
    });
    
    // Create document
    const response = await fetch('/api/documents', {
      method: 'POST',
      body: JSON.stringify({
        name: `${template.name} - Filled`,
        htmlContent: filledHtml,
        templateId: template.id,
        placeholderData: placeholderValues,
        purpose: template.purpose,
        relatedOfferingId: offeringId, // If applicable
      })
    });
    
    // Navigate to signing page
    const doc = await response.json();
    router.push(`/documents/${doc.id}/sign`);
  };
  
  return (
    <div>
      <h1>Fill Template: {template?.name}</h1>
      <form>
        {placeholders.map(placeholder => (
          <div key={placeholder}>
            <label>{placeholder.replace(/_/g, ' ').toUpperCase()}</label>
            <input
              value={placeholderValues[placeholder] || ''}
              onChange={(e) => setPlaceholderValues({
                ...placeholderValues,
                [placeholder]: e.target.value
              })}
            />
          </div>
        ))}
      </form>
      <button onClick={handleFillTemplate}>Preview & Sign</button>
    </div>
  );
}
```

#### 4. **Signature Collection**

**File**: `/src/app/documents/[documentId]/sign/page.tsx`

```tsx
export default function SignDocumentPage() {
  const { documentId } = useParams();
  const { data: session } = useSession();
  const { address } = useAccount();
  
  const [document, setDocument] = useState(null);
  const [mySignatureRole, setMySignatureRole] = useState(null);
  
  const handleSign = async () => {
    // 1. Generate PDF
    const pdfResponse = await fetch(`/api/documents/${documentId}/generate-pdf`, {
      method: 'POST'
    });
    const { pdfUrl, pdfHash } = await pdfResponse.json();
    
    // 2. Sign on-chain via AA
    const signature = await signDocumentAA({
      documentHash: pdfHash,
      signerRole: mySignatureRole,
      smartAccount,
      bundlerClient
    });
    
    // 3. Store signature
    await fetch(`/api/documents/${documentId}/sign`, {
      method: 'POST',
      body: JSON.stringify({
        signerRole: mySignatureRole,
        signatureData: signature,
      })
    });
    
    toast.success('Document signed successfully!');
  };
  
  return (
    <div>
      <h1>Sign Document: {document?.name}</h1>
      
      {/* PDF Preview */}
      <iframe src={document?.pdfUrl} />
      
      {/* Signature Status */}
      <div>
        <h2>Required Signatures</h2>
        {document?.template?.signerRoles.map(role => (
          <div key={role}>
            <span>{role}</span>
            {document.signatures.find(s => s.signerRole === role)
              ? <CheckIcon /> 
              : <PendingIcon />
            }
          </div>
        ))}
      </div>
      
      {/* Sign Button */}
      {mySignatureRole && !document.signatures.find(s => s.signerId === session.user.id) && (
        <button onClick={handleSign}>Sign as {mySignatureRole}</button>
      )}
    </div>
  );
}
```

#### 5. **PDF Generation**

**File**: `/src/app/api/documents/[documentId]/generate-pdf/route.ts`

```typescript
import { renderToStream } from '@react-pdf/renderer';
import { Document, Page, Text, View, StyleSheet } from '@react-pdf/renderer';

export async function POST(request: Request, { params }: { params: { documentId: string } }) {
  const document = await prisma.document.findUnique({
    where: { id: params.documentId },
    include: { template: true }
  });
  
  // Convert HTML to React-PDF components
  const PDFDocument = (
    <Document>
      <Page size="A4" style={styles.page}>
        <View style={styles.section}>
          {/* Parse HTML and render as React-PDF components */}
          {parseHTMLToPDF(document.htmlContent)}
        </View>
      </Page>
    </Document>
  );
  
  // Generate PDF
  const stream = await renderToStream(PDFDocument);
  
  // Upload to storage (e.g., S3, Vercel Blob)
  const pdfUrl = await uploadPDF(stream, `${document.id}.pdf`);
  
  // Calculate PDF hash
  const pdfBuffer = await streamToBuffer(stream);
  const pdfHash = keccak256(pdfBuffer);
  
  // Update document
  await prisma.document.update({
    where: { id: document.id },
    data: { pdfUrl, hash: Buffer.from(pdfHash.slice(2), 'hex') }
  });
  
  return NextResponse.json({ pdfUrl, pdfHash });
}
```

---

### Attach Templates to Offerings

**File**: `/src/app/offerings/create/page.tsx`

Add a step to attach documents:

```tsx
// Step 4: Attach Required Documents
<div>
  <h3>Attach Documents (Optional)</h3>
  <p>Select or create document templates that investors must sign</p>
  
  <TemplateSelect
    multiple
    onChange={setSelectedTemplates}
  />
  
  {selectedTemplates.map(template => (
    <div key={template.id}>
      <span>{template.name}</span>
      <button onClick={() => removeTemplate(template.id)}>Remove</button>
    </div>
  ))}
  
  <button onClick={() => router.push('/documents/templates/create')}>
    Create New Template
  </button>
</div>
```

Store in database:

```prisma
model Offering {
  // ... existing fields ...
  requiredDocuments String[] @map("required_document_template_ids") // Array of template IDs
  // ... rest of model ...
}
```

---

### Migration Strategy

**Phase 1**: Database schema + Template creation (2-3 days)
- Add `DocumentTemplate`, `Document`, `Signature` models
- Copy template creation UI from old interface
- Implement template storage API

**Phase 2**: Template library (1-2 days)
- Create `/documents/templates` list page
- Add search/filter
- Implement template preview

**Phase 3**: Fill & Sign workflow (3-4 days)
- Create template filling UI
- Implement PDF generation
- Add signature collection flow

**Phase 4**: Offering integration (1-2 days)
- Add document attachment to offering creation
- Display required documents on offering detail page
- Track signature completion

---

## Implementation Timeline

### Week 1: Identity System
- **Day 1-2**: Database schema + API endpoints
- **Day 3**: Auto-create on registration + backfill
- **Day 4-5**: `<IdentityDisplay>` component + replace address displays

### Week 2: Identity Search + Templates Start
- **Day 6-7**: `<AddressInput>` component + add to forms
- **Day 8-10**: Template database schema + creation UI

### Week 3: Templates Completion
- **Day 11-12**: Template library + preview
- **Day 13-15**: Fill & sign workflow + PDF generation

### Week 4: Integration + Polish
- **Day 16-17**: Offering integration
- **Day 18-19**: Testing + bug fixes
- **Day 20**: Polish + documentation

**Total**: ~4 weeks for MVP

---

## Security Considerations

### Identity System
1. ✅ No PII in URLs/logs (always use wallet addresses)
2. ✅ Connection-based access (can't see names without relationship)
3. ✅ Audit trail (log all identity lookups)
4. ✅ Rate limiting (prevent brute-force searches)

### Templates System
1. ✅ Template hash verification (on-chain registry)
2. ✅ Signature verification (WebAuthn + smart accounts)
3. ✅ PDF integrity (hash stored with document)
4. ✅ Access control (only parties involved can view/sign)

---

## Future Enhancements (Post-MVP)

### Identity
- Custom privacy settings (public/connections/private)
- Email/phone lookup (opt-in)
- Advanced search (fuzzy matching, filters)
- Profile pictures/avatars
- Organization hierarchies

### Templates
- Rich text placeholders (dropdowns, checkboxes, dates)
- Conditional sections (show/hide based on values)
- Template versioning
- E-signature standards compliance (e.g., ESIGN Act)
- Multi-language support
- Template marketplace

---

## Questions & Answers

### Identity System

**Q1: Should we backfill existing accounts immediately or gradually?**
✅ **A**: Yes, backfill immediately. Run a migration script to:
1. Add `emailHash` to all `BridgeCustomer` records
2. Create `UserEntityRelationship` connections for existing investments (query subgraph for all investments → create connections)

**Q2: Do we show "unverified" badge for non-KYC'd users?**
✅ **A**: Yes, show verified badge for KYC'd users. Absence of badge indicates unverified.

**Q3: Should email lookup be included in MVP?**
✅ **A**: Yes, include email lookup via hashed email index for security.

### Templates System

**Q3: Do we need template approval flow for public templates?**
🤔 **A**: Clarify what "public templates" means:
- If it means "templates others can reuse" → Approval flow might be needed
- For MVP, keep templates private to creator (no sharing)
- Post-MVP: Add template marketplace with approval workflow

**Q4: Should we support external PDFs (upload) or only HTML-generated?**
✅ **A**: Only HTML-generated for MVP. Reasons:
- Simpler implementation
- Better placeholder control
- Easier to maintain formatting
- Post-MVP: Add PDF/Word upload parsing if needed

**Q5: Do we need document expiration/revocation?**
🔍 **For MVP**: No expiration/revocation needed
🚀 **Post-MVP**: Add these features:
- Document expiration dates
- Signature withdrawal/revocation
- Document amendment workflow

---

## Conclusion

This MVP design provides a **solid foundation** for both systems while keeping complexity minimal:

### Identity Resolution System
- ✅ **Reuses existing tables** (`BridgeCustomer`, `UserEntityRelationship`)
- ✅ **Zero data duplication** (leverages Bridge KYC data)
- ✅ **Includes email lookup** (hashed for security)
- ✅ **Context-aware connections** (tracks why/where connections exist)
- ✅ **Backward compatible** with existing `UserEntityRelationship` usage

### Document Templates Engine
- ✅ **Proven 3-step workflow** (copied from old interface)
- ✅ **HTML-only for MVP** (simpler than PDF parsing)
- ✅ **On-chain verification** (template + document hashes)
- ✅ **WebAuthn signatures** (integrated with AA flow)

Both systems are designed to scale naturally as we add more features post-MVP.

---

## Next Steps

Ready to implement! Recommended order:

### Week 1: Identity Foundation
1. **Day 1**: Database migration (add fields, indexes)
2. **Day 2**: Backfill script (email hashes + investment connections)
3. **Day 3**: API endpoints (`resolveIdentity`, `searchIdentities`)
4. **Day 4-5**: `<IdentityDisplay>` component + integrate

### Week 2: Identity Search + Templates Foundation
6. **Day 6-7**: `<AddressInput>` component with email lookup
7. **Day 8-10**: Copy template creation UI from old interface

### Week 3-4: Templates Completion
11. **Day 11-13**: Template library + preview + PDF generation
12. **Day 14-16**: Fill & sign workflow
13. **Day 17-18**: Offering integration
14. **Day 19-20**: Testing + polish

**Estimated MVP Timeline**: ~4 weeks total

---

## Critical Success Metrics

**Identity System:**
- [ ] All wallet addresses replaced with names in UI
- [ ] Email lookup working for issuers
- [ ] Connection auto-created on every investment
- [ ] Privacy respected (no names shown without connection)

**Templates System:**
- [ ] Template creation working (HTML + placeholders)
- [ ] Template library browsable
- [ ] Documents fillable and signable
- [ ] PDFs generated and stored
- [ ] Signatures verified on-chain

When these metrics are met, we'll have a fully functional MVP ready for production use! 🚀

