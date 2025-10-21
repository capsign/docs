# Offerings

Investment offerings on CapSign allow issuers to raise capital by selling securities tokens with built-in compliance.

## What is an Offering?

An **offering** is a structured way to sell securities to investors with:

- **Token to sell** - The security you're offering
- **Pricing** - Price per token, minimum investment, maximum raise
- **Compliance rules** - Reg D (506b/506c), Reg S, Reg A+, etc.
- **Payment processing** - Accept USDC payments
- **Investor management** - Track investments and issue tokens

## Key Features

### Compliance Presets

Choose from common securities exemptions:

- **Reg D 506(b)** - Up to 35 non-accredited + unlimited accredited, no general solicitation
- **Reg D 506(c)** - Accredited investors only, general solicitation allowed
- **Reg S** - Non-US investors only
- **Reg A+** - Up to $75M, mini-IPO alternative
- **Reg D (General)** - General Reg D offering

Each preset includes appropriate compliance modules.

### Hybrid Escrow

Investor protection + issuer control:

- **Investor pays** → Funds held in offering contract
- **Issuer countersigns** → Funds released to issuer
- **Investor can claim refund** if issuer doesn't countersign within grace period

### Automated Token Issuance

When investment is countersigned:

- **Tokens automatically issued** to investor
- **Lot created** with investment details
- **Cap table updated** automatically

### Built-in Compliance

Compliance modules check:

- **KYC verification** - Investor has completed identity verification
- **Accreditation** - Investor has accreditation attestation (if required)
- **Investor limits** - e.g., max 99 investors for 506(b)
- **Document signing** - Investor signed subscription agreement
- **Whitelist** - Investor on issuer's approved list (optional)

## For Investors

### Finding Offerings

Browse available offerings:

1. Navigate to **Offerings**
2. Filter by:
   - Asset type
   - Minimum investment
   - Offering type
3. Click to view details

### Viewing Offering Details

Each offering shows:

- **Token information** - What security you're buying
- **Price per token** - Investment terms
- **Minimum investment** - Smallest amount you can invest
- **Maximum raise** - Total offering size
- **Deadline** - When offering closes
- **Compliance requirements** - What you need (KYC, accreditation, etc.)
- **Documents** - Legal documents to review
- **Issuer information** - Who's raising capital

### Investing

To invest in an offering:

1. **Complete prerequisites** (KYC, accreditation if needed)
2. **Navigate to offering**
3. **Click "Invest"**
4. **Enter amount** (in USDC)
5. **Review and sign** subscription agreement
6. **Submit payment**

Learn more: [Investing in Offerings](investing.md)

## For Issuers

### Creating Offerings

To create an offering:

1. **Create token first** (if you haven't)
2. Navigate to **Offerings** → **Create Offering**
3. **Select token** to offer
4. **Configure pricing** (price, min investment, max raise)
5. **Choose compliance preset** (506b, 506c, etc.)
6. **Add promotional image** (optional)
7. **Deploy offering**

Learn more: [Creating an Offering](creating-an-offering.md)

### Managing Investments

After creating an offering:

- **View investments** - See all pending and accepted investments
- **Countersign investments** - Accept investor payments
- **Reject investments** - Refund investors if not qualified
- **Issue tokens** - Automatically issued upon countersigning
- **Close offering** - Mark as completed

### Raising Capital

Typical workflow:

1. **Create token** - Define your security
2. **Create offering** - Set terms and compliance
3. **Market offering** - Share with investors (if allowed by exemption)
4. **Receive investments** - Investors submit funds
5. **Review investors** - Verify qualifications
6. **Countersign** - Accept qualified investments
7. **Tokens issued automatically**
8. **Close offering** - When raise is complete

## Compliance Options

### Reg D 506(b)

**For:** Private offerings, relationship-based fundraising

**Features:**
- Up to 35 non-accredited investors
- Unlimited accredited investors
- No general solicitation
- No advertising

**Compliance modules:**
- KYC verification
- Document signing
- Investor limits
- Optional accreditation

Learn more: [Compliance Options](compliance-options.md)

### Reg D 506(c)

**For:** Private offerings with marketing

**Features:**
- Accredited investors only
- General solicitation allowed
- Can advertise
- Must verify accreditation

**Compliance modules:**
- KYC verification
- Accreditation verification (required)
- Document signing

### Reg S

**For:** Non-US investors

**Features:**
- Sold to non-US persons only
- No US registration required
- Category 2 or 3 typically

**Compliance modules:**
- KYC verification
- Jurisdiction checking
- Document signing

### Reg A+

**For:** Mini-IPO alternative

**Features:**
- Up to $75M per year
- Tier 1 (<$20M) or Tier 2 ($20-75M)
- Can solicit all investors
- SEC qualification required

**Compliance modules:**
- KYC verification
- Investment limits (Tier 2 non-accredited)
- Document signing

## Offering Lifecycle

### 1. Draft

Offering created but not active yet.

**Actions:**
- Configure settings
- Add documents
- Test investment flow

### 2. Active

Offering open for investments.

**Actions:**
- Receive investments
- Countersign investments
- Manage investors

### 3. Completed

Offering successfully closed.

**Actions:**
- View final cap table
- Export investor list
- Archive offering

### 4. Cancelled

Offering cancelled before completion.

**Actions:**
- Refund all investors
- Archive offering

## Payment Processing

### Accepting Payments

Offerings accept **USDC** (stablecoin):

- Investor pays in USDC
- Funds held in offering contract
- Released upon countersigning
- No chargebacks or reversals

### Why USDC?

- **Stable value** - Pegged to US dollar
- **Instant settlement** - No bank delays
- **Low fees** - Minimal blockchain fees
- **Programmable** - Smart contract compatible

### Receiving Funds

When you countersign:

- USDC transferred to your wallet
- You can:
  - Hold as USDC
  - Swap to ETH or other tokens
  - Off-ramp to bank account (via exchanges)

## Technical Details

### Offering Contract

Each offering is a **diamond contract** with facets:

- **OfferingCore** - Core offering logic
- **OfferingCompliance** - Compliance checks
- **OfferingDocuments** - Document management
- **DiamondCut** - Upgradeability
- **DiamondLoupe** - Introspection
- **AccessControl** - Permissions

### Deployed via OfferingFactory

Offerings are deployed via `OfferingFactory.createOffering()`:

```solidity
function createOffering(
  OfferingConfig memory config,
  FacetCut[] memory facetCuts,
  address init,
  bytes memory initData,
  ComplianceModule[] memory complianceModules
) external returns (
  address offeringDiamond,
  address[] memory deployedModules,
  string[] memory moduleNames
);
```

Learn more: [Offering Architecture](/protocol/offerings.md)

## Best Practices

### Legal Compliance

- **Consult securities attorney** before creating offering
- **File required paperwork** (Form D, offering statement, etc.)
- **Maintain investor records** for audits
- **Follow chosen exemption** strictly

### Investor Relations

- **Clear communication** - Explain terms clearly
- **Responsive** - Answer investor questions promptly
- **Professional** - Maintain professional documentation
- **Transparent** - Provide regular updates

### Risk Management

- **KYC all investors** - Even if not required, good practice
- **Document everything** - Keep records of all communications
- **Use escrow** - Don't accept wire transfers directly
- **Review carefully** - Check investor qualifications before countersigning

## FAQs

**Q: Do I need to file paperwork with SEC?**
A: Depends on exemption. Reg D requires Form D filing within 15 days. Consult your attorney.

**Q: Can I raise an unlimited amount?**
A: Depends on exemption. 506(b)/506(c) have no limit. Reg A+ caps at $75M/year.

**Q: What if an investor doesn't qualify?**
A: Reject the investment and refund them through the platform.

**Q: Can I accept investors from other countries?**
A: Reg S is specifically for non-US investors. Other exemptions may allow foreign investors - check with your attorney.

**Q: What if offering doesn't fill?**
A: You can keep it open past deadline, close early, or cancel and refund all investors.

**Q: Can I do multiple offerings?**
A: Yes, create separate offerings for different rounds or securities.

## Guides

- [Creating an Offering](creating-an-offering.md) - Step-by-step guide
- [Investing in Offerings](investing.md) - How to invest
- [Compliance Options](compliance-options.md) - Detailed compliance info

## Need Help?

- **Legal Questions:** Consult a securities attorney
- **Technical Support:** support@capsign.com
- **Twitter:** [@CapSignInc](https://twitter.com/CapSignInc)

