---
title: Version Control with Git
description: Managing documentation versions with Git in BaiDocs
---

# Version Control with Git

BaiDocs is designed to work seamlessly with Git for version control. Each book can be managed as an independent Git repository, enabling team collaboration, version history, and distributed workflows.

## Book-Level Git Repositories

Each book in BaiDocs can have its own Git repository. This design allows:

- **Independent versioning** per documentation project
- **Separate access control** for different teams
- **Individual release cycles** for each book
- **Easy distribution** via Git hosting platforms

### Directory Structure

```
baidocs/
├── content/
│   ├── user-guide/          # Book 1 (can be a Git repo)
│   │   ├── .git/
│   │   ├── book.config.yaml
│   │   └── en/
│   ├── api-reference/       # Book 2 (separate Git repo)
│   │   ├── .git/
│   │   ├── book.config.yaml
│   │   └── en/
│   └── developer-guide/     # Book 3 (another Git repo)
│       ├── .git/
│       ├── book.config.yaml
│       └── en/
```

:::note
The main BaiDocs project and each book can have separate Git repositories, allowing for modular documentation management.
:::

## Setting Up Git for a Book

### Initialize a New Repository

When you create a book using the editor or manually, you can initialize it as a Git repository:

```bash
cd content/my-book
git init
git add .
git commit -m "Initial commit: Create my-book documentation"
```

### Connect to Remote Repository

Link your book to a remote repository (GitHub, GitLab, etc.):

```bash
# Add remote repository
git remote add origin https://github.com/your-org/my-book.git

# Push to remote
git push -u origin main
```

### Clone an Existing Book Repository

To add an existing documentation repository to your BaiDocs project:

```bash
cd content
git clone https://github.com/your-org/existing-docs.git
```

The cloned repository will automatically appear in your BaiDocs viewer.

## Git Workflow for Documentation

### Basic Workflow

1. **Make Changes**
   ```bash
   # Edit files in content/my-book/en/
   vim content/my-book/en/new-feature.md
   ```

2. **Check Status**
   ```bash
   cd content/my-book
   git status
   ```

3. **Stage Changes**
   ```bash
   git add en/new-feature.md
   # Or stage all changes
   git add .
   ```

4. **Commit Changes**
   ```bash
   git commit -m "docs: Add new feature documentation"
   ```

5. **Push to Remote**
   ```bash
   git push origin main
   ```

### Branching Strategy

Use branches for documentation updates:

```bash
# Create a feature branch
git checkout -b docs/update-api-reference

# Make your changes
vim en/api-reference.md

# Commit changes
git add en/api-reference.md
git commit -m "docs: Update API reference for v2.0"

# Push branch
git push origin docs/update-api-reference
```

Create a pull request for review before merging to main.

## Collaborative Documentation

### Team Workflow

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-org/documentation.git content/docs
   ```

2. **Create a branch for your changes**
   ```bash
   cd content/docs
   git checkout -b docs/my-contribution
   ```

3. **Make and commit changes**
   ```bash
   # Edit documentation
   vim en/getting-started.md

   # Commit
   git add en/getting-started.md
   git commit -m "docs: Improve getting started guide"
   ```

4. **Push and create pull request**
   ```bash
   git push origin docs/my-contribution
   ```

5. **Review and merge** via GitHub/GitLab pull request

### Commit Message Conventions

Follow a consistent commit message format:

```bash
# Good commit messages
git commit -m "docs: Add deployment guide"
git commit -m "docs: Fix typo in installation section"
git commit -m "docs: Update API examples for v2.0"
git commit -m "feat: Add new chapter on testing"
git commit -m "fix: Correct broken links in navigation"

# Include more context in the body
git commit -m "docs: Add deployment guide

- Add Vercel deployment instructions
- Add Docker deployment instructions
- Add environment variable configuration
"
```

**Recommended prefixes:**
- `docs:` - Documentation content changes
- `feat:` - New documentation features or chapters
- `fix:` - Fix errors, broken links, or typos
- `refactor:` - Restructure documentation without content changes
- `style:` - Formatting, whitespace, etc.
- `chore:` - Update configuration, assets, etc.

## Working with Remote Repositories

### Pulling Latest Changes

Keep your local documentation in sync:

```bash
cd content/my-book
git pull origin main
```

### Handling Merge Conflicts

When multiple people edit the same file:

```bash
# Pull latest changes
git pull origin main

# If conflicts occur, edit the conflicting files
vim en/conflicted-file.md

# Look for conflict markers:
# <<<<<<< HEAD
# Your changes
# =======
# Their changes
# >>>>>>> branch-name

# Resolve conflicts, then:
git add en/conflicted-file.md
git commit -m "docs: Resolve merge conflicts"
git push origin main
```

## Remote Books Configuration

BaiDocs can automatically sync books from remote Git repositories during build. Configure in `baidocs.config.yaml`:

```yaml
remoteBooks:
  - id: backend-api-docs
    repository: https://github.com/your-org/backend-api-docs
    branch: main
    path: .
  - id: frontend-guide
    repository: https://github.com/your-org/frontend-guide
    branch: production
    path: docs
```

Or use environment variable for Vercel/production:

```bash
# In .env.local or Vercel environment variables
REMOTE_BOOKS_JSON='{"books":[{"id":"my-docs","repository":"https://github.com/org/docs","branch":"main"}]}'
```

During build, BaiDocs will automatically:
1. Clone the remote repositories
2. Checkout the specified branch
3. Copy content to the `content/` directory
4. Build the documentation

This is perfect for:
- **Production deployments** (Vercel, Netlify)
- **CI/CD pipelines**
- **Multi-repository documentation**

## Version Tags and Releases

### Creating Version Tags

Mark documentation versions with Git tags:

```bash
# Tag a version
git tag -a v1.0.0 -m "Version 1.0.0 documentation"

# Push tags to remote
git push origin v1.0.0

# Push all tags
git push origin --tags
```

### Viewing Tags

```bash
# List all tags
git tag

# View tag details
git show v1.0.0
```

### Checking Out Specific Versions

```bash
# Checkout a specific version
git checkout v1.0.0

# Return to latest
git checkout main
```

## Best Practices

### 1. Use .gitignore

Create a `.gitignore` file in your book directory:

```gitignore
# Editor files
.DS_Store
.vscode/
.idea/
*.swp
*.swo

# Build artifacts (if any)
.cache/
dist/

# Temporary files
*.tmp
*~

# Do NOT ignore images directory
# images/
```

### 2. Commit Frequently

Make small, focused commits:

```bash
# Good: Small, focused commits
git commit -m "docs: Add authentication section"
git commit -m "docs: Add code examples for auth"
git commit -m "docs: Add auth troubleshooting"

# Bad: Large, unfocused commits
git commit -m "Updated everything"
```

### 3. Write Descriptive Commit Messages

Include context in commit messages:

```bash
# Good: Descriptive and specific
git commit -m "docs: Update installation steps for Windows 11

- Add PowerShell requirements
- Update path configuration
- Add troubleshooting for common errors
"

# Bad: Vague
git commit -m "Update docs"
```

### 4. Review Before Committing

Always review your changes:

```bash
# See what changed
git diff

# Review staged changes
git diff --staged

# Check which files changed
git status
```

### 5. Keep Documentation and Code Separate

If your book documents a codebase:
- Keep documentation in its own repository
- Reference specific code versions in docs
- Update docs when releasing new versions

### 6. Use Protected Branches

For important documentation:
- Protect the `main` branch
- Require pull request reviews
- Run CI checks before merging
- Maintain clean history

### 7. Document Your Changes

Add a CHANGELOG.md to track documentation updates:

```markdown
# Changelog

## [1.1.0] - 2024-01-15
### Added
- New chapter on deployment strategies
- Code examples for API v2.0

### Changed
- Updated installation instructions
- Improved navigation structure

### Fixed
- Corrected broken links in getting started guide
```

## GitHub/GitLab Integration

### Automatic Deployments

Connect your book repository to Vercel or Netlify:

1. Push changes to GitHub/GitLab
2. Automatic build triggers
3. Documentation updates automatically

### Pull Request Previews

Configure preview deployments:
- Each pull request gets a preview URL
- Review changes before merging
- Catch issues early

### GitHub Actions Example

Automate documentation checks:

```yaml
# .github/workflows/docs-check.yml
name: Documentation Check

on: [pull_request]

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Check markdown links
        run: |
          npm install -g markdown-link-check
          find . -name "*.md" -exec markdown-link-check {} \;

      - name: Check for typos
        run: |
          npm install -g cspell
          cspell "**/*.md"
```

## Troubleshooting

### Large Files

If you have large images or assets:

```bash
# Check repository size
du -sh .git

# Use Git LFS for large files
git lfs install
git lfs track "*.png"
git lfs track "*.jpg"
git add .gitattributes
git commit -m "chore: Add Git LFS for images"
```

### Accidentally Committed Secrets

If you committed sensitive data:

```bash
# Remove from history (use carefully!)
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch path/to/secret-file" \
  --prune-empty --tag-name-filter cat -- --all

# Force push (coordinate with team!)
git push origin --force --all
```

Better: Use `.gitignore` to prevent committing secrets.

### Repository Too Large

If your book repository grows too large:

1. **Move old versions to archives**
2. **Use Git LFS for binary files**
3. **Remove unnecessary build artifacts**
4. **Consider splitting into multiple books**

## Next Steps

Now that you understand version control in BaiDocs, learn about:
- [Multi-language Support](multi-language.md) for translating your documentation
- [Deployment Options](deployment-options.md) for publishing your versioned docs

---

:::tip
Git makes documentation management scalable. Start simple with basic commits, and adopt more advanced workflows as your team grows.
:::
