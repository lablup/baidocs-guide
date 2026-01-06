---
title: Installation
description: Step-by-step guide to installing and setting up BaiDocs
---

# Installation

This guide will walk you through installing BaiDocs and getting your development environment ready.

## Prerequisites

Before you begin, make sure you have the following installed:

### Required Software

| Software | Minimum Version | Purpose |
|----------|----------------|---------|
| **Node.js** | 18.x or higher | JavaScript runtime |
| **pnpm** | 8.x or higher | Package manager |
| **Git** | 2.x or higher | Version control |

:::warning
BaiDocs requires Node.js 18 or higher. Using older versions may cause compatibility issues.
:::

### Installing Node.js

If you don't have Node.js installed:

**macOS/Linux (using nvm):**

```bash
# Install nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# Install Node.js
nvm install 18
nvm use 18
```

**Windows:**

Download and install from [nodejs.org](https://nodejs.org/)

### Installing pnpm

Once Node.js is installed, install pnpm globally:

```bash
npm install -g pnpm
```

Verify the installation:

```bash
pnpm --version
# Should output 8.x.x or higher
```

:::tip
pnpm is faster and more efficient than npm or yarn, saving both disk space and installation time.
:::

## Installation Steps

### 1. Clone the Repository

```bash
git clone https://github.com/lablup/baidocs.git
cd baidocs
```

### 2. Install Dependencies

```bash
pnpm install
```

This command will:
- Install all project dependencies
- Set up the monorepo workspaces
- Link packages together

:::info
The first installation might take a few minutes. Subsequent installs will be much faster thanks to pnpm's efficient caching.
:::

### 3. Verify Installation

Check that everything is installed correctly:

```bash
# Check if the build works
pnpm type-check

# Should complete without errors
```

## Project Setup

### Understanding the Directory Structure

After installation, your project structure looks like this:

```
baidocs/
├── apps/
│   ├── editor/              # Editor application
│   │   ├── src/
│   │   ├── package.json
│   │   └── next.config.mjs
│   └── viewer/              # Viewer application
│       ├── src/
│       ├── package.json
│       └── next.config.mjs
├── packages/
│   ├── ui/                  # Shared UI components
│   ├── mdx-editor/          # MDX editor components
│   └── config/              # Configuration
├── content/                 # Your documentation books
├── scripts/                 # Build scripts
├── baidocs.config.yaml      # Global configuration
├── package.json
└── pnpm-workspace.yaml      # Monorepo configuration
```

### Configuration Files

#### Global Configuration

The `baidocs.config.yaml` file contains global settings:

```yaml
# Theme configuration
theme:
  name: "default"  # or "backend-ai"
  path: "themes"

# Build options
build:
  generatePDFs: false
  optimizeImages: true
```

#### Book Configuration

Each book has its own `book.config.yaml` (we'll create this later):

```yaml
id: my-book
title: My Documentation
description: My awesome documentation
languages:
  - en
```

## Development Environment

### Starting the Development Server

BaiDocs has two applications you can run:

#### Option 1: Run Both Applications

```bash
pnpm dev
```

This starts:
- **Editor** at http://localhost:3001
- **Viewer** at http://localhost:3000

#### Option 2: Run Individually

**Start only the viewer:**

```bash
pnpm viewer
```

**Start only the editor:**

```bash
pnpm editor
```

:::tip
For documentation writing, you typically only need the viewer. The editor is useful for WYSIWYG editing and managing multiple books.
:::

### Hot Reload

Both applications support hot reload:
- Edit any file
- Save the changes
- See updates instantly in your browser

## Troubleshooting

### Common Issues

#### Port Already in Use

If ports 3000 or 3001 are already taken:

```bash
# Kill processes on port 3000
lsof -ti:3000 | xargs kill -9

# Kill processes on port 3001
lsof -ti:3001 | xargs kill -9

# Or use custom ports
PORT=3002 pnpm viewer
PORT=3003 pnpm editor
```

#### pnpm Install Fails

If `pnpm install` fails:

```bash
# Clear pnpm cache
pnpm store prune

# Remove node_modules and lock file
rm -rf node_modules pnpm-lock.yaml

# Reinstall
pnpm install
```

#### Type Errors

If you see TypeScript errors:

```bash
# Run type checking
pnpm type-check

# If errors persist, try cleaning and rebuilding
pnpm clean
pnpm install
```

:::note
Most type errors can be resolved by ensuring you're using the correct Node.js version (18+) and have all dependencies installed.
:::

#### Build Issues

If the build fails:

```bash
# Try building without PDF generation
ENABLE_PDF_GENERATION=false pnpm build

# Check for specific errors
pnpm build:viewer 2>&1 | grep -i error
```

### Getting Help

If you encounter issues not covered here:

1. **Check existing issues**: Visit [GitHub Issues](https://github.com/lablup/baidocs/issues)
2. **Search documentation**: Use Ctrl/Cmd + K to search this guide
3. **Create a new issue**: Provide error messages and steps to reproduce

## System Requirements

### Minimum Requirements

- **CPU**: 2 cores
- **RAM**: 4 GB
- **Disk**: 2 GB free space
- **OS**: macOS, Linux, or Windows 10+

### Recommended Requirements

- **CPU**: 4+ cores
- **RAM**: 8 GB
- **Disk**: 5 GB free space (for build artifacts and node_modules)
- **OS**: macOS or Linux for best performance

:::info
Build times and performance will be significantly better on systems with SSD storage and more RAM.
:::

## Docker Support

BaiDocs can also run in Docker:

```dockerfile
FROM node:18-alpine

# Install pnpm
RUN npm install -g pnpm

# Set working directory
WORKDIR /app

# Copy files
COPY . .

# Install dependencies
RUN pnpm install

# Build
RUN pnpm build

# Expose ports
EXPOSE 3000 3001

# Start
CMD ["pnpm", "start"]
```

Build and run:

```bash
docker build -t baidocs .
docker run -p 3000:3000 -p 3001:3001 baidocs
```

## Next Steps

Now that BaiDocs is installed and running, let's [Create Your First Book](creating-first-book.md)!

---

:::success
Installation complete! You're ready to start creating amazing documentation.
:::
