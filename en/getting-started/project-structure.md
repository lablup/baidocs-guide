# Project Structure

Understanding BaiDocs project organization is essential for effective content management and collaboration. This section details the file system structure and organizational principles.

## Directory Architecture

### Root-level Structure

```
baidocs/
├── apps/
│   ├── editor/          # Content management interface
│   └── viewer/          # Documentation presentation layer
├── packages/
│   ├── ui/              # Shared UI components
│   └── mdx-editor/      # MDX processing engine
├── content/
│   ├── book-one/        # Individual documentation books
│   ├── book-two/
│   └── shared-assets/   # Cross-book resources
├── .env.local           # Environment configuration
├── package.json         # Project dependencies
└── turbo.json          # Monorepo build configuration
```

### Content Directory Organization

#### Standalone Book Structure

```
content/
└── my-book/
    ├── book.config.yaml    # Book configuration
    ├── en/                 # English content
    │   ├── introduction/
    │   │   ├── index.md
    │   │   └── overview.md
    │   └── user-guide/
    │       └── getting-started.md
    ├── ko/                 # Korean content
    │   ├── introduction/
    │   │   ├── index.md
    │   │   └── overview.md
    │   └── user-guide/
    │       └── getting-started.md
    └── images/             # Shared image assets
        ├── screenshot1.png
        └── diagram.svg
```

#### Monorepo Book Structure

For documentation within larger codebases:

```
project-repository/
├── src/                    # Application source code
├── docs/                   # Documentation subdirectory
│   ├── book.config.yaml
│   ├── en/
│   ├── ko/
│   └── images/
└── README.md
```

## Configuration Files

### Book Configuration (`book.config.yaml`)

The primary configuration file defining book metadata and navigation structure:

#### Required Fields

```yaml
id: unique-book-identifier
title: Book Display Title
description: Book description for metadata
type: standalone  # or 'monorepo'
languages:
  - en
  - ko
defaultLanguage: en
```

#### Navigation Configuration

```yaml
navigation:
  en:
    - title: Section Name
      path: section-directory
      items:
        - title: Page Title
          path: section-directory/page-file.md
```

#### Optional Configuration

```yaml
emoji: 📖                    # Display icon
repoUrl: https://github.com/org/repo
pdf:
  split: chapter             # PDF generation settings
  cover:
    title: Custom PDF Title
    subtitle: PDF Subtitle
```

### Environment Configuration (`.env.local`)

```env
# Content directory location
CONTENT_PATH=./content

# Application ports
EDITOR_PORT=3000
VIEWER_PORT=3001

# Development mode settings
NODE_ENV=development

# Optional: Git integration
GIT_AUTHOR_NAME="Documentation Team"
GIT_AUTHOR_EMAIL="docs@company.com"
```

## Content Organization Principles

### Hierarchical Structure

#### Section-based Organization

- **Logical grouping**: Organize content by functional areas or user workflows
- **Consistent depth**: Maintain similar hierarchical depth across sections
- **Clear naming**: Use descriptive directory and file names

#### File Naming Conventions

```
✅ Recommended:
getting-started.md
user-authentication.md
api-reference.md

❌ Avoid:
GettingStarted.md
user_authentication.md
1-getting-started.md
```

### Multi-language Coordination

#### Synchronized Structure

Maintain identical directory structures across all languages:

```
en/section/subsection/page.md
ko/section/subsection/page.md  # Same path structure
ja/section/subsection/page.md
```

#### Translation Management

- **Filename consistency**: Use identical filenames across languages
- **Navigation synchronization**: Keep navigation structure aligned
- **Asset sharing**: Share images and media files across languages

### Asset Management

#### Image Organization

```
images/
├── screenshots/
│   ├── editor-interface.png
│   └── viewer-navigation.png
├── diagrams/
│   ├── architecture.svg
│   └── workflow.svg
└── logos/
    ├── company-logo.png
    └── partner-logos/
```

#### Best Practices for Assets

- **Descriptive naming**: Use clear, descriptive filenames
- **Optimization**: Compress images appropriately for web delivery
- **Format selection**: Use appropriate formats (PNG for screenshots, SVG for diagrams)
- **Version control**: Include assets in version control for consistency

## Path Resolution

### Virtual Path Mapping

BaiDocs implements virtual path mapping for seamless content access:

#### UI Paths vs. File System Paths

```
UI Path:        en/getting-started/installation.md
File System:    content/book/en/getting-started/installation.md
Git Path:       en/getting-started/installation.md (standalone)
                docs/en/getting-started/installation.md (monorepo)
```

#### Navigation Path Configuration

- **Section paths**: Directory names without extensions
- **File paths**: Relative paths from language root
- **Index files**: Use `index.md` for section landing pages

### Monorepo Considerations

#### Subdirectory Configuration

For monorepo books, specify the documentation subdirectory:

```yaml
type: monorepo
subdirectory: docs
localPath: /path/to/repository
virtualRoot: /path/to/repository/docs
```

#### Branch Management

- **Branch filtering**: Display only relevant documentation branches
- **Sparse checkout**: Clone only documentation subdirectories
- **Path isolation**: Ensure operations remain within documentation boundaries

## Directory Permissions and Security

### File System Access

- **Read permissions**: Required for content directories
- **Write permissions**: Necessary for content editing and Git operations
- **Execution permissions**: Required for build and deployment scripts

### Security Considerations

- **Path traversal prevention**: Validate all file paths
- **Access control**: Implement appropriate user permissions
- **Git security**: Secure credential management for repository access

## Maintenance and Organization

### Regular Maintenance Tasks

1. **Asset optimization**: Periodically review and optimize image files
2. **Link validation**: Verify internal and external link accuracy
3. **Structure review**: Assess navigation organization effectiveness
4. **Translation updates**: Maintain language synchronization

### Scalability Considerations

- **Performance monitoring**: Track build and load times
- **Content modularization**: Consider splitting large books
- **Asset CDN**: Implement content delivery networks for large deployments
- **Search optimization**: Maintain search index efficiency

Understanding this project structure enables effective content management and facilitates collaboration across teams and languages.