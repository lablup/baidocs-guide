# 확장 마크다운 문법

BaiDocs는 MDX 통합과 사용자 정의 확장을 통해 향상된 마크다운 기능을 지원합니다. 이 섹션에서는 풍부하고 상호 작용적인 문서 작성을 위한 고급 문법 옵션을 다룹니다.

## MDX 통합

### 컴포넌트 임베딩

MDX를 사용하면 React 컴포넌트를 마크다운 콘텐츠 내에 직접 임베딩할 수 있습니다:

```mdx
import { Alert, Button } from '@/components'

# 문서 제목

일반적인 마크다운 콘텐츠에 상호 작용 컴포넌트를 포함할 수 있습니다:

<Alert type="info">
마크다운에 임베딩된 상호 작용 알림 컴포넌트입니다.
</Alert>

<Button onClick={() => alert('클릭됨!')}>
상호 작용 버튼
</Button>
```

### 컴포넌트 속성과 데이터

```mdx
import { ApiReference } from '@/components'

<ApiReference
  endpoint="/api/users"
  method="GET"
  description="사용자 정보 검색"
  parameters={[
    { name: 'id', type: 'string', required: true },
    { name: 'include', type: 'array', required: false }
  ]}
/>
```

## 표와 데이터 표현

### 향상된 표 문법

```markdown
| 기능 | 베이직 | 프로페셔널 | 엔터프라이즈 |
|------|-------|-----------|------------|
| 사용자 | 5명 | 50명 | 무제한 |
| 저장소 | 1GB | 10GB | 무제한 |
| 지원 | 이메일 | 우선순위 | 24/7 전화 |
| API 접근 | ❌ | ✅ | ✅ |
| 사용자 정의 테마 | ❌ | ❌ | ✅ |
```

#### 표 정렬

```markdown
| 왼쪽 정렬 | 가운데 정렬 | 오른쪽 정렬 |
|:---------|:---------:|---------:|
| 텍스트 | 텍스트 | 텍스트 |
| 더 긴 텍스트 | 더 긴 텍스트 | 더 긴 텍스트 |
```

#### 복잡한 표 콘텐츠

```markdown
| 컴포넌트 | 문법 | 예시 |
|-----------|--------|---------|
| **굵은 텍스트** | `**text**` | **중요** |
| *기울임 텍스트* | `*text*` | *강조* |
| `코드` | `` `code` `` | `function()` |
| [링크](url) | `[text](url)` | [GitHub](https://github.com) |
```

## 수학적 표현식

### 인라인 수학

단일 달러 기호를 사용하여 인라인 수학적 표현식을 작성:

```markdown
복리 이자 공식은 $A = P(1 + r/n)^{nt}$입니다. 여기서:
- $P$는 원금
- $r$은 연간 이자율
- $n$은 연간 복리 횟수
- $t$는 연 단위 시간
```

### 블록 수학

블록 레벨 수학적 표현식에는 이중 달러 기호를 사용:

```markdown
$$
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
$$

$$
\begin{pmatrix}
a & b \\
c & d
\end{pmatrix}
\begin{pmatrix}
x \\
y
\end{pmatrix} =
\begin{pmatrix}
ax + by \\
cx + dy
\end{pmatrix}
$$
```

## 정의 목록

### 용어와 정의 문법

```markdown
용어 1
: 용어 1에 대한 정의
: 용어 1에 대한 추가 정의

용어 2
: 여러 줄에 걸친 용어 2에 대한 정의
  다음 줄로 계속됩니다
: 용어 2에 대한 두 번째 정의

API 엔드포인트
: API가 요청을 받을 수 있는 특정 URL
: RESTful API는 일반적으로 표준 HTTP 메서드를 사용

인증
: 사용자 신원을 확인하는 프로세스
: 토큰, 세션 또는 인증서를 사용하여 구현 가능
```

## 각주와 인용

### 각주 문법

```markdown
이 텍스트는 각주 참조[^1]와 다른 각주[^note]를 포함합니다.

추가 각주[^2]가 있는 일반 콘텐츠가 여기에 계속됩니다.

[^1]: 간단한 내용을 가진 첫 번째 각주입니다.

[^note]: 이 각주는 사용자 정의 식별자를 가지며 다음을 포함할 수 있습니다:
    - 여러 문단
    - 코드 블록
    - 기타 마크다운 형식

[^2]: 짧은 각주 내용입니다.
```

### 인용 참조

```markdown
문서[^docs2024]에 따르면, API 릴리스에는 시맨틱 버전 관리[^semver]를 사용하는 것이 권장되는 접근 방식입니다.

[^docs2024]: 김철수 (2024). *현대 API 문서화 관행*.
기술 출판사. https://example.com/docs

[^semver]: 시맨틱 버전 관리 명세. https://semver.org에서 가져옴
```

## 사용자 정의 컨테이너와 경고문

### 기본 경고문 문법

```markdown
::: info
메인 콘텐츠 흐름에 중요하지는 않지만 추가 컨텍스트를 제공하는
정보성 경고문입니다.
:::

::: warning
잠재적 문제를 피하기 위해 사용자가 알아야 할 중요한 정보를
강조하는 경고입니다.
:::

::: danger
무시할 경우 데이터 손실이나 시스템 손상을 일으킬 수 있는
중요한 정보를 나타내는 위험 경고문입니다.
:::
```

### 제목이 있는 경고문

```markdown
::: tip 사용자 정의 팁 제목
경고문에 사용자 정의 제목을 제공하여 더욱 구체적이고
문맥에 맞게 만들 수 있습니다.
:::

::: note 성능 고려사항
대용량 데이터셋을 처리할 때는 응답 시간을 개선하고
메모리 사용량을 줄이기 위해 페이지네이션 구현을 고려하십시오.
:::
```

### 경고문 내 중첩 콘텐츠

```markdown
::: warning 데이터베이스 마이그레이션 필요

버전 2.0으로 업그레이드하기 전에 다음을 수행해야 합니다:

1. **데이터베이스 백업**
   ```bash
   pg_dump myapp > backup.sql
   ```

2. **마이그레이션 스크립트 실행**
   ```bash
   npm run migrate
   ```

3. **데이터 무결성 확인**
   - 중요한 테이블 확인
   - 외래 키 관계 검증
   - 애플리케이션 기능 테스트

자세한 지침은 [마이그레이션 가이드](./migration.md)를 참조하십시오.
:::
```

## 상호 작용 요소

### 확장 가능한 섹션

```markdown
<details>
<summary>고급 구성 옵션</summary>

이 섹션에는 일반적으로 필요하지는 않지만 특정 사용 사례에
유용할 수 있는 세부 구성 옵션이 포함되어 있습니다:

- **데이터베이스 연결 풀링**: 연결 제한 구성
- **캐싱 전략**: Redis 대 메모리 캐싱 옵션
- **로깅 레벨**: 디버그, 정보, 경고, 오류 구성

```yaml
advanced:
  database:
    pool_size: 20
    timeout: 5000
  cache:
    strategy: redis
    ttl: 3600
```

</details>
```

### 탭 콘텐츠

```markdown
::: tabs
== 탭 1
코드 예시와 설명을 포함한 첫 번째 탭의 콘텐츠입니다.

```javascript
const config = {
  mode: 'production',
  optimization: true
};
```

== 탭 2
다른 구현을 가진 두 번째 탭의 콘텐츠:

```python
config = {
    'mode': 'production',
    'optimization': True
}
```

== 탭 3
추가 구현 세부사항과 모범 사례입니다.
:::
```

## 코드 향상

### 메타데이터가 있는 구문 강조

````markdown
```javascript {title="config.js" showLineNumbers=true}
const configuration = {
  apiUrl: process.env.API_URL,
  timeout: 5000,
  retries: 3
};

export default configuration;
```
````

### 강조가 있는 코드

````markdown
```javascript {2,4-6}
function processData(input) {
  const validated = validate(input); // 강조된 줄

  if (validated.isValid) {        // 강조 블록 시작
    return transform(validated.data);
  }                               // 강조 블록 끝

  throw new Error('Invalid input');
}
```
````

### 코드 차이점

````markdown
```diff
- const oldConfig = {
-   timeout: 3000,
-   retries: 1
- };
+ const newConfig = {
+   timeout: 5000,
+   retries: 3,
+   cache: true
+ };
```
````

## 미디어 통합

### 고급 이미지 문법

```markdown
![대체 텍스트](./images/diagram.png "이미지 제목")

<!-- 사용자 정의 속성이 있는 이미지 -->
<img src="./images/screenshot.png"
     alt="애플리케이션 스크린샷"
     width="600"
     height="400" />

<!-- 반응형 이미지 -->
<picture>
  <source media="(min-width: 800px)" srcset="./images/large.png">
  <source media="(min-width: 400px)" srcset="./images/medium.png">
  <img src="./images/small.png" alt="반응형 이미지">
</picture>
```

### 비디오 임베딩

```markdown
<!-- 로컬 비디오 파일 -->
<video controls width="100%">
  <source src="./videos/demo.mp4" type="video/mp4">
  <source src="./videos/demo.webm" type="video/webm">
  브라우저가 비디오 재생을 지원하지 않습니다.
</video>

<!-- 외부 비디오 임베딩 -->
<iframe width="560" height="315"
        src="https://www.youtube.com/embed/VIDEO_ID"
        title="비디오 제목"
        frameborder="0"
        allowfullscreen>
</iframe>
```

## 상호 참조와 네비게이션

### 내부 링크 향상

```markdown
<!-- 표준 내부 링크 -->
[시작하기 가이드](../getting-started/installation.md)

<!-- 앵커가 있는 링크 -->
[구성 섹션](./config.md#database-settings)

<!-- 참조 스타일 내부 링크 -->
설정 지침은 [설치 가이드][install]를 참조하십시오.

[install]: ../getting-started/installation.md "설치 가이드"
```

### 자동 상호 참조

```markdown
<!-- 이러한 패턴은 자동으로 링크로 변환됩니다 -->
자세한 내용은 이슈 #123를 참조하십시오.
구현은 커밋 abc123f를 참조하십시오.
코드 검토는 풀 리퀘스트 !456을 확인하십시오.
```

## 확장 마크다운 모범 사례

### 성능 고려사항

- **컴포넌트 사용**: 최적 성능을 위해 상호 작용 컴포넌트를 신중하게 사용
- **이미지 최적화**: 임베딩 전에 이미지를 최적화
- **수학적 표현식**: 더 빠른 렌더링을 위해 복잡한 수학적 표현식을 제한

### 접근성 가이드라인

- **대체 텍스트**: 모든 이미지와 미디어에 의미 있는 대체 텍스트 제공
- **색상 대비**: 사용자 정의 스타일링에서 충분한 대비 보장
- **키보드 네비게이션**: 키보드 네비게이션으로 상호 작용 요소 테스트
- **스크린 리더 호환성**: 보조 기술과 함께 콘텐츠 작동 확인

### 유지보수 및 업데이트

- **버전 호환성**: BaiDocs 업데이트와 함께 확장 문법 테스트
- **컴포넌트 종속성**: 사용자 정의 컴포넌트 기능 모니터링
- **링크 검증**: 내부 및 외부 링크를 정기적으로 확인
- **콘텐츠 검토**: 향상된 콘텐츠 요소를 주기적으로 검토 및 업데이트

BaiDocs의 확장 마크다운 기능을 통해 전문적인 표현 표준을 유지하면서 사용자 참여를 유도하는 풍부하고 상호 작용적인 문서를 작성할 수 있습니다.