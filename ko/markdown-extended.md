---
title: 확장 마크다운 기능
description: 기본을 넘어선 BaiDocs의 고급 마크다운 기능
---

# 확장 마크다운 기능

BaiDocs는 GitHub Flavored Markdown (GFM)과 문서를 더 강력하고 표현력있게 만드는 추가 확장 기능을 지원합니다.

## 테이블

파이프와 하이픈을 사용하여 테이블 생성:

```markdown
| 헤더 1 | 헤더 2 | 헤더 3 |
|--------|--------|--------|
| 셀 1   | 셀 2   | 셀 3   |
| 셀 4   | 셀 5   | 셀 6   |
```

| 헤더 1 | 헤더 2 | 헤더 3 |
|--------|--------|--------|
| 셀 1   | 셀 2   | 셀 3   |
| 셀 4   | 셀 5   | 셀 6   |

### 열 정렬

```markdown
| 왼쪽 | 가운데 | 오른쪽 |
|:-----|:------:|-------:|
| L1   | C1     | R1     |
```

| 왼쪽 | 가운데 | 오른쪽 |
|:-----|:------:|-------:|
| L1   | C1     | R1     |

## 이모지

이모지 단축코드 또는 직접 이모지 사용:

```markdown
😀 ❤️ 🚀 ✅
```

😀 ❤️ 🚀 ✅

## 키보드 키

`<kbd>` 태그를 사용하여 키보드 단축키 표시:

```markdown
<kbd>Ctrl</kbd> + <kbd>C</kbd>를 눌러 복사
```

<kbd>Ctrl</kbd> + <kbd>C</kbd>를 눌러 복사

## 위 첨자와 아래 첨자

```markdown
E = mc<sup>2</sup>
H<sub>2</sub>O
```

E = mc<sup>2</sup>

H<sub>2</sub>O

## 강조 표시

```markdown
<mark>강조된 텍스트</mark>
```

<mark>강조된 텍스트</mark>

## 접을 수 있는 섹션

```markdown
<details>
<summary>확장하려면 클릭</summary>

여기에 숨겨진 콘텐츠가 있습니다.

</details>
```

<details>
<summary>확장하려면 클릭</summary>

여기에 숨겨진 콘텐츠가 있습니다.

</details>

## 다음 단계

BaiDocs의 가장 강력한 기능 중 하나인 [어드모니션](admonitions.md)을 확인하세요!
