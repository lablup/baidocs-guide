# 코드 블록과 구문 강조

BaiDocs는 기술 문서화를 위한 포괄적인 구문 강조와 코드 표현 기능을 제공합니다. 이 섹션에서는 코드 블록 구현, 언어 지원, 고급 형식 옵션을 다룹니다.

## 기본 코드 블록 문법

### 인라인 코드

단일 백틱을 사용하여 인라인 코드 요소 작성:

```markdown
애플리케이션을 시작하기 전에 `API_KEY` 환경 변수를 구성하십시오.
`getUserData()` 함수는 사용자 정보를 포함하는 Promise를 반환합니다.
`/etc/config/app.conf`와 같은 파일 경로는 절대 참조를 사용해야 합니다.
```

### 펜스 코드 블록

여러 줄 코드 블록에는 삼중 백틱을 사용:

````markdown
```
구문 강조가 없는 기본 코드 블록
고정폭 글꼴과 형식 보존 사용
일반 텍스트나 알 수 없는 언어에 적합
```
````

## 언어별 구문 강조

### 지원 언어

BaiDocs는 100개 이상의 프로그래밍 언어에 대한 구문 강조를 지원합니다:

#### 웹 기술

````markdown
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>BaiDocs 예시</title>
</head>
<body>
    <h1>BaiDocs에 오신 것을 환영합니다</h1>
    <p>현대적인 문서화 프레임워크</p>
</body>
</html>
```

```css
.documentation-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 2rem;
    font-family: 'Inter', sans-serif;
}

.code-block {
    background-color: #f6f8fa;
    border-radius: 6px;
    padding: 1rem;
    overflow-x: auto;
}
```

```javascript
const BaiDocsConfig = {
    title: '문서화 사이트',
    description: '포괄적인 가이드',
    languages: ['en', 'ko'],
    theme: {
        primaryColor: '#0066cc',
        secondaryColor: '#f0f0f0'
    }
};

async function initializeDocumentation() {
    try {
        const config = await loadConfiguration();
        const renderer = new DocumentationRenderer(config);
        await renderer.initialize();
        console.log('문서화가 성공적으로 초기화되었습니다');
    } catch (error) {
        console.error('초기화 실패:', error.message);
    }
}
```
````

#### 프로그래밍 언어

````markdown
```python
class DocumentationManager:
    def __init__(self, config_path: str):
        self.config_path = config_path
        self.content = {}
        self.metadata = {}

    async def load_content(self, language: str = 'en') -> dict:
        """지정된 언어의 문서화 콘텐츠를 로드합니다."""
        try:
            with open(f'{self.config_path}/{language}/index.md', 'r') as file:
                content = file.read()
                return self.parse_markdown(content)
        except FileNotFoundError as e:
            raise DocumentationError(f"콘텐츠를 찾을 수 없습니다: {e}")

    def parse_markdown(self, content: str) -> dict:
        """마크다운 콘텐츠를 파싱하고 메타데이터를 추출합니다."""
        lines = content.split('\n')
        metadata = {}
        content_start = 0

        if lines[0] == '---':
            for i, line in enumerate(lines[1:], 1):
                if line == '---':
                    content_start = i + 1
                    break
                key, value = line.split(':', 1)
                metadata[key.strip()] = value.strip()

        return {
            'metadata': metadata,
            'content': '\n'.join(lines[content_start:])
        }
```

```java
public class DocumentationService {
    private final ConfigurationManager configManager;
    private final ContentRepository contentRepository;
    private static final Logger logger = LoggerFactory.getLogger(DocumentationService.class);

    public DocumentationService(ConfigurationManager configManager,
                              ContentRepository contentRepository) {
        this.configManager = configManager;
        this.contentRepository = contentRepository;
    }

    public CompletableFuture<DocumentationResponse> generateDocumentation(
            String bookId, String language, String path) {

        return CompletableFuture.supplyAsync(() -> {
            try {
                BookConfiguration config = configManager.getBookConfig(bookId);
                ContentMetadata metadata = contentRepository.getContentMetadata(bookId, language, path);
                String content = contentRepository.getContent(bookId, language, path);

                return DocumentationResponse.builder()
                    .bookId(bookId)
                    .language(language)
                    .path(path)
                    .metadata(metadata)
                    .content(content)
                    .build();

            } catch (ConfigurationException | ContentNotFoundException e) {
                logger.error("문서 생성 실패", e);
                throw new DocumentationServiceException("생성 실패", e);
            }
        });
    }
}
```
````

#### 시스템 구성

````markdown
```bash
#!/bin/bash

# BaiDocs 설치 스크립트
set -euo pipefail

BAIDOCS_VERSION="2.0.0"
INSTALL_DIR="/opt/baidocs"
CONFIG_DIR="/etc/baidocs"

function install_dependencies() {
    echo "시스템 종속성 설치 중..."

    if command -v apt-get >/dev/null 2>&1; then
        sudo apt-get update
        sudo apt-get install -y nodejs npm git
    elif command -v yum >/dev/null 2>&1; then
        sudo yum install -y nodejs npm git
    else
        echo "지원하지 않는 패키지 매니저입니다" >&2
        exit 1
    fi
}

function setup_baidocs() {
    echo "BaiDocs 설정 중..."

    mkdir -p "$INSTALL_DIR"
    mkdir -p "$CONFIG_DIR"

    git clone https://github.com/lablup/baidocs.git "$INSTALL_DIR"
    cd "$INSTALL_DIR"
    npm install --production

    cp config/production.example.env "$CONFIG_DIR/.env"
    echo "INSTALL_DIR=$INSTALL_DIR" >> "$CONFIG_DIR/.env"
}

function main() {
    echo "BaiDocs 설치 스크립트 v$BAIDOCS_VERSION"
    install_dependencies
    setup_baidocs
    echo "설치가 성공적으로 완료되었습니다!"
}

main "$@"
```

```yaml
# BaiDocs 구성
apiVersion: v1
kind: ConfigMap
metadata:
  name: baidocs-config
  namespace: documentation
data:
  config.yaml: |
    server:
      port: 3000
      host: "0.0.0.0"

    database:
      type: postgresql
      host: postgres-service
      port: 5432
      database: baidocs
      username: ${DB_USERNAME}
      password: ${DB_PASSWORD}
      pool:
        min: 5
        max: 20

    storage:
      type: filesystem
      path: /app/content

    authentication:
      providers:
        - name: github
          client_id: ${GITHUB_CLIENT_ID}
          client_secret: ${GITHUB_CLIENT_SECRET}
        - name: google
          client_id: ${GOOGLE_CLIENT_ID}
          client_secret: ${GOOGLE_CLIENT_SECRET}

    features:
      pdf_generation: true
      search_indexing: true
      analytics: true

    logging:
      level: info
      format: json
      destinations:
        - console
        - file
```
````

## 고급 코드 블록 기능

### 줄 번호 매기기

````markdown
```javascript {showLineNumbers=true}
function calculateDocumentationMetrics(content) {
    const lines = content.split('\n');
    const wordCount = content.split(/\s+/).length;
    const characterCount = content.length;

    return {
        lines: lines.length,
        words: wordCount,
        characters: characterCount,
        readingTime: Math.ceil(wordCount / 200) // 평균 읽기 속도
    };
}
```
````

### 줄 강조

````markdown
```javascript {2,5-7}
function processDocumentationUpdate(bookId, changes) {
    const validation = validateChanges(changes); // 강조된 줄

    if (!validation.isValid) {
        console.error('검증 실패:', validation.errors); // 강조 블록
        throw new ValidationError(validation.errors);
        return false;
    }

    return applyChanges(bookId, changes);
}
```
````

### 코드 제목과 메타데이터

````markdown
```javascript {title="src/utils/documentation.js" showLineNumbers=true}
export class DocumentationUtils {
    static formatTitle(title) {
        return title
            .toLowerCase()
            .replace(/[^a-z0-9\s-]/g, '')
            .replace(/\s+/g, '-')
            .trim();
    }

    static extractMetadata(content) {
        const frontMatterRegex = /^---\n([\s\S]*?)\n---/;
        const match = content.match(frontMatterRegex);

        if (match) {
            return yaml.parse(match[1]);
        }

        return {};
    }
}
```
````

## 코드 차이점과 비교

### Git 스타일 차이점

````markdown
```diff
// 구성 파일 변경사항
  module.exports = {
-   port: 3000,
-   environment: 'development',
+   port: process.env.PORT || 3000,
+   environment: process.env.NODE_ENV || 'development',

    database: {
-     host: 'localhost',
+     host: process.env.DB_HOST || 'localhost',
      port: 5432,
      name: 'baidocs'
    }
  };
```
````

### 나란히 비교

````markdown
```javascript
// 이전: 기본 구현
function loadDocumentation(id) {
    return fetch(`/api/docs/${id}`)
        .then(response => response.json())
        .then(data => data.content);
}
```

```javascript
// 이후: 오류 처리가 향상된 버전
async function loadDocumentation(id) {
    try {
        const response = await fetch(`/api/docs/${id}`);

        if (!response.ok) {
            throw new Error(`HTTP ${response.status}: ${response.statusText}`);
        }

        const data = await response.json();
        return data.content;

    } catch (error) {
        console.error('문서 로딩 실패:', error);
        throw new DocumentationLoadError(error.message);
    }
}
```
````

## 상호 작용 코드 예시

### 실행 가능한 코드 블록

````markdown
```javascript {interactive=true}
// 상호 작용 JavaScript 예시
const greeting = "안녕하세요, BaiDocs!";
const timestamp = new Date().toISOString();

console.log(`${greeting} 현재 시간: ${timestamp}`);

// 이 코드를 수정해 보고 결과를 확인하세요
function generateRandomId() {
    return Math.random().toString(36).substring(2, 15);
}

console.log('랜덤 ID:', generateRandomId());
```
````

### 코드 플레이그라운드 통합

````markdown
```html {playground=true}
<!DOCTYPE html>
<html>
<head>
    <title>BaiDocs 예시</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 2rem; }
        .highlight { background-color: #ffeb3b; padding: 0.5rem; }
    </style>
</head>
<body>
    <h1>상호 작용 HTML 예시</h1>
    <p class="highlight">이 예시는 실시간으로 편집하고 미리보기할 수 있습니다.</p>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            console.log('BaiDocs 상호 작용 예시가 로드되었습니다');
        });
    </script>
</body>
</html>
```
````

## 코드 문서화 통합

### API 문서화

````markdown
```typescript {title="types/api.ts"}
/**
 * 문서화 책 구성 인터페이스
 * @interface BookConfig
 */
interface BookConfig {
    /** 책의 고유 식별자 */
    id: string;

    /** 책의 표시 제목 */
    title: string;

    /** 책 내용의 간략한 설명 */
    description: string;

    /** 지원하는 언어 코드 */
    languages: string[];

    /** 콘텐츠의 기본 언어 */
    defaultLanguage: string;

    /** 각 언어별 네비게이션 구조 */
    navigation: Record<string, NavigationItem[]>;

    /** 시각적 식별을 위한 선택적 이모지 아이콘 */
    emoji?: string;

    /** 책 유형: 독립형 또는 모노레포 */
    type: 'standalone' | 'monorepo';
}

/**
 * 네비게이션 항목 구조
 * @interface NavigationItem
 */
interface NavigationItem {
    /** 네비게이션용 표시 제목 */
    title: string;

    /** 콘텐츠 파일 또는 섹션 경로 */
    path: string;

    /** 하위 네비게이션 항목 */
    items?: NavigationItem[];
}
```
````

### 함수 문서화

````markdown
```javascript {title="src/services/content-service.js"}
/**
 * 문서화 콘텐츠를 로드하고 처리합니다
 * @param {string} bookId - 고유한 책 식별자
 * @param {string} language - 언어 코드 (예: 'en', 'ko')
 * @param {string} path - 책 내 콘텐츠 경로
 * @returns {Promise<ContentResponse>} 메타데이터가 포함된 처리된 콘텐츠
 * @throws {ContentNotFoundError} 요청된 콘텐츠가 존재하지 않을 때
 * @throws {InvalidLanguageError} 언어가 지원되지 않을 때
 *
 * @example
 * // 영어 소개 페이지 로드
 * const content = await loadContent('user-guide', 'en', 'introduction/index.md');
 * console.log(content.title, content.body);
 *
 * @example
 * // 오류를 우아하게 처리
 * try {
 *     const content = await loadContent('api-docs', 'ko', 'authentication.md');
 * } catch (error) {
 *     if (error instanceof ContentNotFoundError) {
 *         console.log('한국어로 콘텐츠를 사용할 수 없습니다');
 *     }
 * }
 */
async function loadContent(bookId, language, path) {
    // 구현 세부사항...
}
```
````

## 성능 및 최적화

### 대용량 코드 블록 처리

````markdown
```javascript {fold=true title="대용량 구성 객체"}
const complexConfiguration = {
    // 이 코드 블록은 기본적으로 접힐 수 있어서
    // 페이지 로딩과 가독성을 향상시킵니다

    server: {
        port: 3000,
        host: '0.0.0.0',
        ssl: {
            enabled: true,
            certificate: '/path/to/cert.pem',
            privateKey: '/path/to/key.pem'
        }
    },

    database: {
        type: 'postgresql',
        connections: {
            primary: {
                host: 'primary-db.example.com',
                port: 5432,
                database: 'baidocs_prod',
                username: 'app_user',
                password: process.env.DB_PASSWORD,
                ssl: true,
                pool: {
                    min: 10,
                    max: 50,
                    idle: 10000
                }
            },
            readonly: {
                host: 'readonly-db.example.com',
                port: 5432,
                database: 'baidocs_prod',
                username: 'readonly_user',
                password: process.env.READONLY_DB_PASSWORD,
                ssl: true,
                pool: {
                    min: 5,
                    max: 20,
                    idle: 10000
                }
            }
        }
    }

    // ... 광범위한 구성 계속
};
```
````

## 모범 사례

### 코드 품질 표준

#### 형식 가이드라인

- **일관된 들여쓰기**: 일관된 간격 사용 (2 또는 4 스페이스)
- **의미 있는 변수명**: 서술적이고 자체 설명적인 이름 사용
- **주석 포함**: 복잡한 로직에 대한 주석 추가
- **오류 처리**: 적절한 오류 처리 패턴 시연

#### 예시 품질

```markdown
✅ 좋은 예시:
```javascript
async function authenticateUser(credentials) {
    try {
        const user = await userService.validateCredentials(credentials);
        const token = await tokenService.generateToken(user.id);
        return { user, token };
    } catch (error) {
        logger.error('인증 실패:', error);
        throw new AuthenticationError('잘못된 자격 증명');
    }
}
```

❌ 나쁜 예시:
```javascript
function auth(c) {
    var u = validate(c);
    if(u) return u;
    else throw "error";
}
```
```

### 접근성 및 사용성

#### 스크린 리더 지원

- **대체 설명**: 복잡한 코드 예시에 대한 컨텍스트 제공
- **명확한 구성**: 일관된 구조와 제목 사용
- **키보드 네비게이션**: 키보드를 통한 코드 블록 접근성 보장

#### 모바일 최적화

- **수평 스크롤**: 모바일에서 코드 블록이 수평으로 스크롤되도록 보장
- **글꼴 크기**: 모바일 디바이스에 적합한 글꼴 크기 사용
- **터치 상호 작용**: 모바일 상호 작용을 위한 터치 대상 최적화

효과적인 코드 표현은 문서 사용성을 향상시키고 개발자가 구현 세부사항을 빠르고 정확하게 이해할 수 있도록 도와줍니다.