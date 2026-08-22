# Confidential Register and Settlement

**Status**: Phase 1 advanced 2026-08-22 — Poseidon Merkle + Halo2 verifier wired; CapSign-native attestation gates; Anduril demo issues KYC/lot attestations. Real JoinSplit prove→verify e2e via TripleBooks `gen-test-proof --same-asset` + golden fixture (`JoinSplitE2ETest`; see `packages/privacy/PROVER.md`). Genealogy (`acquisitionDate`, `parentCommitment`) and `costBasis` bound in note commitment; two-input merge supports cross-date consolidation with `max(acq)` + value-weighted cost basis + multi-parent `Poseidon(cm0,cm1)`.  
**Audience**: CapSign / Eqvista engineering and product  
**Scope**: Design doc + Phase 1 implementation under `protocol/packages/privacy` (see package README).

This document locks product decisions for a confidential **shadow register for verification** and Markets settlement path on CapSign, reusing TripleBooks / Axiomatic ConfidentialLedger concepts (Halo2 UTXO notes, viewing keys, Rust prover) while preserving CapSign's lot / ERC-7752 model. It does not require replacing the issuer's day-to-day SoR.

---

## 1. Purpose / goals

Build a **confidential shadow register for verification** (and a **confidential settlement** path for Markets) such that issuers can keep day-to-day systems of record (Carta, Pulley, etc.) while CapSign certifies holdings and identities onchain:

1. **Import → certify → selectively disclose:** import holdings; certify lots and identities (KYC'd shareholder, certified position); support selective disclosure to authorized parties. The chain is the checkable proof layer, not a forced cutover of the issuer's SoR.
2. **Chain observers** do not see holder balances, lot amounts, or default aggregates (authorized / outstanding) unless an issuer opts in.
3. **Authorized parties** can still operate: issuer / transfer agent (TA) full register view, shareholder own holdings, regulator time-bounded disclosure, Markets capability proofs without full holdings dumps.
4. **Onchain** remains the certification and conservation layer (KYC'd parties, certified issuers, certified lots / assets; JoinSplit-style value and attribute proofs).
5. **UX abstracts the chain**: users see register, holdings, and trades; wallets, proofs, and nullifiers stay under the hood (passkeys, sponsored gas; no blockchain jargon in primary flows).
6. CapSign's existing **UTXO / ERC-7752 lot shape** maps naturally onto Halo2 notes, so genealogy and compliance attributes stay first-class.

Liquidity windows and secondary Markets settlement are **supported use cases** of this stack. They do not define the workstream by themselves.

---

## 2. Non-goals

- Replacing the issuer's day-to-day **system of record** (Carta, Pulley, or equivalent) as a mandatory migration. CapSign confidential register is a shadow verification / certification layer; SoR coexistence is the default product framing.
- Defining this workstream as **liquidity windows first**. Company programs / windows are one potential use case, not the product definition.
- Replacing CapSign's public lot diamond as the only token model in one cutover. Confidential register is an additive / migration path on CapSign's own stack.
- Making the **operator** (issuer portal / TA server with IVK) unable to see plaintext of notes it is authorized to scan. That is a different problem (FHE / MPC).
- Public by default disclosure of authorized shares, outstanding shares, or per-holder positions to chain scrapers.
- Onchain matching of orders (book, price-time priority) as the primary Markets design.
- Putting PII (names, SSNs, addresses) onchain; attestations only.
- Shipping FHE or MPC in the initial phases.
- Changing CapSign deploy scripts or facet versioning policy in this doc (conceptual workstreams only).

---

## 3. Product positioning (locked)

**Primary framing:** CapSign confidential stack is a **shadow register for verification**, not a mandatory replacement of the issuer's day-to-day SoR (Carta, Pulley, etc.). Flow: import holdings → certify lots and identities (KYC'd shareholder, certified position) → selective disclosure. The chain is the checkable proof layer.

**Liquidity windows** are a potential use case for this stack. They are not the definition of this workstream.

**Competitive split:**

| Competitor | What they are | How shadow verification helps |
|------------|---------------|-------------------------------|
| **Nasdaq Private Market (NPM)** | Company programs / liquidity windows | Attested holdings and eligibility without spreadsheet email |
| **Hiive** | Continuous secondary book | Require certified lot + KYC'd wallet before an order is live |

Shadow verification helps both models. It does **not** invent top-50 inventory by itself.

**UX constraints (already agreed):**

- Hard abstraction of blockchain (no chain jargon in primary product surfaces)
- Passkeys for auth / signing where product already targets them
- Sponsored gas so users are not asked to hold native gas for register or settle flows

---

## 4. Threat model

### 4.1 Public chain observer

Sees: transaction metadata that is intentionally public (nullifiers, commitments, Merkle roots, attestation IDs, settlement receipts without amounts if designed that way), and any **issuer-opted-in** public aggregates.

Does **not** see by default: lot amounts, holder balances, authorized / outstanding totals, full genealogy plaintext.

Goal: privacy against casual and adversarial chain scanners, indexers, and competitors.

### 4.2 Operator (issuer portal / TA / CapSign backend with custody of IVK or scoped viewing keys)

Can decrypt / scan notes in scope and run ordinary software analytics (cap table, outstanding, distributions prep).

Private from chain observers; **not** private from that operator. Fits TA / issuer portal analytics **if key custody is strict** (HSM / KMS, least privilege, audit logs, no affiliate bleed).

### 4.3 Markets affiliate barrier

Markets infrastructure (matching, member tape) must not automatically inherit issuer-register plaintext.

- Matching engine sees order intent needed to match (side, size, limit, instrument id) under its own access policy.
- Settlement posts confidential JoinSplits; Markets does **not** get issuer full-register IVK by default.
- Member tape for signed-in Markets users: **prints trades** (price / size / time). **Holdings stay confidential**.
- Capability proofs (e.g. "eligible to buy this lot class") without revealing full portfolio.

Treat CapSign Markets and issuer / TA register as separate trust domains even if the same corporate group operates both.

### 4.4 Shareholder and regulator

- Shareholder: own notes / lots only (plus whatever they voluntarily disclose).
- Regulator: epoch-scoped or warrant-scoped viewing grants; not permanent global IVK unless counsel requires and product explicitly grants it.

---

## 5. Architecture overview

```
┌─────────────────────────────────────────────────────────────────┐
│  UX (abstract chain): Issuer portal · Shareholder · Markets UI │
└─────────────┬───────────────────────┬───────────────────────────┘
              │                       │
              ▼                       ▼
┌─────────────────────────┐  ┌────────────────────────────────────┐
│ Offchain services       │  │ Markets                            │
│ · Scanner (IVK / OVK)   │  │ · Offchain match                   │
│ · Halo2 prover (Rust)   │  │ · Member tape (price/size/time)    │
│ · KMS / viewing grants  │  │ · Capability checks                │
│ · Attestation store     │  └──────────────┬─────────────────────┘
│   (PII offchain)        │                 │ confidential settle
└─────────────┬───────────┘                 │
              │                             ▼
              ▼                 ┌───────────────────────────────┐
┌─────────────────────────────┐ │ CapSign confidential settle   │
│ Certification layer         │ │ JoinSplit / note spend+create │
│ · KYC'd party attestations  │ │ + lot attribute proofs        │
│ · Certified issuer          │ └───────────────┬───────────────┘
│ · Certified lot / asset     │                 │
└─────────────┬───────────────┘                 ▼
              │                 ┌───────────────────────────────┐
              └────────────────►│ Chain: commitments, nullifiers│
                                │ Merkle state, verifier,       │
                                │ attestation anchors           │
                                └───────────────────────────────┘
```

**Reuse (conceptual) from TripleBooks / Axiomatic:**

- `ConfidentialLedger` + Halo2-KZG JoinSplit verifier
- Viewing key hierarchy (MSK → sk / IVK / OVK → ECDH note encryption)
- Rust Halo2 prover / verifier tooling
- Server-side IVK scan for operator analytics (with explicit custody rules)

**CapSign-specific:** notes carry **lot semantics** (asset / share class, acquisition attributes, parent linkage / genealogy commitments), not only fungible token amounts.

---

## 6. Privacy of aggregates (authorized / outstanding)

**Product decision (locked):** Do **not** default to public authorized or outstanding for chain observers. Issuer **opt-in only**.

| Audience | Authorized / outstanding |
|----------|---------------------------|
| Chain observers | Hidden by default; optional public commitment or plaintext if issuer opts in |
| Issuer / TA (IVK or issuer viewing key) | Full, in normal software after decrypt / scan |
| Shareholder | Own position; not issuer-wide totals unless product exposes a disclosed figure |
| Markets | No register-wide aggregates via chain; any displayed stats come from authorized APIs or proofs |
| Regulator | Per grant / epoch |

**How totals stay correct without publishing them:**

- Onchain JoinSplit / Halo2: conservation (inputs = outputs + public delta) and attribute proofs; public does not see lot amounts.
- Issuer / TA with IVK: sum decrypted notes (and known authorized commitments) offchain.
- Optional later: ZK aggregate circuits (e.g. prove outstanding &lt; N) without revealing the exact total, and without FHE.

---

## 7. Halo2 UTXO + lot genealogy

### 7.1 Why CapSign lots fit Halo2 notes

CapSign lots are already **UTXO / ERC-7752 shaped**: discrete acquisition records with amount, cost basis, transfer type, and `parentLotId` lineage. Mapping each confidential lot (or lot fragment) to a **note** is a natural fit:

- Spend note(s) → create note(s) on transfer / split / settle, same as JoinSplit.
- Nullifier prevents double-spend of a lot fragment.
- Commitments hide amount and owner; optional attribute commitments bind compliance fields.

### 7.2 What stays private vs proven

| Property | Public chain | Proven in circuit / attested |
|----------|--------------|------------------------------|
| Amount, owner | Hidden (commitment) | Conservation; ownership / spend authority |
| Asset / share class id | Often public or attested id | Note binds to certified asset |
| Parent genealogy | Prefer commitment to parent note / lot id | **JoinSplit:** single-input → parent cm; two-input merge → `Poseidon(cm0, cm1)`; encrypted memo still carries full plaintext for IVK |
| Acquisition date | Encrypted + commitment-bound | **JoinSplit:** single-input inherit; two-input merge → `max(acq0, acq1)` (conservative Rule 144 clock; cross-date allowed) |
| Cost basis / tax fields | Encrypted + commitment-bound | **JoinSplit:** single-input inherit; two-input merge → floor `((v0*c0+v1*c1)/(v0+v1))` in TokenLots lot units |
| Lockups / holding period | Attribute flags or commitments | Holding period can consume committed `acquisitionDate` + attestations |

### 7.3 Genealogy

Confidential register preserves CapSign's lot genealogy model on the **transfer / split / merge** JoinSplit path:

- **Split / single-input transfer:** child notes commit to the spent parent note commitment and inherit `acquisitionDate` + `costBasis`.
- **Two-input merge (including different acquisition dates):** children set `acquisitionDate = max(acq0, acq1)`, `costBasis` to the value-weighted average, and `parentCommitment = Poseidon(cm0, cm1)`.
- Issuance / shield sets `parentCommitment = 0` and chooses `acquisitionDate` / `costBasis` under issuer policy (not JoinSplit).
- Corporate actions that mint under issuer authority remain a separate policy path.
- Public explorers see graph structure only if we intentionally emit linkage; default is hidden linkage with decryptable memos for IVK holders.
- **Residual:** 3+ lot consolidate requires chained 2-in JoinSplits (circuit arity fixed at 2-in-2-out).

Exact encoding (one note per lot vs note = lot fragment) is an implementation choice; product requirement is **auditable lineage for TA / shareholder / regulator under selective disclosure**, not public scrapable trees.

---

## 8. Selective disclosure / key hierarchy

Align with Axiomatic privacy keys; CapSign roles map as follows.

| Role | Access | Mechanism (conceptual) |
|------|--------|-------------------------|
| Issuer / TA | Full register in scope | Issuer viewing key / IVK custody for issuer notes; scan + decrypt |
| Shareholder | Own holdings only | Own IVK / OVK; client or user-authorized scan |
| Regulator | Epoch-scoped | Time-bounded viewing grant; rotate / expire |
| Markets | Capability proofs only | Prove predicates (accredited, not restricted, inventory ≥ size) without holdings dump |
| Chain observer | Opt-in aggregates only | No viewing keys |

**Custody rules (operator):**

- IVK is read-only relative to spend key; still sensitive (full plaintext of incoming notes in scope).
- Store IVK only encrypted at rest (KMS); audit every decrypt / scan job.
- Do not share issuer IVK with Markets matching or affiliate analytics by default.
- Prefer scoped grants over "one god key" when the same human wears TA and Markets hats.

---

## 9. Server-side private compute: IVK vs ZK aggregates vs FHE

**Clarification (product / eng):**

### Can Halo2 UTXO do private server-side compute?

| Mode | What it is | Privacy property |
|------|------------|------------------|
| **Onchain JoinSplit / Halo2** | Conservation and attribute proofs | Public does not see lots / amounts |
| **Server with IVK** | Decrypt / scan notes; compute in normal software | Private from chain observers; **not** private from that server. Fits TA / issuer portal analytics if key custody is strict |
| **Extra ZK aggregate circuits** | Prove statements like outstanding &lt; N, or inventory ≥ trade size, without revealing values | No FHE required; circuit + witness from party that knows plaintext (or multi-party witness later) |
| **FHE or MPC** | Arbitrary compute without the server ever seeing plaintext (homomorphic sum, policy on ciphertext) | Later optional path; **not** vanilla Halo2 UTXO |

**When to use which:**

1. **IVK + normal compute** (default for issuer portal / TA): cap table, outstanding, waterfall prep, 409A inputs, shareholder statements.
2. **ZK aggregates / capability proofs**: Markets eligibility, regulatory "below threshold" statements, public opt-in proofs without publishing the number.
3. **FHE / MPC**: only if a requirement appears that the **operator must not** see plaintext yet must compute on it. Document as optional future; do not block the register on it.

---

## 10. Certification / attestations (identity + assets)

**Product decision (locked):** Chain as **certification layer**.

| Object | Onchain | Offchain |
|--------|---------|----------|
| Party | Attestation / credential id (KYC'd) | PII, KYC files, vendor payloads |
| Issuer | Certified issuer attestation | Corporate records, officers |
| Lot / asset | Certified asset / share class / offering attestation | Legal docs, legends, OCF mirrors |
| Settlement | Proof + nullifiers + attestation refs | Match ticket, trade confirm detail |

Attestations should be **revocable / expirable** where compliance requires. Circuits and compliance modules check attestation validity, not names onchain.

Reuse CapSign identity / claims / EAS-oriented patterns where they already exist; extend conceptually to "certified lot" and "certified issuer" as first-class anchors for confidential notes.

---

## 11. Markets: match offchain, settle onchain, member tape

**Product decisions (locked):**

1. **Offchain match, onchain confidential settle.**  
   Rationale: matching needs low latency, complex order types, and soft cancel / amend without putting the book onchain. Settlement needs public verifiability of conservation and certification without leaking positions. This split is standard for regulated digital securities venues and maps cleanly onto JoinSplit settlement.

2. **Member tape** for signed-in Markets users: print **trades** (price / size / time). **Holdings stay confidential.**

3. Markets path uses **capability proofs** (and match-time checks) rather than register-wide disclosure.

Flow (conceptual):

1. Eligible parties place orders (API / UI); identity attested.
2. Matcher pairs orders; produces a settlement intent (instrument, size, price, parties).
3. Prover builds JoinSplit(s) spending seller notes and creating buyer notes (plus payment leg as designed).
4. Onchain verify + nullify + insert commitments; emit trade tape fields allowed by policy (price / size / time), not residual holdings.
5. Each party (and TA if granted) scans with own keys.

---

## 12. CapSign contract workstreams (conceptual change map)

No bytecode in this doc. Workstreams relative to current CapSign protocol packages:

| Area | Today (public lots) | Confidential direction |
|------|---------------------|------------------------|
| `TokenLotsFacet` / ERC-7752 | Public lot structs, balances, parentLotId | Parallel confidential note state or shielded lot ids; genealogy via commitments |
| `TokenBalancesFacet` / ERC-20 views | Public totals | Default hide; opt-in public or proof-backed disclosures |
| `TokenTransferFacet` | Public transfers | JoinSplit settle path; compliance gates on proofs + attestations |
| `TokenComplianceFacet` + modules | Onchain readable state | Consume attestations + ZK attributes; avoid requiring public outstanding |
| `TokenMintBurnFacet` / issuance | Public mint | Shielded issuance notes under certified issuer |
| Offerings / escrows | Public subscription flows | Confidential allocate + settle; public offering metadata as product allows |
| Wallets | CapSign wallet diamonds | Spend auth + viewing key registration (stealth meta-address / IVK commitment) |
| Identity / claims | Existing claim patterns | KYC'd party + issuer + asset attestations as settle prerequisites |
| New / adapted | n/a | Verifier, Merkle note tree, nullifier set (ConfidentialLedger-shaped), Markets settlement entrypoints |

Prefer adapting Axiomatic `ConfidentialLedger` patterns over inventing a second privacy stack. CapSign-specific work is **lot attributes, compliance modules, and Markets settle**, not a new proof system.

---

## 13. Offchain services (prover, scanner, KMS)

| Service | Role |
|---------|------|
| **Halo2 prover** | Rust (Axiomatic-style): JoinSplit, attribute, optional aggregate / capability circuits. Client or trusted prover workers for large params. |
| **Scanner** | Index note events; trial-decrypt or scan-tag filter with IVK; materialize confidential register for issuer / TA / user. |
| **KMS / key grants** | Wrap IVKs, issue epoch regulator grants, rotate keys, audit access. Never log plaintext notes. |
| **Attestation service** | Issue / revoke party, issuer, asset attestations; store PII offchain; anchor ids onchain. |
| **Markets matcher** | Offchain book; submits settlement intents; no issuer full-register IVK by default. |
| **Member tape API** | Authenticated trade prints only. |

Operational split: register scanner under TA / issuer tenancy; Markets under Markets tenancy; shared prover binary OK if inputs are tenant-scoped.

---

## 14. Phased roadmap (checkable work items)

### Phase 0: Decisions and threat boundaries

- [x] Lock: no default public authorized / outstanding (issuer opt-in)
- [x] Lock: member tape = price / size / time; holdings confidential
- [x] Lock: offchain match, onchain confidential settle
- [x] Lock: chain as certification layer; PII offchain
- [x] Lock: abstract blockchain in UX (passkeys, sponsored gas)
- [x] Lock: shadow register for verification (not mandatory SoR replacement; liquidity windows are a use case, not the definition)
- [x] Lock: reuse ConfidentialLedger + viewing keys + Rust Halo2 prover conceptually
- [x] Lock: selective disclosure roles (issuer/TA, shareholder, regulator epoch, Markets capabilities)
- [ ] Confirm Markets affiliate barrier with counsel / compliance (see §15)
- [~] Choose note encoding: 1:1 with ERC-7752 lot vs fragment notes — **current assumption: 1:1 lot-like note** (`LotNotePlaintext`); revisit before production cutover

### Phase 1: Contracts + demo fixture (no production cutover)

- [x] Map CapSign lot fields → note plaintext + attribute commitments — **landed** (`IConfidentialRegister.LotNotePlaintext` + Poseidon `LotNoteCrypto` with genealogy-bound commitment)
- [x] Stand up verifier + Merkle note tree against a CapSign test diamond — **Poseidon depth-32 Merkle** + `Halo2KZGVerifier` wired; `emergencyOperatorTrustedMode` for demo / emergency
- [x] Prove JoinSplit conservation for a simple equity lot transfer — **`JoinSplitE2ETest`** with golden fixture from TripleBooks `gen-test-proof --same-asset` (`emergencyOperatorTrustedMode=false`); regen in `packages/privacy/PROVER.md`
- [x] Genealogy in JoinSplit (transfer/split/merge): bind `acquisitionDate` + `parentCommitment` + `costBasis`; prove single-parent inherit, cross-date `max(acq)`, weighted cost basis, multi-parent `Poseidon(cm0,cm1)` — **landed** (TripleBooks circuit + CapSign `LotNoteCrypto` / fixtures); 3+ input consolidate still chained 2-in
- [~] IVK scanner materializes a private "cap table" for one issuer — **Foundry** forgeDev decrypt + scan tag; **production** ECDH+AES-GCM in `scripts/note-crypto/`; no production scanner service yet
- [ ] Document gas / proof size / prove latency on target L2
- [x] Shadow import demo fixture: fictional Anduril holders → commitments + attestations + `ProofRegistry` `REGISTER_ROOT` anchor (`script/demo/ImportAndurilShadowRegister.s.sol`)
- [x] Package README + PROVER.md: `protocol/packages/privacy/`

### Phase 2: Confidential register (issuer / TA / shareholder)

- [ ] Shielded issuance and transfer for one share class
- [ ] Selective disclosure: issuer full, shareholder own
- [ ] Opt-in public aggregate commitments (optional flag)
- [x] Genealogy decryptable to TA; not public by default — **commitment-bound + JoinSplit inheritance**; IVK decrypt for full plaintext graph
- [ ] UX: register and holdings with no chain jargon

### Phase 3: Certification hardening

- [x] Party / certified lot attestations wired into shield / transfer gates — CapSign-native `IRegisterAttestation` on `ConfidentialRegisterFacet` (KYC / accredited / shareholder / certified lot); PII offchain
- [x] Revocation / expiry handling in attestation validity checks
- [ ] Regulator epoch viewing grant flow

### Phase 4: Markets confidential settle

- [ ] Offchain matcher → settlement intent schema
- [ ] Onchain confidential settle for matched trades
- [ ] Member tape API (price / size / time)
- [ ] Capability proofs for eligibility (no holdings dump)
- [ ] Enforce Markets vs TA key separation in infra

### Phase 5: Optional advanced privacy

- [ ] ZK aggregate circuits (outstanding &lt; N, etc.) without publishing totals
- [ ] Evaluate FHE / MPC only if operator-blind compute becomes a hard requirement

---

## 15. Open questions for counsel

1. **Affiliate barrier:** If CapSign / Eqvista operates both TA register and Markets, what legal and operational separation is required so Markets access to trade prints and match data does not become "constructive" access to the full confidential register?
2. **Member tape:** Is price / size / time for signed-in members sufficient for applicable ATS / broker-dealer / state regimes, or are additional fields required (and do any of those re-identify holders)?
3. **Opt-in public aggregates:** When an issuer publishes authorized / outstanding onchain, does that create Reg FD / disclosure or offering-document consistency obligations?
4. **Regulator viewing grants:** Preferred legal instrument and retention for epoch IVK / viewing key disclosure (exam, subpoena, ongoing supervision).
5. **Attestations vs transfer agent books of record:** Product framing is shadow verification (certified mirror / proof layer) alongside the issuer's day-to-day SoR. Confirm with counsel whether the confidential onchain register may also serve as books and records in any regime, or must remain a cryptographically certified mirror of an offchain TA / issuer SoR.
6. **Capability proofs in lieu of broker diligence files:** What still must be retained in cleartext offchain for AML / CIP even if Markets never sees full holdings?
7. **Cross-border:** Any jurisdictions where hiding outstanding from the public chain conflicts with corporate transparency statutes (while still allowing TA visibility)?

---

## 16. References

### CapSign

| Path | Relevance |
|------|-----------|
| `protocol/packages/tokens/src/facets/TokenLotsFacet.sol` | Public ERC-7752 lot management |
| `protocol/packages/tokens/src/facets/TokenERC7752Facet.sol` | ERC-7752 interface surface |
| `protocol/packages/tokens/src/facets/TokenTransferFacet.sol` | Transfer path to evolve / parallel |
| `protocol/packages/tokens/src/facets/TokenBalancesFacet.sol` | Public balance views; opt-in disclosure later |
| `protocol/packages/tokens/src/facets/TokenComplianceFacet.sol` | Compliance gates |
| `protocol/packages/tokens/src/facets/TokenMintBurnFacet.sol` | Issuance |
| `protocol/packages/tokens/` (option / RSU storage) | Authorized pool concepts (today public) |
| `docs/protocol/erc-7752.md` | Lot standard overview |
| `docs/tokens/lot-based-accounting.md` | Product lot model |
| `docs/architecture/TOKEN_DIAMOND_ARCHITECTURE.md` | Diamond / facet layout |
| `docs/internal/` | Internal design docs (this file) |
| `protocol/packages/privacy/` | **Phase 1**: ConfidentialRegisterFacet (Poseidon Merkle, Halo2 verifier wiring, CapSign-native attestations), LotNoteCrypto, Anduril demo fixture import, PROVER.md |
| `protocol/packages/ledger/` | ProofRegistry / SnapshotAnchor (REGISTER_ROOT anchoring) |

### TripleBooks / Axiomatic (reuse conceptually)

| Path | Relevance |
|------|-----------|
| `triplebooks/protocol/packages/privacy/src/ConfidentialLedger.sol` | Private UTXO ledger, JoinSplit verify |
| `triplebooks/protocol/packages/privacy/src/Halo2KZGVerifier.sol` | Onchain Halo2-KZG verifier |
| `triplebooks/platform/docs/architecture/privacy-keys.md` | MSK / IVK / OVK / ECDH hierarchy |
| `triplebooks/platform/docs/modules/wallet.md` | Server IVK scan, prover artifacts |
| `triplebooks/platform/engine` (prover CLI, JoinSplit) | Rust Halo2 prove / verify |
| `triplebooks/platform/packages/db` (shielded notes schema) | Offchain note materialization patterns |

### External

- ERC-7752 lot token discussion / spec (Ethereum Magicians / EIP drafts as linked from CapSign docs)
- Halo2 (PLONK-style proving); Axiomatic stack uses Halo2-KZG on BN254 for JoinSplit

---

## Document control

| Field | Value |
|-------|-------|
| Created | 2026-08-21 |
| Owner | CapSign protocol + product |
| Updates | Amend this file as counsel answers §15 and Phase 0 encoding choice lands |
| Phase 1 | 2026-08-22 — Poseidon + Halo2 verifier + attestation gates + Anduril demo attestations (`protocol/packages/privacy`) |
