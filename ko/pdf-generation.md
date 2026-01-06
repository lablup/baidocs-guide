---
title: PDF 생성
description: BaiDocs 문서에서 아름다운 PDF 생성하기
---

# PDF 생성

BaiDocs는 빌드 시간에 문서에서 자동으로 PDF 파일을 생성할 수 있습니다.

## PDF 생성 활성화

환경 변수로 제어:

```bash
# PDF 생성과 함께 빌드
ENABLE_PDF_GENERATION=true pnpm build

# PDF 없이 빌드 (더 빠름)
ENABLE_PDF_GENERATION=false pnpm build
```

:::tip
더 빠른 빌드를 위해 개발 중에는 PDF 생성을 비활성화하세요. 프로덕션 빌드에만 활성화하세요.
:::

## PDF 다운로드 버튼 표시

PDF 다운로드 버튼 표시는 PDF 생성과 별도로 제어됩니다. 이를 통해 매번 빌드할 때 PDF를 다시 생성하지 않고도 미리 생성된 PDF의 다운로드 버튼을 표시할 수 있습니다.

`next.config.js`에서 설정 (환경 변수에서 자동으로 설정됨):

```bash
# PDF 다운로드 버튼 표시 (기본값)
ENABLE_PDF_DOWNLOAD=true pnpm dev

# PDF 다운로드 버튼 숨기기
ENABLE_PDF_DOWNLOAD=false pnpm dev
```

**사용 사례:**
- **로컬 개발**: `true`로 설정하여 빌드 없이 미리 생성된 PDF 확인
- **Vercel 배포**: PDF 생성이 비활성화된 경우 `false`로 설정
- **스테이징 환경**: 공개 준비가 안 된 PDF 숨기기

:::info
`ENABLE_PDF_DOWNLOAD`는 설정하지 않으면 기본적으로 `true`이며, 미리 생성된 PDF에 기본적으로 접근할 수 있습니다.
:::

## 설정

`book.config.yaml`에서 PDF 설정:

```yaml
pdf:
  enabled: true
  split: auto  # "single", "chapter", 또는 "auto"
  cover:
    title: "내 문서"
    subtitle: "버전 1.0.0"
    author: "작성자"
    date: "2024-01-01"
  imageOptimization:
    enabled: true
    maxWidth: 1000
    quality: 70
  # 선택사항: 자동 분할 설정
  autoSplitConfig:
    maxImagesPerPdf: 15      # PDF당 최대 이미지 개수
    maxSizePerPdf: 8         # PDF당 최대 크기(MB)
    maxPagesEstimate: 200    # 예상 최대 페이지 수
    strategy: balanced       # "balanced" | "sequential" | "size-based"
```

## PDF 분할 옵션

### 단일 PDF

모든 콘텐츠를 포함하는 하나의 PDF:

```yaml
pdf:
  split: single
```

**최적 사용처:**
- 작은 문서 (< 50페이지)
- 빠른 참조 가이드

### 챕터 PDF

챕터별로 별도 PDF:

```yaml
pdf:
  split: chapter
```

**최적 사용처:**
- 큰 문서 (> 50페이지)
- 다중 주제 문서
- 특정 섹션만 필요한 사용자

**결과:**
```
public/pdfs/
├── my-book-ko-chapter-1.pdf
├── my-book-ko-chapter-2.pdf
└── my-book-ko-chapter-3.pdf
```

### 자동 분할 PDF

콘텐츠 분석(이미지 개수, 파일 크기, 페이지 수)을 기반으로 PDF를 자동으로 분할합니다. 이미지가 많은 대용량 문서를 처리하는 가장 스마트한 옵션입니다:

```yaml
pdf:
  split: auto
  autoSplitConfig:
    maxImagesPerPdf: 15       # PDF당 최대 이미지 개수
    maxSizePerPdf: 8          # PDF당 최대 크기(MB)
    maxPagesEstimate: 200     # PDF당 예상 최대 페이지 수
    strategy: balanced        # 분할 전략
```

**최적 사용처:**
- 이미지가 많은 문서
- 매우 큰 문서 (> 200페이지)
- 콘텐츠 밀도가 다양한 문서
- 이미지가 너무 많아 브라우저/PDF 뷰어 충돌 방지

**작동 방식:**
1. 모든 페이지의 이미지 개수와 예상 크기 분석
2. 다음을 기반으로 페이지를 지능적으로 그룹화:
   - 이미지 밀도 (페이지당 이미지 수)
   - 예상 파일 크기
   - 문서 구조 (가능하면 챕터 구조 유지)
3. 설정된 제한 내에서 여러 PDF 생성

**분할 전략:**
- **`balanced`** (권장): 파일 크기와 논리적 구조의 균형
- **`sequential`**: 페이지 수 기준 단순 순차 분할
- **`size-based`**: 구조보다 파일 크기 제한 우선

**결과:**
```
public/pdfs/
├── my-book-ko-part01.pdf  (15개 이미지, 7.8 MB)
├── my-book-ko-part02.pdf  (14개 이미지, 7.5 MB)
├── my-book-ko-part03.pdf  (12개 이미지, 6.2 MB)
└── pdf-mapping.json       (페이지를 PDF에 매핑)
```

`pdf-mapping.json` 파일은 각 페이지를 해당 PDF에 매핑하여, 뷰어가 각 페이지에 올바른 다운로드 링크를 표시할 수 있게 합니다.

:::warning
자동 분할은 이미지가 많은 콘텐츠로 인한 PDF 생성 실패를 방지하기 위해 설계되었습니다. 이미지가 너무 많아 PDF 생성이 실패하는 경우 `split: auto`를 사용하세요.
:::

## 이미지 최적화

PDF용 이미지 최적화:

```yaml
pdf:
  imageOptimization:
    enabled: true
    maxWidth: 1200
    quality: 85
```

## 다음 단계

[검색 설정](search-configuration.md)으로 문서를 쉽게 검색 가능하게 만드세요!
