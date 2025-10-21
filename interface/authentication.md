# Authentication

Implement account creation and login functionality for your CapSign integration.

## Overview

CapSign uses NextAuth.js with custom WebAuthn provider for passkey-based authentication. This provides:

- Password-free authentication
- Biometric security (Face ID, Touch ID)
- Cross-device passkey sync
- Session management
- Protected API routes

## Architecture

```
User Action (Sign Up / Sign In)
       │
       ▼
WebAuthn API (Browser)
       │
       ├─→ Create Passkey (Registration)
       │   └─→ Device Secure Enclave
       │
       ├─→ Sign Challenge (Authentication)
       │   └─→ Device Secure Enclave
       │
       ▼
NextAuth API Route (/api/auth)
       │
       ├─→ Verify Credential
       ├─→ Create/Update User in DB
       ├─→ Generate Session
       │
       ▼
Session Cookie → Authenticated State
```

## Implementation

### 1. NextAuth Configuration

Create NextAuth configuration with WebAuthn provider:

```typescript
// app/api/auth/[...nextauth]/route.ts
import NextAuth from "next-auth";
import CredentialsProvider from "next-auth/providers/credentials";
import { verify } from "@simplewebauthn/server";

export const authOptions = {
  providers: [
    CredentialsProvider({
      id: "webauthn",
      name: "WebAuthn",
      credentials: {
        credential: { type: "text" },
      },
      async authorize(credentials) {
        // Verify WebAuthn credential
        const verification = await verify({
          credential: JSON.parse(credentials.credential),
          // ... verification options
        });

        if (verification.verified) {
          // Return user object
          return {
            id: userId,
            email: userEmail,
            walletAddress: derivedAddress,
          };
        }
        return null;
      },
    }),
  ],
  callbacks: {
    async jwt({ token, user }) {
      if (user) {
        token.walletAddress = user.walletAddress;
      }
      return token;
    },
    async session({ session, token }) {
      session.user.walletAddress = token.walletAddress;
      return session;
    },
  },
};

const handler = NextAuth(authOptions);
export { handler as GET, handler as POST };
```

### 2. Sign Up Flow

Implement account creation:

```typescript
// lib/auth/signup.ts
import { create } from "@github/webauthn-json";
import { signIn } from "next-auth/react";

export async function signUp(email: string) {
  // 1. Request registration options from server
  const optionsResponse = await fetch("/api/auth/webauthn/register/options", {
    method: "POST",
    body: JSON.stringify({ email }),
  });
  const options = await optionsResponse.json();

  // 2. Create passkey via WebAuthn
  const credential = await create(options);

  // 3. Send credential to server for verification
  const verifyResponse = await fetch("/api/auth/webauthn/register/verify", {
    method: "POST",
    body: JSON.stringify({
      email,
      credential,
    }),
  });

  const result = await verifyResponse.json();

  if (result.verified) {
    // 4. Sign in with NextAuth
    await signIn("webauthn", {
      credential: JSON.stringify(credential),
      callbackUrl: "/dashboard",
    });
  }

  return result;
}
```

### 3. Sign In Flow

Implement authentication:

```typescript
// lib/auth/signin.ts
import { get } from "@github/webauthn-json";
import { signIn } from "next-auth/react";

export async function signInWithPasskey(email?: string) {
  // 1. Request authentication options
  const optionsResponse = await fetch("/api/auth/webauthn/authenticate/options", {
    method: "POST",
    body: JSON.stringify({ email }),
  });
  const options = await optionsResponse.json();

  // 2. Get credential via WebAuthn
  const credential = await get(options);

  // 3. Verify and sign in
  const result = await signIn("webauthn", {
    credential: JSON.stringify(credential),
    callbackUrl: "/dashboard",
  });

  return result;
}
```

### 4. Component Integration

Use in React components:

```typescript
"use client";

import { useState } from "react";
import { signUp, signInWithPasskey } from "@/lib/auth";

export function AuthForm() {
  const [email, setEmail] = useState("");
  const [mode, setMode] = useState<"signin" | "signup">("signin");

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();

    try {
      if (mode === "signup") {
        await signUp(email);
      } else {
        await signInWithPasskey(email);
      }
    } catch (error) {
      console.error("Authentication error:", error);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        placeholder="Email address"
      />
      <button type="submit">
        {mode === "signin" ? "Sign In" : "Sign Up"}
      </button>
      <button type="button" onClick={() => setMode(mode === "signin" ? "signup" : "signin")}>
        {mode === "signin" ? "Need an account?" : "Have an account?"}
      </button>
    </form>
  );
}
```

### 5. Protected Routes

Protect pages requiring authentication:

```typescript
// app/dashboard/page.tsx
import { getServerSession } from "next-auth";
import { redirect } from "next/navigation";
import { authOptions } from "@/app/api/auth/[...nextauth]/route";

export default async function DashboardPage() {
  const session = await getServerSession(authOptions);

  if (!session) {
    redirect("/login");
  }

  return (
    <div>
      <h1>Welcome, {session.user.email}</h1>
      <p>Wallet: {session.user.walletAddress}</p>
    </div>
  );
}
```

### 6. Client-Side Session

Access session in client components:

```typescript
"use client";

import { useSession } from "next-auth/react";

export function UserProfile() {
  const { data: session, status } = useSession();

  if (status === "loading") {
    return <div>Loading...</div>;
  }

  if (!session) {
    return <div>Not authenticated</div>;
  }

  return (
    <div>
      <p>Email: {session.user.email}</p>
      <p>Wallet: {session.user.walletAddress}</p>
    </div>
  );
}
```

## Database Schema

Store user credentials in Postgres:

```prisma
// prisma/schema.prisma
model User {
  id        String   @id @default(cuid())
  email     String   @unique
  credentials WebAuthnCredential[]
  walletAddress String
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

model WebAuthnCredential {
  id               String   @id @default(cuid())
  userId           String
  user             User     @relation(fields: [userId], references: [id])
  credentialId     String   @unique
  publicKey        Bytes
  counter          BigInt
  transports       String[]
  createdAt        DateTime @default(now())
}
```

## Session Management

### Session Configuration

```typescript
export const authOptions = {
  session: {
    strategy: "jwt",
    maxAge: 30 * 24 * 60 * 60, // 30 days
  },
  // ... other options
};
```

### Sign Out

```typescript
import { signOut } from "next-auth/react";

function SignOutButton() {
  return (
    <button onClick={() => signOut({ callbackUrl: "/" })}>
      Sign Out
    </button>
  );
}
```

## Error Handling

Handle common authentication errors:

```typescript
try {
  await signInWithPasskey(email);
} catch (error) {
  if (error.name === "NotAllowedError") {
    // User cancelled biometric prompt
    console.log("Authentication cancelled");
  } else if (error.name === "NotSupportedError") {
    // Browser doesn't support WebAuthn
    console.error("WebAuthn not supported");
  } else {
    // Other errors
    console.error("Authentication failed:", error);
  }
}
```

## Best Practices

1. **Always use HTTPS** - WebAuthn requires secure context
2. **Handle browser compatibility** - Check for WebAuthn support
3. **Provide fallback** - Offer alternative authentication methods
4. **Clear error messages** - Help users understand issues
5. **Session security** - Use httpOnly cookies, secure flags
6. **Rate limiting** - Prevent brute force attacks

## Related Documentation

- [Passkeys](/interface/passkeys.md) - Passkey implementation details
- [Context Management](/interface/context-management.md) - Multi-account support
- [API Integration](/interface/api-integration.md) - Backend API endpoints

---

**Next:** [Passkeys](/interface/passkeys.md) →

