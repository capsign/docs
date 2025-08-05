# Contributing to CapSign Documentation

Thank you for your interest in contributing to CapSign documentation! This guide will help you get started with improving our guides, tutorials, and API references.

## 🎯 Ways to Contribute

### 📝 Documentation Improvements

- **Fix typos** and grammatical errors
- **Update outdated** information and broken links
- **Improve clarity** of existing explanations
- **Add missing** details and examples

### 📚 New Content

- **Write tutorials** for specific use cases
- **Create guides** for advanced configurations
- **Add troubleshooting** sections for common issues
- **Translate content** to other languages

### 🐛 Issue Reporting

- **Report inaccuracies** in documentation
- **Suggest improvements** to existing content
- **Request missing** documentation sections
- **Flag accessibility** or usability issues

## 🚀 Getting Started

### Prerequisites

```bash
# Install Node.js (v16 or higher)
node --version

# Install GitBook CLI
npm install -g gitbook-cli

# Clone the repository
git clone https://github.com/capsign/docs.git
cd docs
```

### Local Development

```bash
# Install dependencies
npm install
gitbook install

# Start development server
npm run dev
# Documentation will be available at http://localhost:4000
```

### Making Changes

```bash
# Create a feature branch
git checkout -b improve/kubernetes-guide

# Make your changes
# Edit files in the appropriate directories

# Test locally
npm run dev
# Review your changes at http://localhost:4000

# Build to check for errors
npm run build
```

## 📁 Repository Structure

```
docs/
├── README.md                    # Main documentation homepage
├── SUMMARY.md                   # GitBook table of contents
├── book.json                    # GitBook configuration
├── package.json                 # Node.js dependencies
├── quickstart/                  # Getting started guides
├── infrastructure/              # AWS, Terraform, EKS guides
├── kubernetes/                  # Kubernetes and Helm guides
├── blockchain/                  # Blockchain node documentation
├── monitoring/                  # Monitoring and observability
├── security/                    # Security and compliance
├── advanced/                    # Advanced configuration
├── api/                         # API reference documentation
├── tutorials/                   # Step-by-step tutorials
├── help/                        # FAQ and support information
└── resources/                   # Additional resources
```

## ✍️ Writing Guidelines

### Content Standards

**Tone and Style:**

- Use clear, concise language
- Write in active voice when possible
- Maintain a helpful, professional tone
- Avoid jargon without explanation

**Structure:**

- Start with a brief overview
- Use descriptive headings and subheadings
- Include code examples for technical content
- End with next steps or related resources

**Formatting:**

- Use proper Markdown syntax
- Include syntax highlighting for code blocks
- Add alt text for images
- Use tables for structured data

### Code Examples

**Best Practices:**

```bash
# Good: Include context and explanation
# Deploy infrastructure to AWS
terraform init
terraform apply

# Configure kubectl for the new cluster
aws eks update-kubeconfig --region us-west-2 --name my-cluster
```

```bash
# Avoid: Commands without context
terraform apply
kubectl get pods
```

**Required Elements:**

- **Comments** explaining what each command does
- **Expected output** for important commands
- **Error handling** and troubleshooting tips
- **Prerequisites** clearly stated

### Screenshots and Diagrams

**When to Include:**

- Complex UI interactions
- Architecture diagrams
- Dashboard screenshots
- Step-by-step visual guides

**Requirements:**

- High resolution (at least 1080p)
- Consistent styling and theme
- Descriptive alt text
- Updated for current UI versions

## 🔄 Contribution Process

### 1. Issue First (Recommended)

For significant changes, create an issue first:

```markdown
**Issue Type:** Documentation improvement
**Section:** Kubernetes deployment guide
**Description:** The current Helm chart deployment instructions
are missing prerequisites and troubleshooting steps.
**Proposed Solution:** Add a prerequisites section and common
error resolution guide.
```

### 2. Fork and Branch

```bash
# Fork the repository on GitHub
# Clone your fork
git clone https://github.com/YOUR_USERNAME/docs.git

# Create a feature branch
git checkout -b docs/improve-kubernetes-guide
```

### 3. Make Changes

- Follow the writing guidelines above
- Test all code examples
- Ensure proper formatting
- Update table of contents if needed

### 4. Commit and Push

```bash
# Commit with descriptive messages
git add .
git commit -m "docs: improve Kubernetes deployment prerequisites

- Add Node.js and kubectl installation instructions
- Include troubleshooting section for common Helm errors
- Update code examples with proper error handling
- Add links to related documentation sections"

git push origin docs/improve-kubernetes-guide
```

### 5. Create Pull Request

**PR Template:**

```markdown
## 📋 Documentation Update

### Summary

Brief description of what was changed and why.

### Changes Made

- [ ] Added prerequisites section to Kubernetes guide
- [ ] Updated code examples with error handling
- [ ] Fixed broken links in troubleshooting section
- [ ] Added screenshots for UI interactions

### Testing

- [ ] All code examples tested locally
- [ ] Links verified and working
- [ ] GitBook builds successfully
- [ ] No markdown syntax errors

### Related Issues

Closes #123
```

## 📋 Review Process

### What Reviewers Look For

**Content Quality:**

- Accuracy of technical information
- Clarity and readability
- Completeness of explanations
- Proper code example formatting

**Style Consistency:**

- Follows established writing style
- Consistent terminology usage
- Proper Markdown formatting
- Appropriate use of headings

**User Experience:**

- Easy to follow instructions
- Logical information flow
- Helpful for target audience
- Addresses common questions

### Review Timeline

- **Community PRs:** Reviewed within 3-5 business days
- **Urgent fixes:** Reviewed within 24 hours
- **Major additions:** May require multiple review rounds

## 🎨 Style Guide

### Terminology

**Consistent Terms:**

- "Kubernetes cluster" (not "k8s cluster")
- "Helm chart" (not "helm package")
- "CapSign infrastructure" (not "capsign infra")
- "AWS EKS" (not "EKS" alone on first mention)

**Capitalization:**

- Product names: Kubernetes, Docker, AWS, Terraform
- File names: `values.yaml`, `Chart.yaml`
- Commands: `kubectl`, `helm`, `terraform`

### Code Formatting

**Inline Code:**
Use backticks for commands, file names, and small code snippets:

```markdown
Run `kubectl get pods` to check pod status.
Edit the `values.yaml` file to customize settings.
```

**Code Blocks:**
Always specify the language for syntax highlighting:

````markdown
\```bash

# Deploy the application

helm install my-app ./charts/my-app
\```

\```yaml

# values.yaml

replicaCount: 3
image:
repository: nginx
tag: latest
\```
````

### Links and References

**Internal Links:**

```markdown
See our [installation guide](../installation/README.md) for details.
Check the [troubleshooting section](#troubleshooting) below.
```

**External Links:**

```markdown
Visit the [Kubernetes documentation](https://kubernetes.io/docs/) for more information.
```

## 🔧 Technical Setup

### GitBook Configuration

The documentation uses GitBook with specific plugins configured in `book.json`:

**Key Plugins:**

- `prism`: Syntax highlighting
- `copy-code-button`: Copy code snippets
- `edit-link`: Edit page on GitHub
- `search-pro`: Enhanced search

### Deployment

Documentation is automatically deployed when changes are merged to `main`:

1. **GitHub Actions** builds the GitBook
2. **Generated site** is pushed to `gh-pages` branch
3. **GitHub Pages** serves the documentation
4. **Custom domain** points to docs.capsign.com

### Local Testing

```bash
# Full build test
npm run build

# Check for broken links
# (Add link checker in future)

# Lint markdown
# (Add markdown linter in future)
```

## 🆘 Getting Help

### Discord Community

Join our [Discord server](https://discord.gg/gSmnZ9wmNv) for:

- Quick questions about contributing
- Real-time feedback on changes
- Coordination with other contributors

### GitHub Issues

Use GitHub issues for:

- Bug reports in documentation
- Feature requests for new content
- Technical problems with the repo

### Maintainer Contact

For complex contributions or questions:

- **Email:** docs@capsign.com
- **GitHub:** @capsign/docs-team

## 🏆 Recognition

### Contributors

All contributors will be:

- **Listed** in our contributors page
- **Mentioned** in release notes for significant contributions
- **Invited** to our contributors Discord channel
- **Eligible** for CapSign swag and community recognition

### Types of Contributions

We recognize all types of contributions:

- 📝 **Writing** - New content and improvements
- 🐛 **Bug fixes** - Corrections and updates
- 🎨 **Design** - UI/UX improvements
- 🔧 **Technical** - Infrastructure and tooling
- 🌍 **Translation** - Localization efforts
- 📢 **Community** - Feedback and suggestions

## 📄 License

By contributing to CapSign documentation, you agree that your contributions will be licensed under the [MIT License](LICENSE).

---

**Thank you for helping make CapSign documentation better for everyone!** 🙏

**Questions?** Join our [Discord community](https://discord.gg/gSmnZ9wmNv) or [open an issue](https://github.com/capsign/docs/issues/new).
