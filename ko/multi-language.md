---
title: 다국어 지원
description: BaiDocs로 여러 언어의 문서 만들기
---

# 다국어 지원

BaiDocs는 다국어 문서에 대한 일급 지원을 제공합니다.

## 언어 설정

`book.config.yaml`에서 언어 정의:

```yaml
id: my-book
title: 내 문서
languages:
  - ko
  - en
defaultLanguage: ko
```

## 디렉토리 구조

각 언어는 자체 디렉토리를 가집니다:

```
content/my-book/
├── book.config.yaml
├── ko/
│   ├── index.md
│   └── guide.md
├── en/
│   ├── index.md
│   └── guide.md
└── images/  # 언어 간 공유
```

:::info
`images/` 디렉토리는 모든 언어 간 공유되어 에셋 중복을 방지합니다.
:::

## 언어별 내비게이션

각 언어는 다른 내비게이션을 가질 수 있습니다:

```yaml
navigation:
  ko:
    - title: 시작하기
      items:
        - title: 소개
          path: index.md
  en:
    - title: Getting Started
      items:
        - title: Introduction
          path: index.md
```

## 콘텐츠 번역

각 영어 페이지에 대해 해당하는 페이지를 다른 언어로 생성:

**한국어** (`ko/index.md`):
```markdown
---
title: 환영합니다
---

# BaiDocs에 오신 것을 환영합니다
```

**영어** (`en/index.md`):
```markdown
---
title: Welcome
---

# Welcome to BaiDocs
```

## 다음 단계

배포할 준비가 되셨나요? [프로덕션 빌드](building-production.md)를 배우세요!
