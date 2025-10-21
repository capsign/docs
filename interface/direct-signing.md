# Direct Signing

Integrate traditional EOA wallets (MetaMask, Coinbase Wallet) with CapSign for entity account signing.

## Overview

CapSign supports **direct EIP-1193 signing** for entity accounts that use externally owned accounts (EOAs) as owners:

- Direct `window.ethereum` interaction
- No wagmi connection conflicts
- Works alongside passkey connector
- Supports Gnosis Safe and other smart wallets
- Standard dApp pattern (like OpenSea, Uniswap)

## Architecture

```
Entity Account (Smart Contract Wallet)
       │
       │ Owner
       ▼
EOA Wallet (MetaMask, Coinbase Wallet, etc.)
       │
       │ Sign via EIP-1193
       ▼
window.ethereum.request({ method: "personal_sign" })
       │
       ▼
Wallet Popup → User Approves → Signature
```

## When to Use

### Individual Context
- Uses **passkey** (WebAuthn)
- Biometric authentication
- toCapSignSmartAccount adapter

### Entity Context
- Uses **EOA** (MetaMask, etc.)
- Connected wallet signing
- toCapSignSmartAccountWithEOA adapter

## Implementation

### 1. EIP-1193 Provider Utilities

Create utilities for direct `window.ethereum` interaction:

```typescript
// lib/wagmi/ethereum-provider.ts

// Check if wallet is available
export function hasEthereumProvider(): boolean {
  return typeof window !== "undefined" && !!window.ethereum;
}

// Request wallet connection
export async function requestEthereumAccounts(): Promise<Address[]> {
  if (!hasEthereumProvider()) {
    throw new Error("No Ethereum provider found");
  }

  const accounts = await window.ethereum.request({
    method: "eth_requestAccounts",
  });

  return accounts as Address[];
}

// Get connected accounts (silent)
export async function getEthereumAccounts(): Promise<Address[]> {
  if (!hasEthereumProvider()) return [];

  const accounts = await window.ethereum.request({
    method: "eth_accounts",
  });

  return accounts as Address[];
}

// Sign message via personal_sign (EIP-191)
export async function signMessageWithEthereum(
  message: Hex,
  account: Address
): Promise<Hex> {
  if (!hasEthereumProvider()) {
    throw new Error("No Ethereum provider found");
  }

  const signature = await window.ethereum.request({
    method: "personal_sign",
    params: [message, account],
  });

  return signature as Hex;
}

// Check if address is a contract
export async function isContractAddress(
  client: PublicClient,
  address: Address
): Promise<boolean> {
  const code = await client.getBytecode({ address });
  return code !== undefined && code !== "0x";
}
```

### 2. EOA Smart Account Adapter

Create adapter for EOA-owned smart accounts:

```typescript
// lib/wagmi/eoa-smart-account.ts
import { toSmartAccount } from "viem/account-abstraction";
import { signMessageWithEthereum } from "./ethereum-provider";

export async function toCapSignSmartAccountWithEOA(params: {
  client: Client;
  eoaOwner: Address;
  walletAddress: Address;
}) {
  const { client, eoaOwner, walletAddress } = params;

  // Create smart account with EOA signer
  return toSmartAccount({
    client,
    address: walletAddress,
    
    async sign({ hash }) {
      // Sign via window.ethereum (prompts wallet)
      return await signMessageWithEthereum(hash, eoaOwner);
    },

    async signMessage({ message }) {
      const hash = hashMessage(message);
      return await signMessageWithEthereum(hash, eoaOwner);
    },

    async signTypedData(typedData) {
      const hash = hashTypedData(typedData);
      return await signMessageWithEthereum(hash, eoaOwner);
    },

    // ... other required methods
  });
}
```

### 3. Context-Aware Account Creation

Route to correct adapter based on context:

```typescript
// lib/wagmi/create-context-smart-account.ts

export async function createContextSmartAccount(params: {
  client: Client;
  session: { user: User };
}) {
  const { client, session } = params;
  const contextType = session.user.context?.type || "individual";

  if (contextType === "entity") {
    // Entity context: Use EOA adapter
    const entityAddress = session.user.context.walletAddress;
    
    // Fetch owner from API
    const ownerRes = await fetch(`/api/wallets/owner?address=${entityAddress}`);
    const { ownerAddress, ownerType } = await ownerRes.json();

    if (ownerType !== "EOA") {
      throw new Error(`Unsupported owner type: ${ownerType}`);
    }

    // Verify correct wallet is connected
    const accounts = await getEthereumAccounts();
    if (!accounts.includes(ownerAddress)) {
      // Request connection
      await requestEthereumAccounts();
    }

    // Create EOA-backed smart account
    const account = await toCapSignSmartAccountWithEOA({
      client,
      eoaOwner: ownerAddress,
      walletAddress: entityAddress,
    });

    return { account, walletAddress: entityAddress, contextType };
    
  } else {
    // Individual context: Use passkey adapter
    const passkey = await PasskeyAuth.getPasskey();
    
    const account = await toCapSignSmartAccount({
      client,
      owner: passkey.publicKey,
      ownerType: "passkey",
      walletAddress: passkey.smartWalletAddress,
    });

    return {
      account,
      walletAddress: passkey.smartWalletAddress,
      contextType,
    };
  }
}
```

### 4. Component Integration

Use in React components:

```typescript
"use client";

import { useState } from "react";
import { useSession } from "next-auth/react";
import { createContextSmartAccount } from "@/lib/wagmi/create-context-smart-account";
import { createBundlerClient } from "viem/account-abstraction";

export function SendTransaction() {
  const { data: session } = useSession();
  const [loading, setLoading] = useState(false);

  const handleSend = async () => {
    if (!session) return;

    setLoading(true);
    try {
      // Create context-aware account
      const { account, contextType } = await createContextSmartAccount({
        client: publicClient,
        session,
      });

      console.log(`Using ${contextType} context`);

      // Create bundler client
      const bundlerClient = createBundlerClient({
        account,
        client: publicClient,
        transport: http(BUNDLER_RPC_URL),
      });

      // Send transaction
      // For entity context, this will prompt MetaMask for signature
      const hash = await bundlerClient.sendUserOperation({
        calls: [{
          to: recipientAddress,
          value: parseEther("0.01"),
        }],
      });

      console.log("UserOp hash:", hash);

      // Wait for confirmation
      const receipt = await bundlerClient.waitForUserOperationReceipt({ hash });
      console.log("Transaction:", receipt.receipt.transactionHash);

    } catch (error) {
      console.error("Transaction failed:", error);
    } finally {
      setLoading(false);
    }
  };

  return (
    <button onClick={handleSend} disabled={loading}>
      {loading ? "Sending..." : "Send Transaction"}
    </button>
  );
}
```

## User Flow

### Entity Context Transaction

1. User switches to entity context (e.g., "Acme Corp")
2. User initiates transaction
3. System detects entity context
4. Fetches EOA owner address from API
5. Checks if correct wallet connected
6. If not connected, prompts "Connect with MetaMask"
7. Creates EOA-backed smart account
8. Submits UserOperation
9. MetaMask popup appears for signature
10. User approves in MetaMask
11. Transaction submitted to bundler
12. Confirmation received

## Benefits

### No Connector Conflicts

Traditional approach (problematic):
```typescript
// ❌ Causes conflicts with passkey connector
import { useConnect } from "wagmi";
const { connect } = useConnect();
await connect({ connector: metaMaskConnector });
```

Direct EIP-1193 approach:
```typescript
// ✅ No conflicts, works alongside passkeys
import { signMessageWithEthereum } from "@/lib/wagmi/ethereum-provider";
const signature = await signMessageWithEthereum(hash, eoaAddress);
```

### Works with Any Wallet

Supports all EIP-1193 compatible wallets:
- MetaMask
- Coinbase Wallet
- Rainbow Wallet
- WalletConnect
- Gnosis Safe
- Hardware wallets (via MetaMask/WalletConnect)

### Clean Separation

- Individual accounts → Passkeys (biometric)
- Entity accounts → EOA wallets (MetaMask, etc.)
- No confusion about which auth method to use
- Context determines authentication method

## Security Considerations

### Wallet Connection Verification

Always verify correct wallet is connected:

```typescript
async function verifyWalletConnection(expectedOwner: Address) {
  const accounts = await getEthereumAccounts();
  
  if (!accounts.includes(expectedOwner)) {
    throw new Error(`Please connect wallet: ${expectedOwner}`);
  }
}
```

### Signature Validation

Signatures are validated on-chain by the smart account contract:

```solidity
// In smart account's validateUserOp function
function validateUserOp(UserOperation calldata userOp, bytes32 userOpHash, uint256 missingAccountFunds)
    external
    returns (uint256 validationData)
{
    bytes32 hash = userOpHash.toEthSignedMessageHash();
    address recovered = hash.recover(userOp.signature);
    
    // Check recovered address matches owner
    require(recovered == owner, "Invalid signature");
    
    return 0; // Valid
}
```

### Phishing Protection

Direct signing is phishing-resistant:
- Users see clear transaction details in wallet
- Signature tied to specific UserOperation
- Can't be replayed or modified
- Domain binding via EIP-712 (for typed data)

## Troubleshooting

### Wallet Not Connected

**Error**: "No Ethereum provider found"

**Solutions**:
- Install MetaMask or other wallet
- Enable wallet extension
- Refresh page after installation

### Wrong Wallet Connected

**Error**: "Please connect wallet: 0x..."

**Solutions**:
- Switch to correct account in wallet
- Connect the wallet that owns the entity
- Verify you have access to entity wallet

### Signature Rejected

**Error**: "User rejected signature"

**Solutions**:
- User cancelled in wallet - try again
- Check transaction details are correct
- Ensure sufficient balance for gas

## Related Documentation

- [Context Management](/interface/context-management.md) - Multi-account switching
- [Authentication](/interface/authentication.md) - Overall auth architecture
- [Protocol - Smart Accounts](/protocol/smart-accounts.md) - Smart account contracts

---

**Next:** [Context Management](/interface/context-management.md) →

