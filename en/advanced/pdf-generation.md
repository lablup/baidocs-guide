# PDF Generation

BaiDocs provides comprehensive PDF export capabilities for creating print-ready documentation, offline distribution, and archival purposes. This section covers PDF generation configuration, customization options, and optimization strategies.

## PDF Generation Overview

### Core Features

BaiDocs PDF generation system offers:

- **Automated Layout**: Professional page layouts with proper typography
- **Chapter Organization**: Automatic chapter breaks and section organization
- **Table of Contents**: Generated navigation with page numbers
- **Cross-references**: Functional internal links and references
- **Image Optimization**: Resolution and quality optimization for print
- **Custom Styling**: Brand-specific layouts and formatting options

### Technology Stack

- **Puppeteer**: Headless Chrome rendering engine
- **CSS Print Media**: Optimized print stylesheets
- **Page.js**: PDF layout and pagination control
- **Image Processing**: Automatic image optimization and scaling

## Configuration Setup

### Basic PDF Configuration

```yaml
# book.config.yaml
pdf:
  enabled: true
  split: chapter        # Options: page, chapter, section, book
  format: A4           # Options: A4, Letter, Legal, A3
  orientation: portrait # Options: portrait, landscape

  # Cover page configuration
  cover:
    enabled: true
    title: "Documentation Title"
    subtitle: "Complete User Guide"
    author: "Documentation Team"
    version: "v2.0"
    date: "2026-01-20"
    logo: "./assets/images/logo.png"

  # Page layout settings
  layout:
    margins:
      top: "20mm"
      right: "15mm"
      bottom: "20mm"
      left: "15mm"

    # Header and footer
    header:
      enabled: true
      height: "15mm"
      content: "{title} - {chapter}"

    footer:
      enabled: true
      height: "15mm"
      content: "Page {pageNumber} of {totalPages}"

  # Image optimization
  imageOptimization:
    enabled: true
    maxWidth: 1200
    quality: 85
    dpi: 300

  # Table of contents
  toc:
    enabled: true
    title: "Table of Contents"
    depth: 3
    pageNumbers: true

  # Advanced options
  options:
    embedFonts: true
    generateBookmarks: true
    includeMetadata: true
    printBackground: true
    preferCSSPageSize: true
```

### Advanced Configuration Options

```yaml
pdf:
  # Chapter-specific settings
  chapters:
    - title: "Introduction"
      startNewPage: true
      headerContent: "Getting Started Guide"

    - title: "API Reference"
      startNewPage: true
      headerContent: "API Documentation"
      orientation: landscape  # Override for specific chapters

  # Custom CSS for PDF rendering
  customCSS: |
    @page {
      size: A4;
      margin: 20mm 15mm;
    }

    .pdf-chapter-break {
      page-break-before: always;
    }

    .pdf-no-break {
      page-break-inside: avoid;
    }

  # Preprocessing options
  preprocessing:
    removeInteractiveElements: true
    optimizeImages: true
    inlineCriticalCSS: true

  # Output options
  output:
    filename: "{title}-{version}-{date}"
    path: "./exports/pdf/"
    compression: true

  # Metadata
  metadata:
    title: "Documentation Title"
    author: "Documentation Team"
    subject: "Technical Documentation"
    keywords: ["documentation", "guide", "reference"]
    creator: "BaiDocs PDF Generator"
```

## PDF Styling and Layout

### Print-specific CSS

```css
/* Print media queries for PDF generation */
@media print {
  /* Page setup */
  @page {
    size: A4;
    margin: 20mm 15mm 20mm 15mm;

    /* Header and footer styling */
    @top-center {
      content: string(chapter-title);
      font-size: 10pt;
      color: #666;
    }

    @bottom-center {
      content: "Page " counter(page) " of " counter(pages);
      font-size: 10pt;
      color: #666;
    }
  }

  /* Chapter breaks */
  .pdf-chapter {
    page-break-before: always;
  }

  /* Prevent breaks inside elements */
  .pdf-keep-together {
    page-break-inside: avoid;
  }

  /* Hide interactive elements */
  .interactive-only,
  .navigation,
  .sidebar {
    display: none !important;
  }

  /* Typography optimizations */
  body {
    font-size: 11pt;
    line-height: 1.4;
    color: #000;
    background: white;
  }

  h1 {
    font-size: 20pt;
    font-weight: bold;
    margin-top: 0;
    margin-bottom: 12pt;
    page-break-after: avoid;
  }

  h2 {
    font-size: 16pt;
    font-weight: bold;
    margin-top: 18pt;
    margin-bottom: 8pt;
    page-break-after: avoid;
  }

  /* Code blocks */
  pre, code {
    font-family: 'Courier New', monospace;
    font-size: 9pt;
    background: #f5f5f5;
    border: 1pt solid #ddd;
    padding: 6pt;
    page-break-inside: avoid;
  }

  /* Tables */
  table {
    border-collapse: collapse;
    width: 100%;
    margin: 12pt 0;
    page-break-inside: auto;
  }

  th, td {
    border: 1pt solid #ddd;
    padding: 6pt;
    text-align: left;
  }

  th {
    background-color: #f0f0f0;
    font-weight: bold;
  }

  /* Images */
  img {
    max-width: 100%;
    height: auto;
    page-break-inside: avoid;
  }

  /* Links */
  a {
    color: #000;
    text-decoration: underline;
  }

  /* Remove color backgrounds for print */
  * {
    -webkit-print-color-adjust: exact;
    color-adjust: exact;
  }
}

/* Chapter title styling */
.chapter-title {
  string-set: chapter-title content();
}
```

### Custom Cover Page Design

```html
<!-- Cover page template -->
<div class="pdf-cover-page">
  <div class="cover-header">
    <img src="{logo}" alt="Company Logo" class="cover-logo" />
  </div>

  <div class="cover-content">
    <h1 class="cover-title">{title}</h1>
    <h2 class="cover-subtitle">{subtitle}</h2>

    <div class="cover-metadata">
      <p class="cover-author">by {author}</p>
      <p class="cover-version">Version {version}</p>
      <p class="cover-date">{date}</p>
    </div>
  </div>

  <div class="cover-footer">
    <p class="cover-copyright">© 2026 Company Name. All rights reserved.</p>
  </div>
</div>

<style>
.pdf-cover-page {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  text-align: center;
  padding: 40mm;
  page-break-after: always;
}

.cover-logo {
  height: 60px;
  width: auto;
}

.cover-title {
  font-size: 36pt;
  font-weight: bold;
  color: #333;
  margin: 40mm 0 20mm 0;
}

.cover-subtitle {
  font-size: 18pt;
  color: #666;
  margin-bottom: 40mm;
}

.cover-metadata p {
  font-size: 14pt;
  margin: 8pt 0;
}

.cover-footer {
  font-size: 10pt;
  color: #999;
}
</style>
```

## Generation Process and Automation

### Manual PDF Generation

#### Via BaiDocs Interface

1. **Navigate to PDF Export**:
   - Access the book management interface
   - Select "Export" or "Generate PDF" option
   - Choose generation parameters and settings

2. **Configure Export Options**:
   ```typescript
   interface PDFExportOptions {
     format: 'A4' | 'Letter' | 'Legal';
     orientation: 'portrait' | 'landscape';
     includeTableOfContents: boolean;
     includeCoverPage: boolean;
     chapters?: string[];  // Specific chapters to include
     language?: string;    // Language version to export
   }
   ```

3. **Monitor Generation Progress**:
   - Real-time progress indicators
   - Error reporting and diagnostics
   - Download link upon completion

#### Command Line Interface

```bash
# Generate PDF for entire book
baidocs pdf generate --book my-documentation --output ./exports/

# Generate specific chapters
baidocs pdf generate --book api-docs --chapters "introduction,authentication" --format A4

# Generate with custom settings
baidocs pdf generate \
  --book user-guide \
  --config ./custom-pdf-config.yaml \
  --output-name "UserGuide-v2.0" \
  --optimize-images
```

### Automated PDF Generation

#### CI/CD Integration

```yaml
# GitHub Actions workflow
name: Generate Documentation PDF
on:
  push:
    branches: [main]
  release:
    types: [published]

jobs:
  generate-pdf:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm ci

      - name: Generate PDF
        run: |
          npm run build
          npm run pdf:generate

      - name: Upload PDF artifacts
        uses: actions/upload-artifact@v3
        with:
          name: documentation-pdf
          path: exports/pdf/

      - name: Create release assets
        if: github.event_name == 'release'
        run: |
          gh release upload ${{ github.event.release.tag_name }} \
            exports/pdf/*.pdf
```

#### Scheduled Generation

```javascript
// Automated PDF generation script
const cron = require('node-cron');
const { generatePDF } = require('./pdf-generator');

// Generate PDF every Sunday at 2 AM
cron.schedule('0 2 * * 0', async () => {
  console.log('Starting scheduled PDF generation...');

  try {
    const result = await generatePDF({
      book: 'main-documentation',
      config: {
        includeLatestUpdates: true,
        notifyOnCompletion: true,
        uploadToStorage: true
      }
    });

    console.log('PDF generation completed:', result.filename);

    // Send notification
    await sendNotification({
      type: 'success',
      message: `PDF generated successfully: ${result.filename}`,
      recipients: ['docs-team@company.com']
    });

  } catch (error) {
    console.error('PDF generation failed:', error);

    await sendNotification({
      type: 'error',
      message: `PDF generation failed: ${error.message}`,
      recipients: ['docs-team@company.com']
    });
  }
});
```

## Optimization and Performance

### Image Optimization

```javascript
// Image processing for PDF generation
const sharp = require('sharp');

async function optimizeImageForPDF(imagePath, options = {}) {
  const {
    maxWidth = 1200,
    quality = 85,
    dpi = 300,
    format = 'jpeg'
  } = options;

  return sharp(imagePath)
    .resize(maxWidth, null, {
      withoutEnlargement: true,
      fit: 'inside'
    })
    .jpeg({
      quality,
      progressive: true,
      mozjpeg: true
    })
    .density(dpi)
    .toBuffer();
}

// Batch image optimization
async function optimizeAllImages(imageDirectory, outputDirectory) {
  const images = await glob(`${imageDirectory}/**/*.{jpg,jpeg,png,gif}`);

  const optimizationPromises = images.map(async (imagePath) => {
    const relativePath = path.relative(imageDirectory, imagePath);
    const outputPath = path.join(outputDirectory, relativePath);

    await fs.ensureDir(path.dirname(outputPath));

    const optimizedBuffer = await optimizeImageForPDF(imagePath);
    await fs.writeFile(outputPath, optimizedBuffer);

    console.log(`Optimized: ${relativePath}`);
  });

  await Promise.all(optimizationPromises);
  console.log(`Optimized ${images.length} images for PDF generation`);
}
```

### Performance Monitoring

```typescript
interface PDFGenerationMetrics {
  startTime: Date;
  endTime: Date;
  duration: number;
  pageCount: number;
  fileSize: number;
  imageCount: number;
  processingStages: {
    htmlGeneration: number;
    cssProcessing: number;
    imageOptimization: number;
    pdfRendering: number;
  };
}

class PDFPerformanceMonitor {
  private metrics: Partial<PDFGenerationMetrics> = {};

  startGeneration() {
    this.metrics.startTime = new Date();
  }

  recordStage(stage: keyof PDFGenerationMetrics['processingStages'], duration: number) {
    if (!this.metrics.processingStages) {
      this.metrics.processingStages = {} as any;
    }
    this.metrics.processingStages[stage] = duration;
  }

  finishGeneration(pageCount: number, fileSize: number) {
    this.metrics.endTime = new Date();
    this.metrics.duration = this.metrics.endTime.getTime() - this.metrics.startTime!.getTime();
    this.metrics.pageCount = pageCount;
    this.metrics.fileSize = fileSize;

    this.logMetrics();
  }

  private logMetrics() {
    console.log('PDF Generation Metrics:', {
      duration: `${this.metrics.duration}ms`,
      pages: this.metrics.pageCount,
      fileSize: `${(this.metrics.fileSize! / 1024 / 1024).toFixed(2)}MB`,
      stages: this.metrics.processingStages
    });
  }
}
```

## Troubleshooting and Quality Assurance

### Common Issues and Solutions

#### Font Rendering Problems

```yaml
# Font embedding configuration
pdf:
  fonts:
    embed: true
    families:
      - name: "Inter"
        paths:
          - "./assets/fonts/Inter-Regular.woff2"
          - "./assets/fonts/Inter-Bold.woff2"
      - name: "JetBrains Mono"
        paths:
          - "./assets/fonts/JetBrainsMono-Regular.woff2"
```

#### Page Break Issues

```css
/* Prevent unwanted page breaks */
.keep-together {
  page-break-inside: avoid;
  break-inside: avoid;
}

/* Force page breaks */
.page-break-before {
  page-break-before: always;
  break-before: page;
}

/* Control orphans and widows */
p {
  orphans: 3;
  widows: 3;
}
```

#### Image Quality Problems

```typescript
// High-DPI image handling
const imageOptimizationSettings = {
  lowResolution: {
    maxWidth: 600,
    quality: 70,
    dpi: 150
  },
  highResolution: {
    maxWidth: 1200,
    quality: 85,
    dpi: 300
  },
  print: {
    maxWidth: 2400,
    quality: 95,
    dpi: 600
  }
};
```

### Quality Assurance Checklist

```markdown
## PDF Quality Assurance Checklist

### Content Verification
- [ ] All sections and chapters included
- [ ] Table of contents accurate with correct page numbers
- [ ] Cross-references and internal links functional
- [ ] Code blocks formatted correctly and readable
- [ ] Tables fit within page margins
- [ ] Images display clearly at appropriate resolution

### Layout and Typography
- [ ] Consistent typography throughout document
- [ ] Proper page breaks and section organization
- [ ] Headers and footers display correctly
- [ ] Margins appropriate and consistent
- [ ] No orphaned headings or widowed text

### Technical Validation
- [ ] PDF opens correctly in multiple PDF readers
- [ ] File size reasonable for content volume
- [ ] Bookmarks and navigation functional
- [ ] Metadata populated correctly
- [ ] Print preview shows proper formatting

### Accessibility
- [ ] Document structure tagged properly
- [ ] Alt text included for images
- [ ] Reading order logical and sequential
- [ ] Color contrast sufficient for printing
- [ ] Text selectable and searchable
```

BaiDocs PDF generation provides enterprise-grade documentation export capabilities with extensive customization options for professional documentation distribution and archival requirements.