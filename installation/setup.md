# Repository Setup Guide

This guide walks you through creating and configuring CapSign's GitHub repositories from the local workspace.

## 📚 Quick Links

- **[Secrets Configuration](secrets.md)** - Essential secrets configuration checklist
- **[Optional Services](../advanced/optional-services.md)** - Third-party services guide (Infracost, FOSSA)
- **[SOC Compliance](../security/soc.md)** - SOC 1/2/3 certification requirements

## 📁 Repository Structure

### Individual Repositories to Create

1. **`github.com/capsign/infrastructure`** (PUBLIC)

   - **Source**: `./infrastructure/` directory
   - **Purpose**: Infrastructure as Code for blockchain deployment on AWS

2. **`github.com/capsign/helm-charts`** (PUBLIC)

   - **Source**: `./helm-charts/` directory
   - **Purpose**: Kubernetes Helm charts for blockchain nodes and monitoring

3. **`github.com/capsign/.github`** (PUBLIC)

   - **Source**: `./.github/profile/` directory
   - **Purpose**: Organization profile showcasing the CapSign ecosystem

4. **`github.com/capsign/docs`** (PUBLIC)

   - **Source**: `./docs/` directory
   - **Purpose**: GitBook documentation hosted at docs.capsign.com

5. **`github.com/capsign/.github-private`** (PRIVATE)
   - **Source**: `./.github-private/` directory
   - **Purpose**: Internal team workflows and automation
   - **⚠️ PRIVATE REPOSITORY** - for team members only

## 🚀 Repository Creation Steps

**⚠️ IMPORTANT**: Follow this exact order to avoid CI/CD failures:

### 1. Create Empty Repositories on GitHub

Create these repositories on GitHub **without initializing** them (no README, .gitignore, or license):

#### `github.com/capsign/infrastructure` (PUBLIC)

**Description**: Infrastructure as Code for blockchain deployment on AWS using Terraform and Kubernetes.

#### `github.com/capsign/helm-charts` (PUBLIC)

**Description**: Kubernetes Helm charts for deploying blockchain nodes and monitoring infrastructure.

#### `github.com/capsign/.github` (PUBLIC)

**Description**: CapSign organization profile showcasing our open source blockchain infrastructure ecosystem.

#### `github.com/capsign/docs` (PUBLIC)

**Description**: Comprehensive GitBook documentation with guides, tutorials, and API references.

#### `github.com/capsign/.github-private` (PRIVATE)

**Description**: Internal team workflows, templates, and automation for the CapSign organization.

**📝 Repository Creation Notes:**

- ✅ **Do NOT initialize** with README, .gitignore, or license
- ✅ **Set correct visibility** (PUBLIC for first 4, PRIVATE for .github-private)
- ✅ **Use exact repository names** as shown above
- ✅ **Enable Issues** and **Discussions** for public repos
- ✅ **Restrict .github-private** to team members only

### 2. Configure Secrets (BEFORE Pushing Code!)

**⚠️ CRITICAL**: Set up all required secrets now using **[secrets configuration guide](secrets.md)**

If you push code before setting up secrets, all CI/CD workflows will fail!

### 3. Push Infrastructure Repository

```bash
cd infrastructure/
git init
git add .
git commit -m "feat: initial Terraform infrastructure for AWS EKS blockchain deployment"
git branch -M main
git remote add origin git@github.com:capsign/infrastructure.git
git push -u origin main
```

### 4. Push Helm Charts Repository

```bash
cd ../helm-charts/
git init
git add .
git commit -m "feat: initial Helm charts for blockchain infrastructure"
git branch -M main
git remote add origin git@github.com:capsign/helm-charts.git
git push -u origin main
```

### 5. Push Organization Profile Repository

```bash
mkdir capsign-github-profile
cd capsign-github-profile/
mkdir -p .github/profile/
cp ../capsign/.github/profile/README.md .github/profile/
git init
git add .
git commit -m "feat: initial organization profile"
git branch -M main
git remote add origin git@github.com:capsign/.github.git
git push -u origin main
```

### 6. Push Documentation Repository

```bash
cd ../docs/
git init
git add .
git commit -m "feat: initial GitBook documentation with comprehensive guides"
git branch -M main
git remote add origin git@github.com:capsign/docs.git
git push -u origin main
```

### 7. Push Private Team Workflows Repository

```bash
mkdir capsign-github-private
cd capsign-github-private/
cp -r ../capsign/.github-private/* .
git init
git add .
git commit -m "feat: initial private team workflows and templates"
git branch -M main
git remote add origin git@github.com:capsign/.github-private.git
git push -u origin main
```

**⚠️ Important**: Set this repository to **PRIVATE** immediately after creation.

## 📋 Pre-Creation Checklist

Before creating the repositories, ensure:

### Infrastructure Repository

- [ ] `terraform.tfvars.example` is present (no actual `terraform.tfvars`)
- [ ] All `.tf` files are properly formatted
- [ ] No AWS credentials or secrets in code
- [ ] README.md has correct URLs and badges
- [ ] LICENSE file is present
- [ ] CI/CD workflows are configured

### Helm Charts Repository

- [ ] All charts lint successfully (`helm lint charts/*/`)
- [ ] No secrets in values files (only `.example` files)
- [ ] Chart versions are set correctly
- [ ] Dependencies are properly defined
- [ ] README.md has correct repository URLs
- [ ] LICENSE file is present

### Organization Profile

- [ ] Update Discord invite links with real server ID
- [ ] Update placeholder URLs with actual domains
- [ ] Verify all badge URLs point to correct repositories
- [ ] Check all social media links

## 🔧 Post-Creation Setup

After creating repositories:

### Enable GitHub Features

1. **Branch Protection Rules**

   ```
   - Require pull request reviews before merging
   - Require status checks to pass before merging
   - Require branches to be up to date before merging
   - Include administrators
   ```

2. **GitHub Discussions**

   - Enable for both repositories
   - Create initial categories (General, Q&A, Ideas, etc.)

3. **Security Features**

   - Enable Dependabot alerts
   - Enable secret scanning
   - Enable code scanning (CodeQL)

4. **Repository Settings**
   - Add repository description
   - Add topics/tags for discoverability
   - Set up repository social preview image

## ✅ Verification Checklist

After setup is complete:

- [ ] All repositories are public and accessible
- [ ] README badges display correctly
- [ ] **🔐 All required secrets are configured**
- [ ] **⚙️ CI/CD pipelines run successfully**
- [ ] **🧪 Test workflows complete without errors**
- [ ] Community health files are present in all repos
- [ ] Branch protection rules are active
- [ ] Security features are enabled
- [ ] Discord/social media links work
- [ ] Documentation links resolve correctly
- [ ] **☁️ AWS infrastructure can be deployed**
- [ ] **🚢 Helm charts can be deployed to cluster**

## 🆘 Troubleshooting

### Common Issues

1. **AWS Authentication Failures**

   ```
   Error: could not retrieve caller identity
   ```

   **Solutions:**

   - Verify `AWS_ROLE_ARN` is correct and role exists
   - Check trust policy allows GitHub Actions
   - Ensure OIDC provider thumbprint is current
   - Test with `aws sts get-caller-identity` locally

2. **Terraform State Backend Issues**

   ```
   Error: Failed to configure the backend "s3"
   ```

   **Solutions:**

   - Ensure S3 bucket exists and is accessible
   - Verify DynamoDB table exists for locking
   - Check bucket name in `backend.tf` matches secret
   - Confirm AWS permissions include S3/DynamoDB access

3. **Kubernetes Connection Problems**
   ```
   Error: couldn't get current server API group list
   ```
   **Solutions:**
   - Verify `KUBECONFIG_DATA` is base64 encoded correctly
   - Ensure EKS cluster is running and accessible
   - Check cluster name and region match configuration
   - Update kubeconfig: `aws eks update-kubeconfig --region us-west-2 --name capsign-cluster`

For more troubleshooting help, see our [common issues guide](../help/common-issues.md).

---

**Ready to make CapSign repositories live? Follow this guide step by step! 🚀**
