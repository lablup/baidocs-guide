# 이미지와 미디어

시각적 요소는 문서 이해도와 사용자 참여를 향상시킵니다. 이 섹션에서는 BaiDocs에서 이미지, 비디오 및 상호 작용 콘텐츠를 포괄적으로 통합, 최적화하고 모범 사례를 다룹니다.

## 이미지 통합

### 기본 이미지 문법

```markdown
![대체 텍스트](./images/example-image.png)
![편집기 인터페이스 스크린샷](./images/editor-screenshot.png "편집기 인터페이스")
![아키텍처 다이어그램](https://example.com/architecture.svg "시스템 아키텍처")
```

### 이미지 속성 및 크기 조정

```markdown
<!-- 고급 제어를 위한 HTML 문법 -->
<img src="./images/responsive-design.png"
     alt="반응형 디자인 데모"
     width="600"
     height="400"
     title="반응형 레이아웃 예시" />

<!-- 최대 너비가 있는 반응형 이미지 -->
<img src="./images/wide-diagram.png"
     alt="시스템 아키텍처 개요"
     style="max-width: 100%; height: auto;" />
```

### 이미지 위치 및 정렬

```markdown
<!-- 중앙 정렬 이미지 -->
<div style="text-align: center;">
  <img src="./images/logo.png" alt="BaiDocs 로고" width="200" />
</div>

<!-- 텍스트 랩핑을 위한 플로팅 이미지 -->
<img src="./images/feature-icon.png"
     alt="기능 하이라이트"
     style="float: right; margin: 0 0 1rem 1rem; width: 150px;" />

이 텍스트는 플로팅 이미지 주위로 감싸져서 설명 텍스트와 함께 통합된
시각적 요소의 혜택을 받는 콘텐츠에 잡지 스타일의 레이아웃을 제공합니다.
```

## 고급 이미지 기능

### 반응형 이미지

```markdown
<!-- 다중 해상도 지원 -->
<picture>
  <source media="(min-width: 800px)" srcset="./images/large-screenshot.png">
  <source media="(min-width: 400px)" srcset="./images/medium-screenshot.png">
  <img src="./images/small-screenshot.png" alt="애플리케이션 인터페이스" />
</picture>
```

### 이미지 갤러리 및 비교

```markdown
<!-- 전/후 비교 -->
<div style="display: flex; gap: 1rem; flex-wrap: wrap;">
  <div style="flex: 1; min-width: 300px;">
    <h4>최적화 전</h4>
    <img src="./images/before-optimization.png"
         alt="최적화 전 인터페이스"
         style="width: 100%; border: 1px solid #ccc;" />
  </div>
  <div style="flex: 1; min-width: 300px;">
    <h4>최적화 후</h4>
    <img src="./images/after-optimization.png"
         alt="최적화 후 인터페이스"
         style="width: 100%; border: 1px solid #ccc;" />
  </div>
</div>
```

### 상호 작용 이미지 기능

```markdown
<!-- 확대 기능이 있는 클릭 가능한 이미지 -->
<a href="./images/detailed-architecture.png" target="_blank" title="전체 크기로 보려면 클릭">
  <img src="./images/detailed-architecture-thumb.png"
       alt="시스템 아키텍처 다이어그램 (확대하려면 클릭)"
       style="cursor: pointer; border: 2px solid #e0e0e0;" />
</a>

<!-- 캡션이 있는 이미지 -->
<figure>
  <img src="./images/workflow-diagram.svg"
       alt="문서화 워크플로 프로세스"
       style="width: 100%; max-width: 600px;" />
  <figcaption>
    <strong>그림 1:</strong> 콘텐츠 생성부터 게시 및 유지보수까지의
    완전한 문서화 워크플로
  </figcaption>
</figure>
```

## 비디오 통합

### 로컬 비디오 파일

```markdown
<video controls width="100%" style="max-width: 800px;">
  <source src="./videos/feature-demonstration.mp4" type="video/mp4">
  <source src="./videos/feature-demonstration.webm" type="video/webm">
  <p>
    브라우저가 비디오 재생을 지원하지 않습니다.
    <a href="./videos/feature-demonstration.mp4">비디오 다운로드</a>하여
    데모를 시청하십시오.
  </p>
</video>
```

### 외부 비디오 임베딩

```markdown
<!-- YouTube 비디오 임베딩 -->
<iframe width="560" height="315"
        src="https://www.youtube.com/embed/VIDEO_ID"
        title="BaiDocs 튜토리얼: 시작하기"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        allowfullscreen>
</iframe>

<!-- 반응형 비디오 래퍼 -->
<div style="position: relative; width: 100%; height: 0; padding-bottom: 56.25%;">
  <iframe src="https://www.youtube.com/embed/VIDEO_ID"
          title="고급 BaiDocs 기능"
          style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"
          frameborder="0"
          allowfullscreen>
  </iframe>
</div>
```

### 대체 콘텐츠가 있는 비디오

```markdown
<video controls poster="./images/video-thumbnail.png" width="100%">
  <source src="./videos/tutorial.mp4" type="video/mp4">
  <source src="./videos/tutorial.webm" type="video/webm">

  <!-- 비디오 지원이 없는 브라우저를 위한 대체 콘텐츠 -->
  <div style="text-align: center; padding: 2rem; background-color: #f5f5f5;">
    <h4>비디오: BaiDocs 튜토리얼</h4>
    <p>이 브라우저는 비디오 재생을 지원하지 않습니다.</p>
    <p><a href="./videos/tutorial.mp4">비디오 다운로드 (MP4)</a></p>
    <img src="./images/video-thumbnail.png"
         alt="비디오 썸네일: BaiDocs 튜토리얼 개요"
         style="max-width: 300px;" />
  </div>
</video>
```

## 오디오 콘텐츠

### 오디오 파일 통합

```markdown
<audio controls style="width: 100%;">
  <source src="./audio/pronunciation-guide.mp3" type="audio/mpeg">
  <source src="./audio/pronunciation-guide.ogg" type="audio/ogg">
  <p>
    브라우저가 오디오 재생을 지원하지 않습니다.
    <a href="./audio/pronunciation-guide.mp3">오디오 파일 다운로드</a>
  </p>
</audio>

<!-- 대본이 있는 오디오 -->
<details>
<summary>오디오 대본</summary>

**발표자:** BaiDocs 발음 가이드에 오신 것을 환영합니다. 이 오디오 세그먼트에서는
문서 전체에서 사용되는 기술 용어와 개념의 올바른 발음을 다루겠습니다.

**용어 1:** BaiDocs - "바이-독스"로 발음
**용어 2:** MDX - "엠-디-엑스"로 발음
**용어 3:** Monorepo - "모노-레포"로 발음

</details>
```

## 다이어그램 및 기술 일러스트레이션

### SVG 그래픽 통합

```markdown
<!-- 확장 가능한 다이어그램을 위한 인라인 SVG -->
<svg width="400" height="200" viewBox="0 0 400 200" style="border: 1px solid #ccc;">
  <rect x="10" y="50" width="80" height="60" fill="#e3f2fd" stroke="#1976d2" stroke-width="2"/>
  <text x="50" y="85" text-anchor="middle" font-family="Arial" font-size="12">편집기</text>

  <rect x="160" y="50" width="80" height="60" fill="#f3e5f5" stroke="#7b1fa2" stroke-width="2"/>
  <text x="200" y="85" text-anchor="middle" font-family="Arial" font-size="12">처리기</text>

  <rect x="310" y="50" width="80" height="60" fill="#e8f5e8" stroke="#388e3c" stroke-width="2"/>
  <text x="350" y="85" text-anchor="middle" font-family="Arial" font-size="12">뷰어</text>

  <!-- 컴포넌트를 연결하는 화살표 -->
  <path d="M 90 80 L 160 80" stroke="#666" stroke-width="2" marker-end="url(#arrowhead)"/>
  <path d="M 240 80 L 310 80" stroke="#666" stroke-width="2" marker-end="url(#arrowhead)"/>

  <!-- 화살표 마커 정의 -->
  <defs>
    <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="10" refY="3.5" orient="auto">
      <polygon points="0 0, 10 3.5, 0 7" fill="#666"/>
    </marker>
  </defs>
</svg>

*그림: BaiDocs 콘텐츠 처리 워크플로*
```

### 외부 다이어그램 도구

```markdown
<!-- Mermaid 다이어그램 통합 -->
```mermaid
graph TD
    A[콘텐츠 생성] --> B{검토 프로세스}
    B -->|승인됨| C[게시]
    B -->|변경 필요| D[수정]
    D --> A
    C --> E[배포]
    E --> F[사용자 접근]
```

<!-- 시퀀스 다이어그램 -->
```mermaid
sequenceDiagram
    participant U as 사용자
    participant E as 편집기
    participant G as Git 저장소
    participant V as 뷰어

    U->>E: 콘텐츠 생성
    E->>G: 변경 사항 커밋
    E->>V: 빌드 트리거
    V->>U: 업데이트된 콘텐츠 표시
```
````

## 자산 구성 및 관리

### 미디어 디렉토리 구조

```
images/
├── screenshots/
│   ├── editor-interface.png
│   ├── viewer-navigation.png
│   └── configuration-panel.png
├── diagrams/
│   ├── architecture-overview.svg
│   ├── workflow-process.svg
│   └── data-flow.svg
├── icons/
│   ├── feature-icons/
│   └── ui-elements/
└── logos/
    ├── company-logo.png
    ├── partner-logos/
    └── brand-assets/

videos/
├── tutorials/
│   ├── getting-started.mp4
│   ├── advanced-features.mp4
│   └── troubleshooting.mp4
└── demonstrations/
    ├── feature-demos/
    └── case-studies/

audio/
├── pronunciation-guides/
├── interviews/
└── podcasts/
```

### 이미지 최적화 가이드라인

#### 파일 형식 및 사용법

| 형식 | 최적 사용 사례 | 장점 | 단점 |
|------|---------------|------|------|
| **PNG** | 스크린샷, UI 요소, 투명 이미지 | 무손실, 투명도 지원 | 큰 파일 크기 |
| **JPEG** | 사진, 복잡한 이미지 | 작은 파일 크기, 좋은 압축 | 투명도 없음, 손실 압축 |
| **SVG** | 다이어그램, 로고, 아이콘 | 확장 가능, 작은 파일 크기 | 복잡한 그래픽에 제한적 브라우저 지원 |
| **WebP** | 최신 웹 이미지 | 우수한 압축, 품질 | 구형 브라우저에서 제한적 지원 |

#### 최적화 기법

```markdown
<!-- 다중 형식으로 최적화된 이미지 -->
<picture>
  <source srcset="./images/feature-overview.webp" type="image/webp">
  <source srcset="./images/feature-overview.png" type="image/png">
  <img src="./images/feature-overview.png"
       alt="BaiDocs 기능 개요"
       loading="lazy"
       width="600"
       height="400" />
</picture>
```

## 접근성 및 성능

### 대체 텍스트 모범 사례

```markdown
<!-- 설명적 대체 텍스트 예시 -->

✅ 좋은 대체 텍스트:
![네비게이션 패널이 열린 BaiDocs 편집기 인터페이스를 보여주는 스크린샷](./images/editor-nav.png)

❌ 나쁜 대체 텍스트:
![이미지](./images/editor-nav.png)
![편집기 스크린샷](./images/editor-nav.png)

<!-- 장식 이미지 -->
![](./images/decorative-border.png) <!-- 장식 이미지를 위한 빈 대체 텍스트 -->

<!-- 상세한 설명이 있는 복잡한 다이어그램 -->
![세 가지 주요 컴포넌트를 보여주는 시스템 아키텍처 다이어그램: 편집기 (사용자 인터페이스), 처리기 (콘텐츠 변환), 뷰어 (표현 계층), 컴포넌트 간 데이터 흐름을 나타내는 화살표](./images/architecture.svg)
```

### 성능 최적화

#### 지연 로딩

```markdown
<img src="./images/large-diagram.png"
     alt="상세한 시스템 아키텍처"
     loading="lazy"
     width="800"
     height="600" />
```

#### 점진적 이미지 향상

```markdown
<!-- 점진적 향상이 있는 저품질 플레이스홀더 -->
<img src="./images/placeholder-blur.jpg"
     data-src="./images/high-quality.jpg"
     alt="고해상도 문서 예시"
     class="progressive-image"
     style="filter: blur(5px); transition: filter 0.3s;" />

<script>
// 점진적 이미지 로딩 구현
document.addEventListener('DOMContentLoaded', function() {
    const images = document.querySelectorAll('.progressive-image');

    if ('IntersectionObserver' in window) {
        const imageObserver = new IntersectionObserver((entries, observer) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    const img = entry.target;
                    img.src = img.dataset.src;
                    img.style.filter = 'none';
                    observer.unobserve(img);
                }
            });
        });

        images.forEach(img => imageObserver.observe(img));
    }
});
</script>
```

## 상호 작용 미디어 요소

### 이미지 주석

```markdown
<div style="position: relative; display: inline-block;">
  <img src="./images/interface-annotated.png"
       alt="주요 기능을 보여주는 주석이 달린 인터페이스"
       style="width: 100%; max-width: 600px;" />

  <!-- 주석 포인트 -->
  <div style="position: absolute; top: 20%; left: 10%; width: 20px; height: 20px;
              background-color: #ff4444; border-radius: 50%; border: 2px solid white;
              cursor: pointer;" title="네비게이션 패널"></div>

  <div style="position: absolute; top: 30%; left: 60%; width: 20px; height: 20px;
              background-color: #44ff44; border-radius: 50%; border: 2px solid white;
              cursor: pointer;" title="콘텐츠 편집기"></div>
</div>
```

### 이미지 캐러셀

```markdown
<div class="image-carousel" style="overflow-x: auto; white-space: nowrap; padding: 1rem 0;">
  <img src="./images/step1.png" alt="1단계: 초기 설정"
       style="display: inline-block; margin-right: 1rem; width: 300px;" />
  <img src="./images/step2.png" alt="2단계: 구성"
       style="display: inline-block; margin-right: 1rem; width: 300px;" />
  <img src="./images/step3.png" alt="3단계: 콘텐츠 생성"
       style="display: inline-block; margin-right: 1rem; width: 300px;" />
  <img src="./images/step4.png" alt="4단계: 게시"
       style="display: inline-block; width: 300px;" />
</div>
```

## 모범 사례 및 가이드라인

### 콘텐츠 품질 표준

#### 이미지 선택 기준

- **관련성**: 이미지는 콘텐츠 이해를 직접적으로 지원해야 함
- **품질**: 고해상도 및 전문적인 모습
- **일관성**: 문서 전체에서 균일한 스타일과 형식
- **접근성**: 의미 있는 대체 텍스트와 적절한 대비 비율

#### 기술 표준

- **파일 크기 최적화**: 품질과 로딩 성능의 균형
- **형식 선택**: 콘텐츠 유형에 적합한 형식 선택
- **반응형 디자인**: 모든 디바이스 크기에서 이미지 작동 보장
- **브라우저 호환성**: 다양한 브라우저에서 미디어 요소 테스트

### 법적 및 윤리적 고려사항

#### 저작권 및 라이선싱

```markdown
<!-- 적절한 이미지 출처 표시 -->
<figure>
  <img src="./images/open-source-diagram.png"
       alt="오픈소스 프로젝트 협업 워크플로" />
  <figcaption>
    <strong>그림 2:</strong> 오픈소스 협업 워크플로.
    <a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a> 라이선스로
    배포된 이미지. 원작자 <a href="https://example.com">저자명</a>.
  </figcaption>
</figure>
```

#### 개인정보 보호 및 권한

- **화면 캡처**: 민감한 정보가 보이지 않도록 보장
- **사용자 동의**: 사용자 생성 콘텐츠에 대한 권한 취득
- **데이터 보호**: GDPR 및 기타 개인정보 보호 규정 준수
- **출처 요구사항**: 라이선스된 콘텐츠에 대한 적절한 크레딧 제공

효과적인 미디어 통합은 모든 사용자 디바이스와 보조 기술에서 전문적인 표준과 접근성 준수를 유지하면서 문서 사용성을 향상시킵니다.