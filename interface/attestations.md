# Attestations

Implement attestation viewing and verification in your interface.

## Overview

Display and manage on-chain attestations (identity credentials) in your application.

## Key Components

### Attestation List

Display user's attestations:

```typescript
import { useAttestations } from "@/hooks/use-attestations";

export function AttestationList({ address }: { address: Address }) {
  const { attestations, loading } = useAttestations(address);

  if (loading) return <Spinner />;

  return (
    <div>
      {attestations.map((attestation) => (
        <AttestationCard key={attestation.uid} attestation={attestation} />
      ))}
    </div>
  );
}
```

### Attestation Card

Display individual attestation:

```typescript
function AttestationCard({ attestation }: { attestation: Attestation }) {
  return (
    <div className="attestation-card">
      <div className="type">{getAttestationType(attestation.schema)}</div>
      <div className="issuer">Issued by: {attestation.attester}</div>
      <div className="date">
        {new Date(attestation.time * 1000).toLocaleDateString()}
      </div>
      <div className="status">
        {attestation.revoked ? "Revoked" : "Active"}
      </div>
    </div>
  );
}
```

## Fetching Attestations

### From EAS Contract

```typescript
import { easContract } from "@/constants/contracts";

export async function getAttestationsForAddress(
  client: PublicClient,
  recipient: Address
): Promise<Attestation[]> {
  // Query EAS contract for attestations
  const attestations = await client.readContract({
    address: easContract.address,
    abi: easContract.abi,
    functionName: "getAttestations",
    args: [recipient],
  });

  return attestations.map(parseAttestation);
}
```

### From Subgraph

```typescript
export async function getAttestationsFromSubgraph(
  address: Address
): Promise<Attestation[]> {
  const query = `
    query GetAttestations($recipient: String!) {
      attestations(where: { recipient: $recipient }) {
        id
        attester
        recipient
        schema
        time
        revoked
        data
      }
    }
  `;

  const response = await fetch(SUBGRAPH_URL, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      query,
      variables: { recipient: address.toLowerCase() },
    }),
  });

  const { data } = await response.json();
  return data.attestations;
}
```

## Attestation Types

Map schemas to human-readable types:

```typescript
const ATTESTATION_TYPES: Record<Hex, string> = {
  [KYC_SCHEMA]: "Identity Verification",
  [ACCREDITED_SCHEMA]: "Accredited Investor",
  [AGE_SCHEMA]: "Age Verification",
  [GEOGRAPHIC_SCHEMA]: "Geographic Location",
};

export function getAttestationType(schema: Hex): string {
  return ATTESTATION_TYPES[schema] || "Unknown";
}
```

## Verification

Verify attestation on-chain:

```typescript
export async function verifyAttestation(
  client: PublicClient,
  uid: Hex
): Promise<boolean> {
  const attestation = await client.readContract({
    address: easContract.address,
    abi: easContract.abi,
    functionName: "getAttestation",
    args: [uid],
  });

  // Check not revoked and not expired
  return !attestation.revoked && attestation.expirationTime > Date.now() / 1000;
}
```

## Related Documentation

- [Protocol - Attestations](/protocol/attestations.md) - Smart contract details
- [Viewing Attestations](/guides/viewing-attestations.md) - User guide

---

**Next:** [Components](/interface/components.md) →

