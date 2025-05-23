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
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.x'
      - run: pip install mkdocs mkdocs-material
      - run: mkdocs gh-deploy --clean
        env:
          GITHUB_TOKEN: ${{ secrets.GH_DEPLOY_TOKEN }}
```