---
title: Key Features
description: Explore the powerful features that make BaiDocs stand out
---

# Key Features

BaiDocs is packed with features designed to make documentation easier and more powerful. Let's explore the key capabilities that set it apart.

## 📝 MDX Support

BaiDocs uses **MDX (Markdown + JSX)**, giving you the best of both worlds:

### Standard Markdown

Write in familiar Markdown syntax:

```markdown
# Headings
**Bold** and *italic* text
- Lists
- More lists
[Links](https://example.com)
```

### GitHub Flavored Markdown (GFM)

Support for tables, task lists, and more:

- [x] Task lists with checkboxes
- [x] Tables for structured data
- [x] ~~Strikethrough~~ text
- [ ] Autolinks

### Extensible with React

When you need more power, add React components:

```jsx
<CustomComponent prop="value">
  Content here
</CustomComponent>
```

:::info
Currently, BaiDocs provides a rich set of markdown extensions. Custom React components can be added by extending the MDX renderer.
:::

## 🎨 Theme System

BaiDocs includes a powerful build-time theme system:

### Available Themes

1. **default** - Clean Ant Design-based theme
2. **backend-ai** - Colorful theme matching Backend.AI branding

### Theme Customization

Themes use CSS custom properties for easy customization:

```css
:root {
  --primary-color: #1890ff;
  --text-color: #333;
  --background-color: #fff;
}
```

### Creating Custom Themes

Create your own theme in three steps:

1. Create `apps/viewer/themes/my-theme/theme.css`
2. Define your CSS variables and styles
3. Update `baidocs.config.yaml`:

```yaml
theme:
  name: "my-theme"
  path: "themes"
```

:::tip
Use CSS custom properties to make your theme easily maintainable and consistent.
:::

## 🔍 Search System

BaiDocs includes a powerful client-side search system:

### Features

- **Fast fuzzy search** powered by Fuse.js
- **Build-time indexing** - no server required
- **Full-text search** across all content
- **Book and language filtering**
- **Keyboard shortcuts** (Cmd/Ctrl + K)

### How It Works

During build time, BaiDocs:

1. Crawls all your markdown files
2. Extracts text content
3. Creates a search index (~2MB for 300 pages)
4. Saves as static JSON file

```bash
# Build search index
pnpm build:search
```

The result is instant client-side search with no backend needed!

:::success
Search results include context highlighting and smart ranking based on relevance.
:::

## 📄 PDF Generation

Generate beautiful PDFs automatically during build:

### PDF Options

**Single PDF**: All content in one file

```yaml
pdf:
  split: single
  cover:
    title: "Complete Guide"
    subtitle: "Version 1.0"
```

**Chapter PDFs**: Separate PDF per chapter

```yaml
pdf:
  split: chapter
  cover:
    title: "Documentation"
```

### Image Optimization

PDFs support automatic image optimization:

```yaml
pdf:
  imageOptimization:
    enabled: true
    maxWidth: 1200
    quality: 85
```

### Building with PDFs

```bash
# Enable PDF generation
ENABLE_PDF_GENERATION=true pnpm build
```

:::warning
PDF generation requires Puppeteer and can increase build time significantly. Disable it for faster development builds.
:::

## 🌍 Multi-language Support

First-class support for multilingual documentation:

### Language Configuration

```yaml
languages:
  - code: "en"
    name: "English"
  - code: "ko"
    name: "한국어"
  - code: "ja"
    name: "日本語"
defaultLanguage: "en"
```

### Directory Structure

Each language has its own directory:

```
content/my-book/
├── en/
│   ├── index.md
│   └── guide.md
├── ko/
│   ├── index.md
│   └── guide.md
└── images/  # Shared across languages
```

### Independent Navigation

Each language can have different navigation:

```yaml
navigation:
  en:
    - title: Getting Started
      items:
        - title: Introduction
          path: index.md
  ko:
    - title: 시작하기
      items:
        - title: 소개
          path: index.md
```

## 📦 Import System

Import documentation from other formats:

### Supported Formats

| Format | Features |
|--------|----------|
| **GitBook** | SUMMARY.md parsing, asset conversion |
| **Sphinx** | RST to MDX conversion, .po translation |
| **MkDocs** | mkdocs.yml navigation parsing |
| **Markdown** | Direct markdown import |

### Import Process

1. Clone external repository
2. Parse source format
3. Convert to BaiDocs MDX
4. Generate book.config.yaml
5. Copy to content directory

```bash
# Import from the editor UI:
# 1. Click "New Book"
# 2. Select "Import"
# 3. Choose format
# 4. Enter repository URL
```

:::note
The import system preserves your original structure while converting to BaiDocs format.
:::

## ✏️ WYSIWYG Editor

BaiDocs includes a powerful editor (port 3001):

### Editor Modes

- **WYSIWYG** - What You See Is What You Get
- **Markdown** - Raw markdown editing
- **Split View** - Edit and preview side-by-side
- **Preview** - Full preview mode

### Features

- 📁 **File tree navigation**
- 🖼️ **Image upload with drag & drop**
- 💾 **Auto-save**
- 🔍 **Real-time preview**
- 📝 **Frontmatter support**

### Image Uploads

Images are automatically:

- Saved to the `images/` directory
- Named with UUIDs for uniqueness
- Referenced with relative paths

## 🚀 Performance

BaiDocs is built for speed:

### Static Site Generation

- All pages pre-rendered at build time
- Instant page loads
- Perfect Lighthouse scores

### Optimizations

- Code splitting for faster initial loads
- Image optimization
- Minimal JavaScript bundle
- Fast refresh in development

### Build Times

Typical build times:

- **Small site** (50 pages): ~30 seconds
- **Medium site** (200 pages): ~2 minutes
- **Large site** (500 pages): ~5 minutes

:::tip
Disable PDF generation during development for much faster builds:
```bash
ENABLE_PDF_GENERATION=false pnpm dev
```
:::

## 📐 Admonitions

Rich callout boxes for highlighting important information:

### Available Types

:::note
**Note** - General information
:::

:::info
**Info** - Informational content
:::

:::tip
**Tip** - Helpful suggestions
:::

:::success
**Success** - Positive outcomes
:::

:::warning
**Warning** - Important cautions
:::

:::danger
**Danger** - Critical warnings
:::

### Syntax

```markdown
:::type
Your content here
:::
```

Supported types: `note`, `info`, `tip`, `hint`, `important`, `success`, `warning`, `caution`, `attention`, `danger`, `error`

## 🎯 Code Highlighting

Syntax highlighting for 100+ programming languages:

### Supported Languages

```javascript
// JavaScript
console.log('Hello, World!');
```

```python
# Python
print("Hello, World!")
```

```rust
// Rust
fn main() {
    println!("Hello, World!");
}
```

```yaml
# YAML
key: value
nested:
  - item1
  - item2
```

:::info
Code highlighting is powered by highlight.js with automatic language detection.
:::

## 📊 Tables

Full support for GFM tables:

| Feature | Status | Priority |
|---------|--------|----------|
| Search | ✅ Done | High |
| PDF Export | ✅ Done | High |
| Themes | ✅ Done | Medium |
| Diagrams | 📋 Planned | Low |

Tables support:
- Alignment (left, center, right)
- Markdown formatting in cells
- Any number of columns

## 🔗 Smart Links

Automatic link handling:

- **Relative links** - Between pages in your book
- **Anchor links** - To headings on the same page
- **External links** - To other websites
- **Email links** - Automatic mailto: links

```markdown
[Local page](./another-page.md)
[Heading anchor](#section-name)
[External](https://example.com)
[Email](mailto:user@example.com)
```

## 🏗️ Monorepo Structure

BaiDocs uses pnpm workspaces for efficient development:

```
packages/
├── ui/              # Shared UI components
├── mdx-editor/      # Editor components
└── config/          # Configuration utilities
```

Benefits:
- **Code sharing** between editor and viewer
- **Single dependency tree**
- **Fast installs** with pnpm
- **Easy to contribute**

## Next Steps

Ready to get started? Let's move on to [Installation](installation.md) and set up your first BaiDocs project!
