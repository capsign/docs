# Components

Reusable React components for common CapSign operations.

## Core Components

### Wallet Connect Button

```typescript
// components/WalletConnectButton.tsx
"use client";

import { useSession, signIn, signOut } from "next-auth/react";

export function WalletConnectButton() {
  const { data: session, status } = useSession();

  if (status === "loading") {
    return <button disabled>Loading...</button>;
  }

  if (session) {
    return (
      <div className="flex items-center gap-2">
        <span>{session.user.walletAddress.slice(0, 6)}...{session.user.walletAddress.slice(-4)}</span>
        <button onClick={() => signOut()}>Disconnect</button>
      </div>
    );
  }

  return <button onClick={() => signIn()}>Connect Wallet</button>;
}
```

### Balance Display

```typescript
// components/BalanceDisplay.tsx
"use client";

import { useBalance } from "wagmi";

export function BalanceDisplay({ address }: { address: Address }) {
  const { data: balance, isLoading } = useBalance({ address });

  if (isLoading) return <span>Loading...</span>;

  return (
    <div className="balance">
      {balance?.formatted} {balance?.symbol}
    </div>
  );
}
```

### Transaction Button

```typescript
// components/TransactionButton.tsx
"use client";

import { useState } from "react";
import { useSession } from "next-auth/react";
import { createContextSmartAccount } from "@/lib/wagmi/create-context-smart-account";

interface TransactionButtonProps {
  calls: { to: Address; value?: bigint; data?: Hex }[];
  onSuccess?: (hash: Hex) => void;
  children: React.ReactNode;
}

export function TransactionButton({ calls, onSuccess, children }: TransactionButtonProps) {
  const { data: session } = useSession();
  const [loading, setLoading] = useState(false);

  const handleClick = async () => {
    if (!session) return;

    setLoading(true);
    try {
      const { account } = await createContextSmartAccount({
        client: publicClient,
        session,
      });

      const bundlerClient = createBundlerClient({
        account,
        client: publicClient,
        transport: http(BUNDLER_RPC_URL),
      });

      const hash = await bundlerClient.sendUserOperation({ calls });
      const receipt = await bundlerClient.waitForUserOperationReceipt({ hash });

      onSuccess?.(receipt.receipt.transactionHash);
    } catch (error) {
      console.error("Transaction failed:", error);
    } finally {
      setLoading(false);
    }
  };

  return (
    <button onClick={handleClick} disabled={loading || !session}>
      {loading ? "Processing..." : children}
    </button>
  );
}
```

### Attestation Badge

```typescript
// components/AttestationBadge.tsx

interface AttestationBadgeProps {
  type: "kyc" | "accredited" | "age";
  status: "active" | "expired" | "revoked";
}

export function AttestationBadge({ type, status }: AttestationBadgeProps) {
  const labels = {
    kyc: "✓ Verified",
    accredited: "✓ Accredited",
    age: "✓ Age Verified",
  };

  const colors = {
    active: "bg-green-500",
    expired: "bg-yellow-500",
    revoked: "bg-red-500",
  };

  return (
    <span className={`badge ${colors[status]}`}>
      {labels[type]}
    </span>
  );
}
```

## Hooks

### useCapSignAccount

```typescript
// hooks/use-capsign-account.ts
import { useSession } from "next-auth/react";
import { useQuery } from "@tanstack/react-query";

export function useCapSignAccount() {
  const { data: session } = useSession();

  return useQuery({
    queryKey: ["account", session?.user?.walletAddress],
    queryFn: async () => {
      if (!session) return null;

      const { account, walletAddress } = await createContextSmartAccount({
        client: publicClient,
        session,
      });

      const isDeployed = await isWalletDeployed(publicClient, walletAddress);

      return { account, walletAddress, isDeployed };
    },
    enabled: !!session,
  });
}
```

### useAttestations

```typescript
// hooks/use-attestations.ts
export function useAttestations(address?: Address) {
  return useQuery({
    queryKey: ["attestations", address],
    queryFn: () => getAttestationsForAddress(publicClient, address!),
    enabled: !!address,
  });
}
```

## Styling

CapSign uses Tailwind CSS. Key classes:

```css
/* Buttons */
.btn-primary - Primary action button
.btn-secondary - Secondary action button
.btn-danger - Destructive action

/* Cards */
.card - Basic card container
.card-header - Card title section
.card-body - Card content

/* Status */
.badge - Status badge
.badge-success - Success state
.badge-warning - Warning state
.badge-error - Error state
```

## Best Practices

1. **Error handling** - Always catch and display errors
2. **Loading states** - Show loading indicators
3. **Disabled states** - Disable buttons during operations
4. **Confirmation** - Confirm destructive actions
5. **Accessibility** - Use semantic HTML and ARIA labels

---

**Next:** [API Integration](/interface/api-integration.md) →

