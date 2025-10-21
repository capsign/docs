# API Integration

Backend API endpoints for CapSign application logic.

## API Routes

### Authentication

#### POST /api/auth/webauthn/register/options
Generate WebAuthn registration options.

**Request**:
```json
{
  "email": "user@example.com"
}
```

**Response**:
```json
{
  "challenge": "...",
  "rp": { "name": "CapSign", "id": "capsign.com" },
  "user": { "id": "...", "name": "user@example.com", "displayName": "user@example.com" },
  "pubKeyCredParams": [...]
}
```

#### POST /api/auth/webauthn/register/verify
Verify and store WebAuthn credential.

**Request**:
```json
{
  "email": "user@example.com",
  "credential": { "id": "...", "response": {...} }
}
```

**Response**:
```json
{
  "verified": true,
  "walletAddress": "0x...",
  "userId": "..."
}
```

### Wallets

#### GET /api/wallets/owner?address=0x...
Get wallet owner information.

**Response**:
```json
{
  "ownerAddress": "0x...",
  "ownerType": "EOA" | "PASSKEY" | "MPC"
}
```

#### GET /api/wallets/balance?address=0x...
Get wallet balance.

**Response**:
```json
{
  "address": "0x...",
  "balance": "1000000000000000000",
  "tokens": [
    { "address": "0x...", "symbol": "USDC", "balance": "1000000000" }
  ]
}
```

### Attestations

#### GET /api/attestations?address=0x...
Get attestations for address.

**Response**:
```json
{
  "attestations": [
    {
      "uid": "0x...",
      "schema": "0x...",
      "attester": "0x...",
      "recipient": "0x...",
      "time": 1234567890,
      "revoked": false,
      "data": "0x..."
    }
  ]
}
```

### Entities

#### GET /api/entities
Get entities user has access to.

**Response**:
```json
{
  "entities": [
    {
      "id": "entity_123",
      "name": "Acme Corp",
      "walletAddress": "0x...",
      "role": "CFO"
    }
  ]
}
```

## Implementation

### API Route Example

```typescript
// app/api/wallets/owner/route.ts
import { NextRequest, NextResponse } from "next/server";
import { getServerSession } from "next-auth";

export async function GET(request: NextRequest) {
  // Check authentication
  const session = await getServerSession();
  if (!session) {
    return NextResponse.json({ error: "Unauthorized" }, { status: 401 });
  }

  // Get wallet address from query
  const { searchParams } = new URL(request.url);
  const address = searchParams.get("address");

  if (!address) {
    return NextResponse.json({ error: "Address required" }, { status: 400 });
  }

  // Fetch owner from database or contract
  const owner = await getWalletOwner(address);

  return NextResponse.json(owner);
}
```

### Error Handling

```typescript
try {
  const result = await someOperation();
  return NextResponse.json(result);
} catch (error) {
  console.error("API error:", error);
  return NextResponse.json(
    { error: error.message },
    { status: 500 }
  );
}
```

### Rate Limiting

```typescript
import { Ratelimit } from "@upstash/ratelimit";
import { Redis } from "@upstash/redis";

const ratelimit = new Ratelimit({
  redis: Redis.fromEnv(),
  limiter: Ratelimit.slidingWindow(10, "10 s"),
});

export async function POST(request: NextRequest) {
  const ip = request.ip ?? "127.0.0.1";
  const { success } = await ratelimit.limit(ip);

  if (!success) {
    return NextResponse.json({ error: "Too many requests" }, { status: 429 });
  }

  // Handle request...
}
```

## Client Usage

### Fetch Wrapper

```typescript
// lib/api/client.ts
export async function apiClient<T>(
  endpoint: string,
  options?: RequestInit
): Promise<T> {
  const response = await fetch(`/api${endpoint}`, {
    ...options,
    headers: {
      "Content-Type": "application/json",
      ...options?.headers,
    },
  });

  if (!response.ok) {
    const error = await response.json();
    throw new Error(error.message || "API request failed");
  }

  return response.json();
}
```

### React Query Integration

```typescript
import { useQuery } from "@tanstack/react-query";
import { apiClient } from "@/lib/api/client";

export function useWalletOwner(address: Address) {
  return useQuery({
    queryKey: ["walletOwner", address],
    queryFn: () => apiClient<{ ownerAddress: Address; ownerType: string }>(
      `/wallets/owner?address=${address}`
    ),
  });
}
```

## Security

1. **Always authenticate** - Check session on protected routes
2. **Validate input** - Sanitize and validate all inputs
3. **Rate limit** - Prevent abuse
4. **CORS** - Configure appropriate CORS headers
5. **Error messages** - Don't leak sensitive information

---

**Documentation complete!** Return to [Interface Overview](/interface/README.md)

