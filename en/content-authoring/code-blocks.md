# Code Blocks and Syntax Highlighting

BaiDocs provides comprehensive syntax highlighting and code presentation features for technical documentation. This section covers code block implementation, language support, and advanced formatting options.

## Basic Code Block Syntax

### Inline Code

Use single backticks for inline code elements:

```markdown
Configure the `API_KEY` environment variable before starting the application.
The `getUserData()` function returns a Promise containing user information.
File paths like `/etc/config/app.conf` should use absolute references.
```

### Fenced Code Blocks

Use triple backticks for multi-line code blocks:

````markdown
```
Basic code block without syntax highlighting
Uses monospace font with preserved formatting
Suitable for plain text or unknown languages
```
````

## Language-specific Syntax Highlighting

### Supported Languages

BaiDocs supports syntax highlighting for 100+ programming languages:

#### Web Technologies

````markdown
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>BaiDocs Example</title>
</head>
<body>
    <h1>Welcome to BaiDocs</h1>
    <p>Modern documentation framework</p>
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
    title: 'Documentation Site',
    description: 'Comprehensive guide',
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
        console.log('Documentation initialized successfully');
    } catch (error) {
        console.error('Initialization failed:', error.message);
    }
}
```
````

#### Programming Languages

````markdown
```python
class DocumentationManager:
    def __init__(self, config_path: str):
        self.config_path = config_path
        self.content = {}
        self.metadata = {}

    async def load_content(self, language: str = 'en') -> dict:
        """Load documentation content for specified language."""
        try:
            with open(f'{self.config_path}/{language}/index.md', 'r') as file:
                content = file.read()
                return self.parse_markdown(content)
        except FileNotFoundError as e:
            raise DocumentationError(f"Content not found: {e}")

    def parse_markdown(self, content: str) -> dict:
        """Parse markdown content and extract metadata."""
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
                logger.error("Failed to generate documentation", e);
                throw new DocumentationServiceException("Generation failed", e);
            }
        });
    }
}
```
````

#### System Configuration

````markdown
```bash
#!/bin/bash

# BaiDocs Installation Script
set -euo pipefail

BAIDOCS_VERSION="2.0.0"
INSTALL_DIR="/opt/baidocs"
CONFIG_DIR="/etc/baidocs"

function install_dependencies() {
    echo "Installing system dependencies..."

    if command -v apt-get >/dev/null 2>&1; then
        sudo apt-get update
        sudo apt-get install -y nodejs npm git
    elif command -v yum >/dev/null 2>&1; then
        sudo yum install -y nodejs npm git
    else
        echo "Unsupported package manager" >&2
        exit 1
    fi
}

function setup_baidocs() {
    echo "Setting up BaiDocs..."

    mkdir -p "$INSTALL_DIR"
    mkdir -p "$CONFIG_DIR"

    git clone https://github.com/lablup/baidocs.git "$INSTALL_DIR"
    cd "$INSTALL_DIR"
    npm install --production

    cp config/production.example.env "$CONFIG_DIR/.env"
    echo "INSTALL_DIR=$INSTALL_DIR" >> "$CONFIG_DIR/.env"
}

function main() {
    echo "BaiDocs Installation Script v$BAIDOCS_VERSION"
    install_dependencies
    setup_baidocs
    echo "Installation completed successfully!"
}

main "$@"
```

```yaml
# BaiDocs Configuration
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

## Advanced Code Block Features

### Line Numbering

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
        readingTime: Math.ceil(wordCount / 200) // Average reading speed
    };
}
```
````

### Line Highlighting

````markdown
```javascript {2,5-7}
function processDocumentationUpdate(bookId, changes) {
    const validation = validateChanges(changes); // highlighted line

    if (!validation.isValid) {
        console.error('Validation failed:', validation.errors); // highlighted block
        throw new ValidationError(validation.errors);
        return false;
    }

    return applyChanges(bookId, changes);
}
```
````

### Code Titles and Metadata

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

## Code Diffs and Comparisons

### Git-style Diffs

````markdown
```diff
// Configuration file changes
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

### Side-by-side Comparisons

````markdown
```javascript
// Before: Basic implementation
function loadDocumentation(id) {
    return fetch(`/api/docs/${id}`)
        .then(response => response.json())
        .then(data => data.content);
}
```

```javascript
// After: Enhanced with error handling
async function loadDocumentation(id) {
    try {
        const response = await fetch(`/api/docs/${id}`);

        if (!response.ok) {
            throw new Error(`HTTP ${response.status}: ${response.statusText}`);
        }

        const data = await response.json();
        return data.content;

    } catch (error) {
        console.error('Failed to load documentation:', error);
        throw new DocumentationLoadError(error.message);
    }
}
```
````

## Interactive Code Examples

### Executable Code Blocks

````markdown
```javascript {interactive=true}
// Interactive JavaScript example
const greeting = "Hello, BaiDocs!";
const timestamp = new Date().toISOString();

console.log(`${greeting} Current time: ${timestamp}`);

// Try modifying this code and see the results
function generateRandomId() {
    return Math.random().toString(36).substring(2, 15);
}

console.log('Random ID:', generateRandomId());
```
````

### Code Playground Integration

````markdown
```html {playground=true}
<!DOCTYPE html>
<html>
<head>
    <title>BaiDocs Example</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 2rem; }
        .highlight { background-color: #ffeb3b; padding: 0.5rem; }
    </style>
</head>
<body>
    <h1>Interactive HTML Example</h1>
    <p class="highlight">This example can be edited and previewed live.</p>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            console.log('BaiDocs interactive example loaded');
        });
    </script>
</body>
</html>
```
````

## Code Documentation Integration

### API Documentation

````markdown
```typescript {title="types/api.ts"}
/**
 * Documentation book configuration interface
 * @interface BookConfig
 */
interface BookConfig {
    /** Unique identifier for the book */
    id: string;

    /** Display title of the book */
    title: string;

    /** Brief description of book content */
    description: string;

    /** Supported language codes */
    languages: string[];

    /** Default language for content */
    defaultLanguage: string;

    /** Navigation structure for each language */
    navigation: Record<string, NavigationItem[]>;

    /** Optional emoji icon for visual identification */
    emoji?: string;

    /** Book type: standalone or monorepo */
    type: 'standalone' | 'monorepo';
}

/**
 * Navigation item structure
 * @interface NavigationItem
 */
interface NavigationItem {
    /** Display title for navigation */
    title: string;

    /** Path to content file or section */
    path: string;

    /** Child navigation items */
    items?: NavigationItem[];
}
```
````

### Function Documentation

````markdown
```javascript {title="src/services/content-service.js"}
/**
 * Load and process documentation content
 * @param {string} bookId - Unique book identifier
 * @param {string} language - Language code (e.g., 'en', 'ko')
 * @param {string} path - Content path within book
 * @returns {Promise<ContentResponse>} Processed content with metadata
 * @throws {ContentNotFoundError} When requested content doesn't exist
 * @throws {InvalidLanguageError} When language is not supported
 *
 * @example
 * // Load English introduction page
 * const content = await loadContent('user-guide', 'en', 'introduction/index.md');
 * console.log(content.title, content.body);
 *
 * @example
 * // Handle errors gracefully
 * try {
 *     const content = await loadContent('api-docs', 'ko', 'authentication.md');
 * } catch (error) {
 *     if (error instanceof ContentNotFoundError) {
 *         console.log('Content not available in Korean');
 *     }
 * }
 */
async function loadContent(bookId, language, path) {
    // Implementation details...
}
```
````

## Performance and Optimization

### Large Code Block Handling

````markdown
```javascript {fold=true title="Large configuration object"}
const complexConfiguration = {
    // This code block can be collapsed by default
    // to improve page loading and readability

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

    // ... extensive configuration continues
};
```
````

## Best Practices

### Code Quality Standards

#### Formatting Guidelines

- **Consistent indentation**: Use consistent spacing (2 or 4 spaces)
- **Meaningful variable names**: Use descriptive, self-documenting names
- **Comment inclusion**: Add comments for complex logic
- **Error handling**: Demonstrate proper error handling patterns

#### Example Quality

```markdown
✅ Good Example:
```javascript
async function authenticateUser(credentials) {
    try {
        const user = await userService.validateCredentials(credentials);
        const token = await tokenService.generateToken(user.id);
        return { user, token };
    } catch (error) {
        logger.error('Authentication failed:', error);
        throw new AuthenticationError('Invalid credentials');
    }
}
```

❌ Poor Example:
```javascript
function auth(c) {
    var u = validate(c);
    if(u) return u;
    else throw "error";
}
```
```

### Accessibility and Usability

#### Screen Reader Support

- **Alternative descriptions**: Provide context for complex code examples
- **Clear organization**: Use consistent structure and headings
- **Keyboard navigation**: Ensure code blocks are accessible via keyboard

#### Mobile Optimization

- **Horizontal scrolling**: Ensure code blocks scroll horizontally on mobile
- **Font sizing**: Use appropriate font sizes for mobile devices
- **Touch interaction**: Optimize touch targets for mobile interaction

Effective code presentation enhances documentation usability and helps developers understand implementation details quickly and accurately.