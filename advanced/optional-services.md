# Optional Third-Party Services

CapSign infrastructure includes several **optional** third-party services that enhance your workflow but aren't required for core functionality. Here's what they do and how much they cost.

## 💰 Infracost - Cost Estimation

### What it does:

- Shows **cost estimates** for Terraform changes directly in pull requests
- Helps prevent expensive infrastructure mistakes before deployment
- Provides detailed cost breakdowns and monthly/annual projections
- Tracks cost changes over time

### Example output in PR:

```
📊 Monthly cost change for capsign/infrastructure
Amount:  +$245 ($120 → $365)

+ aws_rds_instance.production
  +$180/month (db.r5.large)

+ aws_eks_node_group.workers
  +$65/month (2x m5.large)
```

### Pricing:

- **Free tier**: Up to 1,000 runs per month
- **Paid tier**: $1,000/month (includes 10 users, policies, reporting)
- **Perfect for**: Teams wanting to control cloud costs proactively

### SOC Compliance:

- ✅ **SOC 2 Type II certified** - helps with your own SOC certification
- ✅ **Audit trails** and detailed logging for compliance reports
- ✅ **Enterprise-grade security** controls and monitoring

### Setup:

1. **Free API Key**: Run `infracost auth login` (no credit card required)
2. **Add secrets**: `INFRACOST_API_KEY` + `INFRACOST_ENABLED=true`
3. **Cost estimates appear in every PR automatically**

---

## 🛡️ FOSSA - License Compliance

### What it does:

- Scans your dependencies for **license compliance** issues
- Identifies security vulnerabilities in open source packages
- Generates Software Bill of Materials (SBOM) for regulatory compliance
- Tracks license changes over time

### Example alerts:

```
⚠️ GPL-3.0 license detected in production dependency
⚠️ High severity vulnerability in express@4.16.1
✅ 284 dependencies scanned, 2 issues found
```

### Pricing:

- **Free tier**: 5 projects, 10 developers, basic scanning
- **Business tier**: $20/project/month (minimum $200/month)
- **Enterprise tier**: Custom pricing for large organizations
- **Perfect for**: Companies needing license compliance for legal/regulatory reasons

### SOC Compliance:

- ✅ **SOC 2 Type II certified** - essential for your SOC certification
- ✅ **SBOM generation** for regulatory compliance and audit requirements
- ✅ **Comprehensive audit trails** and compliance reporting
- ✅ **Enterprise SSO** and access controls for SOC requirements

### When you need it:

- 🏢 **Enterprise customers** requiring license compliance
- ⚖️ **Legal requirements** for open source tracking
- 🔍 **M&A due diligence** requiring SBOM generation
- 📋 **Regulatory compliance** (e.g., medical devices, aerospace)

### Setup:

1. **Create account** at [fossa.com](https://fossa.com)
2. **Add secrets**: `FOSSA_API_KEY` + `FOSSA_ENABLED=true`
3. **License scanning runs automatically in CI/CD**

---

## 🎯 Do You Need These Services?

### ✅ **Start with these enabled:**

- **Checkov** (free security scanning)
- **AWS native security** (free)
- **GitHub security features** (free)

### 📈 **Add Infracost if:**

- Your team frequently deploys expensive resources
- You want to prevent cost surprises in production
- FinOps/cost management is important to your organization
- You're running multiple environments (dev/staging/prod)
- **SOC certification** is a requirement (Infracost is SOC 2 certified)

### 🏢 **Add FOSSA if:**

- You're an enterprise with legal compliance requirements
- You need to track open source license obligations
- You're preparing for audits or M&A due diligence
- You have customers requiring SBOM documentation
- **SOC certification** is a requirement (FOSSA is SOC 2 certified)

### 💡 **Skip these if:**

- You're just getting started with infrastructure
- You're a small team without compliance requirements
- You prefer to manage costs and licenses manually
- Budget is a primary concern

---

## 🔧 How to Enable/Disable

### Enable a service:

```bash
# In repository settings → Variables → Actions
INFRACOST_ENABLED=true
FOSSA_ENABLED=true

# In repository settings → Secrets → Actions
INFRACOST_API_KEY=ico-xxx...
FOSSA_API_KEY=xxx...
```

### Disable a service:

```bash
# Set to false or remove the variable
INFRACOST_ENABLED=false
FOSSA_ENABLED=false

# Remove the API key secret (optional)
```

### Test before enabling:

```bash
# Test Infracost locally
infracost breakdown --path .

# Test FOSSA locally
fossa analyze && fossa test
```

---

## 🆓 Free Alternatives

### For Cost Management:

- **AWS Cost Explorer** (free, but post-deployment)
- **Terraform plan output** (basic resource counts)
- **Manual cost calculation** (time-intensive)
- **AWS Pricing Calculator** (manual)

### For License Compliance:

- **GitHub Dependency Graph** (basic vulnerability scanning)
- **Snyk** (free tier available)
- **Manual license review** (time-intensive)
- **SPDX tools** (open source SBOM generation)

---

## 📊 Cost Comparison

| Service            | Free Tier     | Paid Tier     | Annual Cost (Small Team) |
| ------------------ | ------------- | ------------- | ------------------------ |
| **Infracost**      | 1K runs/month | $1K/month     | $12,000                  |
| **FOSSA**          | 5 projects    | $200+/month   | $2,400+                  |
| **Manual Process** | $0            | Engineer time | $20,000+                 |

**Note**: While these services have subscription costs, they often save significantly more in prevented mistakes, compliance issues, and engineer time.

---

## 🚀 Recommendation

### **For teams pursuing SOC certification:**

1. ✅ **Required**: Both Infracost and FOSSA (both are SOC 2 Type II certified)
2. ✅ **Enable**: All core security scanning (free)
3. ✅ **Configure**: Proper audit trails and compliance reporting

### **For most teams starting out:**

1. ✅ **Enable**: Core security scanning (free)
2. 📈 **Consider**: Infracost if managing significant cloud spend
3. 🏢 **Evaluate**: FOSSA if you have enterprise compliance needs

**You can always add these services later as your needs grow!**

For more information on SOC compliance requirements, see our [SOC Compliance Guide](../security/soc.md).


CapSign infrastructure includes several **optional** third-party services that enhance your workflow but aren't required for core functionality. Here's what they do and how much they cost.

## 💰 Infracost - Cost Estimation

### What it does:

- Shows **cost estimates** for Terraform changes directly in pull requests
- Helps prevent expensive infrastructure mistakes before deployment
- Provides detailed cost breakdowns and monthly/annual projections
- Tracks cost changes over time

### Example output in PR:

```
📊 Monthly cost change for capsign/infrastructure
Amount:  +$245 ($120 → $365)

+ aws_rds_instance.production
  +$180/month (db.r5.large)

+ aws_eks_node_group.workers
  +$65/month (2x m5.large)
```

### Pricing:

- **Free tier**: Up to 1,000 runs per month
- **Paid tier**: $1,000/month (includes 10 users, policies, reporting)
- **Perfect for**: Teams wanting to control cloud costs proactively

### SOC Compliance:

- ✅ **SOC 2 Type II certified** - helps with your own SOC certification
- ✅ **Audit trails** and detailed logging for compliance reports
- ✅ **Enterprise-grade security** controls and monitoring

### Setup:

1. **Free API Key**: Run `infracost auth login` (no credit card required)
2. **Add secrets**: `INFRACOST_API_KEY` + `INFRACOST_ENABLED=true`
3. **Cost estimates appear in every PR automatically**

---

## 🛡️ FOSSA - License Compliance

### What it does:

- Scans your dependencies for **license compliance** issues
- Identifies security vulnerabilities in open source packages
- Generates Software Bill of Materials (SBOM) for regulatory compliance
- Tracks license changes over time

### Example alerts:

```
⚠️ GPL-3.0 license detected in production dependency
⚠️ High severity vulnerability in express@4.16.1
✅ 284 dependencies scanned, 2 issues found
```

### Pricing:

- **Free tier**: 5 projects, 10 developers, basic scanning
- **Business tier**: $20/project/month (minimum $200/month)
- **Enterprise tier**: Custom pricing for large organizations
- **Perfect for**: Companies needing license compliance for legal/regulatory reasons

### SOC Compliance:

- ✅ **SOC 2 Type II certified** - essential for your SOC certification
- ✅ **SBOM generation** for regulatory compliance and audit requirements
- ✅ **Comprehensive audit trails** and compliance reporting
- ✅ **Enterprise SSO** and access controls for SOC requirements

### When you need it:

- 🏢 **Enterprise customers** requiring license compliance
- ⚖️ **Legal requirements** for open source tracking
- 🔍 **M&A due diligence** requiring SBOM generation
- 📋 **Regulatory compliance** (e.g., medical devices, aerospace)

### Setup:

1. **Create account** at [fossa.com](https://fossa.com)
2. **Add secrets**: `FOSSA_API_KEY` + `FOSSA_ENABLED=true`
3. **License scanning runs automatically in CI/CD**

---

## 🎯 Do You Need These Services?

### ✅ **Start with these enabled:**

- **Checkov** (free security scanning)
- **AWS native security** (free)
- **GitHub security features** (free)

### 📈 **Add Infracost if:**

- Your team frequently deploys expensive resources
- You want to prevent cost surprises in production
- FinOps/cost management is important to your organization
- You're running multiple environments (dev/staging/prod)
- **SOC certification** is a requirement (Infracost is SOC 2 certified)

### 🏢 **Add FOSSA if:**

- You're an enterprise with legal compliance requirements
- You need to track open source license obligations
- You're preparing for audits or M&A due diligence
- You have customers requiring SBOM documentation
- **SOC certification** is a requirement (FOSSA is SOC 2 certified)

### 💡 **Skip these if:**

- You're just getting started with infrastructure
- You're a small team without compliance requirements
- You prefer to manage costs and licenses manually
- Budget is a primary concern

---

## 🔧 How to Enable/Disable

### Enable a service:

```bash
# In repository settings → Variables → Actions
INFRACOST_ENABLED=true
FOSSA_ENABLED=true

# In repository settings → Secrets → Actions
INFRACOST_API_KEY=ico-xxx...
FOSSA_API_KEY=xxx...
```

### Disable a service:

```bash
# Set to false or remove the variable
INFRACOST_ENABLED=false
FOSSA_ENABLED=false

# Remove the API key secret (optional)
```

### Test before enabling:

```bash
# Test Infracost locally
infracost breakdown --path .

# Test FOSSA locally
fossa analyze && fossa test
```

---

## 🆓 Free Alternatives

### For Cost Management:

- **AWS Cost Explorer** (free, but post-deployment)
- **Terraform plan output** (basic resource counts)
- **Manual cost calculation** (time-intensive)
- **AWS Pricing Calculator** (manual)

### For License Compliance:

- **GitHub Dependency Graph** (basic vulnerability scanning)
- **Snyk** (free tier available)
- **Manual license review** (time-intensive)
- **SPDX tools** (open source SBOM generation)

---

## 📊 Cost Comparison

| Service            | Free Tier     | Paid Tier     | Annual Cost (Small Team) |
| ------------------ | ------------- | ------------- | ------------------------ |
| **Infracost**      | 1K runs/month | $1K/month     | $12,000                  |
| **FOSSA**          | 5 projects    | $200+/month   | $2,400+                  |
| **Manual Process** | $0            | Engineer time | $20,000+                 |

**Note**: While these services have subscription costs, they often save significantly more in prevented mistakes, compliance issues, and engineer time.

---

## 🚀 Recommendation

### **For teams pursuing SOC certification:**

1. ✅ **Required**: Both Infracost and FOSSA (both are SOC 2 Type II certified)
2. ✅ **Enable**: All core security scanning (free)
3. ✅ **Configure**: Proper audit trails and compliance reporting

### **For most teams starting out:**

1. ✅ **Enable**: Core security scanning (free)
2. 📈 **Consider**: Infracost if managing significant cloud spend
3. 🏢 **Evaluate**: FOSSA if you have enterprise compliance needs

**You can always add these services later as your needs grow!**

For more information on SOC compliance requirements, see our [SOC Compliance Guide](../security/soc.md).
