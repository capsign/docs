# Context Management

Manage multiple accounts and entities from a single CapSign login.

## Overview

CapSign supports **multi-entity workflows** where a single user can access multiple accounts:

- **Individual account** - Personal investments
- **Corporate entities** - Company investment accounts  
- **Fund entities** - SPVs, investment funds
- **Multiple roles** - GP, LP, officer, employee

Switch between contexts instantly without logging out.

## Use Cases

### Fund Manager with Multiple SPVs
```
User: Alice (Fund Manager)
├─ Personal Account (individual)
├─ SPV Alpha LLC (GP role)
├─ SPV Beta LLC (GP role)  
├─ SPV Gamma LLC (GP role)
└─ XYZ Venture Fund (LP role)
```

### Corporate Officer with Multiple Entities
```
User: Bob (CFO)
├─ Personal Account (individual)
├─ Acme Corp (CFO role)
├─ Acme Subsidiary A (board member)
└─ Industry Association (treasurer)
```

### Service Provider
```
User: Carol (Attorney)
├─ Personal Account (individual)
├─ Law Firm PC (partner)
├─ Client Entity A (authorized signer)
└─ Client Entity B (authorized signer)
```

## Implementation

### Data Model

```typescript
interface User {
  id: string;
  email: string;
  walletAddress: string; // Individual wallet
  context?: AccountContext;
}

interface AccountContext {
  type: "individual" | "entity";
  accountId?: string; // Entity account ID
  walletAddress?: string; // Entity wallet address
  role?: string; // User's role in entity
  entityName?: string;
}
```

### Context Switching

```typescript
// contexts/AccountContext.tsx
"use client";

import { createContext, useContext, useState } from "react";

const AccountContext = createContext<{
  currentContext: AccountContext;
  availableContexts: AccountContext[];
  switchContext: (context: AccountContext) => void;
}>(null!);

export function AccountContextProvider({ children }: { children: React.ReactNode }) {
  const { data: session, update } = useSession();
  const [currentContext, setCurrentContext] = useState<AccountContext>(
    session?.user?.context || { type: "individual" }
  );

  const switchContext = async (newContext: AccountContext) => {
    // Update session with new context
    await update({
      ...session,
      user: {
        ...session.user,
        context: newContext,
      },
    });

    setCurrentContext(newContext);
  };

  return (
    <AccountContext.Provider value={{ currentContext, availableContexts: [], switchContext }}>
      {children}
    </AccountContext.Provider>
  );
}

export const useAccountContext = () => useContext(AccountContext);
```

### Context Selector Component

```typescript
// components/ContextSelector.tsx
"use client";

import { useAccountContext } from "@/contexts/AccountContext";

export function ContextSelector() {
  const { currentContext, availableContexts, switchContext } = useAccountContext();

  return (
    <select
      value={JSON.stringify(currentContext)}
      onChange={(e) => switchContext(JSON.parse(e.target.value))}
    >
      <option value={JSON.stringify({ type: "individual" })}>
        Personal Account
      </option>
      {availableContexts.map((ctx) => (
        <option key={ctx.accountId} value={JSON.stringify(ctx)}>
          {ctx.entityName} ({ctx.role})
        </option>
      ))}
    </select>
  );
}
```

### Context-Aware Operations

All operations use current context:

```typescript
import { createContextSmartAccount } from "@/lib/wagmi/create-context-smart-account";

async function sendTransaction() {
  const session = await getSession();
  
  // Automatically routes to correct adapter based on context
  const { account, walletAddress, contextType } = await createContextSmartAccount({
    client,
    session,
  });

  // For individual: uses passkey
  // For entity: uses EOA wallet

  const bundlerClient = createBundlerClient({
    account,
    client,
    transport: http(BUNDLER_RPC_URL),
  });

  return await bundlerClient.sendUserOperation({ calls: [...] });
}
```

## User Experience

### Context Indicator

Always show current context:

```typescript
function ContextBadge() {
  const { currentContext } = useAccountContext();

  return (
    <div className="context-badge">
      {currentContext.type === "individual" ? (
        <>👤 Personal Account</>
      ) : (
        <>🏢 {currentContext.entityName}</>
      )}
    </div>
  );
}
```

### Context-Specific UI

Adapt UI based on context:

```typescript
function Dashboard() {
  const { currentContext } = useAccountContext();

  if (currentContext.type === "individual") {
    return <IndividualDashboard />;
  } else {
    return <EntityDashboard role={currentContext.role} />;
  }
}
```

## Best Practices

1. **Always use context factory** - `createContextSmartAccount()` for all operations
2. **Show current context** - Make it obvious which account user is operating as
3. **Confirm before actions** - "Send as Acme Corp?"
4. **Persist context** - Remember last used context per session
5. **Validate permissions** - Check user has required role for action

## Related Documentation

- [Direct Signing](/interface/direct-signing.md) - EOA wallet integration
- [Authentication](/interface/authentication.md) - Auth architecture

---

**Next:** [Components](/interface/components.md) →

