---
title: 이미지와 미디어
description: BaiDocs에서 이미지와 미디어를 추가하고 관리하는 방법 배우기
---

# 이미지와 미디어

이미지와 시각적 콘텐츠는 문서를 더욱 매력적이고 이해하기 쉽게 만듭니다.

## 이미지 기본

### 이미지 디렉토리

모든 이미지는 북의 `images/` 디렉토리에 배치:

```
content/my-book/ko/
├── index.md
├── guide.md
└── images/
    ├── screenshot.png
    └── diagram.svg
```

### 기본 이미지 문법

표준 마크다운 이미지 문법 사용:

```markdown
![대체 텍스트](images/filename.png)
```

### 제목이 있는 이미지

```markdown
![대체 텍스트](images/filename.png "호버 텍스트")
```

## 지원되는 포맷

| 포맷 | 확장자 | 최적 사용처 |
|------|--------|------------|
| **PNG** | `.png` | 스크린샷, 투명도가 있는 그래픽 |
| **JPG** | `.jpg` | 사진, 복잡한 이미지 |
| **SVG** | `.svg` | 로고, 다이어그램 (확장 가능) |
| **GIF** | `.gif` | 간단한 애니메이션 |

:::tip
로고와 다이어그램에는 SVG를 사용하세요 - 모든 크기에서 완벽하게 확장되고 파일 크기가 작습니다.
:::

## HTML 이미지 문법

더 많은 제어를 위해 HTML `<img>` 태그 사용:

```markdown
<img src="images/logo.png" alt="로고" width="200" />
```

## 이미지 모범 사례

### 1. 설명적인 대체 텍스트 사용

```markdown
<!-- 좋음 -->
![파일 트리와 마크다운 에디터를 보여주는 BaiDocs 에디터 인터페이스](images/editor-ui.png)

<!-- 나쁨 -->
![image1](images/img1.png)
```

### 2. 이미지 크기 최적화

- **스크린샷**: 최대 1200-1600px 너비
- **다이어그램**: SVG 또는 800-1200px 너비

## 다음 단계

[테마 커스터마이징](theme-customization.md)으로 문서의 외관을 스타일링하세요!
