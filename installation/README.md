# Installation Guide

Complete setup instructions for deploying CapSign blockchain infrastructure.

## 🚀 Quick Setup

New to CapSign? Start here:

1. **[Repository Setup](setup.md)** - Create GitHub repositories from workspace
2. **[Secrets Configuration](secrets.md)** - Set up GitHub secrets for CI/CD
3. **[Quick Start Guide](../quickstart/README.md)** - Deploy infrastructure in 30 minutes

## 📋 Prerequisites

Before beginning, ensure you have:

- **AWS Account** with billing enabled
- **GitHub Account** with organization access
- **Domain name** (optional, for custom endpoints)
- **Required CLI tools**: terraform, helm, kubectl, aws-cli

### Tool Installation

```bash
# macOS
brew install terraform helm kubectl awscli

# Ubuntu/Debian
apt-get install terraform helm kubectl awscli

# Windows
choco install terraform helm kubectl awscli
```

### AWS Credentials

Set up AWS access using one of these methods:

**Option A: AWS CLI (Quick)**

```bash
aws configure
# Enter your Access Key ID, Secret Access Key, and region
```

**Option B: Environment Variables (CI/CD)**

```bash
export AWS_ACCESS_KEY_ID="your-access-key"
export AWS_SECRET_ACCESS_KEY="your-secret-key"
export AWS_DEFAULT_REGION="us-west-2"
```

**Option C: GitHub OIDC (Recommended for CI/CD)**
See our [secrets configuration guide](secrets.md) for OIDC setup.

## 🎯 What You'll Deploy

Following these guides will give you:

- ✅ **AWS EKS cluster** with auto-scaling
- ✅ **Reth Ethereum client** (latest stable)
- ✅ **Prysm consensus client** with validator support
- ✅ **Monitoring stack** (Prometheus, Grafana, AlertManager)
- ✅ **Production security** with encrypted storage and RBAC
- ✅ **Cost optimization** with Spot instances and auto-scaling
- ✅ **CI/CD automation** with GitHub Actions

## 🔗 Next Steps

1. **[Set up repositories](setup.md)** - Create GitHub repos with proper structure
2. **[Configure secrets](secrets.md)** - Essential for CI/CD workflows
3. **[Deploy infrastructure](../quickstart/README.md)** - Launch your blockchain infrastructure

## 🆘 Need Help?

- **[FAQ](../help/faq.md)** - Common questions and solutions
- **[Discord Community](https://discord.gg/gSmnZ9wmNv)** - Real-time support
- **[GitHub Issues](https://github.com/capsign/infrastructure/issues)** - Bug reports

---

**Ready to get started?** Begin with [repository setup](setup.md)!
