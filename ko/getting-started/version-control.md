# 버전 관리 연동

BaiDocs는 협업 문서화 워크플로, 변경 추적, 배포 자동화를 위한 포괄적인 Git 통합을 제공합니다. 이 섹션에서는 버전 관리 구현을 위한 구성과 모범 사례를 다룹니다.

## Git 통합 개요

### 내장 Git 기능

BaiDocs는 다음과 같은 네이티브 Git 지원을 포함합니다:

- **저장소 관리**: 복제, 풀, 푸시, 페치 작업
- **브랜치 워크플로**: 기능 브랜치 생성 및 관리
- **충돌 해결**: 내장된 병합 충돌 감지 및 해결
- **커밋 자동화**: 설명이 포함된 자동 커밋 메시지 생성
- **원격 동기화**: GitHub, GitLab 및 기타 Git 공급자와의 통합

### 통합 아키텍처

```
BaiDocs Editor
    ↓
MonorepoGit / SimpleGit
    ↓
Local Git Repository
    ↓
Remote Git Provider (GitHub/GitLab)
```

## 저장소 설정

### 새 저장소 초기화

#### 로컬 저장소 생성

1. **Git 저장소 초기화**:
   ```bash
   cd content/your-book
   git init
   git add .
   git commit -m "Initial documentation setup"
   ```

2. **원격 저장소 구성**:
   ```bash
   git remote add origin https://github.com/your-org/documentation.git
   git push -u origin main
   ```

#### BaiDocs 구성

저장소 정보를 포함하도록 `book.config.yaml` 업데이트:

```yaml
repoUrl: https://github.com/your-org/documentation
branch: main
type: standalone  # 또는 monorepo
```

### 기존 저장소 통합

#### 독립형 저장소

전용 문서 저장소의 경우:

1. **기존 저장소 복제**:
   - BaiDocs 편집기의 "Import from Git" 기능 사용
   - 저장소 URL 및 인증 자격 증명 제공
   - 콘텐츠를 위한 대상 디렉토리 선택

2. **구성 확인**:
   - `book.config.yaml`이 존재하고 적절한 설정을 포함하는지 확인
   - 콘텐츠 디렉토리 구조 검증
   - 네비게이션 및 콘텐츠 렌더링 테스트

#### 모노레포 통합

대규모 코드베이스 내 문서화의 경우:

1. **모노레포 설정 구성**:
   ```yaml
   type: monorepo
   subdirectory: docs
   selectedBranches:
     - main
     - develop
     - documentation-updates
   ```

2. **스파스 체크아웃 구현**:
   - BaiDocs는 모노레포에 대해 자동으로 스파스 체크아웃을 구성
   - 문서 파일만 로컬로 동기화됨
   - 복제 시간 및 저장소 요구사항 감소

## 워크플로 패턴

### 기능 브랜치 워크플로

#### 브랜치 생성

1. BaiDocs 인터페이스를 통한 **기능 브랜치 생성**:
   - 편집기의 Git 패널로 이동
   - "Create Branch" 버튼 클릭
   - 명명 규칙을 따르는 브랜치명 지정

2. **대안 명령줄 생성**:
   ```bash
   git checkout -b feature/user-guide-updates
   git push -u origin feature/user-guide-updates
   ```

#### 콘텐츠 개발

1. BaiDocs 편집기를 사용한 **콘텐츠 변경**
2. 설명이 포함된 메시지로 **변경 사항 커밋**:
   - BaiDocs가 자동으로 커밋 메시지 생성
   - 수동 커밋 메시지 사용자 정의 가능
   - 협업 작업을 위한 공동 저자 표시 포함

3. 원격 저장소에 **변경 사항 푸시**:
   - BaiDocs 푸시 기능 사용
   - 커밋 시 자동 푸시 구성 (선택사항)

### 협업 워크플로

#### 다중 저자 조정

1. **브랜치 할당**: 팀 구성원에게 특정 브랜치 할당
2. **콘텐츠 검토**: 품질 관리를 위한 풀 리퀘스트 워크플로 구현
3. **충돌 해결**: BaiDocs 충돌 해결 인터페이스 사용

#### 병합 전략

- **스쿼시 병합**: 깔끔한 히스토리를 위한 여러 커밋 통합
- **병합 커밋**: 상세한 개발 히스토리 보존
- **리베이스 워크플로**: 선형 커밋 히스토리 유지

## 인증 및 보안

### 인증 방법

#### HTTPS 인증

1. **개인 액세스 토큰** (권장):
   ```bash
   git config --global credential.helper store
   # 프롬프트에서 사용자명과 개인 액세스 토큰 제공
   ```

2. **사용자명/비밀번호 인증**:
   - BaiDocs 설정을 통한 구성
   - 시스템 키체인에 자격 증명 안전하게 저장

#### SSH 키 인증

1. **SSH 키 쌍 생성**:
   ```bash
   ssh-keygen -t ed25519 -C "your-email@company.com"
   ```

2. **Git 공급자에 공개 키 추가**:
   - 공개 키 내용을 클립보드에 복사
   - GitHub/GitLab 계정 설정에 키 추가

3. **BaiDocs 구성**:
   - SSH 형식을 사용하도록 저장소 URL 업데이트
   - BaiDocs 인터페이스를 통해 연결 테스트

### 보안 모범 사례

#### 자격 증명 관리

- **토큰 순환**: 정기적으로 개인 액세스 토큰 순환
- **범위 제한**: 토큰에 대해 최소 필요 권한 사용
- **환경 격리**: 개발 및 프로덕션용 자격 증명 분리

#### 접근 제어

- **저장소 권한**: 적절한 읽기/쓰기 접근 권한 구성
- **브랜치 보호**: 메인 브랜치에 대한 보호 규칙 구현
- **검토 요구사항**: 프로덕션 변경에 대한 코드 검토 의무화

## 고급 Git 기능

### 모노레포 전용 기능

#### 스파스 체크아웃 구성

BaiDocs는 모노레포에 대해 자동으로 스파스 체크아웃을 관리합니다:

```bash
# BaiDocs에 의해 자동 구성됨
git config core.sparseCheckout true
echo "docs/*" > .git/info/sparse-checkout
git read-tree -m -u HEAD
```

#### 브랜치 필터링

관련 브랜치만 표시하기 위한 구성:

```yaml
selectedBranches:
  - main
  - develop
  - docs/*         # 문서 브랜치를 위한 패턴 매칭
```

### 충돌 해결

#### 자동화된 충돌 감지

BaiDocs는 내장 충돌 감지를 제공합니다:

1. **사전 커밋 검증**: 커밋 전 잠재적 충돌 확인
2. **풀 충돌 감지**: 풀 작업 중 충돌 식별
3. **시각적 충돌 해결**: 나란히 비교 인터페이스

#### 수동 해결 과정

1. BaiDocs 인터페이스를 통해 **충돌 파일 식별**
2. diff 뷰를 사용하여 **충돌 변경 사항 검토**
3. **해결 전략 선택**:
   - 들어오는 변경 사항 수락
   - 현재 변경 사항 유지
   - 양쪽 변경 사항의 수동 병합
4. 커밋 전 **해결된 콘텐츠 테스트**

### 훅 및 자동화

#### Git 훅 통합

BaiDocs는 워크플로 자동화를 위한 사용자 정의 Git 훅을 지원합니다:

```bash
# Pre-commit 훅 예시
#!/bin/sh
# 마크다운 형식 검증
npm run lint:markdown

# 깨진 링크 확인
npm run check:links

# 마지막 수정 타임스탬프 업데이트
npm run update:timestamps
```

#### CI/CD 통합

자동화된 워크플로를 위한 지속적 통합 구성:

```yaml
# GitHub Actions 예시
name: Documentation Build
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Build documentation
        run: npm run build
      - name: Deploy to production
        run: npm run deploy
```

## 모범 사례

### 커밋 메시지 규칙

#### 표준화된 형식

```
type(scope): description

feat(user-guide): add installation instructions
fix(api-ref): correct parameter documentation
docs(readme): update getting started section
```

#### BaiDocs 자동화

BaiDocs는 다음과 함께 커밋 메시지를 자동 생성합니다:
- 파일 변경 요약
- 공동 저자 표시
- 표준화된 형식

### 브랜치 관리

#### 명명 규칙

```
feature/section-name-updates
hotfix/broken-link-repair
docs/api-v2-documentation
release/version-2.0
```

#### 라이프사이클 관리

1. **정기적 정리**: 병합 및 버려진 브랜치 제거
2. **보호 규칙**: 메인 브랜치 보호 구성
3. **검토 워크플로**: 의무적 검토 프로세스 구현

### 문서 버전 관리

#### 릴리스 태깅

```bash
# 문서 릴리스를 위한 버전 태그 생성
git tag -a v1.0.0 -m "Documentation version 1.0.0"
git push origin v1.0.0
```

#### 브랜치 기반 버전 관리

- **메인 브랜치**: 현재 문서 버전
- **릴리스 브랜치**: 안정적인 문서 스냅샷
- **기능 브랜치**: 개발 및 미리보기 콘텐츠

버전 관리 통합을 통해 콘텐츠 품질과 배포 안정성을 유지하면서 협업 문서 개발이 가능합니다.