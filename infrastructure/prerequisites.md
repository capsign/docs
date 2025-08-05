# Prerequisites

Everything you need before deploying CapSign blockchain infrastructure.

## 📋 Account Requirements

### AWS Account

- **Billing enabled** with valid payment method
- **IAM permissions** to create resources (or admin access)
- **Service limits** sufficient for EKS and EC2 instances
- **Region selection** (we recommend us-west-2 or us-east-1)

### GitHub Account

- **Organization access** or ability to create repositories
- **Actions enabled** for CI/CD workflows
- **Secrets management** permissions
- **Package registry** access (for container images)

### Optional Accounts

- **Slack workspace** (for notifications)
- **Discord account** (for community support)
- **Domain registrar** (for custom domains)

## 🛠️ Required Tools

### Core Tools

```bash
# Terraform (Infrastructure as Code)
terraform --version  # Should be >= 1.5.0

# Helm (Kubernetes package manager)
helm version         # Should be >= 3.12.0

# kubectl (Kubernetes CLI)
kubectl version      # Should be >= 1.27.0

# AWS CLI (AWS management)
aws --version        # Should be >= 2.13.0
```

### Installation Commands

**macOS (Homebrew):**

```bash
brew install terraform helm kubectl awscli
```

**Ubuntu/Debian:**

```bash
# Terraform
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform

# Helm
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update && sudo apt-get install helm

# kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip && sudo ./aws/install
```

**Windows (Chocolatey):**

```bash
choco install terraform helm kubernetes-cli awscli
```

## 💳 Cost Considerations

### Typical Monthly Costs

| Setup Type           | Components                    | Estimated Cost |
| -------------------- | ----------------------------- | -------------- |
| **Development**      | 1x t3.medium, minimal storage | $50-100/month  |
| **Small Production** | 3x m5.large, HA setup         | $300-500/month |
| **Enterprise**       | Multi-AZ, dedicated nodes     | $1000+/month   |

### Cost Optimization Options

- **Spot Instances** - Save 60-70% on compute costs
- **Reserved Instances** - Save 30-60% with commitment
- **Auto-scaling** - Reduce costs during low usage
- **Storage optimization** - Use appropriate storage classes

See our [cost optimization guide](../advanced/cost-optimization.md) for detailed strategies.

## 🔒 Security Requirements

### AWS Security

- **MFA enabled** on root and IAM accounts
- **CloudTrail logging** enabled
- **IAM roles** instead of access keys (where possible)
- **VPC** with private subnets for resources

### GitHub Security

- **2FA enabled** on all accounts
- **Branch protection** rules for main branches
- **Secret scanning** enabled
- **Dependabot alerts** enabled

### Network Security

- **SSH keys** configured for Git operations
- **VPN access** for private resources (optional)
- **Firewall rules** for administrative access
- **SSL certificates** for custom domains

## 📊 Resource Limits

### AWS Service Limits

Check these limits in your AWS account:

```bash
# Check EC2 limits
aws service-quotas get-service-quota \
  --service-code ec2 \
  --quota-code L-1216C47A  # Running On-Demand instances

# Check EKS limits
aws service-quotas get-service-quota \
  --service-code eks \
  --quota-code L-1194D53C  # Clusters per account
```

**Required minimums:**

- **EC2 instances**: 10+ running instances
- **EKS clusters**: 2+ clusters (dev/prod)
- **EBS volumes**: 20+ volumes
- **VPC**: 2+ VPCs per region

### GitHub Limits

- **Repository limit**: Unlimited for organizations
- **Actions minutes**: 2000/month (free tier)
- **Package storage**: 500MB (free tier)
- **File size limit**: 100MB per file

## 🌐 Network Requirements

### Internet Connectivity

- **Stable connection** for downloading large blockchain data
- **Bandwidth**: 100+ Mbps recommended for initial sync
- **Data transfer**: Expect 100GB+ monthly for active nodes

### Firewall Rules

```bash
# Outbound (required)
443/tcp   # HTTPS (AWS API, GitHub, package downloads)
80/tcp    # HTTP (package repositories)
53/udp    # DNS resolution

# Blockchain P2P (configurable)
30303/tcp # Ethereum P2P (can be customized)
9000/tcp  # Prysm P2P (can be customized)
```

## 📚 Knowledge Requirements

### Recommended Experience

- **AWS basics** - Understanding of EC2, VPC, IAM
- **Kubernetes fundamentals** - Pods, services, deployments
- **Command line** - Comfortable with terminal/bash
- **Git/GitHub** - Basic version control knowledge

### Learning Resources

- **[AWS EKS Workshop](https://www.eksworkshop.com/)**
- **[Kubernetes Documentation](https://kubernetes.io/docs/)**
- **[Terraform Get Started](https://learn.hashicorp.com/terraform)**
- **[Helm Documentation](https://helm.sh/docs/)**

### Not Required (But Helpful)

- Blockchain/Ethereum knowledge
- Advanced Kubernetes operations
- Terraform module development
- Golang or Rust programming

## ✅ Readiness Checklist

Before proceeding to installation:

### Accounts & Access

- [ ] AWS account with billing enabled
- [ ] GitHub account with organization access
- [ ] Domain registered (optional)
- [ ] Slack workspace configured (optional)

### Tools Installed

- [ ] Terraform >= 1.5.0
- [ ] Helm >= 3.12.0
- [ ] kubectl >= 1.27.0
- [ ] AWS CLI >= 2.13.0

### Credentials Configured

- [ ] AWS credentials configured (`aws sts get-caller-identity`)
- [ ] GitHub SSH keys or personal access token
- [ ] Domain DNS access (if using custom domains)

### Resource Planning

- [ ] AWS region selected
- [ ] Instance types chosen
- [ ] Cost budget estimated
- [ ] Network architecture planned

## 🚀 Next Steps

Once all prerequisites are met:

1. **[Repository Setup](../installation/setup.md)** - Create GitHub repositories
2. **[Secrets Configuration](../installation/secrets.md)** - Configure CI/CD secrets
3. **[Quick Start Guide](../quickstart/README.md)** - Deploy your infrastructure

## 🆘 Having Issues?

**Common Problems:**

- **Tool installation failures** - Check your package manager and permissions
- **AWS credential issues** - Verify IAM permissions and MFA settings
- **Network connectivity** - Check firewall and proxy settings

**Get Help:**

- **[FAQ](../help/faq.md)** - Common questions and solutions
- **[Discord Community](https://discord.gg/gSmnZ9wmNv)** - Real-time support
- **[GitHub Issues](https://github.com/capsign/infrastructure/issues)** - Bug reports

---

**All set?** Continue to [repository setup](../installation/setup.md)!


Everything you need before deploying CapSign blockchain infrastructure.

## 📋 Account Requirements

### AWS Account

- **Billing enabled** with valid payment method
- **IAM permissions** to create resources (or admin access)
- **Service limits** sufficient for EKS and EC2 instances
- **Region selection** (we recommend us-west-2 or us-east-1)

### GitHub Account

- **Organization access** or ability to create repositories
- **Actions enabled** for CI/CD workflows
- **Secrets management** permissions
- **Package registry** access (for container images)

### Optional Accounts

- **Slack workspace** (for notifications)
- **Discord account** (for community support)
- **Domain registrar** (for custom domains)

## 🛠️ Required Tools

### Core Tools

```bash
# Terraform (Infrastructure as Code)
terraform --version  # Should be >= 1.5.0

# Helm (Kubernetes package manager)
helm version         # Should be >= 3.12.0

# kubectl (Kubernetes CLI)
kubectl version      # Should be >= 1.27.0

# AWS CLI (AWS management)
aws --version        # Should be >= 2.13.0
```

### Installation Commands

**macOS (Homebrew):**

```bash
brew install terraform helm kubectl awscli
```

**Ubuntu/Debian:**

```bash
# Terraform
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform

# Helm
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update && sudo apt-get install helm

# kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip && sudo ./aws/install
```

**Windows (Chocolatey):**

```bash
choco install terraform helm kubernetes-cli awscli
```

## 💳 Cost Considerations

### Typical Monthly Costs

| Setup Type           | Components                    | Estimated Cost |
| -------------------- | ----------------------------- | -------------- |
| **Development**      | 1x t3.medium, minimal storage | $50-100/month  |
| **Small Production** | 3x m5.large, HA setup         | $300-500/month |
| **Enterprise**       | Multi-AZ, dedicated nodes     | $1000+/month   |

### Cost Optimization Options

- **Spot Instances** - Save 60-70% on compute costs
- **Reserved Instances** - Save 30-60% with commitment
- **Auto-scaling** - Reduce costs during low usage
- **Storage optimization** - Use appropriate storage classes

See our [cost optimization guide](../advanced/cost-optimization.md) for detailed strategies.

## 🔒 Security Requirements

### AWS Security

- **MFA enabled** on root and IAM accounts
- **CloudTrail logging** enabled
- **IAM roles** instead of access keys (where possible)
- **VPC** with private subnets for resources

### GitHub Security

- **2FA enabled** on all accounts
- **Branch protection** rules for main branches
- **Secret scanning** enabled
- **Dependabot alerts** enabled

### Network Security

- **SSH keys** configured for Git operations
- **VPN access** for private resources (optional)
- **Firewall rules** for administrative access
- **SSL certificates** for custom domains

## 📊 Resource Limits

### AWS Service Limits

Check these limits in your AWS account:

```bash
# Check EC2 limits
aws service-quotas get-service-quota \
  --service-code ec2 \
  --quota-code L-1216C47A  # Running On-Demand instances

# Check EKS limits
aws service-quotas get-service-quota \
  --service-code eks \
  --quota-code L-1194D53C  # Clusters per account
```

**Required minimums:**

- **EC2 instances**: 10+ running instances
- **EKS clusters**: 2+ clusters (dev/prod)
- **EBS volumes**: 20+ volumes
- **VPC**: 2+ VPCs per region

### GitHub Limits

- **Repository limit**: Unlimited for organizations
- **Actions minutes**: 2000/month (free tier)
- **Package storage**: 500MB (free tier)
- **File size limit**: 100MB per file

## 🌐 Network Requirements

### Internet Connectivity

- **Stable connection** for downloading large blockchain data
- **Bandwidth**: 100+ Mbps recommended for initial sync
- **Data transfer**: Expect 100GB+ monthly for active nodes

### Firewall Rules

```bash
# Outbound (required)
443/tcp   # HTTPS (AWS API, GitHub, package downloads)
80/tcp    # HTTP (package repositories)
53/udp    # DNS resolution

# Blockchain P2P (configurable)
30303/tcp # Ethereum P2P (can be customized)
9000/tcp  # Prysm P2P (can be customized)
```

## 📚 Knowledge Requirements

### Recommended Experience

- **AWS basics** - Understanding of EC2, VPC, IAM
- **Kubernetes fundamentals** - Pods, services, deployments
- **Command line** - Comfortable with terminal/bash
- **Git/GitHub** - Basic version control knowledge

### Learning Resources

- **[AWS EKS Workshop](https://www.eksworkshop.com/)**
- **[Kubernetes Documentation](https://kubernetes.io/docs/)**
- **[Terraform Get Started](https://learn.hashicorp.com/terraform)**
- **[Helm Documentation](https://helm.sh/docs/)**

### Not Required (But Helpful)

- Blockchain/Ethereum knowledge
- Advanced Kubernetes operations
- Terraform module development
- Golang or Rust programming

## ✅ Readiness Checklist

Before proceeding to installation:

### Accounts & Access

- [ ] AWS account with billing enabled
- [ ] GitHub account with organization access
- [ ] Domain registered (optional)
- [ ] Slack workspace configured (optional)

### Tools Installed

- [ ] Terraform >= 1.5.0
- [ ] Helm >= 3.12.0
- [ ] kubectl >= 1.27.0
- [ ] AWS CLI >= 2.13.0

### Credentials Configured

- [ ] AWS credentials configured (`aws sts get-caller-identity`)
- [ ] GitHub SSH keys or personal access token
- [ ] Domain DNS access (if using custom domains)

### Resource Planning

- [ ] AWS region selected
- [ ] Instance types chosen
- [ ] Cost budget estimated
- [ ] Network architecture planned

## 🚀 Next Steps

Once all prerequisites are met:

1. **[Repository Setup](../installation/setup.md)** - Create GitHub repositories
2. **[Secrets Configuration](../installation/secrets.md)** - Configure CI/CD secrets
3. **[Quick Start Guide](../quickstart/README.md)** - Deploy your infrastructure

## 🆘 Having Issues?

**Common Problems:**

- **Tool installation failures** - Check your package manager and permissions
- **AWS credential issues** - Verify IAM permissions and MFA settings
- **Network connectivity** - Check firewall and proxy settings

**Get Help:**

- **[FAQ](../help/faq.md)** - Common questions and solutions
- **[Discord Community](https://discord.gg/gSmnZ9wmNv)** - Real-time support
- **[GitHub Issues](https://github.com/capsign/infrastructure/issues)** - Bug reports

---

**All set?** Continue to [repository setup](../installation/setup.md)!
