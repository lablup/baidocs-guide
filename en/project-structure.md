---
title: Project Structure
description: Understanding the BaiDocs project structure and organization
---

# Project Structure

Understanding how BaiDocs is organized will help you navigate the codebase and customize it effectively.

## Overview

BaiDocs uses a **monorepo structure** powered by pnpm workspaces. This means multiple related packages live in one repository.

```
baidocs/
├── apps/                # Applications
├── packages/            # Shared packages
├── content/             # Documentation books
├── scripts/             # Build & utility scripts
├── public/              # Static assets
└── docs/                # Technical documentation
```

## Directory Deep Dive

### apps/

Contains the two main applications:

```
apps/
├── editor/              # Documentation editor (Port 3001)
│   ├── src/
│   │   ├── app/        # Next.js app routes
│   │   ├── components/ # React components
│   │   ├── lib/        # Utility functions
│   │   └── styles/     # Global styles
│   ├── public/         # Static assets
│   ├── package.json
│   └── next.config.mjs
│
└── viewer/              # Documentation viewer (Port 3000)
    ├── src/
    │   ├── app/        # Next.js app routes
    │   ├── components/ # React components
    │   ├── lib/        # Utility functions
    │   └── styles/     # Global styles
    ├── themes/         # Theme CSS files
    ├── public/         # Static assets
    ├── package.json
    └── next.config.mjs
```

#### Editor Application

**Purpose**: WYSIWYG interface for creating and editing documentation

**Key Features**:
- File tree navigation
- MDX editor with multiple modes
- Image upload functionality
- Book import system
- Git operations

**Tech Stack**:
- Next.js 15 (App Router)
- @mdxeditor/editor (Lexical-based)
- Ant Design components
- simple-git for version control

#### Viewer Application

**Purpose**: Static documentation website generator

**Key Features**:
- Beautiful documentation UI
- Client-side search
- PDF generation
- Multi-language support
- Theme system

**Tech Stack**:
- Next.js 15 (Static Export)
- react-markdown for rendering
- Fuse.js for search
- Puppeteer for PDF generation

### packages/

Shared code used by both applications:

```
packages/
├── ui/                  # Shared UI components
│   ├── src/
│   │   └── components/
│   └── package.json
│
├── mdx-editor/          # MDX editor components
│   ├── src/
│   │   ├── components/
│   │   └── utils/
│   └── package.json
│
└── config/              # Configuration utilities
    ├── src/
    │   ├── types/
    │   └── utils/
    └── package.json
```

:::info
Using packages allows code sharing between apps without duplication, following the DRY (Don't Repeat Yourself) principle.
:::

### content/

Your documentation books live here:

```
content/
├── book-1/
│   ├── book.config.yaml
│   ├── en/
│   │   ├── index.md
│   │   ├── chapter-1.md
│   │   └── images/
│   └── ko/
│       ├── index.md
│       └── chapter-1.md
│
├── book-2/
│   └── ...
│
└── README.md
```

**Key Points**:
- Each book is self-contained
- Books can be separate Git repositories
- Images directory is shared across languages
- Each language has independent content

### scripts/

Build and utility scripts:

```
scripts/
├── build-search-index.mjs    # Generate search index
├── build-pdfs.mjs            # Generate PDF files
├── copy-themes.mjs           # Copy theme CSS
├── sync-content.mjs          # Sync external repos
└── utils/
    ├── git-operations.mjs
    ├── file-operations.mjs
    └── markdown-parser.mjs
```

#### Important Scripts

**build-search-index.mjs**
- Crawls all markdown files
- Extracts text content
- Creates searchable JSON index
- Output: `public/search-index.json`

**build-pdfs.mjs**
- Uses Puppeteer to render PDFs
- Supports single or chapter splitting
- Optimizes images
- Output: `public/pdfs/`

**copy-themes.mjs**
- Copies theme CSS to public directory
- Runs before build
- Supports custom themes

### public/

Static assets served directly:

```
public/
├── themes/              # Theme CSS files (copied from apps/viewer/themes)
├── search-index.json    # Search index (generated)
├── pdfs/                # Generated PDFs (generated)
├── favicon.ico
└── robots.txt
```

:::warning
Files in `public/` are served at the root URL. Avoid naming conflicts with Next.js routes.
:::

## Configuration Files

### Root Level

#### pnpm-workspace.yaml

Defines the monorepo structure:

```yaml
packages:
  - 'apps/*'
  - 'packages/*'
```

#### package.json (Root)

Scripts for the entire project:

```json
{
  "scripts": {
    "dev": "pnpm --parallel dev",
    "build": "pnpm --parallel build",
    "editor": "pnpm --filter editor dev",
    "viewer": "pnpm --filter viewer dev",
    "type-check": "pnpm --recursive type-check"
  }
}
```

#### baidocs.config.yaml

Global configuration:

```yaml
# Theme settings
theme:
  name: "default"
  path: "themes"

# Build options
build:
  generatePDFs: false
  optimizeImages: true
  searchIndex: true

# Server options
server:
  viewerPort: 3000
  editorPort: 3001
```

### Application Level

#### next.config.mjs

Next.js configuration for each app:

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  output: 'standalone',  // For deployment
  reactStrictMode: true,
  transpilePackages: ['@baidocs/ui', '@baidocs/config'],

  // Image optimization
  images: {
    domains: ['localhost'],
    formats: ['image/webp']
  }
}

export default nextConfig
```

#### package.json (App Level)

Each app has its own dependencies:

```json
{
  "name": "@baidocs/viewer",
  "dependencies": {
    "next": "15.x",
    "react": "19.x",
    "@baidocs/ui": "workspace:*",
    "react-markdown": "^9.0.0",
    "fuse.js": "^7.0.0"
  },
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  }
}
```

## File Naming Conventions

BaiDocs follows these conventions:

### TypeScript Files

- **Components**: PascalCase - `BookList.tsx`
- **Utilities**: camelCase - `parseMarkdown.ts`
- **Types**: PascalCase - `BookConfig.ts`
- **Hooks**: camelCase with 'use' prefix - `useBook.ts`

### Markdown Files

- **Lowercase with hyphens**: `getting-started.md`
- **Descriptive names**: `api-reference.md` (good) vs `doc1.md` (bad)

### Directories

- **Lowercase**: `components/`, `utils/`, `lib/`
- **Hyphen-separated**: `mdx-editor/`, `ui-components/`

## Build Output

### Development

Running `pnpm dev` starts development servers:

```
.next/               # Next.js cache (gitignored)
node_modules/        # Dependencies (gitignored)
dist/                # Compiled packages (gitignored)
```

### Production Build

Running `pnpm build` creates:

```
.next/
├── standalone/      # Standalone deployment
│   ├── apps/
│   │   └── viewer/
│   │       ├── server.js
│   │       ├── .next/
│   │       └── public/
│   └── node_modules/
│
├── static/          # Static assets
└── cache/           # Build cache
```

The `standalone` directory contains everything needed to deploy:

```bash
cd .next/standalone/apps/viewer
PORT=3000 node server.js
```

## Import Resolution

BaiDocs uses TypeScript path aliases:

```json
{
  "compilerOptions": {
    "paths": {
      "@/*": ["./src/*"],
      "@baidocs/ui": ["../../packages/ui/src"],
      "@baidocs/config": ["../../packages/config/src"]
    }
  }
}
```

This allows clean imports:

```typescript
// Instead of: import { Button } from '../../packages/ui/src/Button'
import { Button } from '@baidocs/ui'

// Instead of: import { utils } from '../../lib/utils'
import { utils } from '@/lib/utils'
```

## Hot Module Replacement

BaiDocs supports HMR (Hot Module Replacement):

- **Content changes**: Automatically reload
- **Component changes**: Update without full reload
- **Config changes**: Require server restart

:::tip
If changes don't appear, try:
1. Save the file again
2. Refresh the browser
3. Restart the dev server (if config changed)
:::

## Environment Variables

### Development

Create `.env.local`:

```bash
# Editor
NEXT_PUBLIC_EDITOR_PORT=3001

# Viewer
NEXT_PUBLIC_VIEWER_PORT=3000

# Build options
ENABLE_PDF_GENERATION=false
ENABLE_SEARCH_INDEX=true
```

### Production

Set in your deployment platform:

```bash
NODE_ENV=production
NEXT_PUBLIC_API_URL=https://docs.example.com
```

:::warning
Never commit `.env.local` or any file containing secrets to version control.
:::

## Dependencies

### Workspace Dependencies

Packages in the monorepo reference each other:

```json
{
  "dependencies": {
    "@baidocs/ui": "workspace:*",
    "@baidocs/config": "workspace:*"
  }
}
```

The `workspace:*` protocol tells pnpm to link local packages.

### External Dependencies

Major dependencies by category:

**Framework**:
- next - React framework
- react - UI library
- typescript - Type safety

**Editor**:
- @mdxeditor/editor - MDX WYSIWYG editor
- simple-git - Git operations
- gray-matter - Frontmatter parsing

**Viewer**:
- react-markdown - Markdown rendering
- remark-gfm - GitHub Flavored Markdown
- rehype-highlight - Code highlighting
- fuse.js - Fuzzy search

**Build**:
- puppeteer - PDF generation
- js-yaml - YAML parsing
- sharp - Image optimization

## Gitignore

Important ignored files:

```gitignore
# Dependencies
node_modules/
.pnpm-store/

# Build output
.next/
dist/
out/
*.tsbuildinfo

# Environment
.env*.local

# OS
.DS_Store
Thumbs.db

# IDE
.vscode/
.idea/

# Generated
public/search-index.json
public/pdfs/
public/themes/
```

## Next Steps

Now that you understand the project structure, let's learn about [Markdown Basics](markdown-basics.md) and start writing content!
