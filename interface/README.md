# Interface Overview

Build applications on top of CapSign using our frontend SDK and APIs.

## What is the CapSign Interface?

The CapSign interface is a Next.js application that provides:

- **User-facing application** - Web interface at app.capsign.com
- **Reusable components** - React components for common operations
- **Integration libraries** - TypeScript SDK for blockchain interactions
- **API endpoints** - Backend services for application logic

## Tech Stack

### Frontend

- **Next.js 14** - React framework with App Router
- **TypeScript** - Type-safe JavaScript
- **Tailwind CSS** - Utility-first styling
- **Wagmi** - React hooks for Ethereum
- **Viem** - TypeScript Ethereum library

### Authentication

- **NextAuth.js** - Authentication framework
- **WebAuthn** - Passkey support
- **Prisma** - Database ORM
- **PostgreSQL** - User data storage

### Blockchain

- **ERC-4337** - Account abstraction
- **Viem Account Abstraction** - UserOperation handling
- **Bundler Client** - UserOperation submission
- **EAS** - Ethereum Attestation Service

## Key Features

### Smart Account Management

- Create and manage ERC-4337 smart accounts
- Passkey-based authentication
- EOA wallet integration for entities
- Context switching (personal/entity accounts)

### Identity & Attestations

- Identity verification workflows
- Attestation viewing and management
- Attestation sharing and verification
- Privacy-preserving credentials

### Document Management

- Document upload and signing
- Blockchain-based signatures
- Verification tools
- Document organization

### Transaction Management

- Send and receive tokens
- Transaction history
- Batch operations
- Gasless transactions

## Getting Started

### For Frontend Developers

If you're building a frontend application that integrates with CapSign:

1. **[Authentication](/interface/authentication.md)** - Implement account creation and login
2. **[Passkeys](/interface/passkeys.md)** - Integrate passkey authentication
3. **[Components](/interface/components.md)** - Use pre-built UI components
4. **[API Integration](/interface/api-integration.md)** - Connect to backend services

### For Backend Developers

If you're building backend services that interact with CapSign:

1. **[API Integration](/interface/api-integration.md)** - Backend API endpoints
2. **[Direct Signing](/interface/direct-signing.md)** - Integrate wallet connections
3. **Review Protocol Docs** - [Protocol Documentation](/protocol/README.md)

## Architecture Overview

```
┌─────────────────────────────────────────────────────┐
│                  Next.js Frontend                    │
│                                                      │
│  ┌────────────┐  ┌────────────┐  ┌──────────────┐ │
│  │ Components │  │   Hooks    │  │   Contexts   │ │
│  └─────┬──────┘  └──────┬─────┘  └──────┬───────┘ │
│        │                │                │          │
│        └────────────────┼────────────────┘          │
│                         │                           │
└─────────────────────────┼───────────────────────────┘
                          │
        ┌─────────────────┼─────────────────┐
        │                 │                 │
        ▼                 ▼                 ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│    NextAuth  │  │    Wagmi     │  │  API Routes  │
│   (Auth)     │  │  (Blockchain)│  │  (Backend)   │
└──────┬───────┘  └──────┬───────┘  └──────┬───────┘
       │                 │                 │
       ▼                 ▼                 ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│   Prisma/    │  │  Smart       │  │  Protocol    │
│   Postgres   │  │  Contracts   │  │  Contracts   │
└──────────────┘  └──────────────┘  └──────────────┘
```

## Key Concepts

### Context-Aware Architecture

CapSign supports multiple account contexts:

- **Individual Context** - Personal accounts with passkey authentication
- **Entity Context** - Business accounts with EOA wallet signing
- **Context Switching** - Seamlessly switch between contexts

Learn more: [Context Management](/interface/context-management.md)

### Smart Account Adapter

Bridge between CapSign's diamond wallets and ERC-4337:

- Automatic wallet deployment
- Multiple owner types (passkey, EOA, MPC)
- ERC-4337 compliant
- Integrated with bundler clients

Learn more: [Protocol - Smart Accounts](/protocol/smart-accounts.md)

### Passkey Storage

Database-first approach with localStorage caching:

- Passkey public keys stored in Postgres
- Private keys never leave device
- 7-day cache for performance
- Automatic sync across devices

Learn more: [Passkeys](/interface/passkeys.md)

## Integration Patterns

### Pattern 1: Read-Only Integration

Display CapSign data without authentication:

```typescript
import { createPublicClient, http } from "viem";
import { base } from "viem/chains";

// Create public client
const client = createPublicClient({
  chain: base,
  transport: http(RPC_URL),
});

// Read attestations for an address
const attestations = await getAttestationsForAddress(
  client,
  walletAddress
);
```

### Pattern 2: Authenticated Integration

Full integration with user authentication:

```typescript
import { useSession } from "next-auth/react";
import { useCapSignAccount } from "@/hooks/use-capsign-account";

function MyApp() {
  const { data: session } = useSession();
  const { account, isDeployed } = useCapSignAccount();
  
  // User authenticated, wallet available
  if (session && account) {
    // Perform authenticated actions
  }
}
```

### Pattern 3: Embedded Components

Embed CapSign components in your app:

```typescript
import { AttestationViewer } from "@/components/attestations";
import { DocumentSigner } from "@/components/documents";

function MyApp() {
  return (
    <>
      <AttestationViewer address={userAddress} />
      <DocumentSigner documentId={docId} />
    </>
  );
}
```

## Code Organization

### Directory Structure

```
interface/
├── src/
│   ├── app/              # Next.js pages (App Router)
│   ├── components/       # React components
│   ├── hooks/            # Custom React hooks
│   ├── contexts/         # React contexts
│   ├── lib/              # Utility libraries
│   │   ├── wagmi/        # Blockchain utilities
│   │   └── utils/        # Helper functions
│   ├── types/            # TypeScript types
│   ├── constants/        # Constants and config
│   └── abis/             # Contract ABIs
├── prisma/               # Database schema
└── public/               # Static assets
```

### Key Files

- `src/lib/wagmi/config.ts` - Wagmi configuration
- `src/lib/wagmi/smart-account.ts` - Smart account adapter
- `src/lib/wagmi/passkey-auth.ts` - Passkey authentication
- `src/lib/wagmi/create-context-smart-account.ts` - Context routing
- `src/app/api/auth/[...nextauth]/route.ts` - NextAuth configuration

## Development Setup

### Prerequisites

- Node.js 18+
- pnpm (preferred) or npm
- PostgreSQL database
- Environment variables configured

### Installation

```bash
# Navigate to interface directory
cd interface

# Install dependencies
pnpm install

# Set up environment variables
cp .env.example .env.local
# Edit .env.local with your values

# Set up database
pnpm prisma generate
pnpm prisma migrate dev

# Run development server
pnpm dev
```

Visit http://localhost:3000

### Environment Variables

Required variables:

```bash
# Database
DATABASE_URL="postgresql://..."

# NextAuth
NEXTAUTH_URL="http://localhost:3000"
NEXTAUTH_SECRET="..."

# Blockchain
NEXT_PUBLIC_BUNDLER_RPC_URL="..."
NEXT_PUBLIC_BASE_SEPOLIA_RPC_URL="..."
NEXT_PUBLIC_WALLET_CONNECT_PROJECT_ID="..."

# Application
NEXT_PUBLIC_APP_URL="http://localhost:3000"
```

## Best Practices

### 1. Use Context-Aware Functions

Always use the context factory for smart account operations:

```typescript
// ✅ Good
import { createContextSmartAccount } from "@/lib/wagmi/create-context-smart-account";
const { account } = await createContextSmartAccount({ client, session });

// ❌ Bad
import { toCapSignSmartAccount } from "@/lib/wagmi/smart-account";
const account = await toCapSignSmartAccount(...); // Ignores context!
```

### 2. Handle Loading States

Always handle async operations properly:

```typescript
function MyComponent() {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<Error | null>(null);
  
  const handleAction = async () => {
    setLoading(true);
    setError(null);
    try {
      await performAction();
    } catch (err) {
      setError(err as Error);
    } finally {
      setLoading(false);
    }
  };
  
  if (loading) return <Spinner />;
  if (error) return <ErrorMessage error={error} />;
  return <ActionButton onClick={handleAction} />;
}
```

### 3. Use Type Safety

Leverage TypeScript for safer code:

```typescript
import type { Address } from "viem";
import type { Session } from "next-auth";

function processWalletAddress(address: Address) {
  // address is typed as `0x${string}`
}

function requiresAuth(session: Session | null) {
  if (!session) throw new Error("Not authenticated");
  // session is now Session (not null)
}
```

### 4. Optimize Performance

Use React best practices:

```typescript
import { memo, useCallback, useMemo } from "react";

const ExpensiveComponent = memo(({ data }: Props) => {
  const processedData = useMemo(() => processData(data), [data]);
  const handleClick = useCallback(() => {...}, []);
  return <div>{processedData}</div>;
});
```

## Debugging

### Enable Debug Logging

Set environment variable:

```bash
NEXT_PUBLIC_DEBUG=true
```

Check console for detailed logs:

```
🔍 [Context Account Factory] Detecting context: { contextType: "entity", ... }
🏢 [Context Account Factory] Creating ENTITY account (EOA)
```

### Common Issues

See [Troubleshooting](/advanced/troubleshooting.md) for detailed solutions.

## Next Steps

### Frontend Integration

- **[Authentication](/interface/authentication.md)** - Implement login and signup
- **[Passkeys](/interface/passkeys.md)** - Add passkey support
- **[Components](/interface/components.md)** - Use UI components
- **[Context Management](/interface/context-management.md)** - Multi-account support

### Advanced Topics

- **[Direct Signing](/interface/direct-signing.md)** - EOA wallet integration
- **[Attestations](/interface/attestations.md)** - Attestation UI/UX
- **[API Integration](/interface/api-integration.md)** - Backend services

## Resources

- **[GitHub](https://github.com/capsign/interface)** - Source code
- **[API Reference](/api-reference/README.md)** - Contract documentation
- **[Protocol Docs](/protocol/README.md)** - Smart contract details
- **[Discord](https://discord.gg/gSmnZ9wmNv)** - Community support

---

**Ready to build?** Start with [Authentication](/interface/authentication.md) →

