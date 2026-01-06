---
title: Git으로 버전 관리하기
description: BaiDocs에서 Git을 사용한 문서 버전 관리
---

# Git으로 버전 관리하기

BaiDocs는 Git 버전 관리와 원활하게 통합되도록 설계되었습니다. 각 book을 독립적인 Git 저장소로 관리할 수 있어 팀 협업, 버전 히스토리, 분산 워크플로우가 가능합니다.

## Book별 Git 저장소

BaiDocs의 각 book은 독자적인 Git 저장소를 가질 수 있습니다. 이를 통해:

- **독립적인 버전 관리**를 문서 프로젝트별로 수행
- **별도의 접근 제어**로 팀별 권한 관리
- **개별 릴리스 사이클**을 book마다 운영
- Git 호스팅 플랫폼을 통한 **손쉬운 배포**

### 디렉토리 구조

```
baidocs/
├── content/
│   ├── user-guide/          # Book 1 (Git 저장소 가능)
│   │   ├── .git/
│   │   ├── book.config.yaml
│   │   └── en/
│   ├── api-reference/       # Book 2 (별도 Git 저장소)
│   │   ├── .git/
│   │   ├── book.config.yaml
│   │   └── en/
│   └── developer-guide/     # Book 3 (또 다른 Git 저장소)
│       ├── .git/
│       ├── book.config.yaml
│       └── en/
```

:::note
BaiDocs 메인 프로젝트와 각 book이 별도의 Git 저장소를 가질 수 있어 모듈화된 문서 관리가 가능합니다.
:::

## Book에 Git 설정하기

### 새 저장소 초기화

에디터나 수동으로 book을 생성한 후 Git 저장소로 초기화할 수 있습니다:

```bash
cd content/my-book
git init
git add .
git commit -m "Initial commit: Create my-book documentation"
```

### 원격 저장소 연결

book을 원격 저장소(GitHub, GitLab 등)에 연결:

```bash
# 원격 저장소 추가
git remote add origin https://github.com/your-org/my-book.git

# 원격에 푸시
git push -u origin main
```

### 기존 Book 저장소 클론

기존 문서 저장소를 BaiDocs 프로젝트에 추가:

```bash
cd content
git clone https://github.com/your-org/existing-docs.git
```

클론된 저장소는 자동으로 BaiDocs 뷰어에 나타납니다.

## 문서용 Git 워크플로우

### 기본 워크플로우

1. **변경사항 작성**
   ```bash
   # content/my-book/en/ 파일 편집
   vim content/my-book/en/new-feature.md
   ```

2. **상태 확인**
   ```bash
   cd content/my-book
   git status
   ```

3. **변경사항 스테이징**
   ```bash
   git add en/new-feature.md
   # 또는 모든 변경사항 스테이징
   git add .
   ```

4. **커밋**
   ```bash
   git commit -m "docs: 새 기능 문서 추가"
   ```

5. **원격에 푸시**
   ```bash
   git push origin main
   ```

### 브랜치 전략

문서 업데이트를 위한 브랜치 사용:

```bash
# 기능 브랜치 생성
git checkout -b docs/update-api-reference

# 변경사항 작성
vim en/api-reference.md

# 변경사항 커밋
git add en/api-reference.md
git commit -m "docs: v2.0용 API 레퍼런스 업데이트"

# 브랜치 푸시
git push origin docs/update-api-reference
```

main에 병합하기 전에 풀 리퀘스트로 리뷰를 받습니다.

## 협업 문서 작성

### 팀 워크플로우

1. **저장소 클론**
   ```bash
   git clone https://github.com/your-org/documentation.git content/docs
   ```

2. **작업용 브랜치 생성**
   ```bash
   cd content/docs
   git checkout -b docs/my-contribution
   ```

3. **변경사항 작성 및 커밋**
   ```bash
   # 문서 편집
   vim en/getting-started.md

   # 커밋
   git add en/getting-started.md
   git commit -m "docs: 시작하기 가이드 개선"
   ```

4. **푸시 및 풀 리퀘스트 생성**
   ```bash
   git push origin docs/my-contribution
   ```

5. **리뷰 및 병합** GitHub/GitLab 풀 리퀘스트를 통해

### 커밋 메시지 규칙

일관된 커밋 메시지 형식 사용:

```bash
# 좋은 커밋 메시지
git commit -m "docs: 배포 가이드 추가"
git commit -m "docs: 설치 섹션 오타 수정"
git commit -m "docs: v2.0용 API 예제 업데이트"
git commit -m "feat: 테스팅 관련 새 챕터 추가"
git commit -m "fix: 내비게이션의 깨진 링크 수정"

# 본문에 더 많은 컨텍스트 포함
git commit -m "docs: 배포 가이드 추가

- Vercel 배포 지침 추가
- Docker 배포 지침 추가
- 환경 변수 설정 추가
"
```

**권장 접두사:**
- `docs:` - 문서 내용 변경
- `feat:` - 새로운 문서 기능이나 챕터
- `fix:` - 오류, 깨진 링크, 오타 수정
- `refactor:` - 내용 변경 없는 문서 구조 조정
- `style:` - 포맷팅, 공백 등
- `chore:` - 설정, 에셋 등 업데이트

## 원격 저장소 작업

### 최신 변경사항 가져오기

로컬 문서를 동기화:

```bash
cd content/my-book
git pull origin main
```

### 병합 충돌 처리

여러 사람이 같은 파일을 편집할 때:

```bash
# 최신 변경사항 가져오기
git pull origin main

# 충돌이 발생하면 충돌된 파일 편집
vim en/conflicted-file.md

# 충돌 마커 찾기:
# <<<<<<< HEAD
# 내 변경사항
# =======
# 상대방 변경사항
# >>>>>>> branch-name

# 충돌 해결 후:
git add en/conflicted-file.md
git commit -m "docs: 병합 충돌 해결"
git push origin main
```

## 원격 Books 동기화

BaiDocs는 원격 Git 저장소에서 book을 동기화할 수 있어 여러 소스에서 문서를 가져올 수 있습니다. 다중 저장소 프로젝트나 BaiDocs 메인 프로젝트와 별도로 문서를 관리할 때 유용합니다.

### 설정

`.env.local`에서 `REMOTE_BOOKS_JSON` 환경 변수로 원격 book 설정:

```bash
# .env.local
REMOTE_BOOKS_JSON='{"books":[{"id":"my-docs","repository":"https://github.com/org/docs","branch":"main","path":"."}]}'
```

설정 구조:

```json
{
  "books": [
    {
      "id": "backend-api-docs",           // content/의 book 디렉토리 이름
      "repository": "https://github.com/your-org/backend-api-docs",
      "branch": "main",                    // 클론할 브랜치 (선택, 기본값: main)
      "path": "."                          // 저장소의 하위 디렉토리 (선택, 기본값: .)
    },
    {
      "id": "frontend-guide",
      "repository": "https://github.com/your-org/frontend-guide",
      "branch": "production",
      "path": "docs"                       // docs/ 하위 디렉토리만 동기화
    }
  ]
}
```

### 수동 동기화

로컬 개발 시 `sync:remote` 명령으로 원격 book을 수동으로 동기화:

```bash
# 프로젝트 루트에서
pnpm sync:remote

# 또는 apps/viewer에서
cd apps/viewer
pnpm sync:remote
```

**수행 작업:**
1. 전체 Git 히스토리를 포함한 원격 저장소 클론
2. 모든 원격 브랜치에 대한 로컬 브랜치 생성
3. 지정된 브랜치로 체크아웃
4. 버전 관리를 위한 `.git` 디렉토리 유지

**사용 시기:**
- 새 개발 환경 설정 시
- 원격 book에서 최신 변경사항 가져오기
- 다른 book 버전 간 전환
- `REMOTE_BOOKS_JSON` 설정 업데이트 후

:::warning 중요
`pnpm sync:remote`는 `content/` 디렉토리의 **기존 원격 book을 삭제**합니다 (로컬 전용 book 제외). 이 명령을 실행하기 전에 항상 로컬 변경사항을 커밋하세요.
:::

### 로컬 전용 Books

동기화 중 삭제로부터 특정 book을 보호하려면 `baidocs.config.yaml`에서 로컬 전용으로 설정:

```yaml
# baidocs.config.yaml
deployment:
  localOnlyBooks:
    - my-local-book
    - work-in-progress-docs
```

로컬 전용 book은 `pnpm sync:remote` 실행 시 보존됩니다.

### 프로덕션 배포

프로덕션 빌드(Vercel, Netlify)에서는 빌드 프로세스 중 원격 book이 자동으로 동기화됩니다:

```bash
# Vercel 빌드 명령 (vercel.json이나 프로젝트 설정에서 구성)
pnpm build:vercel
```

실행 내용:
1. `sync:remote` - 원격 book 클론
2. `build` - 모든 앱 빌드 (viewer, editor)

배포 플랫폼 설정에서 `REMOTE_BOOKS_JSON` 환경 변수를 설정하세요.

### Git 브랜치 관리

원격 book은 모든 브랜치를 포함한 전체 Git 저장소를 유지하여 버전별 문서를 지원합니다:

```bash
cd content/my-book

# 사용 가능한 모든 브랜치 보기
git branch -a

# 다른 버전으로 전환
git checkout v2.0

# 기본 브랜치로 돌아가기
git checkout main
```

BaiDocs 에디터는 명령줄 없이 브랜치를 전환할 수 있는 UI를 제공합니다.

### 사용 사례

원격 book 동기화는 다음에 적합합니다:
- **다중 저장소 문서** - 여러 프로젝트에서 문서 가져오기
- **프로덕션 배포** - Vercel/Netlify에서 자동 동기화
- **CI/CD 파이프라인** - 문서를 최신 상태로 유지
- **팀 협업** - 각 팀이 자체 문서 저장소 관리
- **버전 관리** - 문서 버전 간 전환

### 문제 해결

**문제: 원격 book이 동기화되지 않음**

설정 확인:
```bash
# REMOTE_BOOKS_JSON 설정 확인
echo $REMOTE_BOOKS_JSON

# 또는 .env.local 확인
cat .env.local | grep REMOTE_BOOKS_JSON
```

**문제: 프라이빗 저장소 클론 시 권한 거부**

`.env.local`에 GitHub 토큰 설정:
```bash
GITHUB_TOKEN=ghp_xxxxxxxxxxxxxxxxxxxx
```

토큰 생성: https://github.com/settings/tokens

**문제: 로컬 변경사항이 손실됨**

원격 book 동기화는 기존 콘텐츠를 삭제합니다. 이를 방지하려면:
1. `baidocs.config.yaml`의 `localOnlyBooks`에 book 추가
2. 동기화 전에 Git에 변경사항 커밋
3. `REMOTE_BOOKS_JSON` 외부에 로컬 전용 book 유지

## 버전 태그와 릴리스

### 버전 태그 생성

Git 태그로 문서 버전 표시:

```bash
# 버전 태그 지정
git tag -a v1.0.0 -m "Version 1.0.0 documentation"

# 원격에 태그 푸시
git push origin v1.0.0

# 모든 태그 푸시
git push origin --tags
```

### 태그 보기

```bash
# 모든 태그 목록
git tag

# 태그 상세 정보
git show v1.0.0
```

### 특정 버전 체크아웃

```bash
# 특정 버전 체크아웃
git checkout v1.0.0

# 최신으로 돌아가기
git checkout main
```

## 베스트 프랙티스

### 1. .gitignore 사용

book 디렉토리에 `.gitignore` 파일 생성:

```gitignore
# 에디터 파일
.DS_Store
.vscode/
.idea/
*.swp
*.swo

# 빌드 산출물
.cache/
dist/

# 임시 파일
*.tmp
*~

# images 디렉토리는 제외하지 않음
# images/
```

### 2. 자주 커밋

작고 집중된 커밋 만들기:

```bash
# 좋음: 작고 집중된 커밋
git commit -m "docs: 인증 섹션 추가"
git commit -m "docs: 인증 코드 예제 추가"
git commit -m "docs: 인증 문제 해결 추가"

# 나쁨: 크고 산만한 커밋
git commit -m "모든 것 업데이트"
```

### 3. 설명적인 커밋 메시지 작성

커밋 메시지에 컨텍스트 포함:

```bash
# 좋음: 설명적이고 구체적
git commit -m "docs: Windows 11용 설치 단계 업데이트

- PowerShell 요구사항 추가
- 경로 설정 업데이트
- 일반적인 오류 해결 추가
"

# 나쁨: 모호함
git commit -m "문서 업데이트"
```

### 4. 커밋 전 리뷰

항상 변경사항 리뷰:

```bash
# 변경 사항 확인
git diff

# 스테이징된 변경사항 리뷰
git diff --staged

# 변경된 파일 확인
git status
```

### 5. 문서와 코드 분리

book이 코드베이스를 문서화하는 경우:
- 문서를 별도 저장소에 보관
- 문서에서 특정 코드 버전 참조
- 새 버전 릴리스 시 문서 업데이트

### 6. 보호된 브랜치 사용

중요한 문서의 경우:
- `main` 브랜치 보호
- 풀 리퀘스트 리뷰 필수화
- 병합 전 CI 체크 실행
- 깨끗한 히스토리 유지

### 7. 변경사항 문서화

CHANGELOG.md를 추가하여 문서 업데이트 추적:

```markdown
# Changelog

## [1.1.0] - 2024-01-15
### Added
- 배포 전략 관련 새 챕터
- API v2.0 코드 예제

### Changed
- 설치 지침 업데이트
- 내비게이션 구조 개선

### Fixed
- 시작하기 가이드의 깨진 링크 수정
```

## GitHub/GitLab 통합

### 자동 배포

book 저장소를 Vercel이나 Netlify에 연결:

1. GitHub/GitLab에 변경사항 푸시
2. 자동 빌드 트리거
3. 문서 자동 업데이트

### 풀 리퀘스트 미리보기

미리보기 배포 설정:
- 각 풀 리퀘스트마다 미리보기 URL 제공
- 병합 전 변경사항 리뷰
- 조기 문제 발견

### GitHub Actions 예제

문서 체크 자동화:

```yaml
# .github/workflows/docs-check.yml
name: Documentation Check

on: [pull_request]

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Check markdown links
        run: |
          npm install -g markdown-link-check
          find . -name "*.md" -exec markdown-link-check {} \;

      - name: Check for typos
        run: |
          npm install -g cspell
          cspell "**/*.md"
```

## 문제 해결

### 대용량 파일

큰 이미지나 에셋이 있는 경우:

```bash
# 저장소 크기 확인
du -sh .git

# 대용량 파일에 Git LFS 사용
git lfs install
git lfs track "*.png"
git lfs track "*.jpg"
git add .gitattributes
git commit -m "chore: 이미지용 Git LFS 추가"
```

### 실수로 커밋한 비밀 정보

민감한 데이터를 커밋한 경우:

```bash
# 히스토리에서 제거 (신중하게 사용!)
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch path/to/secret-file" \
  --prune-empty --tag-name-filter cat -- --all

# 강제 푸시 (팀과 조율!)
git push origin --force --all
```

더 나은 방법: `.gitignore`를 사용하여 비밀 정보 커밋 방지.

### 저장소가 너무 큰 경우

book 저장소가 너무 커진 경우:

1. **오래된 버전을 아카이브로 이동**
2. **바이너리 파일에 Git LFS 사용**
3. **불필요한 빌드 산출물 제거**
4. **여러 book으로 분할 고려**

## 다음 단계

BaiDocs의 버전 관리를 이해했으니 다음을 학습하세요:
- [다국어 지원](multi-language.md)으로 문서 번역하기
- [배포 옵션](deployment-options.md)으로 버전 관리된 문서 배포하기

---

:::tip
Git은 문서 관리를 확장 가능하게 만듭니다. 기본 커밋부터 시작하고, 팀이 성장하면서 고급 워크플로우를 도입하세요.
:::
