---
title: 설치
description: BaiDocs 설치 및 설정 단계별 가이드
---

# 설치

이 가이드는 BaiDocs를 설치하고 개발 환경을 준비하는 과정을 안내합니다.

## 사전 요구사항

시작하기 전에 다음이 설치되어 있는지 확인하세요:

### 필수 소프트웨어

| 소프트웨어 | 최소 버전 | 목적 |
|-----------|---------|------|
| **Node.js** | 18.x 이상 | JavaScript 런타임 |
| **pnpm** | 8.x 이상 | 패키지 관리자 |
| **Git** | 2.x 이상 | 버전 관리 |

:::warning
BaiDocs는 Node.js 18 이상이 필요합니다. 이전 버전을 사용하면 호환성 문제가 발생할 수 있습니다.
:::

### Node.js 설치

Node.js가 설치되어 있지 않다면:

**macOS/Linux (nvm 사용):**

```bash
# nvm 설치
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# Node.js 설치
nvm install 18
nvm use 18
```

**Windows:**

[nodejs.org](https://nodejs.org/)에서 다운로드하여 설치

### pnpm 설치

Node.js가 설치되면 pnpm을 전역으로 설치:

```bash
npm install -g pnpm
```

설치 확인:

```bash
pnpm --version
# 8.x.x 이상이 출력되어야 함
```

:::tip
pnpm은 npm이나 yarn보다 빠르고 효율적이며 디스크 공간과 설치 시간을 절약합니다.
:::

## 설치 단계

### 1. 저장소 클론

```bash
git clone https://github.com/lablup/baidocs.git
cd baidocs
```

### 2. 의존성 설치

```bash
pnpm install
```

이 명령은:
- 모든 프로젝트 의존성 설치
- 모노레포 워크스페이스 설정
- 패키지들을 함께 연결

:::info
첫 설치는 몇 분 걸릴 수 있습니다. pnpm의 효율적인 캐싱 덕분에 이후 설치는 훨씬 빠릅니다.
:::

### 3. 설치 확인

모든 것이 올바르게 설치되었는지 확인:

```bash
# 빌드가 작동하는지 확인
pnpm type-check

# 오류 없이 완료되어야 함
```

## 개발 환경

### 개발 서버 시작

BaiDocs는 두 개의 애플리케이션을 실행할 수 있습니다:

#### 옵션 1: 두 애플리케이션 모두 실행

```bash
pnpm dev
```

다음이 시작됩니다:
- **에디터** at http://localhost:3001
- **뷰어** at http://localhost:3000

#### 옵션 2: 개별 실행

**뷰어만 시작:**

```bash
pnpm viewer
```

**에디터만 시작:**

```bash
pnpm editor
```

:::tip
문서 작성을 위해서는 일반적으로 뷰어만 필요합니다. 에디터는 WYSIWYG 편집과 여러 북 관리에 유용합니다.
:::

### 핫 리로드

두 애플리케이션 모두 핫 리로드를 지원합니다:
- 파일 편집
- 변경사항 저장
- 브라우저에서 즉시 업데이트 확인

## 문제 해결

### 일반적인 문제

#### 포트가 이미 사용 중

포트 3000이나 3001이 이미 사용 중인 경우:

```bash
# 포트 3000의 프로세스 종료
lsof -ti:3000 | xargs kill -9

# 포트 3001의 프로세스 종료
lsof -ti:3001 | xargs kill -9

# 또는 사용자 정의 포트 사용
PORT=3002 pnpm viewer
PORT=3003 pnpm editor
```

#### pnpm 설치 실패

`pnpm install`이 실패하는 경우:

```bash
# pnpm 캐시 정리
pnpm store prune

# node_modules와 락 파일 제거
rm -rf node_modules pnpm-lock.yaml

# 재설치
pnpm install
```

#### 타입 오류

TypeScript 오류가 보이는 경우:

```bash
# 타입 체크 실행
pnpm type-check

# 오류가 지속되면 정리하고 재빌드
pnpm clean
pnpm install
```

## 시스템 요구사항

### 최소 요구사항

- **CPU**: 2 코어
- **RAM**: 4 GB
- **디스크**: 2 GB 여유 공간
- **OS**: macOS, Linux, 또는 Windows 10+

### 권장 요구사항

- **CPU**: 4+ 코어
- **RAM**: 8 GB
- **디스크**: 5 GB 여유 공간 (빌드 아티팩트 및 node_modules용)
- **OS**: 최상의 성능을 위한 macOS 또는 Linux

## Docker 지원

BaiDocs는 Docker에서도 실행할 수 있습니다:

```dockerfile
FROM node:18-alpine

# pnpm 설치
RUN npm install -g pnpm

# 작업 디렉토리 설정
WORKDIR /app

# 파일 복사
COPY . .

# 의존성 설치
RUN pnpm install

# 빌드
RUN pnpm build

# 포트 노출
EXPOSE 3000 3001

# 시작
CMD ["pnpm", "start"]
```

빌드 및 실행:

```bash
docker build -t baidocs .
docker run -p 3000:3000 -p 3001:3001 baidocs
```

## 다음 단계

BaiDocs가 설치되고 실행 중이니 이제 [첫 번째 북 만들기](creating-first-book.md)를 시작해봅시다!

---

:::success
설치 완료! 이제 놀라운 문서를 만들 준비가 되었습니다.
:::
