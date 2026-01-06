---
title: 주요 기능
description: BaiDocs를 돋보이게 하는 강력한 기능 탐색하기
---

# 주요 기능

BaiDocs는 문서를 더 쉽고 강력하게 만들기 위해 설계된 기능들로 가득합니다. BaiDocs를 차별화하는 주요 기능들을 살펴보겠습니다.

## 📝 MDX 지원

BaiDocs는 **MDX (Markdown + JSX)**를 사용하여 두 세계의 장점을 제공합니다:

### 표준 마크다운

익숙한 마크다운 문법으로 작성:

```markdown
# 제목
**굵게** 그리고 *기울임* 텍스트
- 리스트
- 더 많은 리스트
[링크](https://example.com)
```

### GitHub Flavored Markdown (GFM)

테이블, 작업 목록 등 지원:

- [x] 체크박스가 있는 작업 목록
- [x] 구조화된 데이터를 위한 테이블
- [x] ~~취소선~~ 텍스트
- [ ] 자동링크

### React로 확장 가능

더 많은 기능이 필요할 때 React 컴포넌트 추가:

```jsx
<CustomComponent prop="value">
  여기에 콘텐츠
</CustomComponent>
```

:::info
현재 BaiDocs는 풍부한 마크다운 확장 기능을 제공합니다. MDX 렌더러를 확장하여 사용자 정의 React 컴포넌트를 추가할 수 있습니다.
:::

## 🎨 테마 시스템

BaiDocs는 강력한 빌드 타임 테마 시스템을 포함합니다:

### 사용 가능한 테마

1. **default** - 깔끔한 Ant Design 기반 테마
2. **backend-ai** - Backend.AI 브랜딩에 맞는 다채로운 테마

### 테마 커스터마이징

테마는 쉬운 커스터마이징을 위해 CSS 사용자 정의 속성을 사용합니다:

```css
:root {
  --primary-color: #1890ff;
  --text-color: #333;
  --background-color: #fff;
}
```

### 사용자 정의 테마 생성

세 단계로 자신만의 테마 생성:

1. `apps/viewer/themes/my-theme/theme.css` 생성
2. CSS 변수와 스타일 정의
3. `baidocs.config.yaml` 업데이트:

```yaml
theme:
  name: "my-theme"
  path: "themes"
```

:::tip
테마를 쉽게 유지보수하고 일관성있게 만들려면 CSS 사용자 정의 속성을 사용하세요.
:::

## 🔍 검색 시스템

BaiDocs는 강력한 클라이언트 사이드 검색 시스템을 포함합니다:

### 기능

- **빠른 퍼지 검색** Fuse.js로 구동
- **빌드 타임 인덱싱** - 서버 불필요
- **전체 텍스트 검색** 모든 콘텐츠에 걸쳐
- **북 및 언어 필터링**
- **키보드 단축키** (Cmd/Ctrl + K)

### 작동 방식

빌드 시간 동안 BaiDocs는:

1. 모든 마크다운 파일 크롤링
2. 텍스트 콘텐츠 추출
3. 검색 인덱스 생성 (300페이지에 약 2MB)
4. 정적 JSON 파일로 저장

```bash
# 검색 인덱스 빌드
pnpm build:search
```

결과는 백엔드 없이 즉시 클라이언트 사이드 검색!

:::success
검색 결과에는 컨텍스트 강조 표시와 관련성에 기반한 스마트 순위가 포함됩니다.
:::

## 📄 PDF 생성

빌드 중 자동으로 아름다운 PDF 생성:

### PDF 옵션

**단일 PDF**: 모든 콘텐츠를 하나의 파일로

```yaml
pdf:
  split: single
  cover:
    title: "완전 가이드"
    subtitle: "버전 1.0"
```

**챕터 PDF**: 챕터당 별도 PDF

```yaml
pdf:
  split: chapter
  cover:
    title: "문서"
```

### 이미지 최적화

PDF는 자동 이미지 최적화 지원:

```yaml
pdf:
  imageOptimization:
    enabled: true
    maxWidth: 1200
    quality: 85
```

### PDF로 빌드

```bash
# PDF 생성 활성화
ENABLE_PDF_GENERATION=true pnpm build
```

:::warning
PDF 생성에는 Puppeteer가 필요하며 빌드 시간이 크게 증가할 수 있습니다. 더 빠른 개발 빌드를 위해 비활성화하세요.
:::

## 🌍 다국어 지원

다국어 문서에 대한 일급 지원:

### 언어 설정

```yaml
languages:
  - code: "en"
    name: "English"
  - code: "ko"
    name: "한국어"
  - code: "ja"
    name: "日本語"
defaultLanguage: "en"
```

### 디렉토리 구조

각 언어는 자체 디렉토리를 가집니다:

```
content/my-book/
├── en/
│   ├── index.md
│   └── guide.md
├── ko/
│   ├── index.md
│   └── guide.md
└── images/  # 언어 간 공유
```

### 독립적인 내비게이션

각 언어는 다른 내비게이션을 가질 수 있습니다:

```yaml
navigation:
  en:
    - title: Getting Started
      items:
        - title: Introduction
          path: index.md
  ko:
    - title: 시작하기
      items:
        - title: 소개
          path: index.md
```

## 📦 가져오기 시스템

다른 포맷에서 문서 가져오기:

### 지원되는 포맷

| 포맷 | 기능 |
|------|------|
| **GitBook** | SUMMARY.md 파싱, 에셋 변환 |
| **Sphinx** | RST에서 MDX 변환, .po 번역 |
| **MkDocs** | mkdocs.yml 내비게이션 파싱 |
| **Markdown** | 직접 마크다운 가져오기 |

### 가져오기 프로세스

1. 외부 저장소 클론
2. 소스 포맷 파싱
3. BaiDocs MDX로 변환
4. book.config.yaml 생성
5. 콘텐츠 디렉토리로 복사

```bash
# 에디터 UI에서 가져오기:
# 1. "New Book" 클릭
# 2. "Import" 선택
# 3. 포맷 선택
# 4. 저장소 URL 입력
```

:::note
가져오기 시스템은 BaiDocs 포맷으로 변환하면서 원본 구조를 보존합니다.
:::

## ✏️ WYSIWYG 에디터

BaiDocs는 강력한 에디터를 포함합니다 (포트 3001):

### 에디터 모드

- **WYSIWYG** - 보이는 대로 얻는 것
- **Markdown** - 원시 마크다운 편집
- **분할 뷰** - 편집과 미리보기 나란히
- **미리보기** - 전체 미리보기 모드

### 기능

- 📁 **파일 트리 내비게이션**
- 🖼️ **드래그 앤 드롭 이미지 업로드**
- 💾 **자동 저장**
- 🔍 **실시간 미리보기**
- 📝 **프론트매터 지원**

### 이미지 업로드

이미지는 자동으로:

- `images/` 디렉토리에 저장
- 고유성을 위해 UUID로 명명
- 상대 경로로 참조

## 🚀 성능

BaiDocs는 속도를 위해 구축되었습니다:

### 정적 사이트 생성

- 빌드 시간에 모든 페이지 미리 렌더링
- 즉시 페이지 로드
- 완벽한 Lighthouse 점수

### 최적화

- 더 빠른 초기 로드를 위한 코드 분할
- 이미지 최적화
- 최소 JavaScript 번들
- 개발 중 빠른 새로고침

### 빌드 시간

일반적인 빌드 시간:

- **작은 사이트** (50페이지): ~30초
- **중간 사이트** (200페이지): ~2분
- **큰 사이트** (500페이지): ~5분

:::tip
훨씬 빠른 빌드를 위해 개발 중 PDF 생성을 비활성화하세요:
```bash
ENABLE_PDF_GENERATION=false pnpm dev
```
:::

## 📐 어드모니션

중요한 정보를 강조하기 위한 풍부한 콜아웃 박스:

### 사용 가능한 타입

:::note
**노트** - 일반 정보
:::

:::info
**정보** - 정보성 콘텐츠
:::

:::tip
**팁** - 유용한 제안
:::

:::success
**성공** - 긍정적인 결과
:::

:::warning
**경고** - 중요한 주의사항
:::

:::danger
**위험** - 치명적인 경고
:::

### 문법

```markdown
:::type
여기에 콘텐츠
:::
```

지원되는 타입: `note`, `info`, `tip`, `hint`, `important`, `success`, `warning`, `caution`, `attention`, `danger`, `error`

## 🎯 코드 강조

100개 이상의 프로그래밍 언어에 대한 구문 강조:

### 지원되는 언어

```javascript
// JavaScript
console.log('안녕하세요!');
```

```python
# Python
print("안녕하세요!")
```

```rust
// Rust
fn main() {
    println!("안녕하세요!");
}
```

```yaml
# YAML
key: value
nested:
  - item1
  - item2
```

:::info
코드 강조는 자동 언어 감지 기능이 있는 highlight.js로 구동됩니다.
:::

## 📊 테이블

GFM 테이블 완전 지원:

| 기능 | 상태 | 우선순위 |
|------|------|---------|
| 검색 | ✅ 완료 | 높음 |
| PDF 내보내기 | ✅ 완료 | 높음 |
| 테마 | ✅ 완료 | 중간 |
| 다이어그램 | 📋 계획됨 | 낮음 |

테이블 지원:
- 정렬 (왼쪽, 가운데, 오른쪽)
- 셀 내 마크다운 포맷팅
- 임의의 열 수

## 🏗️ 모노레포 구조

BaiDocs는 효율적인 개발을 위해 pnpm 워크스페이스를 사용합니다:

```
packages/
├── ui/              # 공유 UI 컴포넌트
├── mdx-editor/      # 에디터 컴포넌트
└── config/          # 설정 유틸리티
```

이점:
- **코드 공유** 에디터와 뷰어 간
- **단일 의존성 트리**
- **빠른 설치** pnpm으로
- **기여하기 쉬움**

## 다음 단계

시작할 준비가 되셨나요? [설치](installation.md)로 이동하여 첫 BaiDocs 프로젝트를 설정하세요!
