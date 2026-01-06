---
title: 검색 설정
description: BaiDocs에서 검색 기능 설정 및 최적화하기
---

# 검색 설정

BaiDocs는 Fuse.js로 구동되는 강력한 클라이언트 사이드 검색 시스템을 포함합니다.

## 검색 작동 방식

### 빌드 타임 인덱싱

빌드 중 BaiDocs는:

1. 모든 마크다운 파일 크롤링
2. 텍스트 콘텐츠 추출
3. 검색 인덱스 생성
4. 정적 JSON 파일로 저장

### 클라이언트 사이드 검색

사용자가 검색할 때:

1. 검색 인덱스가 브라우저에 로드
2. Fuse.js가 퍼지 매칭 수행
3. 결과가 관련성으로 순위 매김
4. 즉시 표시, 서버 불필요

## 검색 활성화

기본적으로 활성화됨. 검색 인덱스 재빌드:

```bash
# 검색 인덱스만 빌드
pnpm build:search

# 전체 빌드 (검색 포함)
pnpm build
```

## 검색 기능

### 퍼지 매칭

- 오타 허용
- 부분 매칭
- 유연한 단어 순서

### 검색 모달

접근 방법:
- 키보드: <kbd>Cmd</kbd>/<kbd>Ctrl</kbd> + <kbd>K</kbd>
- 클릭: 내비게이션의 검색 아이콘

## 설정

`baidocs.config.yaml`에서 검색 설정:

```yaml
search:
  enabled: true
  threshold: 0.3          # 매칭 임계값 (0-1)
  maxResults: 50          # 최대 결과 수
  keys:
    - title
    - content
    - headings
```

## 다음 단계

[다국어 지원](multi-language.md)으로 여러 언어로 문서를 만드세요!
