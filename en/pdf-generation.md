---
title: PDF Generation
description: Generate beautiful PDFs from your BaiDocs documentation
---

# PDF Generation

BaiDocs can automatically generate PDF files from your documentation during build time. This is perfect for offline reading, printing, or distribution.

## Enabling PDF Generation

PDF generation is controlled by an environment variable:

```bash
# Build with PDF generation
ENABLE_PDF_GENERATION=true pnpm build

# Build without PDF generation (faster)
ENABLE_PDF_GENERATION=false pnpm build
```

:::tip
Disable PDF generation during development for faster builds. Enable it only for production builds.
:::

## PDF Download Button Display

The PDF download button visibility is controlled separately from PDF generation. This allows you to show download buttons for pre-generated PDFs without regenerating them every build.

Configure in `next.config.js` (automatically set from environment variable):

```bash
# Show PDF download button (default)
ENABLE_PDF_DOWNLOAD=true pnpm dev

# Hide PDF download button
ENABLE_PDF_DOWNLOAD=false pnpm dev
```

**Use cases:**
- **Local development**: Set to `true` to view pre-generated PDFs without rebuilding
- **Vercel deployment**: Set to `false` when PDF generation is disabled
- **Staging environment**: Hide PDFs that aren't ready for public access

:::info
`ENABLE_PDF_DOWNLOAD` defaults to `true` if not set, making pre-generated PDFs accessible by default.
:::

## Configuration

Configure PDF settings in your `book.config.yaml`:

```yaml
pdf:
  enabled: true
  split: auto  # "single", "chapter", or "auto"
  cover:
    title: "My Documentation"
    subtitle: "Version 1.0.0"
    author: "Your Name"
    date: "2024-01-01"
  imageOptimization:
    enabled: true
    maxWidth: 1000
    quality: 70
  # Optional: auto-split configuration
  autoSplitConfig:
    maxImagesPerPdf: 15      # Max images per PDF
    maxSizePerPdf: 8         # Max size in MB
    maxPagesEstimate: 200    # Estimated max pages
    strategy: balanced       # "balanced" | "sequential" | "size-based"
```

## PDF Split Options

### Single PDF

Generate one PDF containing all content:

```yaml
pdf:
  split: single
  cover:
    title: "Complete Guide"
    subtitle: "All-in-One Documentation"
```

**Best for:**
- Small documentation (< 50 pages)
- Quick reference guides
- Single-topic documentation

**Result:**
- `public/pdfs/my-book-en.pdf`

### Chapter PDFs

Generate separate PDFs for each chapter:

```yaml
pdf:
  split: chapter
  cover:
    title: "Documentation"
    subtitle: "Volume {{chapter}}"
```

**Best for:**
- Large documentation (> 50 pages)
- Multi-topic documentation
- Users who need specific sections

**Result:**
```
public/pdfs/
├── my-book-en-chapter-1.pdf
├── my-book-en-chapter-2.pdf
└── my-book-en-chapter-3.pdf
```

### Auto Split PDFs

Automatically split PDFs based on content analysis (image count, file size, page count). This is the smartest option for handling large documentation with many images:

```yaml
pdf:
  split: auto
  autoSplitConfig:
    maxImagesPerPdf: 15       # Maximum images per PDF file
    maxSizePerPdf: 8          # Maximum size per PDF in MB
    maxPagesEstimate: 200     # Estimated maximum pages per PDF
    strategy: balanced        # Splitting strategy
```

**Best for:**
- Image-heavy documentation
- Very large documentation (> 200 pages)
- Documentation with varying content density
- Avoiding browser/PDF viewer crashes from too many images

**How it works:**
1. Analyzes all pages for image count and estimated size
2. Groups pages intelligently based on:
   - Image density (images per page)
   - Estimated file size
   - Document structure (respects chapters when possible)
3. Creates multiple PDFs staying within configured limits

**Splitting strategies:**
- **`balanced`** (recommended): Balances file size and logical structure
- **`sequential`**: Simple sequential splitting by page count
- **`size-based`**: Prioritizes file size limits over structure

**Result:**
```
public/pdfs/
├── my-book-en-part01.pdf  (15 images, 7.8 MB)
├── my-book-en-part02.pdf  (14 images, 7.5 MB)
├── my-book-en-part03.pdf  (12 images, 6.2 MB)
└── pdf-mapping.json       (maps pages to PDFs)
```

The `pdf-mapping.json` file maps each page to its corresponding PDF, enabling the viewer to show the correct download link for each page.

:::warning
Auto-split was designed to prevent PDF generation failures when dealing with image-heavy content. If your PDFs are failing to generate due to too many images, use `split: auto`.
:::

## Cover Page

Customize the PDF cover page:

```yaml
pdf:
  cover:
    title: "BaiDocs Guide"
    subtitle: "Complete Documentation Framework"
    author: "BaiDocs Team"
    version: "1.0.0"
    date: "January 2024"
    logo: "images/logo.png"  # Optional
```

**Cover includes:**
- Title (large, centered)
- Subtitle (below title)
- Author information
- Version and date
- Optional logo

## Image Optimization

Optimize images for PDF to reduce file size:

```yaml
pdf:
  imageOptimization:
    enabled: true
    maxWidth: 1200    # Max width in pixels
    quality: 85       # JPEG quality (1-100)
    format: "jpeg"    # or "png"
```

**Optimization effects:**
- Resize large images
- Compress JPEG quality
- Convert formats if needed
- Maintain aspect ratio

:::warning
Higher quality means larger PDF files. Find the right balance for your needs.
:::

## Page Settings

Configure page layout:

```yaml
pdf:
  page:
    format: "A4"           # or "Letter", "Legal"
    margin:
      top: "20mm"
      right: "20mm"
      bottom: "20mm"
      left: "20mm"
    printBackground: true  # Include background colors
    displayHeaderFooter: true
```

## Header and Footer

Add headers and footers to PDF pages:

```yaml
pdf:
  header:
    enabled: true
    template: "<div style='font-size: 10px; text-align: center; width: 100%;'>{{title}}</div>"
  footer:
    enabled: true
    template: "<div style='font-size: 10px; text-align: center; width: 100%;'>Page <span class='pageNumber'></span> of <span class='totalPages'></span></div>"
```

**Available variables:**
- `{{title}}` - Page title
- `{{book}}` - Book title
- `{{chapter}}` - Chapter title
- `{{date}}` - Current date
- `.pageNumber` - Current page (CSS class)
- `.totalPages` - Total pages (CSS class)

## Table of Contents

PDFs automatically include a table of contents:

```yaml
pdf:
  toc:
    enabled: true
    title: "Table of Contents"
    depth: 3  # Heading levels to include (1-6)
```

## How It Works

### Build Process

When you build with PDF generation:

1. **Start Puppeteer**: Launches headless Chrome
2. **Render Pages**: Converts HTML to PDF-ready format
3. **Optimize Images**: Processes images per configuration
4. **Generate PDFs**: Creates PDF files with Puppeteer
5. **Save Output**: Stores PDFs in `public/pdfs/`

### Build Time

PDF generation is the slowest part of the build:

| Documentation Size | Approximate Time |
|-------------------|------------------|
| Small (10 pages) | 30 seconds |
| Medium (50 pages) | 2-3 minutes |
| Large (200 pages) | 10-15 minutes |
| Very Large (500 pages) | 30+ minutes |

:::info
Build times vary based on:
- Number of pages
- Number of images
- Image optimization settings
- Server performance
:::

## PDF Features

### Included Features

PDFs include:

- ✅ All text content
- ✅ Code blocks with syntax highlighting
- ✅ Tables and lists
- ✅ Images and diagrams
- ✅ Admonition boxes
- ✅ Links (clickable in PDF)
- ✅ Table of contents
- ✅ Page numbers
- ✅ Bookmarks (from headings)

### Not Included

Some features don't work in PDFs:

- ❌ Interactive elements
- ❌ Embedded videos
- ❌ External iframes
- ❌ JavaScript functionality
- ❌ Search functionality

## Best Practices

### 1. Optimize for Print

Consider print layout:

```css
/* In your theme CSS */
@media print {
  /* Avoid page breaks inside elements */
  h2, h3 {
    page-break-after: avoid;
  }

  /* Keep code blocks together */
  pre {
    page-break-inside: avoid;
  }

  /* Adjust margins for printing */
  body {
    margin: 0;
    padding: 20mm;
  }
}
```

### 2. Use Appropriate Images

For PDFs:
- Use high-resolution images (min 150 DPI)
- Avoid very wide images (max 1200px)
- Use vector graphics (SVG) when possible
- Compress large images

### 3. Test PDF Output

Always test generated PDFs:

```bash
# Generate PDF
ENABLE_PDF_GENERATION=true pnpm build

# Check output
open public/pdfs/my-book-en.pdf
```

Verify:
- All pages render correctly
- Images appear properly
- Links work
- Table of contents is correct
- Page breaks are logical

### 4. Consider File Size

Keep PDF files manageable:

- Enable image optimization
- Compress images before adding
- Split large documentation into chapters
- Remove unnecessary images

:::tip
Target PDF sizes:
- Small guide: < 5MB
- Medium documentation: < 20MB
- Large documentation: < 50MB
:::

## Troubleshooting

### PDF Generation Fails

If PDF generation fails:

1. **Check Puppeteer**: Ensure it's installed
   ```bash
   pnpm add -D puppeteer
   ```

2. **Check Memory**: Puppeteer needs RAM
   ```bash
   # Increase Node memory
   NODE_OPTIONS="--max-old-space-size=4096" ENABLE_PDF_GENERATION=true pnpm build
   ```

3. **Check Permissions**: Ensure write access to `public/pdfs/`

4. **Check Logs**: Look for error messages in console

### Images Missing in PDF

If images don't appear:

1. ✅ Use relative paths (not absolute URLs)
2. ✅ Ensure images exist in `images/` directory
3. ✅ Check image formats (PNG, JPG, SVG)
4. ✅ Verify image optimization settings

### PDF Too Large

If PDF file is too large:

1. **Enable Optimization**:
   ```yaml
   pdf:
     imageOptimization:
       enabled: true
       maxWidth: 1000
       quality: 75
   ```

2. **Compress Images**: Use tools before adding
3. **Split Chapters**: Use chapter-based PDFs
4. **Remove Large Images**: Replace with smaller versions

### Slow Build Times

If builds are too slow:

1. **Disable PDFs in Development**:
   ```bash
   pnpm dev  # PDFs disabled by default
   ```

2. **Build Only When Needed**:
   ```bash
   # Regular build (no PDFs)
   pnpm build

   # Production build (with PDFs)
   ENABLE_PDF_GENERATION=true pnpm build
   ```

3. **Use Chapter Split**: Faster than single large PDF

4. **Optimize Images**: Smaller images = faster processing

## Advanced Configuration

### Custom PDF Styles

Add PDF-specific styles:

```css
/* apps/viewer/themes/my-theme/pdf.css */
@media print {
  @page {
    size: A4;
    margin: 2cm;
  }

  body {
    font-size: 11pt;
    line-height: 1.5;
  }

  h1 {
    page-break-before: always;
  }

  a {
    color: #000;
    text-decoration: none;
  }

  a[href^="http"]::after {
    content: " (" attr(href) ")";
    font-size: 0.8em;
  }
}
```

### Puppeteer Options

Customize Puppeteer settings in `scripts/build-pdfs.mjs`:

```javascript
const pdfOptions = {
  format: 'A4',
  printBackground: true,
  margin: {
    top: '20mm',
    right: '20mm',
    bottom: '20mm',
    left: '20mm'
  },
  displayHeaderFooter: true,
  headerTemplate: '<div></div>',
  footerTemplate: '<div style="font-size:10px; text-align:center; width:100%;">Page <span class="pageNumber"></span></div>'
};
```

## CI/CD Integration

Generate PDFs in CI/CD:

```yaml
# .github/workflows/deploy.yml
name: Build and Deploy

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

      - name: Install pnpm
        run: npm install -g pnpm

      - name: Install dependencies
        run: pnpm install

      - name: Build with PDFs
        run: ENABLE_PDF_GENERATION=true pnpm build
        env:
          NODE_OPTIONS: "--max-old-space-size=4096"

      - name: Deploy
        run: # your deployment command
```

## Next Steps

Learn about [Search Configuration](search-configuration.md) to make your documentation easily searchable!
