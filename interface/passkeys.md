# Passkeys

Learn how CapSign implements passkey authentication for secure, password-free access.

## Overview

CapSign uses **passkeys** (WebAuthn credentials) instead of passwords for authentication:

- **Private key**: Stored in device secure enclave, never leaves device
- **Public key**: Registered with CapSign for authentication
- **Biometric unlock**: Face ID, Touch ID, or Windows Hello
- **Cross-device sync**: Via iCloud Keychain or Google Password Manager

## Architecture

### Storage Strategy

CapSign uses a **database-first** approach with localStorage caching:

```
Data Flow:
┌──────────────┐
│ 1. Create    │ → Browser's secure enclave
│    Passkey   │   (credential ID + private key)
└──────┬───────┘
       │
┌──────▼───────┐
│ 2. Store in  │ → Postgres webauthnCredentials table
│    Database  │   (credential ID + public key)
└──────┬───────┘
       │
┌──────▼───────┐
│ 3. Cache in  │ → localStorage (7-day expiration)
│  localStorage│
└──────────────┘
```

### Storage Locations

| Data | Browser Secure Enclave | Database | localStorage |
|------|------------------------|----------|--------------|
| **Private Key** | ✅ Yes | ❌ Never | ❌ Never |
| **Credential ID** | ✅ Yes | ✅ Source of Truth | ✅ Cache |
| **Public Key** | ✅ Yes | ✅ Source of Truth | ✅ Cache |
| **Wallet Address** | ❌ No | ✅ Source of Truth | ✅ Cache |

## Implementation

### Passkey Registration

Create a new passkey during sign up:

```typescript
// lib/wagmi/passkey-auth.ts
import { create } from "@github/webauthn-json";
import { encode } from "@/lib/utils/base64url";

export class PasskeyAuth {
  static async signup(email: string) {
    // 1. Request registration options from server
    const optionsRes = await fetch("/api/auth/webauthn/register/options", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ email }),
    });
    const options = await optionsRes.json();

    // 2. Create passkey via WebAuthn API
    const credential = await create(options);

    // 3. Send to server for verification and storage
    const verifyRes = await fetch("/api/auth/webauthn/register/verify", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ email, credential }),
    });

    const result = await verifyRes.json();

    if (result.verified) {
      // 4. Cache passkey information
      this.cachePasskey({
        credentialId: credential.id,
        publicKey: encode(credential.response.publicKey),
        smartWalletAddress: result.walletAddress,
      });
    }

    return result;
  }
}
```

### Passkey Authentication

Authenticate using existing passkey:

```typescript
export class PasskeyAuth {
  static async signin(email?: string) {
    // 1. Get authentication options
    const optionsRes = await fetch("/api/auth/webauthn/authenticate/options", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ email }),
    });
    const options = await optionsRes.json();

    // 2. Get credential (triggers biometric prompt)
    const credential = await get(options);

    // 3. Verify credential on server
    const verifyRes = await fetch("/api/auth/webauthn/authenticate/verify", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ credential }),
    });

    return await verifyRes.json();
  }
}
```

### Caching for Performance

Cache passkey data locally for quick access:

```typescript
interface CachedPasskey {
  credentialId: string;
  publicKey: string;
  smartWalletAddress: string;
  expiresAt: number;  // Timestamp
  cachedAt: number;   // Timestamp
}

export class PasskeyAuth {
  private static CACHE_KEY = "capsign_passkey";
  private static CACHE_DURATION = 7 * 24 * 60 * 60 * 1000; // 7 days

  static cachePasskey(passkey: Omit<CachedPasskey, "expiresAt" | "cachedAt">) {
    const now = Date.now();
    const cached: CachedPasskey = {
      ...passkey,
      cachedAt: now,
      expiresAt: now + this.CACHE_DURATION,
    };
    
    localStorage.setItem(this.CACHE_KEY, JSON.stringify(cached));
  }

  static getCachedPasskey(): CachedPasskey | null {
    const cached = localStorage.getItem(this.CACHE_KEY);
    if (!cached) return null;

    const passkey: CachedPasskey = JSON.parse(cached);
    
    // Check expiration
    if (Date.now() > passkey.expiresAt) {
      this.clearCache();
      return null;
    }

    return passkey;
  }

  static clearCache() {
    localStorage.removeItem(this.CACHE_KEY);
  }

  // Get passkey from cache or database
  static async getPasskey(): Promise<CachedPasskey | null> {
    // Try cache first
    const cached = this.getCachedPasskey();
    if (cached) return cached;

    // Fetch from database via NextAuth session
    const session = await getSession();
    if (!session?.user) return null;

    const passkey = {
      credentialId: session.user.credentialId,
      publicKey: session.user.publicKey,
      smartWalletAddress: session.user.walletAddress,
    };

    // Update cache
    this.cachePasskey(passkey);

    return passkey;
  }
}
```

## Database Schema

Store passkey credentials in Postgres:

```prisma
model User {
  id            String   @id @default(cuid())
  email         String   @unique
  walletAddress String
  credentials   WebAuthnCredential[]
  createdAt     DateTime @default(now())
  updatedAt     DateTime @updatedAt
}

model WebAuthnCredential {
  id           String   @id @default(cuid())
  userId       String
  user         User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  credentialId String   @unique
  publicKey    Bytes
  counter      BigInt   @default(0)
  transports   String[]
  createdAt    DateTime @default(now())
  
  @@index([userId])
  @@index([credentialId])
}
```

## Smart Wallet Derivation

Derive wallet address from passkey public key:

```typescript
import { p256 } from "@noble/curves/p256";
import { keccak256, toHex } from "viem";

export function deriveWalletAddress(publicKey: Uint8Array): Address {
  // 1. Get P256 public key coordinates
  const point = p256.ProjectivePoint.fromHex(publicKey);
  const x = point.x;
  const y = point.y;

  // 2. Create owner struct for CREATE2 salt calculation
  const owner = {
    x: BigInt(x),
    y: BigInt(y),
  };

  // 3. Calculate CREATE2 address using factory
  // This matches the smart contract's predictDiamondAddress function
  const address = predictWalletAddress(owner);

  return address;
}
```

## Cross-Device Sync

### iOS/macOS (iCloud Keychain)

Passkeys automatically sync via iCloud:

1. Enable iCloud Keychain in Settings
2. Sign in with Apple ID
3. Passkeys sync across all Apple devices
4. End-to-end encrypted

### Android/Chrome (Google Password Manager)

Passkeys sync via Google account:

1. Enable Google Password Manager
2. Sign in to Google account
3. Passkeys sync across Android and Chrome
4. End-to-end encrypted

### Using Synced Passkeys

When signing in from a new device:

1. Navigate to app.capsign.com
2. Click "Sign In"
3. Browser prompts "Use passkey from another device?"
4. Select synced passkey
5. Authenticate with biometric
6. Signed in on new device

## Security Considerations

### Private Key Security

- **Never leaves device**: Private key stored in secure enclave
- **Hardware protection**: Backed by Secure Enclave (iOS), TEE (Android)
- **Biometric access**: Requires Face ID/Touch ID for each use
- **No export**: Cannot be extracted from device

### Public Key Storage

Public keys are safely stored in database:

- Used only for signature verification
- Cannot be used to sign transactions
- Safe to store in database
- No security risk if exposed

### Cache Security

localStorage cache contains only:

- Public information (public key, address)
- Credential ID (non-sensitive)
- No private keys or secrets

Cache cleared:

- On logout
- After 7 days
- When session expires

## Best Practices

### For Users

1. **Secure your iCloud/Google account**:
   - Strong password
   - Two-factor authentication
   - Regular security reviews

2. **Keep devices secure**:
   - Enable device lock
   - Keep OS updated
   - Don't jailbreak/root

3. **Set up multiple devices**:
   - Register passkey on phone
   - Let it sync to other devices
   - Always have backup access

### For Developers

1. **Always check for WebAuthn support**:
```typescript
if (!window.PublicKeyCredential) {
  alert("WebAuthn not supported");
  return;
}
```

2. **Handle errors gracefully**:
```typescript
try {
  await PasskeyAuth.signin();
} catch (error) {
  if (error.name === "NotAllowedError") {
    // User cancelled
  } else if (error.name === "NotSupportedError") {
    // Not supported
  }
}
```

3. **Use HTTPS**:
   - WebAuthn requires secure context
   - Use https:// in production
   - localhost works for development

4. **Implement fallback**:
   - Offer alternative auth methods
   - Email verification backup
   - Recovery mechanisms

## Troubleshooting

### Passkey Creation Fails

**Common Issues**:
- Browser doesn't support WebAuthn
- User cancelled biometric prompt
- Device doesn't have biometric authentication
- Not using HTTPS

**Solutions**:
- Check browser compatibility
- Guide user through process
- Provide fallback authentication
- Ensure HTTPS in production

### Passkey Not Syncing

**Solutions**:
- **iOS**: Check iCloud Keychain is enabled
- **Android**: Verify Google account signed in
- Wait a few minutes for sync
- Try creating new passkey on new device

### Can't Sign In on New Device

**Solutions**:
- Ensure passkeys are syncing (check settings)
- Try "Use passkey from another device"
- Use account recovery if needed
- Contact support for assistance

## Related Documentation

- [Authentication](/interface/authentication.md) - Overall authentication architecture
- [Context Management](/interface/context-management.md) - Multi-account support
- [Security Best Practices](/guides/security-best-practices.md) - User security guide

---

**Next:** [Attestations](/interface/attestations.md) →

