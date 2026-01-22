# 프로젝트 구조

BaiDocs 프로젝트 구성을 이해하는 것은 효과적인 콘텐츠 관리와 협업에 필수적입니다. 이 섹션에서는 파일 시스템 구조와 구성 원칙을 자세히 설명합니다.

## 디렉토리 아키텍처

### 루트 레벨 구조

```
baidocs/
├── apps/
│   ├── editor/          # 콘텐츠 관리 인터페이스
│   └── viewer/          # 문서 표현 계층
├── packages/
│   ├── ui/              # 공유 UI 컴포넌트
│   └── mdx-editor/      # MDX 처리 엔진
├── content/
│   ├── book-one/        # 개별 문서화 책
│   ├── book-two/
│   └── shared-assets/   # 책 간 공유 리소스
├── .env.local           # 환경 구성
├── package.json         # 프로젝트 종속성
└── turbo.json          # 모노레포 빌드 구성
```

### 콘텐츠 디렉토리 구성

#### 독립형 책 구조

```
content/
└── my-book/
    ├── book.config.yaml    # 책 구성
    ├── en/                 # 영어 콘텐츠
    │   ├── introduction/
    │   │   ├── index.md
    │   │   └── overview.md
    │   └── user-guide/
    │       └── getting-started.md
    ├── ko/                 # 한국어 콘텐츠
    │   ├── introduction/
    │   │   ├── index.md
    │   │   └── overview.md
    │   └── user-guide/
    │       └── getting-started.md
    └── images/             # 공유 이미지 자산
        ├── screenshot1.png
        └── diagram.svg
```

#### 모노레포 책 구조

대규모 코드베이스 내 문서화용:

```
project-repository/
├── src/                    # 애플리케이션 소스 코드
├── docs/                   # 문서화 하위 디렉토리
│   ├── book.config.yaml
│   ├── en/
│   ├── ko/
│   └── images/
└── README.md
```

## 구성 파일

### 책 구성 (`book.config.yaml`)

책 메타데이터와 네비게이션 구조를 정의하는 주요 구성 파일:

#### 필수 필드

```yaml
id: unique-book-identifier
title: 책 표시 제목
description: 메타데이터용 책 설명
type: standalone  # 또는 'monorepo'
languages:
  - en
  - ko
defaultLanguage: en
```

#### 네비게이션 구성

```yaml
navigation:
  en:
    - title: Section Name
      path: section-directory
      items:
        - title: Page Title
          path: section-directory/page-file.md
```

#### 선택적 구성

```yaml
emoji: 📖                    # 표시 아이콘
repoUrl: https://github.com/org/repo
pdf:
  split: chapter             # PDF 생성 설정
  cover:
    title: 사용자 정의 PDF 제목
    subtitle: PDF 부제목
```

### 환경 구성 (`.env.local`)

```env
# 콘텐츠 디렉토리 위치
CONTENT_PATH=./content

# 애플리케이션 포트
EDITOR_PORT=3000
VIEWER_PORT=3001

# 개발 모드 설정
NODE_ENV=development

# 선택사항: Git 통합
GIT_AUTHOR_NAME="Documentation Team"
GIT_AUTHOR_EMAIL="docs@company.com"
```

## 콘텐츠 구성 원칙

### 계층적 구조

#### 섹션 기반 구성

- **논리적 그룹화**: 기능 영역이나 사용자 워크플로별로 콘텐츠 구성
- **일관된 깊이**: 섹션 간 유사한 계층 깊이 유지
- **명확한 명명**: 서술적인 디렉토리와 파일명 사용

#### 파일 명명 규칙

```
✅ 권장:
getting-started.md
user-authentication.md
api-reference.md

❌ 피하기:
GettingStarted.md
user_authentication.md
1-getting-started.md
```

### 다국어 조정

#### 동기화된 구조

모든 언어에서 동일한 디렉토리 구조 유지:

```
en/section/subsection/page.md
ko/section/subsection/page.md  # 동일한 경로 구조
ja/section/subsection/page.md
```

#### 번역 관리

- **파일명 일관성**: 언어 간 동일한 파일명 사용
- **네비게이션 동기화**: 네비게이션 구조 정렬 유지
- **자산 공유**: 언어 간 이미지 및 미디어 파일 공유

### 자산 관리

#### 이미지 구성

```
images/
├── screenshots/
│   ├── editor-interface.png
│   └── viewer-navigation.png
├── diagrams/
│   ├── architecture.svg
│   └── workflow.svg
└── logos/
    ├── company-logo.png
    └── partner-logos/
```

#### 자산 모범 사례

- **서술적 명명**: 명확하고 서술적인 파일명 사용
- **최적화**: 웹 전달에 적합하게 이미지 압축
- **형식 선택**: 적절한 형식 사용 (스크린샷은 PNG, 다이어그램은 SVG)
- **버전 관리**: 일관성을 위해 자산을 버전 관리에 포함

## 경로 해결

### 가상 경로 매핑

BaiDocs는 원활한 콘텐츠 접근을 위한 가상 경로 매핑을 구현합니다:

#### UI 경로 vs. 파일 시스템 경로

```
UI 경로:         en/getting-started/installation.md
파일 시스템:     content/book/en/getting-started/installation.md
Git 경로:        en/getting-started/installation.md (독립형)
                docs/en/getting-started/installation.md (모노레포)
```

#### 네비게이션 경로 구성

- **섹션 경로**: 확장자 없는 디렉토리명
- **파일 경로**: 언어 루트에서의 상대 경로
- **인덱스 파일**: 섹션 랜딩 페이지에 `index.md` 사용

### 모노레포 고려사항

#### 하위 디렉토리 구성

모노레포 책의 경우 문서화 하위 디렉토리를 지정:

```yaml
type: monorepo
subdirectory: docs
localPath: /path/to/repository
virtualRoot: /path/to/repository/docs
```

#### 브랜치 관리

- **브랜치 필터링**: 관련 문서 브랜치만 표시
- **스파스 체크아웃**: 문서 하위 디렉토리만 복제
- **경로 격리**: 작업이 문서 경계 내에서 유지되도록 보장

## 디렉토리 권한 및 보안

### 파일 시스템 접근

- **읽기 권한**: 콘텐츠 디렉토리에 필요
- **쓰기 권한**: 콘텐츠 편집 및 Git 작업에 필요
- **실행 권한**: 빌드 및 배포 스크립트에 필요

### 보안 고려사항

- **경로 순회 방지**: 모든 파일 경로 검증
- **접근 제어**: 적절한 사용자 권한 구현
- **Git 보안**: 저장소 접근을 위한 안전한 자격 증명 관리

## 유지보수 및 구성

### 정기 유지보수 작업

1. **자산 최적화**: 주기적으로 이미지 파일 검토 및 최적화
2. **링크 검증**: 내부 및 외부 링크 정확성 확인
3. **구조 검토**: 네비게이션 구성 효과성 평가
4. **번역 업데이트**: 언어 동기화 유지

### 확장성 고려사항

- **성능 모니터링**: 빌드 및 로드 시간 추적
- **콘텐츠 모듈화**: 대형 책 분할 고려
- **자산 CDN**: 대규모 배포를 위한 콘텐츠 전송 네트워크 구현
- **검색 최적화**: 검색 인덱스 효율성 유지

이 프로젝트 구조를 이해하면 효과적인 콘텐츠 관리가 가능해지고 팀과 언어 간 협업이 원활해집니다.