# 빌드 프로세스

BaiDocs는 정적 사이트 생성, 성능 최적화, 다중 환경 배포를 위해 최적화된 포괄적인 빌드 시스템을 구현합니다. 이 섹션에서는 빌드 구성, 최적화 전략, 자동화 워크플로우를 다룹니다.

## 빌드 시스템 개요

### 아키텍처 구성 요소

BaiDocs 빌드 프로세스는 다음으로 구성됩니다:

- **콘텐츠 처리**: Markdown 및 MDX를 HTML로 변환
- **자산 최적화**: 이미지 압축 및 포맷 변환
- **정적 생성**: 최적의 성능을 위한 사전 렌더링된 페이지
- **번들 최적화**: JavaScript 및 CSS 압축과 코드 분할
- **검색 인덱싱**: 전체 텍스트 검색 인덱스 생성
- **다국어 빌드**: 지원되는 모든 언어에 대한 조정된 빌드

### 기술 스택

- **Next.js**: 정적 사이트 생성 기능이 있는 React 기반 프레임워크
- **Turbo**: 모노레포 빌드 오케스트레이션 및 캐싱
- **Webpack**: 모듈 번들링 및 최적화
- **PostCSS**: CSS 처리 및 최적화
- **Sharp**: 이미지 처리 및 최적화
- **Workbox**: 오프라인 지원을 위한 서비스 워커 생성

## 빌드 구성

### 기본 빌드 설정

```javascript
// next.config.js
const { withBaiDocs } = require('@baidocs/next-config');

module.exports = withBaiDocs({
  // 출력 구성
  output: 'export',  // 정적 사이트 생성
  trailingSlash: true,
  images: {
    unoptimized: true,  // 정적 내보내기용
  },

  // 빌드 최적화
  experimental: {
    optimizeCss: true,
    optimizePackageImports: ['@baidocs/ui', 'lucide-react'],
  },

  // Webpack 구성
  webpack: (config, { dev, isServer }) => {
    // 사용자 정의 webpack 최적화
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

  // 환경 변수
  env: {
    BUILD_TIME: new Date().toISOString(),
    BUILD_VERSION: process.env.npm_package_version || '1.0.0',
  },
});
```

### 고급 빌드 구성

```javascript
// baidocs.config.js
module.exports = {
  // 빌드 설정
  build: {
    // 출력 디렉토리
    outDir: './dist',

    // 자산 처리
    assets: {
      // 이미지 최적화
      images: {
        formats: ['webp', 'avif'],
        quality: 85,
        progressive: true,
        optimizationLevel: 7,
      },

      // 폰트 최적화
      fonts: {
        preload: ['Inter', 'JetBrains Mono'],
        display: 'swap',
        subset: true,
      },

      // CSS 최적화
      css: {
        purge: true,
        minify: true,
        extractCritical: true,
      },
    },

    // JavaScript 최적화
    javascript: {
      minify: true,
      mangle: true,
      compress: {
        drop_console: true,
        drop_debugger: true,
        pure_funcs: ['console.log', 'console.debug'],
      },
    },

    // 번들 분석
    analyze: process.env.ANALYZE === 'true',

    // 소스 맵
    sourceMaps: process.env.NODE_ENV === 'development',

    // 성능 예산
    performanceBudgets: {
      maxAssetSize: 250000,  // 250KB
      maxEntrypointSize: 350000,  // 350KB
    },
  },

  // 사전 렌더링 구성
  prerender: {
    routes: 'auto',  // 콘텐츠에서 자동 탐지
    fallback: false,
    concurrency: 4,
  },

  // 검색 인덱스 생성
  search: {
    enabled: true,
    engine: 'lunr',
    languages: ['en', 'ko'],
    indexPath: './public/search',
  },

  // 다국어 빌드
  i18n: {
    defaultLocale: 'en',
    locales: ['en', 'ko', 'ja'],
    buildSeparately: false,  // 모든 언어를 함께 빌드
  },

  // 플러그인 구성
  plugins: [
    '@baidocs/plugin-analytics',
    '@baidocs/plugin-sitemap',
    '@baidocs/plugin-pwa',
    {
      name: '@baidocs/plugin-pdf',
      options: {
        generateOnBuild: false,  // PDF를 별도로 생성
      },
    },
  ],
};
```

## 빌드 스크립트 및 자동화

### Package.json 스크립트

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

### 사용자 정의 빌드 스크립트

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
    console.log('🚀 BaiDocs 빌드 프로세스 시작...');

    try {
      // 빌드 전 검증
      await this.validateEnvironment();
      await this.validateContent();

      // 메인 빌드 단계
      await this.buildApplication();
      await this.optimizeAssets();
      await this.generateSearchIndex();
      await this.generateSitemap();

      // 빌드 후 검증
      await this.verifyBuild();
      await this.generateBuildReport();

      console.log('✅ 빌드가 성공적으로 완료되었습니다!');

    } catch (error) {
      console.error('❌ 빌드 실패:', error.message);
      process.exit(1);
    }
  }

  async validateEnvironment() {
    console.log('📋 빌드 환경 검증 중...');

    // Node.js 버전 확인
    const nodeVersion = process.version;
    const requiredVersion = '18.0.0';
    if (!this.isVersionGreaterOrEqual(nodeVersion.slice(1), requiredVersion)) {
      throw new Error(`Node.js ${requiredVersion} 이상이 필요합니다. 현재: ${nodeVersion}`);
    }

    // 사용 가능한 메모리 확인
    const totalMemory = process.memoryUsage();
    if (totalMemory.heapTotal < 1024 * 1024 * 1024) { // 1GB
      console.warn('⚠️ 메모리 부족이 감지되었습니다. 빌드가 느려질 수 있습니다.');
    }

    // 디스크 공간 확인
    const stats = await fs.statfs('./');
    const freeSpace = stats.bavail * stats.bsize;
    if (freeSpace < 1024 * 1024 * 1024) { // 1GB
      throw new Error('빌드 프로세스를 위한 디스크 공간이 부족합니다');
    }

    console.log('✓ 환경 검증 통과');
  }

  async validateContent() {
    console.log('📝 콘텐츠 구조 검증 중...');

    // 도서 구성 검증
    const contentDir = './content';
    const books = await fs.readdir(contentDir);

    for (const book of books) {
      const bookPath = path.join(contentDir, book);
      const stat = await fs.stat(bookPath);

      if (stat.isDirectory()) {
        const configPath = path.join(bookPath, 'book.config.yaml');
        try {
          await fs.access(configPath);
          console.log(`✓ ${book}의 구성 파일을 찾았습니다`);
        } catch {
          console.warn(`⚠️ ${book}의 구성 파일이 누락되었습니다`);
        }
      }
    }

    // 네비게이션 구조 검증
    await this.validateNavigationStructure();

    // 끊어진 내부 링크 확인
    await this.validateInternalLinks();

    console.log('✓ 콘텐츠 검증 통과');
  }

  async buildApplication() {
    console.log('🔨 애플리케이션 빌드 중...');

    const buildCommand = this.config.isDevelopment ? 'npm run dev' : 'npm run build:production';

    try {
      execSync(buildCommand, { stdio: 'inherit' });
      console.log('✓ 애플리케이션 빌드 완료');
    } catch (error) {
      throw new Error(`애플리케이션 빌드 실패: ${error.message}`);
    }
  }

  async optimizeAssets() {
    console.log('🎨 자산 최적화 중...');

    // 이미지 최적화
    await this.optimizeImages();

    // 폰트 최적화
    await this.optimizeFonts();

    // CSS 최적화
    await this.optimizeCSS();

    console.log('✓ 자산 최적화 완료');
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

        console.log(`최적화됨: ${path.basename(file)} → WebP`);
      } catch (error) {
        console.warn(`${file} 최적화 실패:`, error.message);
      }
    });

    await Promise.all(optimizationPromises);
  }

  async generateSearchIndex() {
    console.log('🔍 검색 인덱스 생성 중...');

    const { SearchIndexGenerator } = require('../lib/search-generator');
    const generator = new SearchIndexGenerator(this.config);

    await generator.generateIndex('./content', './out/search');

    console.log('✓ 검색 인덱스 생성 완료');
  }

  async generateBuildReport() {
    console.log('📊 빌드 보고서 생성 중...');

    const buildTime = Date.now() - this.startTime;
    const buildStats = await this.collectBuildStats();

    const report = {
      timestamp: new Date().toISOString(),
      buildTime: `${Math.round(buildTime / 1000)}초`,
      version: process.env.npm_package_version,
      nodeVersion: process.version,
      environment: process.env.NODE_ENV,
      stats: buildStats
    };

    await fs.writeFile('./build-report.json', JSON.stringify(report, null, 2));

    console.log('✓ 빌드 보고서 생성됨');
    console.log(`📈 빌드가 ${report.buildTime}에 완료됨`);
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

// 빌드 프로세스 실행
if (require.main === module) {
  const config = require('../baidocs.config.js');
  const buildProcess = new BuildProcess(config);
  buildProcess.runFullBuild();
}

module.exports = BuildProcess;
```

## 성능 최적화

### 코드 분할 전략

```javascript
// 더 나은 코드 분할을 위한 동적 임포트
import { lazy, Suspense } from 'react';

// 무거운 컴포넌트를 지연 로드
const PDFViewer = lazy(() => import('./components/PDFViewer'));
const CodeEditor = lazy(() => import('./components/CodeEditor'));
const ChartRenderer = lazy(() => import('./components/ChartRenderer'));

// 라우트 기반 코드 분할
const HomePage = lazy(() => import('./pages/HomePage'));
const DocumentationPage = lazy(() => import('./pages/DocumentationPage'));

// 로딩 폴백이 있는 컴포넌트 래퍼
const LazyComponent = ({ component: Component, ...props }) => (
  <Suspense fallback={<div className="loading-spinner">로딩 중...</div>}>
    <Component {...props} />
  </Suspense>
);

// 청크 이름 지정을 위한 Webpack 매직 코멘트
const AsyncComponent = lazy(() =>
  import(/* webpackChunkName: "async-component" */ './AsyncComponent')
);
```

### 자산 최적화 파이프라인

```javascript
// webpack.config.js 최적화
module.exports = {
  optimization: {
    // 번들 크기 최소화
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

    // 청크를 효율적으로 분할
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

    // 더 나은 캐싱을 위한 런타임 청크
    runtimeChunk: 'single',
  },

  // 모듈 해결 최적화
  resolve: {
    // 해결 시간 단축
    modules: ['node_modules'],
    extensions: ['.js', '.jsx', '.ts', '.tsx', '.json'],

    // 빠른 임포트를 위한 별칭
    alias: {
      '@': path.resolve(__dirname, 'src'),
      '@components': path.resolve(__dirname, 'src/components'),
      '@lib': path.resolve(__dirname, 'src/lib'),
    },
  },
};
```

## 빌드 검증 및 테스트

### 자동화된 빌드 검증

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
    console.log('🔍 빌드 출력 검증 중...');

    await this.verifyStructure();
    await this.verifyAssets();
    await this.verifyPages();
    await this.verifyPerformance();
    await this.verifyAccessibility();

    if (this.errors.length > 0) {
      console.error('❌ 빌드 검증 실패:');
      this.errors.forEach(error => console.error(`  - ${error}`));
      process.exit(1);
    }

    if (this.warnings.length > 0) {
      console.warn('⚠️ 빌드 경고:');
      this.warnings.forEach(warning => console.warn(`  - ${warning}`));
    }

    console.log('✅ 빌드 검증 통과');
  }

  async verifyStructure() {
    // 필수 파일이 존재하는지 확인
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
        this.errors.push(`필수 파일 누락: ${file}`);
      }
    }

    // 빈 디렉토리 확인
    await this.checkEmptyDirectories(this.buildDir);
  }

  async verifyAssets() {
    // 이미지 최적화 확인
    const images = await this.glob('**/*.{jpg,jpeg,png}');
    for (const image of images) {
      const stat = await fs.stat(image);
      if (stat.size > 500 * 1024) { // 500KB
        this.warnings.push(`큰 이미지 파일: ${image} (${Math.round(stat.size / 1024)}KB)`);
      }
    }

    // 사용되지 않는 자산 확인
    await this.checkUnusedAssets();
  }

  async verifyPages() {
    // 모든 HTML 페이지가 유효한지 확인
    const htmlFiles = await this.glob('**/*.html');

    for (const htmlFile of htmlFiles) {
      await this.validateHtml(htmlFile);
      await this.checkPageMetadata(htmlFile);
    }
  }

  async verifyPerformance() {
    // 번들 크기 확인
    const jsFiles = await this.glob('**/*.js');
    let totalJsSize = 0;

    for (const jsFile of jsFiles) {
      const stat = await fs.stat(jsFile);
      totalJsSize += stat.size;
    }

    const maxBundleSize = 350 * 1024; // 350KB
    if (totalJsSize > maxBundleSize) {
      this.warnings.push(`전체 JS 번들 크기가 제한을 초과합니다: ${Math.round(totalJsSize / 1024)}KB > ${Math.round(maxBundleSize / 1024)}KB`);
    }

    // 중복 의존성 확인
    await this.checkDuplicateDependencies();
  }

  async validateHtml(filePath) {
    const content = await fs.readFile(filePath, 'utf-8');

    // 기본 HTML 검증
    if (!content.includes('<!DOCTYPE html>')) {
      this.errors.push(`${filePath}에서 DOCTYPE 누락`);
    }

    if (!content.includes('<title>')) {
      this.errors.push(`${filePath}에서 title 태그 누락`);
    }

    if (!content.includes('lang=')) {
      this.warnings.push(`${filePath}에서 language 속성 누락`);
    }

    // 중요한 성능 문제 확인
    if (content.includes('render-blocking')) {
      this.warnings.push(`${filePath}에서 렌더링 차단 리소스 발견`);
    }
  }
}

// 검증 실행
if (require.main === module) {
  const verifier = new BuildVerifier();
  verifier.verify();
}
```

## 지속적 통합

### GitHub Actions 빌드 파이프라인

```yaml
name: 문서 빌드 및 배포
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    name: 문서 빌드
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [18.x, 20.x]

    steps:
      - name: 저장소 체크아웃
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Node.js 설정
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'

      - name: 의존성 설치
        run: npm ci --frozen-lockfile

      - name: 린팅 실행
        run: npm run lint

      - name: 타입 검사 실행
        run: npm run typecheck

      - name: 테스트 실행
        run: npm run test

      - name: 애플리케이션 빌드
        run: npm run build:production
        env:
          NODE_ENV: production
          ANALYZE: false

      - name: 빌드 검증
        run: npm run verify

      - name: 빌드 아티팩트 업로드
        uses: actions/upload-artifact@v3
        if: success()
        with:
          name: build-output-${{ matrix.node-version }}
          path: out/
          retention-days: 7

      - name: 라이트하우스 보고서 생성
        if: matrix.node-version == '18.x'
        run: |
          npm install -g @lhci/cli
          lhci autorun
        env:
          LHCI_GITHUB_APP_TOKEN: ${{ secrets.LHCI_GITHUB_APP_TOKEN }}

  performance-audit:
    name: 성능 감사
    needs: build
    runs-on: ubuntu-latest
    if: github.event_name == 'pull_request'

    steps:
      - name: 저장소 체크아웃
        uses: actions/checkout@v4

      - name: 빌드 아티팩트 다운로드
        uses: actions/download-artifact@v3
        with:
          name: build-output-18.x
          path: out/

      - name: 번들 분석기 실행
        run: |
          npx webpack-bundle-analyzer out/_next/static/chunks/*.js --mode static --report bundle-report.html --no-open

      - name: 번들 분석 업로드
        uses: actions/upload-artifact@v3
        with:
          name: bundle-analysis
          path: bundle-report.html
```

BaiDocs 빌드 프로세스는 여러 환경과 호스팅 플랫폼에서 고성능하고 안정적인 문서 사이트 배포를 보장하기 위한 포괄적인 자동화, 최적화, 검증을 제공합니다.
