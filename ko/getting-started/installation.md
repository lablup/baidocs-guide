# 설치

이 섹션에서는 개발 환경에서 BaiDocs를 설치하고 구성하는 단계별 지침을 제공합니다.

## 시스템 요구사항

### 하드웨어 요구사항
- **CPU**: 최적의 성능을 위해 2개 이상의 코어 권장
- **메모리**: 최소 4GB RAM, 대규모 문서 프로젝트의 경우 8GB 권장
- **저장소**: 설치 및 종속성을 위해 1GB의 사용 가능한 디스크 공간

### 소프트웨어 종속성
- **Node.js**: 버전 18.17.0 이상
- **npm**: 버전 9.0.0 이상 (Node.js에 포함됨)
- **Git**: 버전 관리 통합을 위한 버전 2.25.0 이상

### 운영 체제 지원
- **macOS**: 11.0 (Big Sur) 이상
- **Windows**: Windows 10 버전 1903 이상
- **Linux**: Ubuntu 20.04 LTS 또는 동등한 배포판

## 설치 방법

### 방법 1: 저장소에서 복제

1. **BaiDocs 저장소 복제**:
   ```bash
   git clone https://github.com/lablup/baidocs.git
   cd baidocs
   ```

2. **종속성 설치**:
   ```bash
   npm install
   ```

3. **작업 공간 초기화**:
   ```bash
   npm run setup
   ```

### 방법 2: 패키지 설치 (향후 릴리스)

*참고: 패키지 기반 설치는 향후 릴리스에서 제공될 예정입니다.*

```bash
# 향후 구현
npm install -g @baidocs/cli
baidocs init my-documentation
```

## 구성 설정

### 환경 구성

1. **환경 파일 생성**:
   ```bash
   cp .env.example .env.local
   ```

2. **환경 변수 구성**:
   ```env
   # 콘텐츠 디렉토리 경로
   CONTENT_PATH=./content

   # 개발 모드 활성화
   NODE_ENV=development

   # 포트 구성
   EDITOR_PORT=3000
   VIEWER_PORT=3001
   ```

### 작업 공간 초기화

1. **콘텐츠 디렉토리 생성**:
   ```bash
   mkdir -p content
   ```

2. **설치 확인**:
   ```bash
   npm run build
   ```

## 개발 서버 설정

### 편집기 애플리케이션 시작

```bash
# 편집기 인터페이스 시작
npm run dev:editor
```

편집기 인터페이스는 `http://localhost:3000`에서 사용할 수 있습니다.

### 뷰어 애플리케이션 시작

```bash
# 뷰어 인터페이스 시작
npm run dev:viewer
```

뷰어 인터페이스는 `http://localhost:3001`에서 사용할 수 있습니다.

### 두 애플리케이션 모두 시작

```bash
# 두 애플리케이션을 동시에 시작
npm run dev
```

## 확인 단계

### 설치 확인

1. **애플리케이션 상태 확인**:
   - 편집기의 경우 `http://localhost:3000`으로 이동
   - 뷰어의 경우 `http://localhost:3001`로 이동
   - 두 애플리케이션이 오류 없이 로드되는지 확인

2. **콘텐츠 생성 테스트**:
   - 편집기 인터페이스를 통해 새 책 생성
   - 기능 확인을 위한 샘플 콘텐츠 추가
   - 콘텐츠가 뷰어에 표시되는지 확인

### 일반적인 문제 해결

#### 포트 충돌
포트 3000 또는 3001이 사용 중인 경우:
```bash
# 사용자 정의 포트 사용
EDITOR_PORT=4000 VIEWER_PORT=4001 npm run dev
```

#### 권한 오류
Unix 계열 시스템에서:
```bash
# 권한 문제 해결
sudo chown -R $(whoami) ~/.npm
```

#### Node.js 버전 문제
Node.js를 필요한 버전으로 업데이트:
```bash
# nvm 사용 (권장)
nvm install 18
nvm use 18
```

## 다음 단계

성공적인 설치 후:

1. **첫 번째 책 생성** (다음 섹션 참조)
2. **콘텐츠 관리를 위한 버전 관리 구성**
3. **요구사항에 따른 테마 및 설정 사용자 정의**
4. **프로덕션 환경을 위한 배포 파이프라인 설정**

설치 프로세스가 완료되었습니다. 다음 섹션으로 진행하여 첫 번째 문서 프로젝트를 생성하십시오.