# Self-Hosted CMX Protocol

**Deploy the CMX Protocol in your own environment with complete control - from simple node deployment to enterprise-grade infrastructure.**

## What Is Self-Hosting?

Self-hosting means running the CMX Protocol stack in your own environment. You have two main options:

### 🆓 **Open Source Deployment** (Free)

Deploy the CMX Protocol however you want using our open-source components:

- **CMX geth node** from GitHub - The core blockchain client
- **Connect to Ethereum L1** - Attach to any Ethereum node
- **Your infrastructure** - Use any cloud, on-premise, or hybrid setup
- **Community support** - GitHub issues and Discord community

### 🏗️ **Recommended Architecture** (Battle-Tested)

Use CapSign's production-ready infrastructure blueprints:

- **Terraform modules** - AWS VPC, EKS, security configurations
- **Kubernetes Helm charts** - Production-ready deployment manifests
- **Monitoring stack** - Prometheus, Grafana, AlertManager
- **Security hardening** - SOC compliance configurations
- **Step-by-step guides** - From setup to production operations

## Core Requirements (All Deployments)

Regardless of how you deploy, every CMX Protocol deployment needs:

### Essential Components

1. **CMX geth node** - Download from [GitHub](https://github.com/capsign/cmx-geth)
2. **Ethereum L1 connection** - Mainnet, Sepolia, or your preferred testnet
3. **Database storage** - For blockchain state and transaction data
4. **Network connectivity** - P2P networking for blockchain sync

### Basic Architecture

```
[Your Application]
       ↓
[CMX geth node] ← Your Infrastructure Choice
       ↓
[Ethereum L1 node] ← Can be external provider
```

## Deployment Options

### Option 1: Simple Node Deployment

**Perfect for**: Development, testing, small applications

**What you need**:

- Linux server (cloud or on-premise)
- Docker or direct binary installation
- Connection to Ethereum L1 (can use Infura, Alchemy, etc.)

**Getting started**:

1. Download CMX geth from GitHub releases
2. Configure connection to Ethereum L1
3. Start the node and sync blockchain state
4. Connect your application via RPC

**Cost**: Infrastructure only (typically $50-200/month)

### Option 2: CapSign Recommended Architecture

**Perfect for**: Production applications, enterprise deployments, compliance requirements

**What you get**:

- **Production-ready Terraform** - AWS VPC, EKS, security groups, IAM
- **Kubernetes Helm charts** - Scalable, monitored, highly available
- **Security configurations** - SOC compliance, encryption, access controls
- **Monitoring & alerting** - Full observability stack
- **Documentation** - Step-by-step deployment and operations guides

**What you need**:

- AWS account and DevOps expertise
- Kubernetes and infrastructure management skills
- 2-4 weeks for initial setup and customization

**Cost**: Infrastructure only (typically $1,000-3,000/month depending on scale)

## Enterprise Support (Optional)

For organizations wanting additional support beyond community resources:

### Support Packages

**🥉 Professional Support - $500/month**

- Email support with 48-hour response SLA
- Documentation access and updates
- Best practices consultation

**🥈 Enterprise Support - $2,000/month**

- Private Slack channel with engineering team
- Priority bug fixes and feature requests
- Architecture review and optimization
- Compliance consultation (SOC, regulatory)

**🥇 Strategic Partnership - Custom Pricing**

- Dedicated customer success manager
- Custom development and integrations
- On-site training and workshops
- 24/7 incident support

## Why Use Our Recommended Architecture?

### Battle-Tested Components

Our Terraform and Kubernetes setup has been proven in production with:

- **99.99% uptime** across customer deployments
- **SOC 2 Type II compliance** ready configurations
- **Security best practices** built-in from day one
- **Cost optimization** through right-sizing and automation

### Enterprise Features

- **High availability** across multiple AWS availability zones
- **Auto-scaling** for both infrastructure and blockchain nodes
- **Monitoring & alerting** with Prometheus and Grafana
- **Backup & disaster recovery** automated procedures
- **Security scanning** with Checkov and other tools

### Operational Excellence

- **Infrastructure as Code** - All changes tracked and versioned
- **CI/CD integration** - Automated testing and deployment
- **Secrets management** - Secure handling of keys and credentials
- **Documentation** - Comprehensive guides for all operations

## Getting Started

### 🚀 Quick Start (Open Source)

1. **Download CMX geth**: Visit [GitHub releases](https://github.com/capsign/cmx-geth/releases)
2. **Configure L1 connection**: Point to your Ethereum node or provider
3. **Start the node**: Follow README instructions
4. **Connect your app**: Use standard JSON-RPC interface

### 🏗️ Production Deployment (Recommended Architecture)

1. **[Prerequisites](/infrastructure/prerequisites.md)** - AWS account, tools setup
2. **[Quick Start Guide](/infrastructure/quickstart.md)** - 30-minute deployment overview
3. **[Repository Setup](/infrastructure/setup.md)** - Fork our templates
4. **[Secrets Configuration](/infrastructure/secrets.md)** - Configure credentials
5. **[Full Installation](/infrastructure/installation.md)** - Complete production setup

### 💬 Get Support

**Community Support (Free)**:

- [GitHub Issues](https://github.com/capsign/cmx-geth/issues) - Bug reports and features
- [Discord](https://discord.gg/capsign) - Community discussion

**Enterprise Support (Paid)**:

- [sales@capsign.com](mailto:sales@capsign.com) - Support package pricing
- [enterprise@capsign.com](mailto:enterprise@capsign.com) - Strategic partnerships

## Migration and Flexibility

### Start Simple, Scale Up

Many customers follow this progression:

1. **Development**: Simple node deployment for testing
2. **Staging**: Recommended architecture for pre-production
3. **Production**: Full enterprise setup with support

### Hybrid Options

- **Self-hosted development** + **Managed production**
- **Multiple regions**: Self-hosted in some, managed in others
- **Gradual migration**: Move workloads as needs change

## Compare Your Options

| Aspect                  | Simple Node   | Recommended Architecture | Managed Service |
| ----------------------- | ------------- | ------------------------ | --------------- |
| **Control**             | Complete      | Complete                 | Shared          |
| **Setup Time**          | Hours         | 2-4 weeks                | Minutes         |
| **Expertise Required**  | Basic Linux   | DevOps/Kubernetes        | None            |
| **Infrastructure Cost** | $50-200/month | $1-3K/month              | Included        |
| **Support**             | Community     | Optional paid            | Included        |
| **Compliance**          | DIY           | SOC-ready                | SOC certified   |
| **High Availability**   | DIY           | Built-in                 | Guaranteed      |

**📊 Detailed comparison**: [Self-Hosted vs Managed Analysis](/pricing/comparison.md)

---

## Ready to Deploy?

### For Simple Deployment:

👉 **[Download CMX Node from GitHub](https://github.com/capsign/cmx-node)** and follow the README

### For Production Architecture:

👉 **[Start with Infrastructure Overview](/infrastructure/README.md)** to understand the full setup

### Need Help Deciding?

👉 **[Try our Interactive Demos](/demos/README.md)** to experience CMX Protocol without any infrastructure

**The CMX Protocol gives you the flexibility to deploy however fits your needs - from simple development setups to enterprise-grade production infrastructure.**
