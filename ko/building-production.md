---
title: 프로덕션 빌드
description: 프로덕션 배포를 위해 BaiDocs 빌드 및 최적화하기
---

# 프로덕션 빌드

이 가이드는 프로덕션용 BaiDocs 빌드, 최적화 및 배포 준비를 다룹니다.

## 프로덕션 빌드

### 기본 빌드

프로덕션용 뷰어 빌드:

```bash
# 뷰어만 빌드
pnpm build:viewer

# 또는 전체 빌드 (두 앱 모두)
pnpm build
```

### PDF 생성과 함께

PDF 생성 활성화:

```bash
ENABLE_PDF_GENERATION=true pnpm build
```

## 빌드 출력

### 디렉토리 구조

빌드는 다음 출력을 생성:

```
.next/
├── standalone/              # 독립 서버 빌드
│   ├── apps/viewer/
│   │   ├── server.js       # Node.js 서버
│   │   ├── .next/          # Next.js 빌드 출력
│   │   └── public/         # 정적 에셋
│   └── node_modules/       # 필요한 의존성
└── static/                  # 정적 청크
```

## 최적화

### 1. 이미지 최적화

빌드 전 이미지 최적화:

```bash
# ImageOptim 사용 (macOS)
imageoptim content/**/images/*.{png,jpg,jpeg}
```

### 2. 검색 인덱스 최적화

검색 인덱스 크기 줄이기:

```yaml
search:
  # 제목과 제목만 인덱싱
  keys:
    - title
    - headings
```

## 빌드 검증

### 빌드 출력 확인

빌드가 성공했는지 확인:

```bash
# standalone 출력 존재 확인
ls -la .next/standalone/apps/viewer/

# public 에셋 확인
ls -la .next/standalone/apps/viewer/public/
```

### 로컬 테스트

배포 전 프로덕션 빌드 테스트:

```bash
# standalone 출력으로 이동
cd .next/standalone/apps/viewer

# 프로덕션 서버 시작
PORT=3000 node server.js
```

http://localhost:3000 방문하여 확인.

## 환경 변수

### 프로덕션 변수

`.env.production` 생성:

```bash
NODE_ENV=production
NEXT_PUBLIC_SITE_URL=https://docs.example.com
ENABLE_PDF_GENERATION=true
```

## 빌드 체크리스트

배포 전 확인:

- [ ] 빌드가 오류 없이 완료
- [ ] 모든 페이지가 올바르게 렌더링
- [ ] 검색 작동
- [ ] PDF 생성됨 (활성화된 경우)
- [ ] 이미지가 올바르게 로드
- [ ] 링크 작동
- [ ] 내비게이션 기능
- [ ] 모바일 반응형
- [ ] 성능 허용 가능

## 다음 단계

빌드가 준비되었습니다! [배포 옵션](deployment-options.md)으로 문서를 게시하세요.
