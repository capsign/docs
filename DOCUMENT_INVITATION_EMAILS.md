# Document Signing Invitation Email System

## Overview
Automatic email invitations are sent to signers who don't have CapSign accounts yet, guiding them through signup and document review.

---

## Email Template Features

### Professional Design
- **CapSign branded** with gradient header
- **Responsive layout** optimized for mobile and desktop
- **Clear call-to-action** button
- **Secure messaging** about blockchain verification

### Key Information Displayed
- Document title and type
- Sender name and email
- Due date (if applicable)
- Reply-to sender for questions

### Content Sections
1. **Header** - Gradient banner with document emoji
2. **Greeting** - Personalized with signer's role
3. **Document Details** - Card with key info
4. **Security Badge** - Blockchain verification message
5. **Next Steps** - Numbered instructions
6. **CTA Button** - "Sign Up & Review Document"
7. **Benefits** - Why use CapSign
8. **Help** - Contact info for sender and support
9. **Footer** - Legal links and copyright

---

## How It Works

### 1. User Fills Template
User enters signer emails on the fill page:
- `matt@capsign.com` ✅ (has account)
- `newuser@example.com` ❌ (no account)

### 2. Email Resolution
System tries to resolve each email to a wallet address:
- Check `bridgeCustomer.emailHash` (KYC users)
- Fallback to `account.email` (regular users)
- Return 404 if not found

### 3. Auto-Send Invitations
For unresolved signers:
```typescript
await fetch("/api/documents/invite", {
  method: "POST",
  body: JSON.stringify({
    signerEmail: "newuser@example.com",
    signerRole: "Party B",
    documentTitle: "CapSign License Agreement",
    documentCategory: "LICENSE_AGREEMENT"
  })
});
```

### 4. Email Delivered
Recipient receives professional HTML email with:
- Document details
- Sender information
- Sign-up link with context
- Instructions for next steps

### 5. Sign-Up Flow
Sign-up URL includes context:
```
https://app.capsign.com/signup
  ?email=newuser@example.com
  &source=document_invite
  &redirect=/documents/{id}/sign
```

After signup → redirected to document signing page

---

## File Structure

```
src/
├── lib/
│   └── email/
│       ├── index.ts                              # Exports
│       ├── claim-notification.ts                 # Token claims
│       └── document-signing-invitation.ts        # NEW: Doc invites
└── app/
    └── api/
        └── documents/
            └── invite/
                └── route.ts                      # NEW: Send invites
```

---

## API Endpoint

### `POST /api/documents/invite`

**Request:**
```json
{
  "signerEmail": "newuser@example.com",
  "signerRole": "Party B",
  "documentTitle": "License Agreement",
  "documentCategory": "LICENSE_AGREEMENT",
  "documentId": "cmj123..." // Optional
}
```

**Response:**
```json
{
  "success": true,
  "message": "Invitation sent to newuser@example.com"
}
```

**Features:**
- Authenticated (requires session)
- Gets sender info from session
- Builds context-aware sign-up URL
- Uses SendGrid for delivery
- Reply-to set to sender's email

---

## Email Content Preview

### Subject Line
```
[Sender Name] invited you to sign: [Document Title]
```

### HTML Email Preview
```
┌────────────────────────────────────────────┐
│  📝 Document Ready for Signature           │
│  (Blue gradient header)                    │
├────────────────────────────────────────────┤
│                                            │
│  Hi Party B,                               │
│                                            │
│  John Smith has invited you to review     │
│  and sign a document on CapSign.          │
│                                            │
│  ┌──────────────────────────────────────┐ │
│  │ Document:  License Agreement         │ │
│  │ Type:      License Agreement         │ │
│  │ From:      John Smith                │ │
│  └──────────────────────────────────────┘ │
│                                            │
│  🔐 Secure & On-Chain: Documents are      │
│  cryptographically signed and stored on   │
│  the blockchain...                        │
│                                            │
│  Next Steps:                              │
│  1. Create your free CapSign account      │
│  2. Verify your email address             │
│  3. Review the document                   │
│  4. Sign with your digital signature      │
│                                            │
│  ┌────────────────────────────────────┐   │
│  │  Sign Up & Review Document         │   │
│  │  (Blue button)                     │   │
│  └────────────────────────────────────┘   │
│                                            │
│  Why CapSign?                             │
│  • Blockchain-verified signatures         │
│  • Legally binding and tamper-proof       │
│  • Complete audit trail                   │
│  • No subscription required               │
│                                            │
│  Questions? Contact John Smith at         │
│  john@example.com                         │
└────────────────────────────────────────────┘
```

### Plain Text Version
Also includes a plain text version for email clients that don't support HTML.

---

## Environment Variables Required

```bash
SENDGRID_API_KEY=SG.xxx...
NEXT_PUBLIC_BASE_URL=https://app.capsign.com
```

---

## User Experience Flow

```
┌─────────────────────────────────────────────────────────────┐
│ 1. Creator fills template                                   │
│    ├─ Enter signer emails                                   │
│    └─ Submit for signing                                    │
└─────────────────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────────┐
│ 2. System resolves emails                                   │
│    ├─ ✅ matt@capsign.com → Found (wallet address)         │
│    └─ ❌ newuser@example.com → Not found                   │
└─────────────────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────────┐
│ 3. Auto-send invitations                                    │
│    └─ Email sent to newuser@example.com                     │
└─────────────────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────────┐
│ 4. Recipient receives email                                 │
│    ├─ Professional branded email                            │
│    ├─ Document details                                      │
│    ├─ Sender contact info                                   │
│    └─ Clear "Sign Up" button                                │
└─────────────────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────────┐
│ 5. User clicks sign-up link                                 │
│    └─ Redirected to signup page with:                       │
│       - Email pre-filled                                    │
│       - Source tracked (document_invite)                    │
│       - Redirect path to document                           │
└─────────────────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────────┐
│ 6. User completes signup                                    │
│    ├─ Create account                                        │
│    ├─ Verify email                                          │
│    └─ Auto-redirected to document                           │
└─────────────────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────────┐
│ 7. Review & sign document                                   │
│    ├─ View document                                         │
│    ├─ Generate EIP-712 signature                            │
│    └─ Submit to blockchain                                  │
└─────────────────────────────────────────────────────────────┘
```

---

## Toast Messages

When sending invitations:

```
⏳ "Sending invitations to 1 signer(s) who need to create accounts..."

❌ "Invitations sent! The following signers need to create CapSign 
    accounts: Party B. They will receive an email with instructions."
```

---

## Security & Privacy

### Email Tracking
- **Click tracking:** Disabled
- **Open tracking:** Disabled
- No tracking pixels or analytics

### Reply-To
- Set to document sender's email
- Recipients can reply directly to ask questions

### Data Protection
- Email only sent when user explicitly tries to send document
- No spam or marketing
- Clear sender identification

---

## Future Enhancements

### Phase 2 Ideas:
1. **Document preview in email** - Thumbnail or first page
2. **Mobile-optimized signing** - Better mobile experience
3. **Reminder emails** - If not signed after X days
4. **Multi-language support** - i18n for global users
5. **Custom branding** - White-label for enterprise
6. **SMS notifications** - Alternative to email
7. **Batch invitations** - Send to multiple docs at once
8. **Status updates** - Email when all parties sign

---

## Testing

### Test Email Sending

```bash
# In development, emails logged to console
NODE_ENV=development npm run dev

# In production, uses real SendGrid
SENDGRID_API_KEY=your_key npm start
```

### Preview Email Template

Create a preview endpoint:
```typescript
// src/app/api/email-preview/route.ts
export async function GET() {
  // Return HTML template for testing
}
```

Visit: `http://localhost:3002/api/email-preview`

---

## Success Metrics

Track in analytics:
- ✉️ Invitations sent
- 📧 Email delivery rate
- 🔗 Sign-up link clicks
- ✅ Signup completions from invites
- ✍️ Documents signed within 24h/7d/30d
- 🔄 Invitation → Signature conversion rate

---

Built with ❤️ by CapSign
