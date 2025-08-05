# Quick Start Guide

Deploy production-ready **CMX Protocol infrastructure** in **30 minutes** using our automated tools.

## 🎯 What You'll Deploy

By the end of this guide, you'll have infrastructure ready for the CapSign ecosystem:

- ✅ **AWS EKS cluster** optimized for CMX Protocol and CMX Network
- ✅ **Reth Ethereum client** for L1 connectivity
- ✅ **Prysm consensus client** with validator support
- ✅ **Optimism-ready infrastructure** for CMX Network deployment
- ✅ **Monitoring stack** with Prometheus and Grafana for protocol monitoring
- ✅ **Production security** with encrypted storage and network policies

## 📋 Prerequisites

Before starting, ensure you have:

- **AWS Account** with billing enabled
- **GitHub Account** for repository access
- **Domain name** (optional, for custom endpoints)
- **30 minutes** of focused time

### Required Tools

```bash
# Install required CLI tools
brew install terraform helm kubectl awscli

# Or using package managers:
# Ubuntu: apt-get install terraform helm kubectl awscli
# Windows: choco install terraform helm kubectl awscli
```

### AWS Credentials

Set up AWS access using one of these methods:

**Option A: AWS CLI**

```bash
aws configure
# Enter your Access Key ID, Secret Access Key, and region
```

**Option B: Environment Variables**

```bash
export AWS_ACCESS_KEY_ID="your-access-key"
export AWS_SECRET_ACCESS_KEY="your-secret-key"
export AWS_DEFAULT_REGION="us-west-2"
```

## 🚀 Step 1: Deploy Infrastructure

### Clone Infrastructure Repository

```bash
git clone https://github.com/capsign/infrastructure.git
cd infrastructure
```

### Configure Variables

```bash
# Copy the example configuration
cp terraform.tfvars.example terraform.tfvars

# Edit with your preferences
nano terraform.tfvars
```

**Key variables to customize:**

```hcl
# terraform.tfvars
project_name = "my-blockchain"
environment  = "production"
region       = "us-west-2"

# Node configuration
node_desired_size = 3
node_instance_types = ["m5.large"]

# Enable blockchain-specific features
enable_blockchain_dedicated_nodes = true
enable_monitoring = true
```

### Deploy Infrastructure

```bash
# Initialize Terraform
terraform init

# Review the deployment plan
terraform plan

# Deploy (takes ~15 minutes)
terraform apply
```

**☕ Coffee break!** This creates your VPC, EKS cluster, security groups, and IAM roles.

### Configure kubectl

```bash
# Connect to your new cluster
aws eks update-kubeconfig --region us-west-2 --name my-blockchain-cluster

# Verify connection
kubectl get nodes
```

## 🚢 Step 2: Deploy Blockchain Stack

### Clone Helm Charts Repository

```bash
cd ..
git clone https://github.com/capsign/helm-charts.git
cd helm-charts
```

### Deploy Everything at Once

```bash
# Deploy all components to production
./scripts/deploy-all.sh production
```

This script deploys:

1. **Monitoring stack** (Prometheus, Grafana, AlertManager)
2. **Reth execution client**
3. **Prysm consensus client**
4. **Network policies and RBAC**

### Monitor Deployment Progress

```bash
# Watch deployments
kubectl get pods -A --watch

# Check specific services
kubectl get pods -n blockchain
kubectl get pods -n monitoring
```

## 📊 Step 3: Access Your Infrastructure

### Grafana Dashboard

```bash
# Get Grafana admin password
kubectl get secret --namespace monitoring monitoring-stack-grafana \
  -o jsonpath="{.data.admin-password}" | base64 --decode ; echo

# Port forward to access locally
kubectl port-forward svc/monitoring-stack-grafana 3000:80 -n monitoring
```

**Open:** http://localhost:3000

- **Username:** admin
- **Password:** [from command above]

### Blockchain Node Endpoints

```bash
# Get Reth RPC endpoint
kubectl get svc reth -n blockchain

# Test RPC connection
kubectl exec -n blockchain deployment/reth -- \
  curl -X POST -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}' \
  http://localhost:8545
```

### Consensus Client Status

```bash
# Check Prysm beacon node
kubectl logs -n blockchain deployment/prysm-beacon --tail=50

# Check sync status
kubectl exec -n blockchain deployment/prysm-beacon -- \
  curl http://localhost:8080/healthz
```

## ✅ Verification Checklist

Confirm your deployment is working:

- [ ] **Infrastructure:** All Terraform resources created successfully
- [ ] **Kubernetes:** All pods are `Running` or `Completed`
- [ ] **Reth:** Execution client is syncing Ethereum mainnet
- [ ] **Prysm:** Consensus client is syncing beacon chain
- [ ] **Monitoring:** Grafana dashboards show live metrics
- [ ] **Security:** Network policies and RBAC are active

### Health Check Commands

```bash
# Overall cluster health
kubectl get nodes
kubectl get pods -A | grep -v Running

# Blockchain sync status
kubectl logs -n blockchain deployment/reth --tail=10
kubectl logs -n blockchain deployment/prysm-beacon --tail=10

# Monitoring stack
kubectl get pods -n monitoring
```

## 🎉 Success! What's Next?

Your blockchain infrastructure is now live! Here are recommended next steps:

### Immediate Actions

- **[Configure alerting](/monitoring/alerting.md)** for production monitoring
- **[Set up backups](/monitoring/backup.md)** for critical data
- **[Review security settings](/security/hardening.md)** for production hardening

### Optional Enhancements

- **[Add validators](/blockchain/prysm.md#validator-setup)** to earn staking rewards
- **[Deploy Optimism L2](/blockchain/optimism.md)** for Layer 2 scaling
- **[Set up custom domains](/infrastructure/dns.md)** for external access
- **[Configure CI/CD](/advanced/cicd.md)** for automated deployments

### Learning Resources

- **[Architecture Overview](/infrastructure/architecture.md)** - Understand your infrastructure
- **[Monitoring Guide](/monitoring/metrics.md)** - Key metrics to watch
- **[Troubleshooting](/kubernetes/troubleshooting.md)** - Common issues and fixes
- **[Best Practices](/advanced/best-practices.md)** - Production optimization

## 🆘 Need Help?

**Something not working?** We're here to help:

- **[Troubleshooting Guide](/help/common-issues.md)** - Common solutions
- **[Discord Community](https://discord.gg/gSmnZ9wmNv)** - Real-time support
- **[GitHub Issues](https://github.com/capsign/infrastructure/issues)** - Bug reports
- **[Enterprise Support](mailto:support@capsign.com)** - Commercial assistance

## 💰 Cost Estimation

**Typical monthly costs for this setup:**

| Component         | Instance Type | Monthly Cost    |
| ----------------- | ------------- | --------------- |
| EKS Control Plane | -             | $73             |
| Worker Nodes      | 3x m5.large   | $195            |
| Storage (EBS)     | 500GB gp3     | $40             |
| Load Balancer     | ALB           | $25             |
| **Total**         |               | **~$333/month** |

**💡 Cost optimization tips:**

- Use [Spot instances](/advanced/cost-optimization.md#spot-instances) for 60% savings
- Enable [cluster autoscaling](/kubernetes/scaling.md) to reduce idle costs
- Monitor with [Infracost](/infrastructure/cost-monitoring.md) for detailed breakdowns

---

**🚀 Congratulations!** You've deployed enterprise-grade infrastructure ready for the CMX Protocol and CMX Network. Welcome to the CapSign community!
