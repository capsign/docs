# Frequently Asked Questions

## 🚀 Getting Started

### What is CapSign?

CapSign is a **capital markets blockchain ecosystem** consisting of:

- **🏦 CMX Protocol** - Smart contracts for capital markets operations
- **🌐 CMX Network** - Optimism-based L2 with CMX Protocol preinstalled
- **💎 CMX Token** - Native asset for seamless capital markets transactions
- **🏗️ Infrastructure Tools** - Open-source deployment tools for running the ecosystem

Our open-source Infrastructure as Code (Terraform) and Kubernetes Helm charts make it easy to deploy and manage CMX Protocol infrastructure on AWS.

### How long does deployment take?

A complete deployment typically takes **20-30 minutes**:

- Infrastructure setup (Terraform): ~15 minutes
- Blockchain stack deployment (Helm): ~10 minutes
- Initial blockchain sync: 2-24 hours (depending on sync method)

### What does it cost to run?

**Typical monthly costs:**

- **Small setup**: ~$200-400/month (development/testing)
- **Production setup**: ~$500-1500/month (high availability)
- **Enterprise setup**: $1500+/month (multi-region, compliance)

Use our [cost calculator](/advanced/cost-optimization.md) for detailed estimates.

### Can I run this locally?

Yes! We provide:

- **[Local development guide](/tutorials/local-dev.md)** for testing
- **Kind/minikube** configurations for laptop development
- **Docker Compose** stacks for quick prototyping

---

## 🏗️ Infrastructure

### Which cloud providers are supported?

Currently supported:

- ✅ **AWS** (fully supported)
- 🚧 **Azure** (coming Q2 2024)
- 🚧 **GCP** (coming Q3 2024)
- 🚧 **Self-hosted** (coming Q4 2024)

### Can I deploy to existing infrastructure?

Yes! Our Terraform modules can be integrated into existing AWS infrastructure:

- Use existing VPC and subnets
- Integrate with existing monitoring
- Connect to existing databases
- Use existing security groups

See our [custom deployment guide](/advanced/custom.md).

### What about compliance (SOC 2, PCI, etc.)?

CapSign infrastructure is designed for compliance:

- **SOC 2 Type II** controls built-in
- **PCI DSS** compatible configurations
- **GDPR** data protection features
- **HIPAA** security controls available

Read our [compliance guide](/security/soc.md) for details.

---

## ⚡ Blockchain

### Which blockchain networks are supported?

**Currently supported:**

- ✅ **Ethereum Mainnet** (Reth + Prysm)
- ✅ **Ethereum Testnets** (Goerli, Sepolia)
- ✅ **Optimism Mainnet** (full L2 stack)
- ✅ **Optimism Testnets** (Goerli, Sepolia)

**Coming soon:**

- 🚧 **Arbitrum** (Q2 2024)
- 🚧 **Polygon** (Q2 2024)
- 🚧 **Base** (Q3 2024)

### How long does initial sync take?

**Ethereum mainnet sync times:**

- **Snap sync**: 4-12 hours (recommended)
- **Fast sync**: 12-24 hours
- **Full sync**: 3-7 days (not recommended)

**Optimism sync times:**

- **L2 sync**: 2-6 hours (depends on L1 sync)

Use our [sync optimization guide](/blockchain/sync.md) for fastest sync.

### Can I run validators?

Yes! We provide:

- **[Validator setup guide](/blockchain/validators.md)** for staking
- **Automated key management** with secure storage
- **Slashing protection** and monitoring
- **MEV-Boost** integration for increased rewards

**⚠️ Warning:** Validator setup requires careful security consideration and ETH staking.

---

## 🚢 Kubernetes

### Do I need Kubernetes experience?

Not required, but helpful! We provide:

- **Automated deployment** scripts that handle everything
- **Pre-configured** security and monitoring
- **Step-by-step guides** for common operations
- **[Kubernetes basics](/kubernetes/basics.md)** tutorial

### Can I customize the Helm charts?

Absolutely! Our charts are designed for customization:

- **Values files** for each environment
- **Custom resource** configurations
- **Plugin architecture** for extensions
- **[Customization guide](/advanced/custom.md)** with examples

### What about updates and maintenance?

We provide automated update mechanisms:

- **Rolling updates** with zero downtime
- **Automated backups** before updates
- **Rollback procedures** if issues occur
- **[Maintenance guide](/monitoring/maintenance.md)** for operations

---

## 📊 Monitoring

### What monitoring tools are included?

**Included by default:**

- **Prometheus** for metrics collection
- **Grafana** for dashboards and visualization
- **AlertManager** for alerting and notifications
- **Node Exporter** for system metrics
- **Custom exporters** for blockchain metrics

### How do I set up alerts?

We provide pre-configured alerts for:

- **Node health** and connectivity
- **Blockchain sync** status and performance
- **Resource usage** (CPU, memory, disk)
- **Security events** and anomalies

See our [alerting guide](/monitoring/alerting.md) for custom alerts.

### Can I use external monitoring?

Yes! Integration guides available for:

- **Datadog** APM and infrastructure monitoring
- **New Relic** application performance monitoring
- **Splunk** log aggregation and analysis
- **PagerDuty** incident management

---

## 🔐 Security

### How secure is the default configuration?

Very secure! We implement security best practices by default:

- **Network isolation** with VPC and security groups
- **Encryption at rest** for all data storage
- **RBAC controls** for Kubernetes access
- **Regular security** scanning and updates

### Can I customize security settings?

Yes! Our [security hardening guide](/security/hardening.md) covers:

- **Additional encryption** options
- **Custom firewall** rules
- **Advanced authentication** methods
- **Compliance** configurations

### What about secrets management?

We use multiple layers of secrets protection:

- **AWS Secrets Manager** for cloud credentials
- **Kubernetes Secrets** for application secrets
- **Sealed Secrets** for GitOps workflows
- **External secret** operators for enterprise systems

---

## 💰 Cost Management

### How can I reduce costs?

**Cost optimization strategies:**

- **Spot instances** for 60-70% savings on compute
- **Auto-scaling** to reduce idle resources
- **Reserved instances** for predictable workloads
- **Storage optimization** with efficient data management

See our [cost optimization guide](/advanced/cost-optimization.md).

### Do you provide cost monitoring?

Yes! We integrate with:

- **AWS Cost Explorer** for detailed billing analysis
- **Infracost** for Terraform cost estimation
- **Custom dashboards** for real-time cost tracking
- **Budget alerts** for spend management

### What about data transfer costs?

Blockchain nodes can generate significant data transfer:

- **Optimize peering** to reduce external bandwidth
- **Use CloudFront** for static content delivery
- **Configure sync** settings to minimize transfers
- **Monitor usage** with detailed metrics

---

## 🆘 Support

### Where can I get help?

**Community support:**

- **[Discord community](https://discord.gg/gSmnZ9wmNv)** for real-time chat
- **[GitHub issues](https://github.com/capsign/infrastructure/issues)** for bug reports
- **[Documentation](/help/common-issues.md)** for troubleshooting guides

**Enterprise support:**

- **Email support** for commercial users
- **SLA-backed** response times
- **Custom consulting** for complex deployments
- **Training programs** for your team

### How do I report security issues?

**For security vulnerabilities:**

- **DO NOT** open public GitHub issues
- **Email** security@capsign.io with details
- **Use our** [security policy](/security/reporting.md) guidelines
- **Expect** acknowledgment within 24 hours

### Can I contribute to the project?

We welcome contributions! Ways to help:

- **Code contributions** via GitHub pull requests
- **Documentation** improvements and translations
- **Testing** new features and reporting bugs
- **Community support** in Discord and forums

See our [contributing guide](/resources/contributing.md) to get started.

---

## 🔄 Updates & Roadmap

### How often are updates released?

**Release schedule:**

- **Security patches**: As needed (immediately)
- **Bug fixes**: Weekly releases
- **Feature updates**: Monthly releases
- **Major versions**: Quarterly releases

### What's on the roadmap?

**2024 priorities:**

- **Multi-cloud support** (Azure, GCP)
- **Additional blockchains** (Arbitrum, Polygon)
- **Advanced automation** and self-healing
- **Enhanced monitoring** and AI-powered insights

See our [public roadmap](/resources/roadmap.md) for details.

### How do I stay updated?

**Stay informed:**

- **[GitHub releases](https://github.com/capsign/infrastructure/releases)** for version updates
- **[Discord announcements](https://discord.gg/gSmnZ9wmNv)** for community updates
- **[Blog](https://blog.capsign.io)** for technical deep-dives
- **[Newsletter](https://capsign.io/newsletter)** for monthly summaries

---

**Still have questions?** Join our [Discord community](https://discord.gg/gSmnZ9wmNv) or check our [support options](/help/enterprise.md)!


## 🚀 Getting Started

### What is CapSign?

CapSign is a **capital markets blockchain ecosystem** consisting of:

- **🏦 CMX Protocol** - Smart contracts for capital markets operations
- **🌐 CMX Network** - Optimism-based L2 with CMX Protocol preinstalled
- **💎 CMX Token** - Native asset for seamless capital markets transactions
- **🏗️ Infrastructure Tools** - Open-source deployment tools for running the ecosystem

Our open-source Infrastructure as Code (Terraform) and Kubernetes Helm charts make it easy to deploy and manage CMX Protocol infrastructure on AWS.

### How long does deployment take?

A complete deployment typically takes **20-30 minutes**:

- Infrastructure setup (Terraform): ~15 minutes
- Blockchain stack deployment (Helm): ~10 minutes
- Initial blockchain sync: 2-24 hours (depending on sync method)

### What does it cost to run?

**Typical monthly costs:**

- **Small setup**: ~$200-400/month (development/testing)
- **Production setup**: ~$500-1500/month (high availability)
- **Enterprise setup**: $1500+/month (multi-region, compliance)

Use our [cost calculator](/advanced/cost-optimization.md) for detailed estimates.

### Can I run this locally?

Yes! We provide:

- **[Local development guide](/tutorials/local-dev.md)** for testing
- **Kind/minikube** configurations for laptop development
- **Docker Compose** stacks for quick prototyping

---

## 🏗️ Infrastructure

### Which cloud providers are supported?

Currently supported:

- ✅ **AWS** (fully supported)
- 🚧 **Azure** (coming Q2 2024)
- 🚧 **GCP** (coming Q3 2024)
- 🚧 **Self-hosted** (coming Q4 2024)

### Can I deploy to existing infrastructure?

Yes! Our Terraform modules can be integrated into existing AWS infrastructure:

- Use existing VPC and subnets
- Integrate with existing monitoring
- Connect to existing databases
- Use existing security groups

See our [custom deployment guide](/advanced/custom.md).

### What about compliance (SOC 2, PCI, etc.)?

CapSign infrastructure is designed for compliance:

- **SOC 2 Type II** controls built-in
- **PCI DSS** compatible configurations
- **GDPR** data protection features
- **HIPAA** security controls available

Read our [compliance guide](/security/soc.md) for details.

---

## ⚡ Blockchain

### Which blockchain networks are supported?

**Currently supported:**

- ✅ **Ethereum Mainnet** (Reth + Prysm)
- ✅ **Ethereum Testnets** (Goerli, Sepolia)
- ✅ **Optimism Mainnet** (full L2 stack)
- ✅ **Optimism Testnets** (Goerli, Sepolia)

**Coming soon:**

- 🚧 **Arbitrum** (Q2 2024)
- 🚧 **Polygon** (Q2 2024)
- 🚧 **Base** (Q3 2024)

### How long does initial sync take?

**Ethereum mainnet sync times:**

- **Snap sync**: 4-12 hours (recommended)
- **Fast sync**: 12-24 hours
- **Full sync**: 3-7 days (not recommended)

**Optimism sync times:**

- **L2 sync**: 2-6 hours (depends on L1 sync)

Use our [sync optimization guide](/blockchain/sync.md) for fastest sync.

### Can I run validators?

Yes! We provide:

- **[Validator setup guide](/blockchain/validators.md)** for staking
- **Automated key management** with secure storage
- **Slashing protection** and monitoring
- **MEV-Boost** integration for increased rewards

**⚠️ Warning:** Validator setup requires careful security consideration and ETH staking.

---

## 🚢 Kubernetes

### Do I need Kubernetes experience?

Not required, but helpful! We provide:

- **Automated deployment** scripts that handle everything
- **Pre-configured** security and monitoring
- **Step-by-step guides** for common operations
- **[Kubernetes basics](/kubernetes/basics.md)** tutorial

### Can I customize the Helm charts?

Absolutely! Our charts are designed for customization:

- **Values files** for each environment
- **Custom resource** configurations
- **Plugin architecture** for extensions
- **[Customization guide](/advanced/custom.md)** with examples

### What about updates and maintenance?

We provide automated update mechanisms:

- **Rolling updates** with zero downtime
- **Automated backups** before updates
- **Rollback procedures** if issues occur
- **[Maintenance guide](/monitoring/maintenance.md)** for operations

---

## 📊 Monitoring

### What monitoring tools are included?

**Included by default:**

- **Prometheus** for metrics collection
- **Grafana** for dashboards and visualization
- **AlertManager** for alerting and notifications
- **Node Exporter** for system metrics
- **Custom exporters** for blockchain metrics

### How do I set up alerts?

We provide pre-configured alerts for:

- **Node health** and connectivity
- **Blockchain sync** status and performance
- **Resource usage** (CPU, memory, disk)
- **Security events** and anomalies

See our [alerting guide](/monitoring/alerting.md) for custom alerts.

### Can I use external monitoring?

Yes! Integration guides available for:

- **Datadog** APM and infrastructure monitoring
- **New Relic** application performance monitoring
- **Splunk** log aggregation and analysis
- **PagerDuty** incident management

---

## 🔐 Security

### How secure is the default configuration?

Very secure! We implement security best practices by default:

- **Network isolation** with VPC and security groups
- **Encryption at rest** for all data storage
- **RBAC controls** for Kubernetes access
- **Regular security** scanning and updates

### Can I customize security settings?

Yes! Our [security hardening guide](/security/hardening.md) covers:

- **Additional encryption** options
- **Custom firewall** rules
- **Advanced authentication** methods
- **Compliance** configurations

### What about secrets management?

We use multiple layers of secrets protection:

- **AWS Secrets Manager** for cloud credentials
- **Kubernetes Secrets** for application secrets
- **Sealed Secrets** for GitOps workflows
- **External secret** operators for enterprise systems

---

## 💰 Cost Management

### How can I reduce costs?

**Cost optimization strategies:**

- **Spot instances** for 60-70% savings on compute
- **Auto-scaling** to reduce idle resources
- **Reserved instances** for predictable workloads
- **Storage optimization** with efficient data management

See our [cost optimization guide](/advanced/cost-optimization.md).

### Do you provide cost monitoring?

Yes! We integrate with:

- **AWS Cost Explorer** for detailed billing analysis
- **Infracost** for Terraform cost estimation
- **Custom dashboards** for real-time cost tracking
- **Budget alerts** for spend management

### What about data transfer costs?

Blockchain nodes can generate significant data transfer:

- **Optimize peering** to reduce external bandwidth
- **Use CloudFront** for static content delivery
- **Configure sync** settings to minimize transfers
- **Monitor usage** with detailed metrics

---

## 🆘 Support

### Where can I get help?

**Community support:**

- **[Discord community](https://discord.gg/gSmnZ9wmNv)** for real-time chat
- **[GitHub issues](https://github.com/capsign/infrastructure/issues)** for bug reports
- **[Documentation](/help/common-issues.md)** for troubleshooting guides

**Enterprise support:**

- **Email support** for commercial users
- **SLA-backed** response times
- **Custom consulting** for complex deployments
- **Training programs** for your team

### How do I report security issues?

**For security vulnerabilities:**

- **DO NOT** open public GitHub issues
- **Email** security@capsign.io with details
- **Use our** [security policy](/security/reporting.md) guidelines
- **Expect** acknowledgment within 24 hours

### Can I contribute to the project?

We welcome contributions! Ways to help:

- **Code contributions** via GitHub pull requests
- **Documentation** improvements and translations
- **Testing** new features and reporting bugs
- **Community support** in Discord and forums

See our [contributing guide](/resources/contributing.md) to get started.

---

## 🔄 Updates & Roadmap

### How often are updates released?

**Release schedule:**

- **Security patches**: As needed (immediately)
- **Bug fixes**: Weekly releases
- **Feature updates**: Monthly releases
- **Major versions**: Quarterly releases

### What's on the roadmap?

**2024 priorities:**

- **Multi-cloud support** (Azure, GCP)
- **Additional blockchains** (Arbitrum, Polygon)
- **Advanced automation** and self-healing
- **Enhanced monitoring** and AI-powered insights

See our [public roadmap](/resources/roadmap.md) for details.

### How do I stay updated?

**Stay informed:**

- **[GitHub releases](https://github.com/capsign/infrastructure/releases)** for version updates
- **[Discord announcements](https://discord.gg/gSmnZ9wmNv)** for community updates
- **[Blog](https://blog.capsign.io)** for technical deep-dives
- **[Newsletter](https://capsign.io/newsletter)** for monthly summaries

---

**Still have questions?** Join our [Discord community](https://discord.gg/gSmnZ9wmNv) or check our [support options](/help/enterprise.md)!
