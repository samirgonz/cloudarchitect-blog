# Hugo Blog Deployment Guide

This guide will help you deploy your Cloud Solutions Architect blog using Hugo and Netlify.

## Quick Start

### 1. Install Hugo

**macOS:**
```bash
brew install hugo
```

**Windows:**
```bash
winget install Hugo.Hugo.Extended
```

**Linux:**
```bash
# Download latest release from GitHub
wget https://github.com/gohugoio/hugo/releases/download/v0.121.1/hugo_extended_0.121.1_linux-amd64.tar.gz
tar -xzf hugo_extended_0.121.1_linux-amd64.tar.gz
sudo mv hugo /usr/local/bin/
```

### 2. Create Your Hugo Site

```bash
# Create new Hugo site
hugo new site cloudarchitect-blog
cd cloudarchitect-blog

# Initialize git repository
git init
```

### 3. Set Up the File Structure

Create the following directory structure:

```
cloudarchitect-blog/
├── hugo.yaml                 # Main configuration
├── netlify.toml              # Netlify deployment config
├── content/
│   ├── _index.md            # Homepage content
│   ├── about.md             # About page
│   ├── contact.md           # Contact page
│   └── posts/
│       └── azure-identity-types/
│           └── index.md     # Your first article
├── layouts/
│   ├── _default/
│   │   ├── baseof.html      # Base template
│   │   ├── single.html      # Single post template
│   │   └── list.html        # List template
│   ├── partials/
│   │   ├── head.html        # Head partial
│   │   ├── header.html      # Header partial
│   │   └── footer.html      # Footer partial
│   └── index.html           # Homepage template
├── static/
│   ├── images/              # Static images
│   ├── css/                 # Additional CSS files
│   └── js/                  # JavaScript files
└── assets/
    └── css/
        └── main.css         # Main stylesheet
```

### 4. Copy Configuration Files

Copy the configuration content from the artifacts above:

**hugo.yaml:**
```yaml
baseURL: 'https://your-site-name.netlify.app'
languageCode: 'en-us'
title: 'Cloud Solutions Architect Blog'

params:
  author: 'Your Name'
  description: 'Sharing insights on Azure, Kubernetes, and cloud-native technologies'
  email: 'your-email@example.com'
  github: 'yourgithub'
  linkedin: 'yourlinkedin'

markup:
  goldmark:
    renderer:
      unsafe: true
  highlight:
    style: 'github'
    lineNos: true

menu:
  main:
    - identifier: 'home'
      name: 'Home'
      url: '/'
      weight: 1
    - identifier: 'about'
      name: 'About'
      url: '/about/'
      weight: 2
    - identifier: 'articles'
      name: 'Articles'
      url: '/posts/'
      weight: 3
    - identifier: 'contact'
      name: 'Contact'
      url: '/contact/'
      weight: 4
```

**netlify.toml:**
```toml
[build]
  publish = "public"
  command = "hugo --gc --minify"

[context.production.environment]
  HUGO_VERSION = "0.121.1"
  HUGO_ENV = "production"
  HUGO_ENABLEGITINFO = "true"

[[headers]]
  for = "/*"
  [headers.values]
    X-Frame-Options = "DENY"
    X-XSS-Protection = "1; mode=block"
```

### 5. Create Your First Article

Create the Azure Identity article:

```bash
# Create the article directory
mkdir -p content/posts/azure-identity-types

# Copy the article content from the artifact to:
# content/posts/azure-identity-types/index.md
```

Add the frontmatter to your article:

```yaml
---
title: "Azure Identity Types: A Comprehensive Guide"
date: 2025-08-22
draft: false
summary: "Demystifying the confusion between User Accounts, Service Accounts, Service Principals, and Managed Identities in Azure. Complete with Terraform examples and decision flowcharts."
tags: ["Azure", "Identity Management", "Security", "Terraform", "DevOps", "Cloud Architecture"]
categories: ["Azure", "Security"]
---
```

### 6. Create CSS Styling

Create `assets/css/main.css` and copy the CSS styles from the HTML artifact.

### 7. Test Locally

```bash
# Start Hugo development server
hugo server -D

# Open http://localhost:1313 in your browser
```

## Deploy to Netlify

### Option 1: GitHub + Netlify (Recommended)

1. **Push to GitHub:**
```bash
# Add all files
git add .
git commit -m "Initial blog setup with Azure identity article"

# Create GitHub repository and push
gh repo create cloudarchitect-blog --public
git remote add origin https://github.com/yourusername/cloudarchitect-blog.git
git push -u origin main
```

2. **Connect to Netlify:**
   - Go to [netlify.com](https://netlify.com)
   - Click "New site from Git"
   - Connect your GitHub account
   - Select your repository
   - Netlify will auto-detect Hugo settings
   - Click "Deploy site"

### Option 2: Netlify CLI

```bash
# Install Netlify CLI
npm install -g netlify-cli

# Login to Netlify
netlify login

# Initialize and deploy
netlify init
netlify deploy --prod
```

### Option 3: Manual Deployment

```bash
# Build the site
hugo --gc --minify

# Upload the 'public' folder to Netlify manually
```

## Terraform Repository Structure

Create separate repositories for your Terraform examples:

### Repository: `azure-identity-terraform-examples`

```
azure-identity-terraform-examples/
├── README.md
├── user-accounts/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   ├── terraform.tfvars.example
│   └── README.md
├── service-principals/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   ├── terraform.tfvars.example
│   └── README.md
├── system-assigned-mi/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   ├── terraform.tfvars.example
│   └── README.md
├── user-assigned-mi/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   ├── terraform.tfvars.example
│   └── README.md
├── guest-users/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   ├── terraform.tfvars.example
│   └── README.md
└── modules/
    ├── identity-management/
    ├── rbac-assignments/
    └── security-policies/
```

### Repository Structure Commands

```bash
# Create the Terraform examples repository
mkdir azure-identity-terraform-examples
cd azure-identity-terraform-examples
git init

# Create directory structure
mkdir -p {user-accounts,service-principals,system-assigned-mi,user-assigned-mi,guest-users,modules/{identity-management,rbac-assignments,security-policies}}

# Create README files
echo "# Azure Identity Terraform Examples" > README.md
echo "# User Accounts Example" > user-accounts/README.md
echo "# Service Principals Example" > service-principals/README.md
echo "# System-Assigned Managed Identity Example" > system-assigned-mi/README.md
echo "# User-Assigned Managed Identity Example" > user-assigned-mi/README.md
echo "# Guest Users Example" > guest-users/README.md
```

## Content Management Workflow

### Adding New Articles

1. **Create new article:**
```bash
hugo new posts/kubernetes-best-practices/index.md
```

2. **Edit the article** with frontmatter:
```yaml
---
title: "Kubernetes Security Best Practices"
date: 2025-08-23
draft: false
summary: "Production-ready Kubernetes security patterns and implementation guides."
tags: ["Kubernetes", "Security", "DevOps", "Container"]
categories: ["Kubernetes", "Security"]
---
```

3. **Test locally:**
```bash
hugo server -D
```

4. **Deploy:**
```bash
git add .
git commit -m "Add Kubernetes security article"
git push
```

### Content Ideas for Future Articles

1. **Kubernetes Security Best Practices**
   - Pod Security Standards
   - Network Policies
   - RBAC Implementation
   - Secret Management

2. **Terraform Best Practices**
   - Module Design Patterns
   - State Management
   - CI/CD Integration
   - Security Scanning

3. **Azure Architecture Patterns**
   - Multi-region Deployments
   - Disaster Recovery
   - Cost Optimization
   - Monitoring and Observability

4. **DevOps Implementation**
   - GitOps Workflows
   - Infrastructure Pipelines
   - Security Integration
   - Compliance Automation

## SEO and Performance Optimization

### Add to `hugo.yaml`:
```yaml
# SEO and social media
params:
  author: 'Your Name'
  description: 'Expert insights on Azure, Kubernetes, and cloud architecture'
  keywords: ['Azure', 'Kubernetes', 'Cloud Architecture', 'DevOps', 'Terraform']
  
  # Social media
  twitter: 'yourtwitter'
  linkedin: 'yourlinkedin'
  github: 'yourgithub'
  
  # Analytics (optional)
  googleAnalytics: 'G-XXXXXXXXXX'
```

### Performance Optimizations:
- Minified CSS and JS
- Optimized images in WebP format
- CDN delivery through Netlify
- Lazy loading for images
- Compressed assets

## Maintenance and Updates

### Regular Tasks:
1. **Update Hugo version** in `netlify.toml`
2. **Review and update articles** for accuracy
3. **Monitor site performance** using Netlify Analytics
4. **Update Terraform examples** with latest provider versions
5. **Backup content** regularly

### Analytics Setup:
```yaml
# Add to hugo.yaml
googleAnalytics: 'G-XXXXXXXXXX'

# Or use Netlify Analytics (privacy-focused)
# Available in Netlify dashboard
```

## Security Considerations

1. **Content Security Policy** (CSP) headers in `netlify.toml`
2. **HTTPS enforcement** (automatic with Netlify)
3. **Regular dependency updates** for Hugo and themes
4. **Secure handling of any API keys** or sensitive data
5. **Regular security scanning** of Terraform code

## Custom Domain Setup

1. **Purchase domain** from registrar
2. **Add custom domain** in Netlify dashboard
3. **Update DNS records** to point to Netlify
4. **Enable HTTPS** (automatic with Let's Encrypt)

```bash
# Update baseURL in hugo.yaml
baseURL: 'https://cloudarchitect.dev'
```

## Backup Strategy

1. **Git repository** backup (GitHub)
2. **Content backup** to cloud storage
3. **Database exports** if using dynamic content
4. **Regular site exports** from Netlify

Your blog is now ready for deployment! The combination of Hugo's speed, Netlify's reliability, and your expert content will create a powerful platform for sharing cloud architecture knowledge.

---

## Quick Commands Reference

```bash
# Development
hugo server -D                 # Start dev server
hugo new posts/article/index.md # Create new article
hugo --gc --minify             # Build for production

# Deployment
git add . && git commit -m "message" && git push  # Deploy via Git
netlify deploy --prod          # Direct deploy via CLI

# Maintenance
hugo mod get -u               # Update Hugo modules
netlify status                # Check deployment status
hugo --printI18nWarnings      # Check for issues
```