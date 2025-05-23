# ddd

1. Generate a Personal Access Token
Go to  https://github.com/settings/personal-access-tokens and create a classic token with:

the repo check box checked

Expiration: Choose your preference (30 days or more)

2. Add the PAT as a GitHub Secret
Go to your repo → Settings → Secrets and variables → Actions → New repository secret:

Name: GH_DEPLOY_TOKEN

Value: (paste your PAT)


create the workflow in `.github/workflows/deploy.yml`:
```
name: Deploy MkDocs to GitHub Pages

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

permissions:
  contents: write
  pages: write
  id-token: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
      with:
        fetch-depth: 0
    
    - name: Setup Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.x'
    
    - name: Install dependencies
      run: |
        pip install mkdocs mkdocs-material
    
    - name: Deploy to GitHub Pages
      run: |
        git config --global user.name 'github-actions[bot]'
        git config --global user.email 'github-actions[bot]@users.noreply.github.com'
        mkdocs gh-deploy --clean --force
```