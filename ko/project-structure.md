---
title: 프로젝트 구조
description: BaiDocs 프로젝트 구조와 구성 이해하기
---

# 프로젝트 구조

BaiDocs의 구조를 이해하면 코드베이스를 탐색하고 효과적으로 커스터마이징하는 데 도움이 됩니다.

## 개요

BaiDocs는 pnpm 워크스페이스로 구동되는 **모노레포 구조**를 사용합니다.

```
baidocs/
├── apps/                # 애플리케이션
├── packages/            # 공유 패키지
├── content/             # 문서 북
├── scripts/             # 빌드 & 유틸리티 스크립트
└── public/              # 정적 에셋
```

## 디렉토리 상세

### apps/

두 개의 주요 애플리케이션 포함:

```
apps/
├── editor/              # 문서 에디터 (포트 3001)
│   ├── src/
│   │   ├── app/        # Next.js app routes
│   │   ├── components/ # React 컴포넌트
│   │   └── lib/        # 유틸리티 함수
│   └── package.json
│
└── viewer/              # 문서 뷰어 (포트 3000)
    ├── src/
    │   ├── app/        # Next.js app routes
    │   ├── components/ # React 컴포넌트
    │   └── lib/        # 유틸리티 함수
    ├── themes/         # 테마 CSS 파일
    └── package.json
```

### packages/

앱 간 공유 코드:

```
packages/
├── ui/                  # 공유 UI 컴포넌트
├── mdx-editor/          # MDX 에디터 컴포넌트
└── config/              # 설정 유틸리티
```

### content/

문서 북이 여기에 있습니다:

```
content/
├── book-1/
│   ├── book.config.yaml
│   ├── ko/
│   │   ├── index.md
│   │   └── images/
│   └── en/
│       └── index.md
└── book-2/
    └── ...
```

**핵심 포인트**:
- 각 북은 자체 포함
- 북은 별도 Git 저장소 가능
- 이미지 디렉토리는 언어 간 공유

### scripts/

빌드 및 유틸리티 스크립트:

```
scripts/
├── build-search-index.mjs    # 검색 인덱스 생성
├── build-pdfs.mjs            # PDF 파일 생성
└── copy-themes.mjs           # 테마 CSS 복사
```

## 설정 파일

### 루트 레벨

#### pnpm-workspace.yaml

```yaml
packages:
  - 'apps/*'
  - 'packages/*'
```

#### baidocs.config.yaml

```yaml
theme:
  name: "default"
  path: "themes"

build:
  generatePDFs: false
  optimizeImages: true
```

### 애플리케이션 레벨

#### next.config.mjs

```javascript
const nextConfig = {
  output: 'standalone',
  reactStrictMode: true,
  transpilePackages: ['@baidocs/ui']
}
```

## 빌드 출력

### 프로덕션 빌드

`pnpm build` 실행 시 생성:

```
.next/
├── standalone/      # 독립 배포
│   ├── apps/viewer/
│   │   ├── server.js
│   │   ├── .next/
│   │   └── public/
│   └── node_modules/
└── static/          # 정적 에셋
```

## 다음 단계

이제 프로젝트 구조를 이해했으니 [마크다운 기본 문법](markdown-basics.md)으로 콘텐츠 작성을 시작하세요!
