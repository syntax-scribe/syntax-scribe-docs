# Deploy MkDocs to GitHub Pages with GitHub Actions

This guide shows you how to automatically deploy your MkDocs site to GitHub Pages using GitHub Actions whenever you push changes to your repository.

## Prerequisites

- A GitHub repository with your MkDocs project
- MkDocs configuration file (`mkdocs.yml`) in your repository root

## Step 1: Configure Your MkDocs Site

Update your `mkdocs.yml` file with the correct site information:

```yaml
site_name: My Project Documentation
site_url: https://<your-username>.github.io/<your-repo>/
site_description: Description of your project

# Optional: Add a theme for better appearance
theme:
  name: material  # or readthedocs, etc.
```

**Replace** `<your-username>` with your GitHub username and `<your-repo>` with your repository name.

## Step 2: Create the GitHub Actions Workflow

Create the following directory structure in your repository:
```
your-repo/
├── .github/
│   └── workflows/
│       └── deploy.yml
├── docs/
├── mkdocs.yml
└── requirements.txt (optional)
```

Create `.github/workflows/deploy.yml` with the following content:

```yaml
name: Deploy MkDocs to GitHub Pages

on:
  push:
    branches: [ main ]
  workflow_dispatch:  # Allows manual triggering

permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment
concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0  # Fetch full history for git info

      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.x'

      - name: Cache pip dependencies
        uses: actions/cache@v4
        with:
          path: ~/.cache/pip
          key: ${{ runner.os }}-pip-${{ hashFiles('**/requirements.txt') }}
          restore-keys: |
            ${{ runner.os }}-pip-

      - name: Install MkDocs and dependencies
        run: |
          pip install --upgrade pip
          pip install mkdocs mkdocs-material
          # If you have a requirements.txt file:
          # pip install -r requirements.txt

      - name: Build MkDocs site
        run: mkdocs build --clean

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./site

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

## Step 3: Enable GitHub Pages

1. Go to your repository on GitHub
2. Navigate to **Settings** → **Pages**
3. Under **Source**, select **GitHub Actions**
4. Save the settings

## Step 4: Deploy Your Site

Push your changes to the `main` branch:

```bash
git add .
git commit -m "Add GitHub Actions workflow for MkDocs deployment"
git push origin main
```

The workflow will automatically run and deploy your site. You can monitor the progress in the **Actions** tab of your repository.

## Your Site URL

After successful deployment, your MkDocs site will be available at:

**https://`<your-username>`.github.io/`<your-repo>`/**

## Optional Enhancements

### Custom Dependencies

If your MkDocs site uses additional plugins or themes, create a `requirements.txt` file:

```txt
mkdocs>=1.5.0
mkdocs-material>=9.0.0
mkdocs-mermaid2-plugin
# Add other dependencies as needed
```

Then update the workflow to use it:

```yaml
- name: Install MkDocs and dependencies
  run: |
    pip install --upgrade pip
    pip install -r requirements.txt
```

### Custom Domain

To use a custom domain, add a `CNAME` file to your `docs/` directory:

```
your-domain.com
```

### Branch Protection

Consider protecting your `main` branch to require pull request reviews before merging, ensuring quality control over your documentation.

## Troubleshooting

- **Build fails**: Check the Actions logs for specific error messages
- **Site not updating**: Ensure you're pushing to the correct branch (`main`)
- **404 errors**: Verify your `site_url` in `mkdocs.yml` matches your repository structure
- **Permission issues**: Make sure GitHub Actions has the necessary permissions in your repository settings