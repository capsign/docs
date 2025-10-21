# Multi-Entity Accounts

CapSign supports managing multiple organizational contexts from a single login through **context switching**.

## What is Multi-Entity Support?

With multi-entity support, you can access multiple investment accounts from a single CapSign account:

- **Personal Account** - Your individual investments
- **Corporate Accounts** - Companies you're authorized to represent
- **Fund Accounts** - Investment funds and SPVs you manage
- **Multiple Roles** - Different roles in different organizations

**No need to log in and out** - just switch contexts from the account menu.

## How It Works

### Context Switching

Each context has its own:

- **Wallet address** - Separate smart wallet for each entity
- **Identity** - Separate legal identity (individual vs. entity)
- **Permissions** - Different capabilities in each context
- **Assets** - Separate token holdings and investments

### Account Menu

The account menu (top right of the app) shows:

- **Current context** - Which entity you're operating as
- **Available contexts** - All entities you have access to
- **Context switcher** - One-click switching between entities

## Use Cases

### 1. Fund Manager with Multiple SPVs

**Scenario:** You manage 3 SPVs and are also an LP in another fund.

**Contexts:**
1. **Personal Account** - Your individual investments
2. **SPV Alpha** (GP role) - Real estate fund
3. **SPV Beta** (GP role) - Venture capital fund
4. **SPV Gamma** (GP role) - Secondary market fund
5. **XYZ Fund** (LP role) - LP investment in another fund

**Workflow:**
- Create an offering as **SPV Alpha**
- Invest in an offering as **Personal Account**
- Review investments as **SPV Beta**
- View LP statements as **XYZ Fund**

### 2. Corporate Employee

**Scenario:** You work at a company that makes corporate investments.

**Contexts:**
1. **Personal Account** - Your individual investments
2. **Acme Corp** (Employee role) - Company investment account

**Workflow:**
- Invest personally as **Personal Account**
- Sign documents on behalf of the company as **Acme Corp**
- Review company portfolio as **Acme Corp**

### 3. Multi-Fund LP

**Scenario:** You're an LP in multiple investment funds.

**Contexts:**
1. **Personal Account** - Your individual account
2. **Fund A** (LP role) - LP in Fund A
3. **Fund B** (LP role) - LP in Fund B
4. **Fund C** (LP role) - LP in Fund C

**Workflow:**
- Make new investments as **Personal Account**
- Review Fund A performance as **Fund A**
- Sign subscription docs as **Fund B**

## Context Features

### Personal Context

**Available to:** Everyone (default)

**Capabilities:**
- Browse offerings
- Invest in offerings (with KYC)
- Hold tokens
- Sign documents
- View attestations

**Cannot:**
- Create tokens
- Create offerings
- Issue attestations

### Entity Context

**Available to:** Authorized representatives of legal entities

**Capabilities:**
- All personal context features, plus:
- Create tokens
- Create offerings
- Issue attestations (accreditation, etc.)
- Manage entity profile
- Add/remove authorized signers

**Requirements:**
- Entity must complete KYC
- You must be an authorized representative

## Setting Up Multi-Entity Access

### Adding an Entity Context

1. **Complete personal KYC** if you haven't already
2. **Entity completes KYC** through their admin
3. **Entity adds you** as an authorized signer
4. **You receive invitation** via email
5. **Accept invitation** in the app
6. **New context appears** in your account menu

### Switching Contexts

1. Click the **account menu** (top right)
2. **Select the context** you want to switch to
3. The app **reloads in that context**
4. All actions now operate as that entity

### Current Context Indicator

The current context is always displayed in the account menu and at the top of key pages:

```
🏢 Operating as: SPV Alpha
```

This helps you confirm which entity you're acting on behalf of.

## Security Considerations

### Signatures

When you sign a transaction or document in an entity context:

- The signature is **legally binding** for that entity
- You are representing **the entity, not yourself**
- Make sure you have **proper authorization**

### Wallet Ownership

- **Personal context** - You own the smart wallet (passkey-controlled)
- **Entity context** - The entity owns the wallet (EOA-controlled)
  - You sign on behalf of the entity with your EOA wallet
  - Multiple people can be authorized signers

### Permissions

Different users may have different permissions within an entity:

- **Admin** - Full control, can add/remove users
- **Signer** - Can sign documents and transactions
- **Viewer** - Read-only access to entity data

(Permission system coming soon)

## Technical Details

### How Context Switching Works

1. **Authentication** - You authenticate once with your personal passkey
2. **Session storage** - Your current context is stored in your session
3. **API calls** - All API calls include your current context
4. **Smart contract interactions** - Transactions use the context's wallet

### Wallet Types by Context

- **Personal context** - ERC-4337 smart wallet (passkey-controlled)
- **Entity context** - ERC-4337 smart wallet (EOA-controlled)
  - You sign with an EOA wallet (MetaMask, Coinbase Wallet, etc.)
  - The entity wallet then executes the transaction

Learn more: [Wallet Architecture](/protocol/wallets.md)

## Best Practices

### 1. Always Check Current Context

Before signing anything, verify you're in the correct context:

```
🏢 Operating as: [Entity Name]
```

### 2. Keep Contexts Separate

Don't mix personal and entity funds:

- Personal investments → Personal context
- Entity investments → Entity context

### 3. Maintain Proper Records

Keep records of which context you used for each action for:

- Tax purposes
- Audit trails
- Legal compliance

### 4. Use Descriptive Entity Names

When setting up entities, use clear names:

- ✅ "Acme Ventures Fund I"
- ✅ "Beta Real Estate SPV"
- ❌ "My Fund"
- ❌ "SPV 1"

## Troubleshooting

### I don't see an entity context I should have access to

1. Check that the **entity admin added you** as an authorized signer
2. **Accept the invitation** if you received an email
3. **Refresh the page** to reload contexts
4. Contact the entity admin if you still don't see it

### I'm seeing transactions I didn't make

Check the **current context** - you may be viewing an entity's transactions, not your personal transactions.

### I can't create a token in my personal context

Only **entity contexts** can create tokens. Switch to an entity context or create one.

## FAQs

**Q: How many contexts can I have?**
A: Unlimited. You can be associated with any number of entities.

**Q: Can I have multiple personal contexts?**
A: No. You have one personal context. Use entity contexts for organizational accounts.

**Q: Who owns the tokens in an entity context?**
A: The entity owns the tokens, not you personally.

**Q: What happens if I leave an organization?**
A: The entity admin removes your access, and the context disappears from your account menu.

**Q: Can I transfer tokens between contexts?**
A: Yes, but it's a transfer between two separate wallets. The sender context sends to the receiver context's wallet address.

## Need Help?

- **Email:** support@capsign.com
- **Twitter:** [@CapSignInc](https://twitter.com/CapSignInc)

