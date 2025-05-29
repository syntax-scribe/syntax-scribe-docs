---
title: "Getting Started"
author: "Patrick Wright"
published_date: "05/01/2025"
---

# Getting Started with Syntax Scribe

Welcome to Syntax Scribe, a powerful tool for automatically generating beautiful code documentation from your TypeScript and JavaScript projects. This guide will walk you through installation, basic usage, and creating your first documentation site.

## What is Syntax Scribe?

Syntax Scribe analyzes your TypeScript and JavaScript codebases to automatically generate comprehensive documentation. It integrates seamlessly with MkDocs to create professional documentation sites with minimal configuration.

**Key Features:**
- Automatic code analysis and documentation generation
- MkDocs integration with Material theme
- Support for TypeScript and JavaScript projects
- Built-in navigation generation
- Local development server
- Static site generation for easy deployment

## Installation

Install Syntax Scribe globally using npm:

```bash
npm install -g syntax-scribe
```

**System Requirements:**
- Node.js 14+ 
- npm or yarn package manager
- Git (for project analysis)

## Quick Start

### Step 1: Generate Documentation

Generate documentation from your project source code:

```bash
# Basic documentation generation
syntax-scribe docs document -s "~/code/my-project" -d "~/code/my-docs/docs"

# With license key (for advanced features)
syntax-scribe docs document -s "~/code/my-project" -d "~/code/my-docs/docs" -l your-license-key
```

**Command Options:**
- `-s, --source`: Path to your source code directory
- `-d, --destination`: Path where documentation will be generated
- `-l, --license`: Your Syntax Scribe license key (optional)

### Step 2: Initialize MkDocs Site

Create a complete MkDocs site structure around your generated documentation:

```bash
syntax-scribe docs init -d "~/code/my-docs" -n "My Project Documentation" -l your-license-key
```

**What this creates:**
- `mkdocs.yml` configuration file
- Proper site structure with Material theme
- Integration with your generated documentation

**Command Options:**
- `-d, --directory`: Project root directory (one level above your docs folder)
- `-n, --name`: Display name for your documentation site
- `-l, --license`: Your license key (required for MkDocs features)


### Step 3: Preview Your Site

Start a local development server to preview your documentation:

```bash
syntax-scribe docs serve -d "~/code/my-docs" -l your-license-key
```

**What happens behind the scenes:**
- Creates a `.venv` Python virtual environment in your project directory
- Installs MkDocs, Material theme, and required plugins in the virtual environment
- Uses MkDocs to serve your site locally

Your site will be available at `http://localhost:8000` with live reloading for development.

**Running the site independently:**
If you want to run your documentation site again without using Syntax Scribe, you can use MkDocs directly:

```bash
# Navigate to your documentation project directory
cd ~/code/my-docs

# Activate the virtual environment
source .venv/bin/activate

# Run MkDocs serve
mkdocs serve
```

To deactivate the virtual environment when you're done, simply run:
```bash
deactivate
```

### Step 5: Build for Production

Generate static files ready for deployment:

```bash
syntax-scribe docs build -d "~/code/my-docs" -l your-license-key
```

The built site will be created in the `site/` directory, ready for hosting.

You can also build using MkDocs directly after activating the virtual environment:
```bash
source .venv/bin/activate
mkdocs build
```

## Complete Example Workflow

Here's a complete example documenting a project called "CopilotKit":

```bash
# 1. Generate documentation from source
syntax-scribe docs document \
  -s "~/code/CopilotKit" \
  -d "~/code/copilotkit-docs/docs" \
  -l your-license-key

# 2. Initialize MkDocs site structure
syntax-scribe docs init \
  -d "~/code/copilotkit-docs" \
  -n "CopilotKit Documentation" \
  -l your-license-key

# 3. Preview locally
syntax-scribe docs serve \
  -d "~/code/copilotkit-docs" \
  -l your-license-key
```

## Project Structure

After running the initialization commands, your project structure will look like this:

```
copilotkit-docs/
├── mkdocs.yml           # MkDocs configuration
├── .venv/               # Python virtual environment (created after first serve)
├── docs/                # Generated documentation
│   ├── index.md         # Homepage
│   ├── api/             # API documentation
│   └── guides/          # Additional guides
└── site/                # Built static site (after build)
```

## Directory Path Tips

**Important considerations for directory paths:**

- **Source directory (`-s`)**: Point to your project's root or main source directory
- **Destination directory (`-d`)**: 
  - For documentation generation: Use a `docs/` subdirectory
  - For site initialization: Use the parent directory (one level up from `docs/`)
- **Use absolute paths or proper relative paths** to avoid confusion
- **Avoid spaces in directory names** for better cross-platform compatibility

**Example directory structure:**
```
~/code/
├── my-project/          # Your source code (use for -s)
└── my-project-docs/     # Documentation project
    ├── mkdocs.yml       # Use parent directory for init (-d)
    ├── .venv/           # Virtual environment (auto-created)
    └── docs/            # Generated docs go here
```

## Deployment Options

Your built documentation site consists of static files that can be hosted anywhere:

### GitHub Pages
Follow our [GitHub Pages deployment guide](guides/mkdocs-deploy-guide.md) for step-by-step instructions.

### Other Hosting Options
- **Netlify**: Drag and drop the `site/` folder
- **Vercel**: Connect your repository for automatic deployments  
- **AWS S3**: Upload static files to an S3 bucket
- **Traditional web hosting**: Upload `site/` contents via FTP

For detailed deployment instructions, see the [MkDocs deployment documentation](https://www.mkdocs.org/user-guide/deploying-your-docs/).

## Troubleshooting

**Common Issues:**

**"Command not found" error**
- Ensure Syntax Scribe is installed globally: `npm install -g syntax-scribe`
- Check your PATH includes npm global binaries

**Documentation not generating correctly**
- Verify your source path points to a valid TypeScript/JavaScript project
- Ensure the destination directory exists or can be created
- Check file permissions for the destination directory

**License key issues**
- Verify your license key is valid and active
- Some features require a valid license key to function

**Local server not starting**
- Ensure port 8000 is available (or specify different port)
- Check that `mkdocs.yml` exists in the specified directory
- If running MkDocs directly, make sure the virtual environment is activated

**Virtual environment issues**
- If `.venv` directory is missing, re-run `syntax-scribe docs serve` to recreate it
- On Windows, use `.venv\Scripts\activate` instead of `source .venv/bin/activate`
- Ensure Python 3.6+ is installed on your system

## Next Steps

Once you have your basic documentation site running:

1. **Customize your content** - Edit the generated markdown files in your `docs/` directory
2. **Configure your theme** - Modify `mkdocs.yml` to customize colors, fonts, and layout
3. **Add custom pages** - Create additional guides, tutorials, or reference materials
4. **Set up automated deployment** - Use GitHub Actions or other CI/CD tools for automatic updates

## Additional Resources

**MkDocs Documentation:**
- [MkDocs Getting Started Guide](https://www.mkdocs.org/getting-started/)
- [MkDocs User Guide](https://www.mkdocs.org/user-guide/)
- [MkDocs Developer Guide](https://www.mkdocs.org/dev-guide/)

**Material Theme:**
- [Material for MkDocs Documentation](https://squidfunk.github.io/mkdocs-material/)
- [Material Theme Configuration](https://squidfunk.github.io/mkdocs-material/setup/)

**Syntax Scribe Resources:**
- [Advanced Configuration Options](https://syntax-scribe.github.io/syntax-scribe-docs/) 

---

**Need help?** Check our troubleshooting section above or reach out to our support team with any questions about getting started with Syntax Scribe.

