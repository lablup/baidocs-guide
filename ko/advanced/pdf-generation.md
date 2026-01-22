# PDF 생성

BaiDocs는 인쇄용 문서 작성, 오프라인 배포 및 아카이브 목적을 위한 포괄적인 PDF 내보내기 기능을 제공합니다. 이 섹션에서는 PDF 생성 설정, 커스터마이징 옵션 및 최적화 전략을 다룹니다.

## PDF 생성 개요

### 핵심 기능

BaiDocs PDF 생성 시스템은 다음을 제공합니다:

- **자동 레이아웃**: 적절한 타이포그래피를 가진 전문적인 페이지 레이아웃
- **챕터 구성**: 자동 챕터 구분 및 섹션 구성
- **목차**: 페이지 번호가 포함된 생성된 네비게이션
- **상호 참조**: 기능적인 내부 링크 및 참조
- **이미지 최적화**: 인쇄용 해상도 및 품질 최적화
- **사용자 정의 스타일링**: 브랜드별 레이아웃 및 형식 옵션

### 기술 스택

- **Puppeteer**: Headless Chrome 렌더링 엔진
- **CSS Print Media**: 최적화된 인쇄 스타일시트
- **Page.js**: PDF 레이아웃 및 페이지네이션 제어
- **Image Processing**: 자동 이미지 최적화 및 크기 조정

## 설정 구성

### 기본 PDF 설정

```yaml
# book.config.yaml
pdf:
  enabled: true
  split: chapter        # 옵션: page, chapter, section, book
  format: A4           # 옵션: A4, Letter, Legal, A3
  orientation: portrait # 옵션: portrait, landscape

  # 표지 페이지 설정
  cover:
    enabled: true
    title: "문서 제목"
    subtitle: "완전한 사용자 가이드"
    author: "문서 팀"
    version: "v2.0"
    date: "2026-01-20"
    logo: "./assets/images/logo.png"

  # 페이지 레이아웃 설정
  layout:
    margins:
      top: "20mm"
      right: "15mm"
      bottom: "20mm"
      left: "15mm"

    # 헤더 및 푸터
    header:
      enabled: true
      height: "15mm"
      content: "{title} - {chapter}"

    footer:
      enabled: true
      height: "15mm"
      content: "페이지 {pageNumber} / {totalPages}"

  # 이미지 최적화
  imageOptimization:
    enabled: true
    maxWidth: 1200
    quality: 85
    dpi: 300

  # 목차
  toc:
    enabled: true
    title: "목차"
    depth: 3
    pageNumbers: true

  # 고급 옵션
  options:
    embedFonts: true
    generateBookmarks: true
    includeMetadata: true
    printBackground: true
    preferCSSPageSize: true
```

### 고급 설정 옵션

```yaml
pdf:
  # 챕터별 설정
  chapters:
    - title: "소개"
      startNewPage: true
      headerContent: "시작 가이드"

    - title: "API 참조"
      startNewPage: true
      headerContent: "API 문서"
      orientation: landscape  # 특정 챕터에 대한 재정의

  # PDF 렌더링용 사용자 정의 CSS
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

  # 전처리 옵션
  preprocessing:
    removeInteractiveElements: true
    optimizeImages: true
    inlineCriticalCSS: true

  # 출력 옵션
  output:
    filename: "{title}-{version}-{date}"
    path: "./exports/pdf/"
    compression: true

  # 메타데이터
  metadata:
    title: "문서 제목"
    author: "문서 팀"
    subject: "기술 문서"
    keywords: ["문서", "가이드", "참조"]
    creator: "BaiDocs PDF Generator"
```

## PDF 스타일링 및 레이아웃

### 인쇄 전용 CSS

```css
/* PDF 생성용 인쇄 미디어 쿼리 */
@media print {
  /* 페이지 설정 */
  @page {
    size: A4;
    margin: 20mm 15mm 20mm 15mm;

    /* 헤더 및 푸터 스타일링 */
    @top-center {
      content: string(chapter-title);
      font-size: 10pt;
      color: #666;
    }

    @bottom-center {
      content: "페이지 " counter(page) " / " counter(pages);
      font-size: 10pt;
      color: #666;
    }
  }

  /* 챕터 구분 */
  .pdf-chapter {
    page-break-before: always;
  }

  /* 요소 내부 구분 방지 */
  .pdf-keep-together {
    page-break-inside: avoid;
  }

  /* 상호 작용 요소 숨김 */
  .interactive-only,
  .navigation,
  .sidebar {
    display: none !important;
  }

  /* 타이포그래피 최적화 */
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

  /* 코드 블록 */
  pre, code {
    font-family: 'Courier New', monospace;
    font-size: 9pt;
    background: #f5f5f5;
    border: 1pt solid #ddd;
    padding: 6pt;
    page-break-inside: avoid;
  }

  /* 테이블 */
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

  /* 이미지 */
  img {
    max-width: 100%;
    height: auto;
    page-break-inside: avoid;
  }

  /* 링크 */
  a {
    color: #000;
    text-decoration: underline;
  }

  /* 인쇄용 색상 배경 제거 */
  * {
    -webkit-print-color-adjust: exact;
    color-adjust: exact;
  }
}

/* 챕터 제목 스타일링 */
.chapter-title {
  string-set: chapter-title content();
}
```

### 사용자 정의 표지 페이지 디자인

```html
<!-- 표지 페이지 템플릿 -->
<div class="pdf-cover-page">
  <div class="cover-header">
    <img src="{logo}" alt="회사 로고" class="cover-logo" />
  </div>

  <div class="cover-content">
    <h1 class="cover-title">{title}</h1>
    <h2 class="cover-subtitle">{subtitle}</h2>

    <div class="cover-metadata">
      <p class="cover-author">{author} 저</p>
      <p class="cover-version">버전 {version}</p>
      <p class="cover-date">{date}</p>
    </div>
  </div>

  <div class="cover-footer">
    <p class="cover-copyright">© 2026 회사명. 모든 권리 보유.</p>
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

## 생성 프로세스 및 자동화

### 수동 PDF 생성

#### BaiDocs 인터페이스를 통해

1. **PDF 내보내기로 이동**:
   - 책 관리 인터페이스 접근
   - "내보내기" 또는 "PDF 생성" 옵션 선택
   - 생성 매개변수 및 설정 선택

2. **내보내기 옵션 설정**:
   ```typescript
   interface PDFExportOptions {
     format: 'A4' | 'Letter' | 'Legal';
     orientation: 'portrait' | 'landscape';
     includeTableOfContents: boolean;
     includeCoverPage: boolean;
     chapters?: string[];  // 포함할 특정 챕터
     language?: string;    // 내보낼 언어 버전
   }
   ```

3. **생성 진행 상황 모니터링**:
   - 실시간 진행 상황 표시기
   - 오류 보고 및 진단
   - 완료 시 다운로드 링크

#### 명령줄 인터페이스

```bash
# 전체 책에 대한 PDF 생성
baidocs pdf generate --book my-documentation --output ./exports/

# 특정 챕터 생성
baidocs pdf generate --book api-docs --chapters "introduction,authentication" --format A4

# 사용자 정의 설정으로 생성
baidocs pdf generate \
  --book user-guide \
  --config ./custom-pdf-config.yaml \
  --output-name "UserGuide-v2.0" \
  --optimize-images
```

### 자동화된 PDF 생성

#### CI/CD 통합

```yaml
# GitHub Actions 워크플로
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

#### 예약된 생성

```javascript
// 자동화된 PDF 생성 스크립트
const cron = require('node-cron');
const { generatePDF } = require('./pdf-generator');

// 매주 일요일 오전 2시에 PDF 생성
cron.schedule('0 2 * * 0', async () => {
  console.log('예약된 PDF 생성 시작...');

  try {
    const result = await generatePDF({
      book: 'main-documentation',
      config: {
        includeLatestUpdates: true,
        notifyOnCompletion: true,
        uploadToStorage: true
      }
    });

    console.log('PDF 생성 완료:', result.filename);

    // 알림 전송
    await sendNotification({
      type: 'success',
      message: `PDF가 성공적으로 생성되었습니다: ${result.filename}`,
      recipients: ['docs-team@company.com']
    });

  } catch (error) {
    console.error('PDF 생성 실패:', error);

    await sendNotification({
      type: 'error',
      message: `PDF 생성 실패: ${error.message}`,
      recipients: ['docs-team@company.com']
    });
  }
});
```

## 최적화 및 성능

### 이미지 최적화

```javascript
// PDF 생성용 이미지 처리
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

// 일괄 이미지 최적화
async function optimizeAllImages(imageDirectory, outputDirectory) {
  const images = await glob(`${imageDirectory}/**/*.{jpg,jpeg,png,gif}`);

  const optimizationPromises = images.map(async (imagePath) => {
    const relativePath = path.relative(imageDirectory, imagePath);
    const outputPath = path.join(outputDirectory, relativePath);

    await fs.ensureDir(path.dirname(outputPath));

    const optimizedBuffer = await optimizeImageForPDF(imagePath);
    await fs.writeFile(outputPath, optimizedBuffer);

    console.log(`최적화 완료: ${relativePath}`);
  });

  await Promise.all(optimizationPromises);
  console.log(`PDF 생성을 위해 ${images.length}개의 이미지를 최적화했습니다`);
}
```

### 성능 모니터링

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
    console.log('PDF 생성 지표:', {
      duration: `${this.metrics.duration}ms`,
      pages: this.metrics.pageCount,
      fileSize: `${(this.metrics.fileSize! / 1024 / 1024).toFixed(2)}MB`,
      stages: this.metrics.processingStages
    });
  }
}
```

## 문제 해결 및 품질 보증

### 일반적인 문제 및 해결책

#### 폰트 렌더링 문제

```yaml
# 폰트 임베딩 설정
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

#### 페이지 구분 문제

```css
/* 원하지 않는 페이지 구분 방지 */
.keep-together {
  page-break-inside: avoid;
  break-inside: avoid;
}

/* 강제 페이지 구분 */
.page-break-before {
  page-break-before: always;
  break-before: page;
}

/* 고아 및 미망인 제어 */
p {
  orphans: 3;
  widows: 3;
}
```

#### 이미지 품질 문제

```typescript
// 고해상도 이미지 처리
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

### 품질 보증 체크리스트

```markdown
## PDF 품질 보증 체크리스트

### 콘텐츠 검증
- [ ] 모든 섹션 및 챕터 포함
- [ ] 올바른 페이지 번호로 목차 정확성 확인
- [ ] 상호 참조 및 내부 링크 기능 확인
- [ ] 코드 블록이 올바르게 형식화되고 가독성 확인
- [ ] 테이블이 페이지 여백 내에 맞음
- [ ] 이미지가 적절한 해상도로 명확히 표시

### 레이아웃 및 타이포그래피
- [ ] 문서 전체에 일관된 타이포그래피
- [ ] 적절한 페이지 구분 및 섹션 구성
- [ ] 헤더 및 푸터가 올바르게 표시
- [ ] 여백이 적절하고 일관성 있음
- [ ] 고아 제목이나 미망인 텍스트 없음

### 기술적 검증
- [ ] 여러 PDF 리더에서 올바르게 열림
- [ ] 콘텐츠 양에 비해 적절한 파일 크기
- [ ] 북마크 및 네비게이션 기능 작동
- [ ] 메타데이터가 올바르게 채워짐
- [ ] 인쇄 미리보기에서 적절한 형식 표시

### 접근성
- [ ] 문서 구조가 올바르게 태그됨
- [ ] 이미지에 대체 텍스트 포함
- [ ] 읽기 순서가 논리적이고 순차적
- [ ] 인쇄에 충분한 색상 대비
- [ ] 텍스트가 선택 가능하고 검색 가능
```

BaiDocs PDF 생성은 전문 문서 배포 및 아카이브 요구사항을 위한 광범위한 커스터마이징 옵션을 갖춘 엔터프라이즈급 문서 내보내기 기능을 제공합니다.
