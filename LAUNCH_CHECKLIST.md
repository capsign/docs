# CapSign Production Launch Checklist

**Last Updated**: January 26, 2026  
**Target Launch**: TBD  
**Status**: 🟡 In Progress

---

## Executive Summary

CapSign is a comprehensive platform for tokenized securities, digital equity management, and compliant investment workflows. This checklist covers all workstreams required for a production-ready launch.

### Key Pre-Launch Priorities

| Priority | Feature | Est. Effort | Status |
|----------|---------|-------------|--------|
| P0 | Capital Call Workflow | 3-5 days | ✅ |
| P0 | SPV/Fund Admin Dashboard | 5-7 days | ✅ |
| P0 | NAV Update Workflow | 2-3 days | ✅ |
| P0 | Capital Account Statements | 3 days | ✅ |
| P0 | K-1 Generation | 5+ days | ✅ |
| P0 | ROFR Workflow | 2-3 days | ✅ |
| P0 | Performance Metrics (IRR, MOIC) | 3-4 days | ✅ |
| P0 | Multi-Currency (EUR, GBP) | 2-3 days | 🟡 |
| P1 | Custodian Integration Polish | 2-3 days | 🟡 |

**Total estimated effort: ~4-6 weeks**

### Components

| Component | Status | Priority | Owner |
|-----------|--------|----------|-------|
| Protocol (Smart Contracts) | ✅ Deployed to mainnet | P0 | - |
| Interface (Web App) | 🟡 75% | P0 | - |
| Subgraph (Indexing) | ✅ Deployed (syncing) | P0 | - |
| Infrastructure (Bundler, RPC) | ✅ Configured | P1 | - |
| Marketing Website | ✅ Ready | P1 | - |
| SDK | 🟡 50% | P2 | - |
| Documentation | 🟡 70% | P1 | - |
| Legal | ✅ Complete (Termly-hosted) | P0 | - |

---

## 🔴 P0: Critical Path (Must Have for Launch)

### 1. Protocol Deployment

| Task | Status | Notes |
|------|--------|-------|
| Deploy all facets to Base mainnet | ✅ Complete | 20+ deployment runs in `broadcast/8453/` |
| Verify all contracts on Basescan | ✅ Complete | Auto-verified during deployment |
| Configure FacetRegistry for mainnet | ✅ Complete | `0x4261A5238F3C90b404AfDC72Fb9eB75Ae506DeBd` |
| Deploy DiamondFactory to mainnet | ✅ Complete | `0x29b6EcCf401DDDC7B7A52cb5645380e6Fd9Cd687` |
| Test wallet creation on mainnet | ✅ Complete | Live wallets created |
| Test token deployment on mainnet | ✅ Complete | Tokens deployed on mainnet |

### 2. Subgraph Stability

| Task | Status | Notes |
|------|--------|-------|
| Deploy Base mainnet subgraph | ✅ Complete | Configured with all factory addresses |
| Verify entity indexing (offerings, tokens) | 🟡 In Progress | Base Sepolia v0.13.3 syncing |
| Add promissory note indexing | ✅ Complete | `PromissoryNote` entity, `NoteInitialized` handler |
| Add promissory notes to stats | ✅ Complete | Included in "Financings" |
| Test grafting for upgrades | ℹ️ N/A | Goldsky requires re-indexing for major changes |
| Document subgraph versioning process | ✅ Complete | `docs/VERSIONING_STRATEGY.md` |

### 3. Interface Polish

| Task | Status | Notes |
|------|--------|-------|
| Loading states on all pages | ✅ Complete | All major pages have loading states |
| Error handling for failed transactions | ✅ Complete | `error-handler.ts` + enhanced messages |
| Mobile responsiveness audit | 🟡 Partial | Entity creation fixed, smoke tests pass |
| Accessibility (a11y) audit | ⬜ Not Started | WCAG 2.1 AA |
| Performance audit (Lighthouse) | ⬜ Not Started | Target 90+ |
| Remove console.log statements | ✅ Complete | Logger utility with Sentry breadcrumbs |
| Fix all TypeScript errors | 🟡 Ongoing | `pnpm build` clean |
| Test all happy paths E2E | 🟡 Partial | Entity creation, document signing flows added |

### 4. Authentication & Security

| Task | Status | Notes |
|------|--------|-------|
| Passkey creation flow tested | ✅ Complete | WebAuthn working |
| Session management secure | ✅ Complete | NextAuth configured |
| CSRF protection | ✅ Complete | Built into NextAuth |
| Rate limiting on APIs | ✅ Complete | Edge middleware in `middleware.ts` |
| Input validation on all endpoints | ✅ Complete | Zod schemas + DOMPurify |
| SQL injection prevention | ✅ Complete | Prisma parameterized |
| XSS prevention | ✅ Complete | DOMPurify on all HTML rendering |

### 5. Legal & Compliance

| Task | Status | Notes |
|------|--------|-------|
| Terms of Service | ✅ Complete | Termly-hosted |
| Privacy Policy | ✅ Complete | Termly-hosted |
| Acceptable Use Policy | ⬜ Not Started | For issuers |
| Securities disclaimers | ⬜ Not Started | Per jurisdiction |
| SAFE template legal review | ⬜ Not Started | Based on YC SAFE |
| Debt instrument legal review | ⬜ Not Started | Promissory note terms |
| 506(c) compliance verification | ⬜ Not Started | For offerings |

---

## 🟡 P1: Important (Should Have for Launch)

### 6. Infrastructure

| Task | Status | Notes |
|------|--------|-------|
| Production RPC endpoints | ✅ Complete | CDP mainnet + sepolia configured |
| Bundler for mainnet (Pimlico/CDP) | 🟡 Partial | CDP bundler set, Pimlico key present |
| Paymaster policy for mainnet | 🟡 Feature flag | `NEXT_PUBLIC_USE_PRODUCTION_PAYMASTER=false` |
| Database backup strategy | ✅ Complete | Neon with PITR |
| CDN for static assets | ✅ Complete | Vercel |
| SSL certificates | ✅ Complete | Vercel auto |
| Domain configuration | ✅ Complete | `app.capsign.com` |
| Monitoring (uptime) | ⬜ Not Started | Better Stack, Datadog |
| Error tracking (Sentry) | ✅ Complete | SDK installed, logger utility created |

### 7. Email & Notifications

| Task | Status | Notes |
|------|--------|-------|
| SendGrid production account | 🟡 Configured | Verify sender domain |
| Email templates styled | 🟡 Partial | Some plain text |
| Transactional emails working | ✅ Complete | Signing notifications |
| Email delivery monitoring | ⬜ Not Started | Track bounces |
| Unsubscribe handling | ⬜ Not Started | CAN-SPAM compliance |

### 8. Documentation

| Task | Status | Notes |
|------|--------|-------|
| User guides (getting started) | 🟡 Partial | `/docs` folder |
| API documentation | 🟡 Partial | Some endpoints documented |
| Smart contract documentation | ✅ Complete | NatSpec + docs |
| Video tutorials | ⬜ Not Started | Loom recordings |
| FAQ page | ⬜ Not Started | Common questions |
| Help/support contact | ⬜ Not Started | Email or chat |

### 9. Lending Platform (California Capital)

| Task | Status | Notes |
|------|--------|-------|
| Loan application flow | ✅ Complete | 5-step wizard |
| Underwriting dashboard | ✅ Complete | Admin review |
| Document generation | ✅ Complete | 3 templates |
| Document signing | ✅ Complete | Multi-party |
| Self-describing signatures | ✅ Complete | Template-driven |
| On-chain note deployment | 🟡 Partial | Fund loan button |
| Payment tracking UI | ⬜ Not Started | Phase 2F |
| Portfolio dashboard | ⬜ Not Started | Admin view |
| LP legal formation | 🔴 Pending | Counsel engagement |

---

## 🟠 Private Capital Coverage (Comprehensive Assessment)

### Current Instrument Support

| Instrument | Protocol | Interface | Status |
|------------|----------|-----------|--------|
| Common/Preferred Stock | ✅ TokenBalancesFacet | ✅ Create equity UI | Production |
| LLC Membership Units | ✅ MembershipUnit type | ✅ Create unit UI | Production |
| LP Interests | ✅ Same as units | ✅ Available | Production |
| SAFEs | ✅ TokenSAFEFacet | ✅ Create SAFE UI | Production |
| Promissory Notes | ✅ TokenNoteFacet | ✅ Create debt UI | Production |
| Convertible Notes | ✅ SAFE with interest | 🟡 Needs UI | Protocol ready |
| Employee Options | ✅ TokenOptionFacet | 🟡 Needs UI | Protocol ready |
| RSUs | ✅ TokenRSUFacet | 🟡 Needs UI | Protocol ready |
| Warrants | ✅ TokenOptionFacet | 🟡 Needs UI | Protocol ready |
| Bonds | ✅ TokenBondFacet | ⬜ No UI | Protocol ready |

### Investment Vehicle Support

| Vehicle | Protocol | Interface | Status |
|---------|----------|-----------|--------|
| SPV (Single Asset) | ✅ VehicleCoreFacet | 🟡 Partial | Protocol ready |
| PE Fund | ✅ VehicleType.PE_FUND | ⬜ No UI | Protocol ready |
| VC Fund | ✅ VehicleType.VC_FUND | ⬜ No UI | Protocol ready |
| Hedge Fund | ✅ VehicleType.HEDGE_FUND | ⬜ No UI | Protocol ready |
| Real Estate Fund | ✅ VehicleType.REAL_ESTATE_FUND | ⬜ No UI | Protocol ready |
| Trust | ✅ TrustDistributionFacet | ⬜ No UI | Protocol ready |

### Corporate Actions & Compliance

| Feature | Protocol | Interface | Status |
|---------|----------|-----------|--------|
| Stock Splits | ✅ TokenCorporateActionsFacet | ✅ UI | Production |
| Stock Dividends | ✅ Same facet | ✅ UI | Production |
| Cash Distributions | ✅ VehicleDistributionFacet | 🟡 Needs testing | Protocol ready |
| Waterfall Distributions | ✅ WaterfallTier struct | ⬜ No UI | Protocol ready |
| Vesting Schedules | ✅ VestingComplianceModule | ✅ UI | 29 tests |
| Rule 144 Compliance | ✅ Rule144ComplianceModule | ✅ UI (auto-applied for US) | Production |
| Volume Limits | ✅ VolumeLimitModule | ✅ UI | 36 tests |
| 409A Valuations | ✅ Token409AFacet | ✅ UI | Production |

### Pre-Launch Gaps (Required for Launch)

| Gap | Priority | Effort | Status | Notes |
|-----|----------|--------|--------|-------|
| Capital Call Workflow | P0 | 3-5 days | ✅ Complete | CapitalCallsTab with create, notify, record contributions |
| SPV/Fund Admin Dashboard | P0 | 5-7 days | ✅ Complete | Performance, Members, Capital, Distributions, Tax tabs |
| NAV Update Workflow | P0 | 2-3 days | ✅ Complete | NAV endpoint, daily snapshots in Prisma |
| Capital Account Statements | P0 | 3 days | ✅ Complete | Monthly PDF statements via Puppeteer |
| K-1 Generation | P0 | 5+ days | ✅ Complete | TaxDocumentsTab with partner import, bulk distribution |
| ROFR Workflow | P0 | 2-3 days | ✅ Complete | GP approval required for secondary transfers |
| Performance Metrics | P0 | 3-4 days | ✅ Complete | IRR, MOIC, TVPI, DPI, RVPI calculations |
| Multi-Currency Support | P0 | 2-3 days | 🟡 Partial | USDC done, need EUR/GBP stablecoins |
| Custodian Integration Polish | P1 | 2-3 days | 🟡 Partial | API integration refinement |
| Options/RSU Grant UI | P1 | 2-3 days | ⬜ Not started | Exercise flow, vesting display |
| Distribution Claims E2E Test | P1 | 1-2 days | ⬜ Not started | Verify claim flow works |

### Use Case Coverage Estimate

| Use Case | Current | After Pre-Launch | Notes |
|----------|---------|------------------|-------|
| Startup Equity (SAFEs, options, cap table) | 90% | 98% | Core strength, market-leading |
| Direct Lending (promissory notes) | 85% | 95% | California Capital validated |
| SPV Investments | 70% | 95% | Dashboard + distributions + K-1 |
| VC Fund Administration | 60% | 95% | Capital calls + K-1 + statements |
| PE Fund Administration | 55% | 95% | Capital calls + ROFR + waterfall |
| Hedge Fund Administration | 40% | 90% | NAV engine + redemptions |
| Real Estate Fund | 50% | 90% | Distributions + statements |
| Family Office / Trust | 60% | 85% | Trust facets + distributions |

**Target: 90-95% coverage of US private capital use cases at launch.**

Remaining edge cases NOT covered (by design):
- Complex derivatives (swaps, options on options)
- Prime brokerage services
- High-frequency trading infrastructure
- Exotic multi-currency hedging

These are outside scope and not handled by competitors (Carta, Securitize) either.

---

## 🟢 P2: Nice to Have (Can Follow Launch)

### 10. SDK & Developer Experience

| Task | Status | Notes |
|------|--------|-------|
| TypeScript SDK core | 🟡 Started | `/sdk` folder |
| Token operations | 🟡 Partial | Basic methods |
| Wallet operations | ⬜ Not Started | - |
| Document operations | ⬜ Not Started | - |
| NPM package published | ⬜ Not Started | @capsign/sdk |
| SDK documentation | ⬜ Not Started | README + examples |

### 11. Analytics & Insights

| Task | Status | Notes |
|------|--------|-------|
| Platform analytics (Posthog/Mixpanel) | ⬜ Not Started | User behavior |
| On-chain analytics dashboard | ⬜ Not Started | Protocol metrics |
| Cap table export (CSV/PDF) | 🟡 Partial | Basic export |
| Investor reports | ⬜ Not Started | Periodic statements |

### 12. Advanced Features

| Task | Status | Notes |
|------|--------|-------|
| Multi-chain support | ⬜ Not Started | Base, Ethereum |
| Fiat on/off ramp (Bridge) | 🟡 Started | Banking page exists |
| KYC/AML integration | ⬜ Not Started | Third-party |
| Accreditation verification | 🟡 Schema ready | Integration needed |
| Corporate actions (splits/dividends) | ✅ Complete | Tested |
| Vesting schedules | ✅ Complete | 29 tests |
| Volume limits | ✅ Complete | 36 tests |

### 13. Testing & QA

| Task | Status | Notes |
|------|--------|-------|
| Unit test coverage >80% | 🟡 Partial | Focus on critical paths |
| E2E test suite complete | 🟡 Partial | Happy paths covered |
| Load testing | ⬜ Not Started | k6 or Artillery |
| Security audit (external) | ⬜ Not Started | Budget TBD |
| Bug bounty program | ⬜ Not Started | Post-launch |
| Test harness data | 🟡 Partial | Scenario prep done |

---

## Pre-Launch Checklist (Final Week)

### Environment Verification

- [ ] All environment variables set for production
- [ ] Database migrations applied to production
- [ ] Seed data applied (templates, workflows)
- [ ] RPC endpoints verified and rate limits checked
- [ ] Bundler configured and funded for mainnet
- [ ] Subgraph fully synced

### Smoke Tests

- [ ] Create a passkey wallet
- [ ] Create an entity account
- [ ] Deploy a test token (Common Stock)
- [ ] Issue shares to a test holder
- [ ] Create an offering
- [ ] Upload and sign a document
- [ ] Create a promissory note
- [ ] Transfer tokens between wallets

### Communications

- [ ] Launch announcement prepared
- [ ] Social media posts scheduled
- [ ] Email to waitlist/beta users drafted
- [ ] Support channels ready (email, Discord?)
- [ ] Status page configured (statuspage.io?)

### Rollback Plan

- [ ] Previous subgraph version tagged
- [ ] Database snapshot taken
- [ ] Previous interface deployment available
- [ ] Rollback runbook documented

---

## Post-Launch (First 30 Days)

### Week 1: Stabilization

- [ ] Monitor error rates and performance
- [ ] Address critical bugs immediately
- [ ] Gather initial user feedback
- [ ] Daily standup on launch issues

### Week 2-4: Iteration

- [ ] Prioritize bug fixes vs. new features
- [ ] Onboard first real users/issuers
- [ ] Collect testimonials
- [ ] Plan Phase 2 features

### Ongoing

- [ ] Weekly release cadence
- [ ] Monthly security review
- [ ] Quarterly external audit (budget permitting)
- [ ] Community building

---

## Risk Register

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Smart contract vulnerability | Low | Critical | External audit, bug bounty |
| Subgraph sync issues | Medium | High | Grafting, backup indexer |
| RPC rate limits | Medium | Medium | Multiple providers, fallback |
| Regulatory action | Low | Critical | Legal counsel, compliance focus |
| Key person dependency | Medium | Medium | Documentation, knowledge sharing |
| Bundler downtime | Low | High | Self-hosted backup |

---

## Decision Log

| Date | Decision | Rationale |
|------|----------|-----------|
| 2025-12-23 | Launch with promissory notes | 71 tests passing, comprehensive coverage |
| 2026-01-20 | Delaware LP for fund | Standard structure, counsel familiar |
| 2026-01-26 | Self-describing signatures | Template-driven for flexibility |

---

## Contacts

| Role | Name | Contact |
|------|------|---------|
| Technical Lead | - | - |
| Legal Counsel | TBD | - |
| Security Auditor | TBD | - |
| Infrastructure | - | - |

---

## Appendix: Repository Structure

```
capsign/
├── protocol/        # Solidity smart contracts (Diamond pattern)
├── interface/       # Next.js web application
├── subgraph/        # TheGraph indexer
├── marketing/       # Marketing website
├── sdk/             # TypeScript SDK
├── alto/            # ERC-4337 bundler (fork)
├── test-harness/    # Test data generation
├── docs/            # Technical documentation
└── fund-docs/       # California Capital fund docs
```

---

*This is a living document. Update as items are completed or priorities change.*
