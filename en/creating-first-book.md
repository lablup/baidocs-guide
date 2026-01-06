---
title: Creating Your First Book
description: Learn how to create your first documentation book in BaiDocs
---

# Creating Your First Book

Let's create your first documentation book! This tutorial will guide you through the entire process from start to finish.

## What is a Book?

In BaiDocs, a **book** is a collection of related documentation pages organized into a logical structure. Think of it like a physical book with chapters and sections.

Each book:
- Lives in its own directory under `content/`
- Has independent configuration
- Can support multiple languages
- Has its own navigation structure

## Three Ways to Create a Book

BaiDocs offers three methods for creating books:

1. **Using the Editor** - Visual interface (easiest)
2. **Manual Creation** - Direct file creation (most control)
3. **Import from External** - Convert existing documentation

Let's explore each method!

## Method 1: Using the Editor

The editor provides a user-friendly interface for creating books.

### Step 1: Start the Editor

```bash
pnpm editor
```

Open http://localhost:3001 in your browser.

### Step 2: Create a New Book

1. Click the **"New Book"** button
2. Fill in the book details:
   - **Book ID**: `my-first-book` (URL-safe identifier)
   - **Title**: `My First Book`
   - **Description**: `Learning to use BaiDocs`
   - **Languages**: Select `English (en)`

3. Click **"Create"**

:::tip
The Book ID must be URL-safe (lowercase, hyphens allowed, no spaces). It becomes part of your documentation URLs.
:::

### Step 3: Add Your First Page

1. In the file tree, you'll see `my-first-book/en/`
2. Right-click on the `en` folder
3. Select **"New File"**
4. Name it `index.md`
5. Start writing!

```markdown
---
title: Welcome
---

# Welcome to My First Book

This is my first page in BaiDocs!

## What I'll Document

- Getting started guide
- API reference
- Best practices
```

### Step 4: Add More Pages

Create additional pages:

```markdown
getting-started.md
api-reference.md
best-practices.md
```

### Step 5: View Your Book

Open http://localhost:3000 to see your book in the viewer!

## Method 2: Manual Creation

For more control, create books manually:

### Step 1: Create Directory Structure

```bash
cd content
mkdir -p my-first-book/en/images
```

Your structure should look like:

```
content/
└── my-first-book/
    ├── book.config.yaml
    └── en/
        ├── index.md
        └── images/
```

### Step 2: Create Book Configuration

Create `content/my-first-book/book.config.yaml`:

```yaml
id: my-first-book
title: My First Book
description: Learning to use BaiDocs
type: document
languages:
  - en
defaultLanguage: en
navigation:
  en:
    - title: Getting Started
      items:
        - title: Welcome
          path: index.md
        - title: Installation
          path: installation.md
    - title: Guides
      items:
        - title: Basic Usage
          path: basic-usage.md
        - title: Advanced Features
          path: advanced-features.md
```

:::info
The `navigation` section defines your book's table of contents and sidebar structure.
:::

### Step 3: Create Content Pages

Create `content/my-first-book/en/index.md`:

```markdown
---
title: Welcome
description: Welcome to my first book
---

# Welcome to My First Book

This is the homepage of my documentation.

## Quick Links

- [Installation](installation.md)
- [Basic Usage](basic-usage.md)
- [Advanced Features](advanced-features.md)

Let's get started!
```

### Step 4: Create Additional Pages

Create the other pages referenced in your navigation:

**installation.md:**

```markdown
---
title: Installation
---

# Installation

Follow these steps to install...
```

**basic-usage.md:**

```markdown
---
title: Basic Usage
---

# Basic Usage

Learn the basics of...
```

**advanced-features.md:**

```markdown
---
title: Advanced Features
---

# Advanced Features

Discover advanced capabilities...
```

### Step 5: Start the Viewer

```bash
pnpm viewer
```

Visit http://localhost:3000/books/my-first-book/en to see your book!

## Method 3: Import from External Source

BaiDocs can import documentation from other formats.

### Supported Import Sources

| Source | Format |
|--------|--------|
| **GitBook** | SUMMARY.md based |
| **Sphinx** | RST files with conf.py |
| **MkDocs** | mkdocs.yml based |
| **Markdown** | Plain markdown files |

### Import via Editor

1. Open the editor at http://localhost:3001
2. Click **"New Book"**
3. Click **"Import"** tab
4. Select your source type
5. Enter the repository URL:
   ```
   https://github.com/user/repo
   ```
6. Configure import options:
   - Branch name
   - Source directory
   - Target language
7. Click **"Import"**

The editor will:
- Clone the repository
- Parse the source format
- Convert to BaiDocs format
- Create the book configuration
- Copy content to `content/` directory

:::warning
Import can take several minutes depending on the size of the documentation. The process runs in the background.
:::

## Understanding Book Configuration

Let's break down the `book.config.yaml` file:

### Basic Information

```yaml
id: my-book              # Unique identifier (URL-safe)
title: My Book           # Display title
description: About...    # Short description
type: document           # Book type (document, api, guide)
```

### Languages

```yaml
languages:
  - en                   # English
  - ko                   # Korean
defaultLanguage: en      # Default language
```

### Navigation Structure

```yaml
navigation:
  en:                    # Navigation for English
    - title: Chapter 1   # Chapter title
      items:             # Pages in this chapter
        - title: Page 1
          path: page1.md
        - title: Page 2
          path: page2.md
    - title: Chapter 2
      items:
        - title: Page 3
          path: page3.md
```

:::note
Navigation structure can be different for each language, allowing you to organize content differently per locale.
:::

### PDF Configuration (Optional)

```yaml
pdf:
  split: chapter         # "single" or "chapter"
  cover:
    title: "My Book"
    subtitle: "v1.0"
  imageOptimization:
    enabled: true
    maxWidth: 1200
    quality: 85
```

## Adding Images

Images should be placed in the `images/` directory:

```
content/my-first-book/en/
├── index.md
├── page1.md
└── images/
    ├── screenshot.png
    ├── diagram.svg
    └── logo.jpg
```

Reference images in your markdown:

```markdown
![My Screenshot](images/screenshot.png)

Or with HTML:
<img src="images/diagram.svg" alt="Architecture Diagram" />
```

:::tip
The `images/` directory is special - it's excluded from navigation but files are accessible for embedding.
:::

## Best Practices

### 1. Choose Clear Book IDs

Good:
- ✅ `user-guide`
- ✅ `api-reference`
- ✅ `developer-handbook`

Bad:
- ❌ `UserGuide` (not URL-safe)
- ❌ `my guide` (contains spaces)
- ❌ `docs123` (not descriptive)

### 2. Organize Content Logically

Structure your content from general to specific:

```
1. Introduction
2. Getting Started
3. Basic Concepts
4. Advanced Topics
5. API Reference
6. Troubleshooting
```

### 3. Write Good Frontmatter

Every page should have frontmatter:

```yaml
---
title: Clear, Descriptive Title
description: Brief summary for search and SEO
---
```

### 4. Use Consistent File Names

Match file names to content:
- `installation.md` for installation guides
- `getting-started.md` for getting started guides
- `api-reference.md` for API docs

### 5. Create a Good Index Page

Your `index.md` should:
- Welcome readers
- Explain what the book covers
- Provide quick navigation links
- Set expectations

## Testing Your Book

Before sharing, test thoroughly:

### 1. Check Navigation

- [ ] All links work correctly
- [ ] Navigation hierarchy makes sense
- [ ] No broken internal links

### 2. Verify Content

- [ ] All pages have titles
- [ ] Frontmatter is complete
- [ ] Images load correctly
- [ ] Code blocks render properly

### 3. Test Search

Use Cmd/Ctrl + K to search:
- [ ] Your pages appear in results
- [ ] Search finds relevant content
- [ ] Results are ranked well

### 4. Build Test

```bash
# Try a production build
ENABLE_PDF_GENERATION=false pnpm build

# Check for errors
```

## Common Issues

### Book Not Appearing

**Problem**: Book doesn't show up in the viewer.

**Solutions**:
1. Check that `book.config.yaml` exists
2. Verify the YAML syntax is correct
3. Ensure the `id` field matches the directory name
4. Restart the development server

### Broken Links

**Problem**: Internal links return 404.

**Solutions**:
1. Use relative paths: `./page.md` or `page.md`
2. Check file names match exactly (case-sensitive)
3. Verify the path in navigation matches the file

### Images Not Loading

**Problem**: Images show as broken.

**Solutions**:
1. Ensure images are in the `images/` directory
2. Use relative paths: `images/file.png`
3. Check file extensions are correct
4. Verify file names (case-sensitive)

## Next Steps

Congratulations! You've created your first book. Now let's dive deeper into the [Project Structure](project-structure.md) to understand how everything works together.

---

:::success
You're now ready to create professional documentation with BaiDocs!
:::
