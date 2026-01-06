---
title: 첫 번째 북 만들기
description: BaiDocs에서 첫 문서 북을 만드는 방법 배우기
---

# 첫 번째 북 만들기

첫 문서 북을 만들어봅시다! 이 튜토리얼은 시작부터 끝까지 전체 과정을 안내합니다.

## 북이란?

BaiDocs에서 **북**은 논리적 구조로 구성된 관련 문서 페이지의 모음입니다.

각 북은:
- `content/` 아래 자체 디렉토리에 존재
- 독립적인 설정을 가짐
- 여러 언어 지원 가능
- 자체 내비게이션 구조를 가짐

## 북을 만드는 세 가지 방법

1. **에디터 사용** - 시각적 인터페이스 (가장 쉬움)
2. **수동 생성** - 직접 파일 생성 (가장 많은 제어)
3. **외부에서 가져오기** - 기존 문서 변환

## 방법 1: 에디터 사용

### 1단계: 에디터 시작

```bash
pnpm editor
```

브라우저에서 http://localhost:3001 열기

### 2단계: 새 북 생성

1. **"New Book"** 버튼 클릭
2. 북 세부정보 입력:
   - **Book ID**: `my-first-book`
   - **Title**: `나의 첫 번째 북`
   - **Description**: `BaiDocs 사용 배우기`
   - **Languages**: `한국어 (ko)` 선택
3. **"Create"** 클릭

## 방법 2: 수동 생성

### 1단계: 디렉토리 구조 생성

```bash
cd content
mkdir -p my-first-book/ko/images
```

### 2단계: 북 설정 생성

`content/my-first-book/book.config.yaml` 생성:

```yaml
id: my-first-book
title: 나의 첫 번째 북
description: BaiDocs 사용 배우기
type: document
languages:
  - ko
defaultLanguage: ko
navigation:
  ko:
    - title: 시작하기
      items:
        - title: 환영합니다
          path: index.md
        - title: 설치
          path: installation.md
```

### 3단계: 콘텐츠 페이지 생성

`content/my-first-book/ko/index.md` 생성:

```markdown
---
title: 환영합니다
description: 첫 북에 오신 것을 환영합니다
---

# 나의 첫 번째 북에 오신 것을 환영합니다

이것은 문서의 홈페이지입니다.

## 빠른 링크

- [설치](installation.md)

시작해봅시다!
```

### 4단계: 뷰어 시작

```bash
pnpm viewer
```

http://localhost:3000/books/my-first-book/ko 방문하여 북 확인!

## 북 설정 이해하기

`book.config.yaml` 파일 분석:

### 기본 정보

```yaml
id: my-book              # 고유 식별자 (URL 안전)
title: 나의 북           # 표시 제목
description: 설명...     # 짧은 설명
type: document           # 북 타입 (document, api, guide)
```

### 언어

```yaml
languages:
  - ko                   # 한국어
  - en                   # 영어
defaultLanguage: ko      # 기본 언어
```

### 내비게이션 구조

```yaml
navigation:
  ko:                    # 한국어 내비게이션
    - title: 챕터 1      # 챕터 제목
      items:             # 챕터의 페이지
        - title: 페이지 1
          path: page1.md
```

## 이미지 추가

이미지는 `images/` 디렉토리에 배치:

```
content/my-first-book/ko/
├── index.md
├── page1.md
└── images/
    ├── screenshot.png
    └── logo.jpg
```

마크다운에서 이미지 참조:

```markdown
![스크린샷](images/screenshot.png)
```

:::tip
`images/` 디렉토리는 특별합니다 - 내비게이션에서 제외되지만 파일은 삽입 가능합니다.
:::

## 모범 사례

### 1. 명확한 북 ID 선택

좋음:
- ✅ `user-guide`
- ✅ `api-reference`

나쁨:
- ❌ `UserGuide` (URL 안전하지 않음)
- ❌ `my guide` (공백 포함)

### 2. 콘텐츠를 논리적으로 구성

일반에서 구체적으로 콘텐츠 구조화:

```
1. 소개
2. 시작하기
3. 기본 개념
4. 고급 주제
5. API 참조
```

### 3. 좋은 프론트매터 작성

모든 페이지에 프론트매터가 있어야 합니다:

```yaml
---
title: 명확하고 설명적인 제목
description: 검색과 SEO를 위한 간단한 요약
---
```

## 북 테스트

공유하기 전에 철저히 테스트:

### 1. 내비게이션 확인

- [ ] 모든 링크가 올바르게 작동
- [ ] 내비게이션 계층이 의미있음
- [ ] 깨진 내부 링크 없음

### 2. 콘텐츠 확인

- [ ] 모든 페이지에 제목 있음
- [ ] 프론트매터 완성
- [ ] 이미지가 올바르게 로드됨
- [ ] 코드 블록이 적절히 렌더링됨

### 3. 빌드 테스트

```bash
# 프로덕션 빌드 시도
ENABLE_PDF_GENERATION=false pnpm build

# 오류 확인
```

## 다음 단계

축하합니다! 첫 북을 만들었습니다. 이제 [프로젝트 구조](project-structure.md)를 깊이 이해해봅시다.

---

:::success
이제 BaiDocs로 전문 문서를 만들 준비가 되었습니다!
:::
