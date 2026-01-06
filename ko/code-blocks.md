---
title: 코드 블록
description: BaiDocs에서 구문 강조 코드 블록 사용 방법 배우기
---

# 코드 블록

코드 블록은 기술 문서에 필수적입니다. BaiDocs는 100개 이상의 프로그래밍 언어에 대한 아름다운 구문 강조를 제공합니다.

## 기본 코드 블록

### 언어 지정

언어 식별자와 함께 삼중 백틱 사용:

````markdown
```javascript
function hello() {
  console.log('안녕하세요!');
}
```
````

**렌더링됨:**

```javascript
function hello() {
  console.log('안녕하세요!');
}
```

## 지원되는 언어

### JavaScript / TypeScript

```javascript
const greeting = '안녕하세요!';
console.log(greeting);
```

```typescript
interface User {
  id: number;
  name: string;
}
```

### Python

```python
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)
```

### Bash / Shell

```bash
# 의존성 설치
pnpm install

# 개발 서버 시작
pnpm dev
```

### YAML

```yaml
id: my-book
title: 내 문서
languages:
  - ko
  - en
```

### JSON

```json
{
  "name": "@baidocs/viewer",
  "version": "1.0.0",
  "dependencies": {
    "next": "^15.0.0"
  }
}
```

## 인라인 코드

단일 백틱 사용:

```markdown
`useState` 훅은 React에서 상태 관리에 사용됩니다.
```

**렌더링됨:**

`useState` 훅은 React에서 상태 관리에 사용됩니다.

## 모범 사례

### 1. 항상 언어 지정

```markdown
<!-- 좋음 -->
\`\`\`javascript
console.log('안녕');
\`\`\`

<!-- 나쁨 -->
\`\`\`
console.log('안녕');
\`\`\`
```

### 2. 주석 추가

```javascript
// 애플리케이션 초기화
const app = express();

// 미들웨어 설정
app.use(express.json());
```

## 다음 단계

[테이블과 리스트](tables-lists.md)에서 구조화된 정보를 구성하는 방법을 배우세요!
