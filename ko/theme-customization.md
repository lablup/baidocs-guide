---
title: 테마 커스터마이징
description: BaiDocs 문서의 외관 커스터마이징하기
---

# 테마 커스터마이징

BaiDocs는 문서의 모양과 느낌을 커스터마이징할 수 있는 강력한 테마 시스템을 포함합니다.

## 사용 가능한 테마

### 기본 테마

깔끔하고 전문적인 Ant Design 기반 테마

### Backend.AI 테마

Backend.AI 브랜딩에 맞는 다채로운 테마

## 테마 선택

`baidocs.config.yaml`에서 테마 설정:

```yaml
theme:
  name: "default"  # 또는 "backend-ai"
  path: "themes"
```

## 사용자 정의 테마 생성

### 1단계: 테마 디렉토리 생성

```bash
mkdir -p apps/viewer/themes/my-theme
```

### 2단계: 테마 CSS 생성

`apps/viewer/themes/my-theme/theme.css` 생성:

```css
:root {
  /* 기본 색상 */
  --primary-color: #6366f1;
  --text-color: #1f2937;
  --background: #ffffff;
  
  /* 간격 */
  --spacing-sm: 8px;
  --spacing-md: 16px;
  --spacing-lg: 24px;
}
```

### 3단계: 설정 업데이트

```yaml
theme:
  name: "my-theme"
  path: "themes"
```

### 4단계: 빌드 및 테스트

```bash
pnpm build
pnpm dev
```

## CSS 변수

### 필수 변수

```css
:root {
  --primary-color: #1890ff;      /* 메인 브랜드 색상 */
  --text-color: #000000;          /* 메인 텍스트 */
  --background: #ffffff;          /* 메인 배경 */
}
```

## 다음 단계

[PDF 생성](pdf-generation.md)으로 문서의 아름다운 PDF를 만드는 방법을 배우세요!
