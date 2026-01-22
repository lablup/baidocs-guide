# 첫 번째 책 만들기

이 섹션은 BaiDocs를 사용하여 첫 번째 문서화 책을 생성하는 과정을 초기 설정부터 콘텐츠 퍼블리싱까지 안내합니다.

## 책 생성 워크플로

### 편집기 인터페이스 접근

1. **편집기로 이동**:
   ```
   http://localhost:3000
   ```

2. **작업 공간 상태 확인**:
   - 편집기 인터페이스가 성공적으로 로드되는지 확인
   - 브라우저 콘솔에서 오류 메시지 확인
   - 콘텐츠 디렉토리에 접근 가능한지 확인

### 새 책 생성

#### 편집기 인터페이스 사용

1. **책 생성 시작**:
   - 메인 대시보드에서 "Create New Book" 버튼 클릭
   - 대안: 책 목록 사이드바의 "+" 아이콘 사용

2. **책 메타데이터 구성**:
   - **Book ID**: 고유 식별자 입력 (영숫자, 하이픈 허용)
   - **Title**: 문서의 표시 제목 지정
   - **Description**: 책 콘텐츠의 간결한 설명 제공
   - **Languages**: 지원 언어 선택 (기본값: 영어)

3. **초기 구성 설정**:
   - **Default Language**: 콘텐츠 작성을 위한 기본 언어 선택
   - **Emoji**: 시각적 식별을 위한 이모지 아이콘 선택 (선택사항)
   - **Repository URL**: 외부 Git 저장소 연결 시 구성

#### 수동 책 생성

또는 디렉토리 구조를 설정하여 수동으로 책을 생성할 수 있습니다:

```bash
# 콘텐츠 디렉토리로 이동
cd content

# 책 디렉토리 생성
mkdir my-first-book
cd my-first-book

# 구성 파일 생성
touch book.config.yaml

# 언어 디렉토리 생성
mkdir en ko
```

### 책 구성

#### 구성 파일 구조

다음 구조로 `book.config.yaml`을 생성하십시오:

```yaml
id: my-first-book
title: 나의 첫 번째 문서화 책
description: 제품 기능에 대한 종합 안내서
emoji: 📖
type: standalone
languages:
  - en
  - ko
defaultLanguage: en
navigation:
  en:
    - title: Introduction
      path: introduction
      items:
        - title: Welcome
          path: introduction/index.md
        - title: Overview
          path: introduction/overview.md
    - title: User Guide
      path: user-guide
      items:
        - title: Getting Started
          path: user-guide/getting-started.md
        - title: Advanced Features
          path: user-guide/advanced.md
  ko:
    - title: 소개
      path: introduction
      items:
        - title: 환영합니다
          path: introduction/index.md
        - title: 개요
          path: introduction/overview.md
    - title: 사용자 가이드
      path: user-guide
      items:
        - title: 시작하기
          path: user-guide/getting-started.md
        - title: 고급 기능
          path: user-guide/advanced.md
repoUrl: https://github.com/your-org/documentation
createdAt: 2026-01-20T00:00:00.000Z
updatedAt: 2026-01-20T00:00:00.000Z
```

#### 네비게이션 구조 가이드라인

- **섹션**: 파일 확장자 없이 `path`를 가진 최상위 컨테이너
- **파일**: `.md` 또는 `.mdx` 확장자를 가진 개별 콘텐츠 페이지
- **계층구조**: 모든 언어에서 일관된 구조 유지
- **경로 매핑**: 경로가 실제 파일 위치와 일치하는지 확인

### 콘텐츠 생성

#### 첫 번째 페이지 생성

1. 편집기 인터페이스에서 **책으로 이동**
2. **초기 디렉토리 생성**:
   - `introduction` 폴더 생성
   - `user-guide` 폴더 생성

3. **환영 페이지 추가**:
   ```markdown
   # 문서에 오신 것을 환영합니다

   이 문서는 제품 사용에 대한 종합적인 안내를 제공합니다.

   ## 빠른 시작

   시작하려면 다음 단계를 따르십시오:

   1. 개요 섹션 검토
   2. 초기 설정 완료
   3. 고급 기능 탐색

   ## 지원

   추가 지원이 필요하면 지원팀에 문의하십시오.
   ```

4. **저장 및 미리보기**:
   - Ctrl+S (macOS에서 Cmd+S) 사용하여 저장
   - 미리보기 모드로 전환하여 렌더링 확인

#### 추가 콘텐츠 추가

1. **섹션 페이지 생성**:
   - introduction 폴더에 `overview.md` 추가
   - user-guide 폴더에 `getting-started.md` 추가

2. **파일 명명 일관성 유지**:
   - 모든 언어에서 동일한 파일명 사용
   - kebab-case 명명 규칙 준수
   - 적절한 파일 확장자 포함

### 언어 조정

#### 다국어 설정

1. 각 언어 디렉토리에서 **해당 파일 생성**:
   ```
   en/introduction/index.md
   ko/introduction/index.md
   en/introduction/overview.md
   ko/introduction/overview.md
   ```

2. **네비게이션 동기화 유지**:
   - 언어 간 네비게이션 구조 일치 확인
   - 새 섹션 추가 시 `book.config.yaml` 업데이트
   - 모든 네비게이션 항목의 경로 일관성 확인

#### 번역 워크플로

1. **기본 언어 개발**:
   - 기본 언어로 콘텐츠 우선 생성
   - 완전한 네비게이션 구조 설정
   - 콘텐츠 구성 확정

2. **보조 언어 구현**:
   - `book.config.yaml`에서 네비게이션 레이블 번역
   - 해당 콘텐츠 파일 생성
   - 동일한 파일 및 폴더 구조 유지

### 테스트 및 검증

#### 콘텐츠 확인

1. **편집기 검증**:
   - 모든 네비게이션 항목이 클릭 가능한지 확인
   - 인터페이스를 통한 파일 생성 확인
   - 오류 표시기 확인

2. **뷰어 확인**:
   - `http://localhost:3001`로 이동
   - 사용 가능한 옵션에서 책 선택
   - 모든 언어에서 네비게이션 테스트
   - 콘텐츠 렌더링 및 형식 확인

#### 품질 보증

- **링크 검증**: 모든 내부 링크가 올바르게 작동하는지 확인
- **이미지 검증**: 참조된 모든 이미지에 접근 가능한지 확인
- **언어 일관성**: 번역 완성도 확인
- **네비게이션 정확성**: 모든 네비게이션 경로 테스트

### 다음 단계

첫 번째 책을 생성한 후:

1. **프로젝트 구조 탐색**으로 파일 구성 이해
2. **버전 관리 구성**으로 협업 및 백업 설정
3. **고급 콘텐츠 작성** 기술 학습
4. 프로덕션 환경을 위한 **배포 전략 구현**

첫 번째 BaiDocs 책이 이제 작동하며 콘텐츠 개발을 위한 준비가 완료되었습니다.