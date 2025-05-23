# ✅ How to Deploy MkDocs to GitHub Pages

## ✅ 1. Prerequisites

Make sure you have the following:

1. A GitHub repo (e.g., `my-repo`) containing your MkDocs project  
2. A `mkdocs.yml` configuration file  
3. Your documentation is in the default `/docs` folder  

---

## ✅ 2. (Optional but Recommended) Update `mkdocs.yml`

Add or update the following settings:

```yaml
site_name: My Project
site_url: https://<your-username>.github.io/<your-repo>/
```

This helps MkDocs generate correct links in your site.

---

## ✅ 3. Create a Python Virtual Environment

```bash
python3 -m venv .venv
```

---

## ✅ 4. Activate the Virtual Environment

```bash
source .venv/bin/activate
```

Your prompt will change to indicate you're in the `.venv` environment.

---

## ✅ 5. Install MkDocs and the Material Theme

```bash
pip install mkdocs mkdocs-material
```

> You can add more plugins later as needed.

---

## ✅ 6. Create a `.gitignore` File

```gitignore
# MkDocs build output
/site/

# Python virtual environment
.venv/

# Python cache
__pycache__/
*.py[cod]
*.pyo
*.pyd

# Editor files
.vscode/
.idea/
*.swp
*~

# macOS files
.DS_Store

# Logs and misc
*.log
```

---

## ✅ 7. Preview Your Site Locally

```bash
mkdocs serve
```

Open your browser at [http://127.0.0.1:8000](http://127.0.0.1:8000) to preview your docs.

---

## ✅ 8. Deploy to GitHub Pages

```bash
mkdocs gh-deploy --clean
```

This command will:
- Build your site
- Push the contents to the `gh-pages` branch
- Clean out old files

---

## 🛠️ What Does `mkdocs gh-deploy` Do?

**Default behavior:**
- Uses the `origin` remote
- Assumes you're deploying from the current repo
- Cleans the `gh-pages` branch to remove stale files
- Commits a message like: `Deployed {timestamp} with MkDocs {version}`

**Optional flags:**
- `--clean` – remove old files before deploying
- `--message "Custom commit message"` – use a custom commit message
- `--remote-name origin` – use a different remote
- `--branch gh-pages` – deploy to a different branch

**⚠️ Important:**
- It **overwrites** the `gh-pages` branch
- You must have **write access** to the repo
- For **private repos**, set up authentication (e.g., via a GitHub token)
