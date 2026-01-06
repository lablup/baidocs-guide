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

## Remote Books Synchronization

BaiDocs can sync books from remote Git repositories, allowing you to pull documentation from multiple sources. This is useful for multi-repository projects or when documentation is maintained separately from the main BaiDocs project.

### Configuration

Configure remote books using the `REMOTE_BOOKS_JSON` environment variable in `.env.local`:

```bash
# .env.local
REMOTE_BOOKS_JSON='{"books":[{"id":"my-docs","repository":"https://github.com/org/docs","branch":"main","path":"."}]}'
```

Configuration structure:

```json
{
  "books": [
    {
      "id": "backend-api-docs",           // Book directory name in content/
      "repository": "https://github.com/your-org/backend-api-docs",
      "branch": "main",                    // Branch to clone (optional, default: main)
      "path": "."                          // Subdirectory in repo (optional, default: .)
    },
    {
      "id": "frontend-guide",
      "repository": "https://github.com/your-org/frontend-guide",
      "branch": "production",
      "path": "docs"                       // Only sync docs/ subdirectory
    }
  ]
}
```

### Manual Synchronization

For local development, use the `sync:remote` command to manually sync remote books:

```bash
# From project root
pnpm sync:remote

# Or from apps/viewer
cd apps/viewer
pnpm sync:remote
```

**What it does:**
1. Clones remote repositories with full Git history
2. Creates local branches for all remote branches
3. Checkouts the specified branch
4. Preserves `.git` directory for version control

**When to use:**
- Setting up a new development environment
- Pulling latest changes from remote books
- Switching between different book versions
- After updating `REMOTE_BOOKS_JSON` configuration

:::warning Important
`pnpm sync:remote` will **delete existing remote books** in your `content/` directory (except local-only books). Always commit your local changes before running this command.
:::

### Local-Only Books

To protect certain books from being deleted during sync, configure them as local-only in `baidocs.config.yaml`:

```yaml
# baidocs.config.yaml
deployment:
  localOnlyBooks:
    - my-local-book
    - work-in-progress-docs
```

Local-only books will be preserved when running `pnpm sync:remote`.

### Production Deployment

For production builds (Vercel, Netlify), remote books are automatically synced during the build process:

```bash
# Vercel build command (configured in vercel.json or project settings)
pnpm build:vercel
```

This runs:
1. `sync:remote` - Clone remote books
2. `build` - Build all apps (viewer, editor)

Set the `REMOTE_BOOKS_JSON` environment variable in your deployment platform's settings.

### Git Branch Management

Remote books maintain full Git repositories with all branches, enabling version-specific documentation:

```bash
cd content/my-book

# View all available branches
git branch -a

# Switch to a different version
git checkout v2.0

# Return to default branch
git checkout main
```

The BaiDocs editor provides a UI for switching branches without using the command line.

### Use Cases

Remote books synchronization is perfect for:
- **Multi-repository documentation** - Pull docs from multiple projects
- **Production deployments** - Automatically sync on Vercel/Netlify
- **CI/CD pipelines** - Keep documentation up-to-date
- **Team collaboration** - Each team maintains their own docs repo
- **Version management** - Switch between documentation versions

### Troubleshooting

**Problem: Remote books not syncing**

Check your configuration:
```bash
# Verify REMOTE_BOOKS_JSON is set
echo $REMOTE_BOOKS_JSON

# Or check .env.local
cat .env.local | grep REMOTE_BOOKS_JSON
```

**Problem: Permission denied when cloning private repositories**

Set a GitHub token in `.env.local`:
```bash
GITHUB_TOKEN=ghp_xxxxxxxxxxxxxxxxxxxx
```

Create a token at: https://github.com/settings/tokens

**Problem: Local changes were lost**

Remote books sync will delete existing content. To prevent this:
1. Add books to `localOnlyBooks` in `baidocs.config.yaml`
2. Commit changes to Git before syncing
3. Keep local-only books outside of `REMOTE_BOOKS_JSON`

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
