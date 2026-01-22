# Version Control Integration

BaiDocs provides comprehensive Git integration for collaborative documentation workflows, change tracking, and deployment automation. This section covers configuration and best practices for version control implementation.

## Git Integration Overview

### Built-in Git Functionality

BaiDocs includes native Git support featuring:

- **Repository management**: Clone, pull, push, and fetch operations
- **Branch workflows**: Feature branch creation and management
- **Conflict resolution**: Built-in merge conflict detection and resolution
- **Commit automation**: Automatic commit generation with descriptive messages
- **Remote synchronization**: Integration with GitHub, GitLab, and other Git providers

### Integration Architecture

```
BaiDocs Editor
    ↓
MonorepoGit / SimpleGit
    ↓
Local Git Repository
    ↓
Remote Git Provider (GitHub/GitLab)
```

## Repository Setup

### New Repository Initialization

#### Local Repository Creation

1. **Initialize Git repository**:
   ```bash
   cd content/your-book
   git init
   git add .
   git commit -m "Initial documentation setup"
   ```

2. **Configure remote repository**:
   ```bash
   git remote add origin https://github.com/your-org/documentation.git
   git push -u origin main
   ```

#### BaiDocs Configuration

Update `book.config.yaml` to include repository information:

```yaml
repoUrl: https://github.com/your-org/documentation
branch: main
type: standalone  # or monorepo
```

### Existing Repository Integration

#### Standalone Repository

For dedicated documentation repositories:

1. **Clone existing repository**:
   - Use BaiDocs editor "Import from Git" functionality
   - Provide repository URL and authentication credentials
   - Select target directory for content

2. **Verify configuration**:
   - Confirm `book.config.yaml` exists and contains proper settings
   - Validate content directory structure
   - Test navigation and content rendering

#### Monorepo Integration

For documentation within larger codebases:

1. **Configure monorepo settings**:
   ```yaml
   type: monorepo
   subdirectory: docs
   selectedBranches:
     - main
     - develop
     - documentation-updates
   ```

2. **Implement sparse checkout**:
   - BaiDocs automatically configures sparse checkout for monorepos
   - Only documentation files are synchronized locally
   - Reduces clone time and storage requirements

## Workflow Patterns

### Feature Branch Workflow

#### Branch Creation

1. **Create feature branch** through BaiDocs interface:
   - Navigate to Git panel in editor
   - Click "Create Branch" button
   - Specify branch name following naming conventions

2. **Alternative command-line creation**:
   ```bash
   git checkout -b feature/user-guide-updates
   git push -u origin feature/user-guide-updates
   ```

#### Content Development

1. **Make content changes** using the BaiDocs editor
2. **Commit changes** with descriptive messages:
   - BaiDocs automatically generates commit messages
   - Manual commit message customization available
   - Include co-author attribution for collaborative work

3. **Push changes** to remote repository:
   - Use BaiDocs push functionality
   - Configure automatic push on commit (optional)

### Collaboration Workflow

#### Multi-author Coordination

1. **Branch assignment**: Assign specific branches to team members
2. **Content review**: Implement pull request workflows for quality control
3. **Conflict resolution**: Use BaiDocs conflict resolution interface

#### Merge Strategies

- **Squash merging**: Consolidate multiple commits for clean history
- **Merge commits**: Preserve detailed development history
- **Rebase workflows**: Maintain linear commit history

## Authentication and Security

### Authentication Methods

#### HTTPS Authentication

1. **Personal access tokens** (recommended):
   ```bash
   git config --global credential.helper store
   # Provide username and personal access token when prompted
   ```

2. **Username/password authentication**:
   - Configure through BaiDocs settings
   - Store credentials securely in system keychain

#### SSH Key Authentication

1. **Generate SSH key pair**:
   ```bash
   ssh-keygen -t ed25519 -C "your-email@company.com"
   ```

2. **Add public key to Git provider**:
   - Copy public key content to clipboard
   - Add key to GitHub/GitLab account settings

3. **Configure BaiDocs**:
   - Update repository URL to use SSH format
   - Test connection through BaiDocs interface

### Security Best Practices

#### Credential Management

- **Token rotation**: Regularly rotate personal access tokens
- **Scope limitation**: Use minimal required permissions for tokens
- **Environment isolation**: Separate credentials for development and production

#### Access Control

- **Repository permissions**: Configure appropriate read/write access
- **Branch protection**: Implement protection rules for main branches
- **Review requirements**: Mandate code review for production changes

## Advanced Git Features

### Monorepo-specific Functionality

#### Sparse Checkout Configuration

BaiDocs automatically manages sparse checkout for monorepos:

```bash
# Automatically configured by BaiDocs
git config core.sparseCheckout true
echo "docs/*" > .git/info/sparse-checkout
git read-tree -m -u HEAD
```

#### Branch Filtering

Configuration for displaying relevant branches only:

```yaml
selectedBranches:
  - main
  - develop
  - docs/*         # Pattern matching for documentation branches
```

### Conflict Resolution

#### Automated Conflict Detection

BaiDocs provides built-in conflict detection:

1. **Pre-commit validation**: Check for potential conflicts before commit
2. **Pull conflict detection**: Identify conflicts during pull operations
3. **Visual conflict resolution**: Side-by-side comparison interface

#### Manual Resolution Process

1. **Identify conflicted files** through BaiDocs interface
2. **Review conflicting changes** using diff view
3. **Select resolution strategy**:
   - Accept incoming changes
   - Keep current changes
   - Manual merge of both changes
4. **Test resolved content** before committing

### Hooks and Automation

#### Git Hooks Integration

BaiDocs supports custom Git hooks for workflow automation:

```bash
# Pre-commit hook example
#!/bin/sh
# Validate markdown formatting
npm run lint:markdown

# Check for broken links
npm run check:links

# Update last modified timestamps
npm run update:timestamps
```

#### CI/CD Integration

Configure continuous integration for automated workflows:

```yaml
# GitHub Actions example
name: Documentation Build
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Build documentation
        run: npm run build
      - name: Deploy to production
        run: npm run deploy
```

## Best Practices

### Commit Message Conventions

#### Standardized Format

```
type(scope): description

feat(user-guide): add installation instructions
fix(api-ref): correct parameter documentation
docs(readme): update getting started section
```

#### BaiDocs Automation

BaiDocs automatically generates commit messages with:
- File change summaries
- Co-author attribution
- Standardized formatting

### Branch Management

#### Naming Conventions

```
feature/section-name-updates
hotfix/broken-link-repair
docs/api-v2-documentation
release/version-2.0
```

#### Lifecycle Management

1. **Regular cleanup**: Remove merged and abandoned branches
2. **Protection rules**: Configure main branch protection
3. **Review workflows**: Implement mandatory review processes

### Documentation Versioning

#### Release Tagging

```bash
# Create version tags for documentation releases
git tag -a v1.0.0 -m "Documentation version 1.0.0"
git push origin v1.0.0
```

#### Branch-based Versioning

- **Main branch**: Current documentation version
- **Release branches**: Stable documentation snapshots
- **Feature branches**: Development and preview content

Version control integration enables collaborative documentation development while maintaining content quality and deployment reliability.