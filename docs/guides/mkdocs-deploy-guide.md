# Deploying MkDocs to GitHub Pages

A step-by-step guide to get your MkDocs documentation site hosted on GitHub Pages with automated deployment.

## Prerequisites

Before starting, ensure you have:

- A GitHub repository containing your MkDocs project
- Python 3.7+ installed on your system
- Git configured with your GitHub credentials
- A `mkdocs.yml` configuration file in your repository root
- Documentation files in a `/docs` folder (or your configured docs directory)

## Project Setup

### Configure Your Site Settings

Update your `mkdocs.yml` file with the proper site configuration:

```yaml
site_name: Your Project Name
site_url: https://your-username.github.io/your-repo-name/
site_description: A brief description of your project
site_author: Your Name

theme:
  name: material  # or your preferred theme

nav:
  - Home: index.md
  - Getting Started: getting-started.md
  # Add your navigation structure here
```

**Why this matters:** The `site_url` ensures all internal links work correctly when deployed to GitHub Pages.

### Set Up Your Development Environment

Create and activate a Python virtual environment to isolate your project dependencies:

```bash
# Create virtual environment
python3 -m venv .venv

# Activate it (Linux/macOS)
source .venv/bin/activate

# On Windows
.venv\Scripts\activate
```

Install the required packages:

```bash
pip install mkdocs mkdocs-material
```

For additional functionality, consider these popular plugins:

```bash
pip install mkdocs-awesome-pages-plugin mkdocs-git-revision-date-localized-plugin
```

### Create Essential Project Files

Add a `.gitignore` file to exclude build artifacts and development files:

```gitignore
# MkDocs build output
/site/

# Python virtual environment
.venv/
venv/

# Python cache files
__pycache__/
*.py[cod]
*$py.class
*.so
*.egg-info/

# Development tools
.vscode/
.idea/
*.swp
*.swo
*~

# System files
.DS_Store
Thumbs.db

# Logs
*.log
```

Create a `requirements.txt` file to track dependencies:

```bash
pip freeze > requirements.txt
```

## Local Development and Testing

Before deploying, always test your site locally:

```bash
mkdocs serve
```

This starts a development server at `http://127.0.0.1:8000` with live reloading. Make sure all your pages load correctly and navigation works as expected.

To build the static site without serving:

```bash
mkdocs build
```

This creates a `/site` directory with your compiled documentation.

## Deployment to GitHub Pages

### Method 1: Using MkDocs Built-in Deployment

The simplest approach uses MkDocs' built-in GitHub Pages deployment:

```bash
mkdocs gh-deploy
```

This command automatically:
- Builds your documentation site
- Creates or updates the `gh-pages` branch
- Pushes the built site to GitHub
- Cleans up old files to prevent stale content

**Advanced deployment options:**

```bash
# Clean deployment with custom message
mkdocs gh-deploy --clean --message "Deploy documentation update {version}"

# Deploy from a different branch
mkdocs gh-deploy --remote-branch main

# Use a different remote
mkdocs gh-deploy --remote-name upstream
```

### Method 2: GitHub Actions (Recommended for Teams)

For automated deployments on every push, create `.github/workflows/docs.yml`:

```yaml
name: Deploy Documentation

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v4
      with:
        fetch-depth: 0
    
    - name: Setup Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.11'
    
    - name: Install dependencies
      run: |
        pip install -r requirements.txt
    
    - name: Build documentation
      run: mkdocs build
    
    - name: Upload artifact
      uses: actions/upload-pages-artifact@v2
      with:
        path: ./site
  
  deploy:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    
    steps:
    - name: Deploy to GitHub Pages
      id: deployment
      uses: actions/deploy-pages@v2
```

## GitHub Repository Configuration

### Enable GitHub Pages

1. Go to your repository on GitHub
2. Navigate to **Settings** → **Pages**
3. Under **Source**, select:
   - **Deploy from a branch** if using `mkdocs gh-deploy`
   - **GitHub Actions** if using the workflow method
4. If using branch deployment, select `gh-pages` as the branch
5. Click **Save**

### Set Up Repository Secrets (if needed)

For private repositories or advanced setups, you may need to configure authentication:

1. Go to **Settings** → **Secrets and variables** → **Actions**
2. Add necessary secrets like `GITHUB_TOKEN` (usually provided automatically)

## Troubleshooting Common Issues

**Site not updating after deployment:**
- Check that GitHub Pages is configured correctly in repository settings
- Verify the `site_url` in your `mkdocs.yml` matches your GitHub Pages URL
- Clear your browser cache or try an incognito window

**Build failures:**
- Ensure all markdown files are properly formatted
- Check for broken internal links using `mkdocs build --strict`
- Verify all required plugins are listed in `requirements.txt`

**Permission errors:**
- Confirm you have write access to the repository
- For organization repositories, check if GitHub Actions are enabled

**Custom domain setup:**
- Add a `CNAME` file to your `/docs` folder containing your domain
- Configure DNS settings with your domain provider
- Update `site_url` in `mkdocs.yml` to use your custom domain

## Best Practices

**Content Organization:**
- Use clear navigation structure in `mkdocs.yml`
- Organize files logically in your `/docs` folder
- Include a comprehensive `index.md` as your homepage

**Performance:**
- Optimize images and use appropriate formats
- Consider using CDN links for external resources
- Enable site compression if using custom themes

**Maintenance:**
- Regularly update dependencies in `requirements.txt`
- Test locally before deploying significant changes
- Use semantic commit messages for better change tracking

**Collaboration:**
- Document your build process in a `README.md`
- Use branch protection rules for important repositories
- Consider using pull request reviews for documentation changes

Your documentation site will be available at `https://your-username.github.io/your-repo-name/` once deployment completes. Changes typically take 1-2 minutes to appear after successful deployment.