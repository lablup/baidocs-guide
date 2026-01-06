---
title: Building for Production
description: Build and optimize BaiDocs for production deployment
---

# Building for Production

This guide covers building BaiDocs for production, optimization, and preparing for deployment.

## Production Build

### Basic Build

Build the viewer for production:

```bash
# Build viewer only
pnpm build:viewer

# Or full build (both apps)
pnpm build
```

### With PDF Generation

Enable PDF generation:

```bash
ENABLE_PDF_GENERATION=true pnpm build
```

### With All Features

Full production build:

```bash
ENABLE_PDF_GENERATION=true pnpm build
```

:::tip
Expect build times of 2-10 minutes depending on documentation size and enabled features.
:::

## Build Output

### Directory Structure

Build creates the following output:

```
.next/
├── standalone/              # Standalone server build
│   ├── apps/viewer/
│   │   ├── server.js       # Node.js server
│   │   ├── .next/          # Next.js build output
│   │   ├── public/         # Static assets
│   │   │   ├── search-index.json
│   │   │   ├── pdfs/
│   │   │   └── themes/
│   │   └── package.json
│   └── node_modules/       # Required dependencies
│
├── static/                  # Static chunks
└── cache/                   # Build cache
```

### Output Size

Typical build sizes:

| Component | Size |
|-----------|------|
| Next.js bundle | 500KB - 2MB |
| Search index | 500KB - 3MB |
| PDFs | 1MB - 50MB |
| Images | Varies |
| Total | 5MB - 100MB |

## Build Scripts

### Available Scripts

```json
{
  "scripts": {
    "build": "pnpm --parallel build",
    "build:viewer": "cd apps/viewer && next build",
    "build:editor": "cd apps/editor && next build",
    "build:search": "node scripts/build-search-index.mjs",
    "build:pdf": "ENABLE_PDF_GENERATION=true node scripts/build-pdfs.mjs"
  }
}
```

### Custom Build Script

Create a production build script:

```bash
#!/bin/bash
# build-production.sh

echo "Starting production build..."

# Clean previous build
echo "Cleaning previous build..."
rm -rf .next public/search-index.json public/pdfs

# Build search index
echo "Building search index..."
pnpm build:search

# Build PDFs
echo "Generating PDFs..."
ENABLE_PDF_GENERATION=true pnpm build:pdf

# Build application
echo "Building application..."
pnpm build:viewer

echo "Production build complete!"
```

Make it executable:

```bash
chmod +x build-production.sh
./build-production.sh
```

## Optimization

### 1. Image Optimization

Optimize images before building:

```bash
# Using ImageOptim (macOS)
imageoptim content/**/images/*.{png,jpg,jpeg}

# Using sharp-cli
sharp -i "content/**/images/*.png" -o optimized/ --webp

# Using tinypng-cli
tinypng content/**/images/*.png
```

### 2. Search Index Optimization

Reduce search index size:

```yaml
# baidocs.config.yaml
search:
  # Only index titles and headings
  keys:
    - title
    - headings
  # Limit content length
  maxContentLength: 200
```

### 3. PDF Optimization

Optimize PDF settings:

```yaml
# book.config.yaml
pdf:
  imageOptimization:
    enabled: true
    maxWidth: 1000     # Reduce from 1200
    quality: 75        # Reduce from 85
```

### 4. Code Splitting

Next.js automatically splits code. Verify:

```bash
# Analyze bundle
pnpm build:viewer && pnpm analyze
```

## Environment Variables

### Production Variables

Create `.env.production`:

```bash
# App
NODE_ENV=production
NEXT_PUBLIC_SITE_URL=https://docs.example.com

# Build Options
ENABLE_PDF_GENERATION=true
ENABLE_SEARCH_INDEX=true

# Analytics (optional)
NEXT_PUBLIC_GA_ID=G-XXXXXXXXXX

# Performance
NODE_OPTIONS="--max-old-space-size=4096"
```

Load environment:

```bash
# Build with environment
set -a
source .env.production
set +a
pnpm build
```

## Build Validation

### Check Build Output

Verify build succeeded:

```bash
# Check standalone output exists
ls -la .next/standalone/apps/viewer/

# Check public assets
ls -la .next/standalone/apps/viewer/public/

# Check server file
ls -la .next/standalone/apps/viewer/server.js
```

### Test Build Locally

Test production build before deploying:

```bash
# Navigate to standalone output
cd .next/standalone/apps/viewer

# Start production server
PORT=3000 node server.js
```

Visit http://localhost:3000 to verify.

### Run Checks

```bash
# Type checking
pnpm type-check

# Linting
pnpm lint

# Build size
du -sh .next/standalone
```

## Build Optimization Tips

### 1. Use Build Cache

Speed up builds with cache:

```bash
# Keep .next/cache between builds
# In CI/CD, cache this directory
```

### 2. Parallel Builds

Build apps in parallel:

```bash
# Already parallelized
pnpm build
```

### 3. Skip Unnecessary Steps

Development builds (faster):

```bash
# Skip PDFs and optimization
ENABLE_PDF_GENERATION=false pnpm build
```

### 4. Incremental Builds

Only rebuild what changed:

```bash
# Next.js handles this automatically
pnpm build:viewer
```

## CI/CD Integration

### GitHub Actions

```yaml
# .github/workflows/build.yml
name: Build Production

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'pnpm'

      - name: Install pnpm
        run: npm install -g pnpm

      - name: Install dependencies
        run: pnpm install --frozen-lockfile

      - name: Build
        run: ENABLE_PDF_GENERATION=true pnpm build
        env:
          NODE_OPTIONS: "--max-old-space-size=4096"

      - name: Upload artifacts
        uses: actions/upload-artifact@v3
        with:
          name: build-output
          path: .next/standalone

      - name: Deploy
        run: # Your deployment command
```

### GitLab CI

```yaml
# .gitlab-ci.yml
build:
  stage: build
  image: node:18
  cache:
    paths:
      - node_modules/
      - .next/cache/
  script:
    - npm install -g pnpm
    - pnpm install --frozen-lockfile
    - ENABLE_PDF_GENERATION=true pnpm build
  artifacts:
    paths:
      - .next/standalone
    expire_in: 1 day
```

## Troubleshooting

### Build Fails

If build fails:

1. **Check Node version**: Requires Node 18+
   ```bash
   node --version
   ```

2. **Clean and rebuild**:
   ```bash
   rm -rf .next node_modules
   pnpm install
   pnpm build
   ```

3. **Check memory**: Increase if needed
   ```bash
   NODE_OPTIONS="--max-old-space-size=8192" pnpm build
   ```

4. **Check logs**: Look for specific errors

### Build Too Slow

If builds are too slow:

1. **Disable PDFs**: Build without PDFs
   ```bash
   ENABLE_PDF_GENERATION=false pnpm build
   ```

2. **Use cache**: Enable build cache in CI/CD

3. **Optimize images**: Compress before building

4. **Parallelize**: Ensure parallel builds enabled

### Build Too Large

If output is too large:

1. **Optimize images**: Compress and resize
2. **Reduce search index**: Limit indexed content
3. **Split PDFs**: Use chapter-based PDFs
4. **Remove unused assets**: Clean up unused files

## Build Checklist

Before deploying, verify:

- [ ] Build completes without errors
- [ ] All pages render correctly
- [ ] Search works
- [ ] PDFs generated (if enabled)
- [ ] Images load properly
- [ ] Links work
- [ ] Navigation functions
- [ ] Mobile responsive
- [ ] Performance acceptable
- [ ] No console errors

## Performance Monitoring

### Lighthouse

Run Lighthouse audit:

```bash
# Using Chrome DevTools
# Or using CLI
lighthouse https://your-deployed-site.com --view
```

Target scores:
- **Performance**: 90+
- **Accessibility**: 90+
- **Best Practices**: 90+
- **SEO**: 90+

### Bundle Analysis

Analyze bundle size:

```bash
# Install analyzer
pnpm add -D @next/bundle-analyzer

# Analyze
ANALYZE=true pnpm build:viewer
```

## Next Steps

Your build is ready! Learn about [Deployment Options](deployment-options.md) to publish your documentation.
