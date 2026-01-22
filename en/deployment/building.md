# Build Process

BaiDocs implements a comprehensive build system optimized for documentation sites with static site generation, performance optimization, and multi-environment deployment. This section covers build configuration, optimization strategies, and automation workflows.

## Build System Overview

### Architecture Components

BaiDocs build process consists of:

- **Content Processing**: Markdown and MDX transformation to HTML
- **Asset Optimization**: Image compression and format conversion
- **Static Generation**: Pre-rendered pages for optimal performance
- **Bundle Optimization**: JavaScript and CSS minification and code splitting
- **Search Indexing**: Full-text search index generation
- **Multi-language Building**: Coordinated builds across all supported languages

### Technology Stack

- **Next.js**: React-based framework with static site generation
- **Turbo**: Monorepo build orchestration and caching
- **Webpack**: Module bundling and optimization
- **PostCSS**: CSS processing and optimization
- **Sharp**: Image processing and optimization
- **Workbox**: Service worker generation for offline support

## Build Configuration

### Basic Build Setup

```javascript
// next.config.js
const { withBaiDocs } = require('@baidocs/next-config');

module.exports = withBaiDocs({
  // Output configuration
  output: 'export',  // Static site generation
  trailingSlash: true,
  images: {
    unoptimized: true,  // For static export
  },

  // Build optimization
  experimental: {
    optimizeCss: true,
    optimizePackageImports: ['@baidocs/ui', 'lucide-react'],
  },

  // Webpack configuration
  webpack: (config, { dev, isServer }) => {
    // Custom webpack optimizations
    if (!dev && !isServer) {
      config.optimization.splitChunks = {
        chunks: 'all',
        cacheGroups: {
          default: {
            minChunks: 2,
            priority: -20,
            reuseExistingChunk: true,
          },
          vendor: {
            test: /[\\/]node_modules[\\/]/,
            name: 'vendors',
            priority: -10,
            chunks: 'all',
          },
          baidocs: {
            test: /[\\/]node_modules[\\/]@baidocs/,
            name: 'baidocs',
            priority: 0,
            chunks: 'all',
          },
        },
      };
    }

    return config;
  },

  // Environment variables
  env: {
    BUILD_TIME: new Date().toISOString(),
    BUILD_VERSION: process.env.npm_package_version || '1.0.0',
  },
});
```

### Advanced Build Configuration

```javascript
// baidocs.config.js
module.exports = {
  // Build settings
  build: {
    // Output directory
    outDir: './dist',

    // Asset handling
    assets: {
      // Image optimization
      images: {
        formats: ['webp', 'avif'],
        quality: 85,
        progressive: true,
        optimizationLevel: 7,
      },

      // Font optimization
      fonts: {
        preload: ['Inter', 'JetBrains Mono'],
        display: 'swap',
        subset: true,
      },

      // CSS optimization
      css: {
        purge: true,
        minify: true,
        extractCritical: true,
      },
    },

    // JavaScript optimization
    javascript: {
      minify: true,
      mangle: true,
      compress: {
        drop_console: true,
        drop_debugger: true,
        pure_funcs: ['console.log', 'console.debug'],
      },
    },

    // Bundle analysis
    analyze: process.env.ANALYZE === 'true',

    // Source maps
    sourceMaps: process.env.NODE_ENV === 'development',

    // Performance budgets
    performanceBudgets: {
      maxAssetSize: 250000,  // 250KB
      maxEntrypointSize: 350000,  // 350KB
    },
  },

  // Pre-rendering configuration
  prerender: {
    routes: 'auto',  // Auto-discover from content
    fallback: false,
    concurrency: 4,
  },

  // Search index generation
  search: {
    enabled: true,
    engine: 'lunr',
    languages: ['en', 'ko'],
    indexPath: './public/search',
  },

  // Multi-language build
  i18n: {
    defaultLocale: 'en',
    locales: ['en', 'ko', 'ja'],
    buildSeparately: false,  // Build all languages together
  },

  // Plugin configuration
  plugins: [
    '@baidocs/plugin-analytics',
    '@baidocs/plugin-sitemap',
    '@baidocs/plugin-pwa',
    {
      name: '@baidocs/plugin-pdf',
      options: {
        generateOnBuild: false,  // Generate PDFs separately
      },
    },
  ],
};
```

## Build Scripts and Automation

### Package.json Scripts

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "build:analyze": "cross-env ANALYZE=true next build",
    "build:production": "cross-env NODE_ENV=production next build",
    "build:static": "next build && next export",
    "build:pdf": "baidocs pdf generate --all",

    "prebuild": "npm run clean && npm run validate",
    "postbuild": "npm run optimize && npm run verify",

    "clean": "rimraf .next dist out",
    "validate": "npm run lint && npm run typecheck",
    "lint": "next lint",
    "typecheck": "tsc --noEmit",

    "optimize": "npm run optimize:images && npm run optimize:sitemap",
    "optimize:images": "node scripts/optimize-images.js",
    "optimize:sitemap": "node scripts/generate-sitemap.js",

    "verify": "npm run verify:build && npm run verify:links",
    "verify:build": "node scripts/verify-build.js",
    "verify:links": "node scripts/check-links.js",

    "preview": "npm run build:static && serve out",
    "lighthouse": "lighthouse http://localhost:3000 --output-path=./lighthouse-report.html"
  }
}
```

### Custom Build Scripts

```javascript
// scripts/build-process.js
const { execSync } = require('child_process');
const fs = require('fs').promises;
const path = require('path');

class BuildProcess {
  constructor(config) {
    this.config = config;
    this.startTime = Date.now();
  }

  async runFullBuild() {
    console.log('🚀 Starting BaiDocs build process...');

    try {
      // Pre-build validation
      await this.validateEnvironment();
      await this.validateContent();

      // Main build steps
      await this.buildApplication();
      await this.optimizeAssets();
      await this.generateSearchIndex();
      await this.generateSitemap();

      // Post-build verification
      await this.verifyBuild();
      await this.generateBuildReport();

      console.log('✅ Build completed successfully!');

    } catch (error) {
      console.error('❌ Build failed:', error.message);
      process.exit(1);
    }
  }

  async validateEnvironment() {
    console.log('📋 Validating build environment...');

    // Check Node.js version
    const nodeVersion = process.version;
    const requiredVersion = '18.0.0';
    if (!this.isVersionGreaterOrEqual(nodeVersion.slice(1), requiredVersion)) {
      throw new Error(`Node.js ${requiredVersion} or higher required. Current: ${nodeVersion}`);
    }

    // Check available memory
    const totalMemory = process.memoryUsage();
    if (totalMemory.heapTotal < 1024 * 1024 * 1024) { // 1GB
      console.warn('⚠️ Low memory detected. Build may be slower.');
    }

    // Check disk space
    const stats = await fs.statfs('./');
    const freeSpace = stats.bavail * stats.bsize;
    if (freeSpace < 1024 * 1024 * 1024) { // 1GB
      throw new Error('Insufficient disk space for build process');
    }

    console.log('✓ Environment validation passed');
  }

  async validateContent() {
    console.log('📝 Validating content structure...');

    // Validate book configurations
    const contentDir = './content';
    const books = await fs.readdir(contentDir);

    for (const book of books) {
      const bookPath = path.join(contentDir, book);
      const stat = await fs.stat(bookPath);

      if (stat.isDirectory()) {
        const configPath = path.join(bookPath, 'book.config.yaml');
        try {
          await fs.access(configPath);
          console.log(`✓ Found config for ${book}`);
        } catch {
          console.warn(`⚠️ Missing config for ${book}`);
        }
      }
    }

    // Validate navigation structure
    await this.validateNavigationStructure();

    // Check for broken internal links
    await this.validateInternalLinks();

    console.log('✓ Content validation passed');
  }

  async buildApplication() {
    console.log('🔨 Building application...');

    const buildCommand = this.config.isDevelopment ? 'npm run dev' : 'npm run build:production';

    try {
      execSync(buildCommand, { stdio: 'inherit' });
      console.log('✓ Application build completed');
    } catch (error) {
      throw new Error(`Application build failed: ${error.message}`);
    }
  }

  async optimizeAssets() {
    console.log('🎨 Optimizing assets...');

    // Image optimization
    await this.optimizeImages();

    // Font optimization
    await this.optimizeFonts();

    // CSS optimization
    await this.optimizeCSS();

    console.log('✓ Asset optimization completed');
  }

  async optimizeImages() {
    const sharp = require('sharp');
    const glob = require('glob');

    const imageFiles = glob.sync('./out/**/*.{jpg,jpeg,png,gif}');

    const optimizationPromises = imageFiles.map(async (file) => {
      try {
        const outputPath = file.replace(/\.(jpg|jpeg|png|gif)$/, '.webp');
        await sharp(file)
          .webp({ quality: 85, effort: 6 })
          .toFile(outputPath);

        console.log(`Optimized: ${path.basename(file)} → WebP`);
      } catch (error) {
        console.warn(`Failed to optimize ${file}:`, error.message);
      }
    });

    await Promise.all(optimizationPromises);
  }

  async generateSearchIndex() {
    console.log('🔍 Generating search index...');

    const { SearchIndexGenerator } = require('../lib/search-generator');
    const generator = new SearchIndexGenerator(this.config);

    await generator.generateIndex('./content', './out/search');

    console.log('✓ Search index generation completed');
  }

  async generateBuildReport() {
    console.log('📊 Generating build report...');

    const buildTime = Date.now() - this.startTime;
    const buildStats = await this.collectBuildStats();

    const report = {
      timestamp: new Date().toISOString(),
      buildTime: `${Math.round(buildTime / 1000)}s`,
      version: process.env.npm_package_version,
      nodeVersion: process.version,
      environment: process.env.NODE_ENV,
      stats: buildStats
    };

    await fs.writeFile('./build-report.json', JSON.stringify(report, null, 2));

    console.log('✓ Build report generated');
    console.log(`📈 Build completed in ${report.buildTime}`);
  }

  async collectBuildStats() {
    const glob = require('glob');

    const jsFiles = glob.sync('./out/**/*.js');
    const cssFiles = glob.sync('./out/**/*.css');
    const htmlFiles = glob.sync('./out/**/*.html');
    const imageFiles = glob.sync('./out/**/*.{jpg,jpeg,png,gif,webp,svg}');

    const calculateSize = async (files) => {
      let totalSize = 0;
      for (const file of files) {
        const stat = await fs.stat(file);
        totalSize += stat.size;
      }
      return totalSize;
    };

    return {
      files: {
        javascript: {
          count: jsFiles.length,
          size: await calculateSize(jsFiles)
        },
        css: {
          count: cssFiles.length,
          size: await calculateSize(cssFiles)
        },
        html: {
          count: htmlFiles.length,
          size: await calculateSize(htmlFiles)
        },
        images: {
          count: imageFiles.length,
          size: await calculateSize(imageFiles)
        }
      }
    };
  }
}

// Execute build process
if (require.main === module) {
  const config = require('../baidocs.config.js');
  const buildProcess = new BuildProcess(config);
  buildProcess.runFullBuild();
}

module.exports = BuildProcess;
```

## Performance Optimization

### Code Splitting Strategy

```javascript
// Dynamic imports for better code splitting
import { lazy, Suspense } from 'react';

// Lazy load heavy components
const PDFViewer = lazy(() => import('./components/PDFViewer'));
const CodeEditor = lazy(() => import('./components/CodeEditor'));
const ChartRenderer = lazy(() => import('./components/ChartRenderer'));

// Route-based code splitting
const HomePage = lazy(() => import('./pages/HomePage'));
const DocumentationPage = lazy(() => import('./pages/DocumentationPage'));

// Component wrapper with loading fallback
const LazyComponent = ({ component: Component, ...props }) => (
  <Suspense fallback={<div className="loading-spinner">Loading...</div>}>
    <Component {...props} />
  </Suspense>
);

// Webpack magic comments for chunk naming
const AsyncComponent = lazy(() =>
  import(/* webpackChunkName: "async-component" */ './AsyncComponent')
);
```

### Asset Optimization Pipeline

```javascript
// webpack.config.js optimization
module.exports = {
  optimization: {
    // Minimize bundle size
    minimize: true,
    minimizer: [
      new TerserPlugin({
        terserOptions: {
          compress: {
            drop_console: true,
            drop_debugger: true,
            pure_funcs: ['console.log'],
          },
          mangle: {
            safari10: true,
          },
        },
      }),
      new CssMinimizerPlugin({
        minimizerOptions: {
          preset: [
            'default',
            {
              discardComments: { removeAll: true },
              normalizeWhitespace: true,
            },
          ],
        },
      }),
    ],

    // Split chunks efficiently
    splitChunks: {
      chunks: 'all',
      maxInitialRequests: 20,
      maxAsyncRequests: 20,
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          chunks: 'all',
          priority: 10,
        },
        common: {
          name: 'common',
          minChunks: 2,
          chunks: 'all',
          priority: 5,
        },
      },
    },

    // Runtime chunk for better caching
    runtimeChunk: 'single',
  },

  // Module resolution optimization
  resolve: {
    // Reduce resolution time
    modules: ['node_modules'],
    extensions: ['.js', '.jsx', '.ts', '.tsx', '.json'],

    // Alias for faster imports
    alias: {
      '@': path.resolve(__dirname, 'src'),
      '@components': path.resolve(__dirname, 'src/components'),
      '@lib': path.resolve(__dirname, 'src/lib'),
    },
  },
};
```

## Build Verification and Testing

### Automated Build Verification

```javascript
// scripts/verify-build.js
const fs = require('fs').promises;
const path = require('path');
const { execSync } = require('child_process');

class BuildVerifier {
  constructor(buildDir = './out') {
    this.buildDir = buildDir;
    this.errors = [];
    this.warnings = [];
  }

  async verify() {
    console.log('🔍 Verifying build output...');

    await this.verifyStructure();
    await this.verifyAssets();
    await this.verifyPages();
    await this.verifyPerformance();
    await this.verifyAccessibility();

    if (this.errors.length > 0) {
      console.error('❌ Build verification failed:');
      this.errors.forEach(error => console.error(`  - ${error}`));
      process.exit(1);
    }

    if (this.warnings.length > 0) {
      console.warn('⚠️ Build warnings:');
      this.warnings.forEach(warning => console.warn(`  - ${warning}`));
    }

    console.log('✅ Build verification passed');
  }

  async verifyStructure() {
    // Check essential files exist
    const requiredFiles = [
      'index.html',
      '_next/static',
      'sitemap.xml',
      'robots.txt'
    ];

    for (const file of requiredFiles) {
      const filePath = path.join(this.buildDir, file);
      try {
        await fs.access(filePath);
      } catch {
        this.errors.push(`Missing required file: ${file}`);
      }
    }

    // Check for empty directories
    await this.checkEmptyDirectories(this.buildDir);
  }

  async verifyAssets() {
    // Check image optimization
    const images = await this.glob('**/*.{jpg,jpeg,png}');
    for (const image of images) {
      const stat = await fs.stat(image);
      if (stat.size > 500 * 1024) { // 500KB
        this.warnings.push(`Large image file: ${image} (${Math.round(stat.size / 1024)}KB)`);
      }
    }

    // Check for unused assets
    await this.checkUnusedAssets();
  }

  async verifyPages() {
    // Check all HTML pages are valid
    const htmlFiles = await this.glob('**/*.html');

    for (const htmlFile of htmlFiles) {
      await this.validateHtml(htmlFile);
      await this.checkPageMetadata(htmlFile);
    }
  }

  async verifyPerformance() {
    // Check bundle sizes
    const jsFiles = await this.glob('**/*.js');
    let totalJsSize = 0;

    for (const jsFile of jsFiles) {
      const stat = await fs.stat(jsFile);
      totalJsSize += stat.size;
    }

    const maxBundleSize = 350 * 1024; // 350KB
    if (totalJsSize > maxBundleSize) {
      this.warnings.push(`Total JS bundle size exceeds limit: ${Math.round(totalJsSize / 1024)}KB > ${Math.round(maxBundleSize / 1024)}KB`);
    }

    // Check for duplicate dependencies
    await this.checkDuplicateDependencies();
  }

  async validateHtml(filePath) {
    const content = await fs.readFile(filePath, 'utf-8');

    // Basic HTML validation
    if (!content.includes('<!DOCTYPE html>')) {
      this.errors.push(`Missing DOCTYPE in ${filePath}`);
    }

    if (!content.includes('<title>')) {
      this.errors.push(`Missing title tag in ${filePath}`);
    }

    if (!content.includes('lang=')) {
      this.warnings.push(`Missing language attribute in ${filePath}`);
    }

    // Check for critical performance issues
    if (content.includes('render-blocking')) {
      this.warnings.push(`Render-blocking resources in ${filePath}`);
    }
  }
}

// Execute verification
if (require.main === module) {
  const verifier = new BuildVerifier();
  verifier.verify();
}
```

## Continuous Integration

### GitHub Actions Build Pipeline

```yaml
name: Build and Deploy Documentation
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    name: Build Documentation
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [18.x, 20.x]

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci --frozen-lockfile

      - name: Run linting
        run: npm run lint

      - name: Run type checking
        run: npm run typecheck

      - name: Run tests
        run: npm run test

      - name: Build application
        run: npm run build:production
        env:
          NODE_ENV: production
          ANALYZE: false

      - name: Verify build
        run: npm run verify

      - name: Upload build artifacts
        uses: actions/upload-artifact@v3
        if: success()
        with:
          name: build-output-${{ matrix.node-version }}
          path: out/
          retention-days: 7

      - name: Generate lighthouse report
        if: matrix.node-version == '18.x'
        run: |
          npm install -g @lhci/cli
          lhci autorun
        env:
          LHCI_GITHUB_APP_TOKEN: ${{ secrets.LHCI_GITHUB_APP_TOKEN }}

  performance-audit:
    name: Performance Audit
    needs: build
    runs-on: ubuntu-latest
    if: github.event_name == 'pull_request'

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Download build artifacts
        uses: actions/download-artifact@v3
        with:
          name: build-output-18.x
          path: out/

      - name: Run bundle analyzer
        run: |
          npx webpack-bundle-analyzer out/_next/static/chunks/*.js --mode static --report bundle-report.html --no-open

      - name: Upload bundle analysis
        uses: actions/upload-artifact@v3
        with:
          name: bundle-analysis
          path: bundle-report.html
```

The BaiDocs build process provides comprehensive automation, optimization, and verification to ensure high-performance, reliable documentation site deployment across multiple environments and hosting platforms.