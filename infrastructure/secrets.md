# 🔐 Secrets Setup Checklist

Quick reference for setting up GitHub secrets after creating repositories.

**🏆 SOC Certification Note**: If pursuing SOC 1/2/3 certification, Infracost and FOSSA are **required** (both are SOC 2 Type II certified). See **[SOC Compliance](../security/soc.md)** for details.

## 🔄 Correct Setup Workflow

1. ✅ **Create empty repositories** on GitHub (no initialization)
2. ✅ **Configure ALL secrets** using this checklist (you are here!)
3. ✅ **Push code** - workflows will now succeed
4. ❌ **DON'T push before secrets** - workflows will fail and spam your inbox

## 🎯 Setup Order

**Follow this sequence to avoid dependency issues:**

1. **First**: Set up AWS credentials and Terraform backend secrets
2. **Second**: Deploy infrastructure using Terraform workflows
3. **Third**: Get kubeconfig and set up Kubernetes secrets
4. **Fourth**: Deploy Helm charts to verify everything works
5. **Last**: Set up optional services (Slack, Infracost, etc.)

## 🔍 Where to Add Secrets

### Repository Level Secrets:

**GitHub Repository → Settings → Secrets and variables → Actions**

### Organization Level Secrets (Recommended):

**GitHub Organization → Settings → Secrets and variables → Actions**

### Repository Variables:

**GitHub Repository → Settings → Secrets and variables → Actions → Variables tab**

**💡 Best Practice**: Use organization-level secrets for shared services (Slack, FOSSA) and repository-level secrets for specific infrastructure (AWS roles, Kubernetes).

## 📝 How to Get Each Secret

### 🔐 AWS Authentication

**Option 1: OIDC (Recommended - No long-lived keys)**

1. **Create OIDC Provider** (run once per AWS account):

   ```bash
   aws iam create-open-id-connect-provider \
     --url https://token.actions.githubusercontent.com \
     --client-id-list sts.amazonaws.com \
     --thumbprint-list 6938fd4d98bab03faadb97b34396831e3780aea1
   ```

2. **Create IAM Role** with this trust policy:

   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Federated": "arn:aws:iam::YOUR_ACCOUNT:oidc-provider/token.actions.githubusercontent.com"
         },
         "Action": "sts:AssumeRole",
         "Condition": {
           "StringEquals": {
             "token.actions.githubusercontent.com:sub": "repo:capsign/infrastructure:ref:refs/heads/main",
             "token.actions.githubusercontent.com:aud": "sts.amazonaws.com"
           }
         }
       }
     ]
   }
   ```

3. **Add these Repository Secrets**:
   - `AWS_ROLE_ARN`: `arn:aws:iam::YOUR_ACCOUNT:role/GitHubActionsRole`
   - `AWS_REGION`: `us-west-2` (or your preferred region)

**Option 2: Access Keys (Less secure)**

1. **Create IAM User** with programmatic access
2. **Add these Repository Secrets**:
   - `AWS_ACCESS_KEY_ID`: From IAM user
   - `AWS_SECRET_ACCESS_KEY`: From IAM user
   - `AWS_REGION`: `us-west-2` (or your preferred region)

### 🗄️ Terraform Backend

**Create S3 bucket and DynamoDB table**, then add **Repository Secrets**:

- `TF_VAR_terraform_state_bucket`: Your S3 bucket name (e.g., `capsign-terraform-state`)
- `TF_VAR_terraform_state_dynamodb_table`: Your DynamoDB table name (e.g., `capsign-terraform-locks`)
- `TF_VAR_environment`: `production` or `staging`

### ☸️ Kubernetes Access

**After EKS cluster is deployed**:

1. **Update local kubeconfig**:

   ```bash
   aws eks update-kubeconfig --region us-west-2 --name capsign-cluster
   ```

2. **Encode kubeconfig**:

   ```bash
   cat ~/.kube/config | base64 | tr -d '\n'
   ```

3. **Add Repository Secret**:
   - `KUBECONFIG_DATA`: The base64 encoded output from above

### 💬 Slack Integration

1. **Go to** [Slack Apps](https://api.slack.com/apps) → Create New App
2. **Enable Incoming Webhooks** → Add New Webhook to Workspace
3. **Copy webhook URL** and add as **Organization Secrets**:
   - `SLACK_WEBHOOK_URL`: General notifications
   - `SLACK_SECURITY_WEBHOOK`: Security alerts (#alerts-security)
   - `SLACK_RELEASES_WEBHOOK`: Release notifications (#alerts-releases)

### 💰 Infracost (Optional - Cost Estimation)

1. **Sign up** at [infracost.io](https://www.infracost.io/)
2. **Get API key** from dashboard
3. **Add Organization Secrets**:
   - `INFRACOST_API_KEY`: Your API key
   - **Add Organization Variable**: `INFRACOST_ENABLED`: `true`

**Free Tier**: 1,000 runs/month

### 📄 FOSSA (Optional - License Compliance)

1. **Sign up** at [fossa.com](https://fossa.com/)
2. **Get API key** from settings
3. **Add Organization Secrets**:
   - `FOSSA_API_KEY`: Your API key
   - **Add Organization Variable**: `FOSSA_ENABLED`: `true`

**Free Tier**: 5 projects

### 🔄 GitHub Release Token

1. **GitHub Settings** → Developer settings → Personal access tokens → Fine-grained tokens
2. **Create token** with Repository permissions: `Contents: Write`, `Metadata: Read`
3. **Add Organization Secret**:
   - `RELEASE_TOKEN`: Your personal access token

## 📋 Required Secrets by Repository

### 🏗️ Infrastructure Repository (`capsign/infrastructure`)

**AWS Authentication (Choose One):**

- [ ] `AWS_ROLE_ARN` + `AWS_REGION` (OIDC - Recommended)
- [ ] `AWS_ACCESS_KEY_ID` + `AWS_SECRET_ACCESS_KEY` + `AWS_REGION` (Keys)

**Terraform Backend:**

- [ ] `TF_VAR_terraform_state_bucket`
- [ ] `TF_VAR_terraform_state_dynamodb_table`
- [ ] `TF_VAR_environment`

**Optional Services (see [Optional Services](../advanced/optional-services.md)):**

- [ ] `SLACK_WEBHOOK_URL` (Notifications)
- [ ] `INFRACOST_API_KEY` (Cost estimation - FREE up to 1K runs/month)
- [ ] `INFRACOST_ENABLED=true` (Variable to enable cost estimation)

### 🚢 Helm Charts Repository (`capsign/helm-charts`)

**Kubernetes Access:**

- [ ] `KUBECONFIG_DATA` (Base64 encoded kubeconfig)

**Optional Services:**

- [ ] `SLACK_WEBHOOK_URL` (Notifications)
- [ ] `HELM_REGISTRY_USER` (Private registry)
- [ ] `HELM_REGISTRY_PASSWORD` (Private registry)

### 🔒 Organization Level (All Repositories)

**Team Coordination:**

- [ ] `SLACK_SECURITY_WEBHOOK` (Security alerts)
- [ ] `SLACK_RELEASES_WEBHOOK` (Release notifications)
- [ ] `RELEASE_TOKEN` (Cross-repo operations)

**Optional Compliance Services (see [Optional Services](../advanced/optional-services.md)):**

- [ ] `FOSSA_API_KEY` (License compliance - FREE for 5 projects)
- [ ] `FOSSA_ENABLED=true` (Variable to enable license scanning)

## ⚡ Quick Setup Commands

### 1. Create AWS OIDC Provider

```bash
aws iam create-open-id-connect-provider \
  --url https://token.actions.githubusercontent.com \
  --client-id-list sts.amazonaws.com \
  --thumbprint-list 6938fd4d98bab03faadb97b34396831e3780aea1
```

### 2. Get Base64 Kubeconfig

```bash
# After EKS cluster is created
aws eks update-kubeconfig --region us-west-2 --name capsign-cluster
cat ~/.kube/config | base64 | tr -d '\n'
```

### 3. Test Secrets

```bash
# Test AWS
aws sts get-caller-identity

# Test Kubernetes
kubectl get nodes

# Test Slack webhook
curl -X POST -H 'Content-type: application/json' \
  --data '{"text":"CapSign test"}' \
  $SLACK_WEBHOOK_URL
```

## ✅ Ready to Push Code?

Once all secrets are configured:

1. **Return to [repository setup guide](setup.md)**
2. **Continue with Step 3** (Push Infrastructure Repository)
3. **Your CI/CD workflows will now succeed!** 🎉

**Double-check**: All required secrets for your repositories are set up before pushing code.

---

💡 **Tip**: Use organization-level secrets for shared services (Slack, FOSSA) and repository-level secrets for specific infrastructure (AWS, Kubernetes).

🚨 **Important**: Test each secret after adding to ensure CI/CD workflows work correctly!


Quick reference for setting up GitHub secrets after creating repositories.

**🏆 SOC Certification Note**: If pursuing SOC 1/2/3 certification, Infracost and FOSSA are **required** (both are SOC 2 Type II certified). See **[SOC Compliance](../security/soc.md)** for details.

## 🔄 Correct Setup Workflow

1. ✅ **Create empty repositories** on GitHub (no initialization)
2. ✅ **Configure ALL secrets** using this checklist (you are here!)
3. ✅ **Push code** - workflows will now succeed
4. ❌ **DON'T push before secrets** - workflows will fail and spam your inbox

## 🎯 Setup Order

**Follow this sequence to avoid dependency issues:**

1. **First**: Set up AWS credentials and Terraform backend secrets
2. **Second**: Deploy infrastructure using Terraform workflows
3. **Third**: Get kubeconfig and set up Kubernetes secrets
4. **Fourth**: Deploy Helm charts to verify everything works
5. **Last**: Set up optional services (Slack, Infracost, etc.)

## 🔍 Where to Add Secrets

### Repository Level Secrets:

**GitHub Repository → Settings → Secrets and variables → Actions**

### Organization Level Secrets (Recommended):

**GitHub Organization → Settings → Secrets and variables → Actions**

### Repository Variables:

**GitHub Repository → Settings → Secrets and variables → Actions → Variables tab**

**💡 Best Practice**: Use organization-level secrets for shared services (Slack, FOSSA) and repository-level secrets for specific infrastructure (AWS roles, Kubernetes).

## 📝 How to Get Each Secret

### 🔐 AWS Authentication

**Option 1: OIDC (Recommended - No long-lived keys)**

1. **Create OIDC Provider** (run once per AWS account):

   ```bash
   aws iam create-open-id-connect-provider \
     --url https://token.actions.githubusercontent.com \
     --client-id-list sts.amazonaws.com \
     --thumbprint-list 6938fd4d98bab03faadb97b34396831e3780aea1
   ```

2. **Create IAM Role** with this trust policy:

   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Federated": "arn:aws:iam::YOUR_ACCOUNT:oidc-provider/token.actions.githubusercontent.com"
         },
         "Action": "sts:AssumeRole",
         "Condition": {
           "StringEquals": {
             "token.actions.githubusercontent.com:sub": "repo:capsign/infrastructure:ref:refs/heads/main",
             "token.actions.githubusercontent.com:aud": "sts.amazonaws.com"
           }
         }
       }
     ]
   }
   ```

3. **Add these Repository Secrets**:
   - `AWS_ROLE_ARN`: `arn:aws:iam::YOUR_ACCOUNT:role/GitHubActionsRole`
   - `AWS_REGION`: `us-west-2` (or your preferred region)

**Option 2: Access Keys (Less secure)**

1. **Create IAM User** with programmatic access
2. **Add these Repository Secrets**:
   - `AWS_ACCESS_KEY_ID`: From IAM user
   - `AWS_SECRET_ACCESS_KEY`: From IAM user
   - `AWS_REGION`: `us-west-2` (or your preferred region)

### 🗄️ Terraform Backend

**Create S3 bucket and DynamoDB table**, then add **Repository Secrets**:

- `TF_VAR_terraform_state_bucket`: Your S3 bucket name (e.g., `capsign-terraform-state`)
- `TF_VAR_terraform_state_dynamodb_table`: Your DynamoDB table name (e.g., `capsign-terraform-locks`)
- `TF_VAR_environment`: `production` or `staging`

### ☸️ Kubernetes Access

**After EKS cluster is deployed**:

1. **Update local kubeconfig**:

   ```bash
   aws eks update-kubeconfig --region us-west-2 --name capsign-cluster
   ```

2. **Encode kubeconfig**:

   ```bash
   cat ~/.kube/config | base64 | tr -d '\n'
   ```

3. **Add Repository Secret**:
   - `KUBECONFIG_DATA`: The base64 encoded output from above

### 💬 Slack Integration

1. **Go to** [Slack Apps](https://api.slack.com/apps) → Create New App
2. **Enable Incoming Webhooks** → Add New Webhook to Workspace
3. **Copy webhook URL** and add as **Organization Secrets**:
   - `SLACK_WEBHOOK_URL`: General notifications
   - `SLACK_SECURITY_WEBHOOK`: Security alerts (#alerts-security)
   - `SLACK_RELEASES_WEBHOOK`: Release notifications (#alerts-releases)

### 💰 Infracost (Optional - Cost Estimation)

1. **Sign up** at [infracost.io](https://www.infracost.io/)
2. **Get API key** from dashboard
3. **Add Organization Secrets**:
   - `INFRACOST_API_KEY`: Your API key
   - **Add Organization Variable**: `INFRACOST_ENABLED`: `true`

**Free Tier**: 1,000 runs/month

### 📄 FOSSA (Optional - License Compliance)

1. **Sign up** at [fossa.com](https://fossa.com/)
2. **Get API key** from settings
3. **Add Organization Secrets**:
   - `FOSSA_API_KEY`: Your API key
   - **Add Organization Variable**: `FOSSA_ENABLED`: `true`

**Free Tier**: 5 projects

### 🔄 GitHub Release Token

1. **GitHub Settings** → Developer settings → Personal access tokens → Fine-grained tokens
2. **Create token** with Repository permissions: `Contents: Write`, `Metadata: Read`
3. **Add Organization Secret**:
   - `RELEASE_TOKEN`: Your personal access token

## 📋 Required Secrets by Repository

### 🏗️ Infrastructure Repository (`capsign/infrastructure`)

**AWS Authentication (Choose One):**

- [ ] `AWS_ROLE_ARN` + `AWS_REGION` (OIDC - Recommended)
- [ ] `AWS_ACCESS_KEY_ID` + `AWS_SECRET_ACCESS_KEY` + `AWS_REGION` (Keys)

**Terraform Backend:**

- [ ] `TF_VAR_terraform_state_bucket`
- [ ] `TF_VAR_terraform_state_dynamodb_table`
- [ ] `TF_VAR_environment`

**Optional Services (see [Optional Services](../advanced/optional-services.md)):**

- [ ] `SLACK_WEBHOOK_URL` (Notifications)
- [ ] `INFRACOST_API_KEY` (Cost estimation - FREE up to 1K runs/month)
- [ ] `INFRACOST_ENABLED=true` (Variable to enable cost estimation)

### 🚢 Helm Charts Repository (`capsign/helm-charts`)

**Kubernetes Access:**

- [ ] `KUBECONFIG_DATA` (Base64 encoded kubeconfig)

**Optional Services:**

- [ ] `SLACK_WEBHOOK_URL` (Notifications)
- [ ] `HELM_REGISTRY_USER` (Private registry)
- [ ] `HELM_REGISTRY_PASSWORD` (Private registry)

### 🔒 Organization Level (All Repositories)

**Team Coordination:**

- [ ] `SLACK_SECURITY_WEBHOOK` (Security alerts)
- [ ] `SLACK_RELEASES_WEBHOOK` (Release notifications)
- [ ] `RELEASE_TOKEN` (Cross-repo operations)

**Optional Compliance Services (see [Optional Services](../advanced/optional-services.md)):**

- [ ] `FOSSA_API_KEY` (License compliance - FREE for 5 projects)
- [ ] `FOSSA_ENABLED=true` (Variable to enable license scanning)

## ⚡ Quick Setup Commands

### 1. Create AWS OIDC Provider

```bash
aws iam create-open-id-connect-provider \
  --url https://token.actions.githubusercontent.com \
  --client-id-list sts.amazonaws.com \
  --thumbprint-list 6938fd4d98bab03faadb97b34396831e3780aea1
```

### 2. Get Base64 Kubeconfig

```bash
# After EKS cluster is created
aws eks update-kubeconfig --region us-west-2 --name capsign-cluster
cat ~/.kube/config | base64 | tr -d '\n'
```

### 3. Test Secrets

```bash
# Test AWS
aws sts get-caller-identity

# Test Kubernetes
kubectl get nodes

# Test Slack webhook
curl -X POST -H 'Content-type: application/json' \
  --data '{"text":"CapSign test"}' \
  $SLACK_WEBHOOK_URL
```

## ✅ Ready to Push Code?

Once all secrets are configured:

1. **Return to [repository setup guide](setup.md)**
2. **Continue with Step 3** (Push Infrastructure Repository)
3. **Your CI/CD workflows will now succeed!** 🎉

**Double-check**: All required secrets for your repositories are set up before pushing code.

---

💡 **Tip**: Use organization-level secrets for shared services (Slack, FOSSA) and repository-level secrets for specific infrastructure (AWS, Kubernetes).

🚨 **Important**: Test each secret after adding to ensure CI/CD workflows work correctly!
