---
title: Code Blocks
description: Learn how to use syntax-highlighted code blocks in BaiDocs
---

# Code Blocks

Code blocks are essential for technical documentation. BaiDocs provides beautiful syntax highlighting for over 100 programming languages.

## Basic Code Blocks

### Fenced Code Blocks

Use triple backticks with a language identifier:

````markdown
```javascript
function hello() {
  console.log('Hello, World!');
}
```
````

**Rendered:**

```javascript
function hello() {
  console.log('Hello, World!');
}
```

### Without Language

Code blocks without language specification still work:

````markdown
```
Plain text code block
No syntax highlighting
```
````

**Rendered:**

```
Plain text code block
No syntax highlighting
```

## Supported Languages

BaiDocs supports syntax highlighting for many languages:

### JavaScript / TypeScript

````markdown
```javascript
const greeting = 'Hello, World!';
console.log(greeting);

async function fetchData() {
  const response = await fetch('/api/data');
  return response.json();
}
```
````

**Rendered:**

```javascript
const greeting = 'Hello, World!';
console.log(greeting);

async function fetchData() {
  const response = await fetch('/api/data');
  return response.json();
}
```

````markdown
```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

function getUser(id: number): Promise<User> {
  return fetch(`/api/users/${id}`).then(res => res.json());
}
```
````

**Rendered:**

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

function getUser(id: number): Promise<User> {
  return fetch(`/api/users/${id}`).then(res => res.json());
}
```

### Python

````markdown
```python
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

# List comprehension
squares = [x**2 for x in range(10)]
print(squares)
```
````

**Rendered:**

```python
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

# List comprehension
squares = [x**2 for x in range(10)]
print(squares)
```

### Bash / Shell

````markdown
```bash
#!/bin/bash

# Install dependencies
pnpm install

# Start development server
pnpm dev

# Check status
echo "Server started on port 3000"
```
````

**Rendered:**

```bash
#!/bin/bash

# Install dependencies
pnpm install

# Start development server
pnpm dev

# Check status
echo "Server started on port 3000"
```

### YAML

````markdown
```yaml
id: my-book
title: My Documentation
description: Comprehensive guide
languages:
  - en
  - ko
navigation:
  en:
    - title: Getting Started
      items:
        - title: Introduction
          path: index.md
```
````

**Rendered:**

```yaml
id: my-book
title: My Documentation
description: Comprehensive guide
languages:
  - en
  - ko
navigation:
  en:
    - title: Getting Started
      items:
        - title: Introduction
          path: index.md
```

### JSON

````markdown
```json
{
  "name": "@baidocs/viewer",
  "version": "1.0.0",
  "dependencies": {
    "next": "^15.0.0",
    "react": "^19.0.0"
  },
  "scripts": {
    "dev": "next dev",
    "build": "next build"
  }
}
```
````

**Rendered:**

```json
{
  "name": "@baidocs/viewer",
  "version": "1.0.0",
  "dependencies": {
    "next": "^15.0.0",
    "react": "^19.0.0"
  },
  "scripts": {
    "dev": "next dev",
    "build": "next build"
  }
}
```

### CSS / SCSS

````markdown
```css
:root {
  --primary-color: #1890ff;
  --text-color: #333;
  --background: #fff;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.button {
  background-color: var(--primary-color);
  color: white;
  border: none;
  border-radius: 4px;
  padding: 10px 20px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.button:hover {
  background-color: #40a9ff;
}
```
````

**Rendered:**

```css
:root {
  --primary-color: #1890ff;
  --text-color: #333;
  --background: #fff;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.button {
  background-color: var(--primary-color);
  color: white;
  border: none;
  border-radius: 4px;
  padding: 10px 20px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.button:hover {
  background-color: #40a9ff;
}
```

### HTML

````markdown
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>BaiDocs</title>
</head>
<body>
  <header>
    <h1>Welcome to BaiDocs</h1>
    <nav>
      <a href="/">Home</a>
      <a href="/docs">Documentation</a>
    </nav>
  </header>
  <main>
    <p>Create beautiful documentation.</p>
  </main>
</body>
</html>
```
````

**Rendered:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>BaiDocs</title>
</head>
<body>
  <header>
    <h1>Welcome to BaiDocs</h1>
    <nav>
      <a href="/">Home</a>
      <a href="/docs">Documentation</a>
    </nav>
  </header>
  <main>
    <p>Create beautiful documentation.</p>
  </main>
</body>
</html>
```

### SQL

````markdown
```sql
-- Create users table
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  username VARCHAR(50) UNIQUE NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Insert sample data
INSERT INTO users (username, email) VALUES
  ('alice', 'alice@example.com'),
  ('bob', 'bob@example.com');

-- Query with JOIN
SELECT u.username, p.title
FROM users u
LEFT JOIN posts p ON u.id = p.user_id
WHERE u.created_at > '2024-01-01'
ORDER BY u.username;
```
````

**Rendered:**

```sql
-- Create users table
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  username VARCHAR(50) UNIQUE NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Insert sample data
INSERT INTO users (username, email) VALUES
  ('alice', 'alice@example.com'),
  ('bob', 'bob@example.com');

-- Query with JOIN
SELECT u.username, p.title
FROM users u
LEFT JOIN posts p ON u.id = p.user_id
WHERE u.created_at > '2024-01-01'
ORDER BY u.username;
```

### Go

````markdown
```go
package main

import (
    "fmt"
    "net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello, %s!", r.URL.Path[1:])
}

func main() {
    http.HandleFunc("/", handler)
    http.ListenAndServe(":8080", nil)
}
```
````

**Rendered:**

```go
package main

import (
    "fmt"
    "net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello, %s!", r.URL.Path[1:])
}

func main() {
    http.HandleFunc("/", handler)
    http.ListenAndServe(":8080", nil)
}
```

### Rust

````markdown
```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];

    let sum: i32 = numbers.iter().sum();
    println!("Sum: {}", sum);

    let doubled: Vec<i32> = numbers.iter()
        .map(|x| x * 2)
        .collect();
    println!("Doubled: {:?}", doubled);
}
```
````

**Rendered:**

```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];

    let sum: i32 = numbers.iter().sum();
    println!("Sum: {}", sum);

    let doubled: Vec<i32> = numbers.iter()
        .map(|x| x * 2)
        .collect();
    println!("Doubled: {:?}", doubled);
}
```

### Java

````markdown
```java
public class HelloWorld {
    private String message;

    public HelloWorld(String message) {
        this.message = message;
    }

    public void printMessage() {
        System.out.println(message);
    }

    public static void main(String[] args) {
        HelloWorld hello = new HelloWorld("Hello, World!");
        hello.printMessage();
    }
}
```
````

**Rendered:**

```java
public class HelloWorld {
    private String message;

    public HelloWorld(String message) {
        this.message = message;
    }

    public void printMessage() {
        System.out.println(message);
    }

    public static void main(String[] args) {
        HelloWorld hello = new HelloWorld("Hello, World!");
        hello.printMessage();
    }
}
```

### More Languages

BaiDocs supports many more languages:

| Language | Code | Language | Code |
|----------|------|----------|------|
| C | `c` | C++ | `cpp` |
| C# | `csharp` | PHP | `php` |
| Ruby | `ruby` | Swift | `swift` |
| Kotlin | `kotlin` | Scala | `scala` |
| Dart | `dart` | Elixir | `elixir` |
| Haskell | `haskell` | Clojure | `clojure` |
| Lua | `lua` | Perl | `perl` |
| R | `r` | MATLAB | `matlab` |
| Julia | `julia` | Fortran | `fortran` |

## Inline Code

Use single backticks for inline code:

```markdown
The `useState` hook is used for state management in React.

Run the `pnpm install` command to install dependencies.

The variable `maxCount` should be a positive integer.
```

**Rendered:**

The `useState` hook is used for state management in React.

Run the `pnpm install` command to install dependencies.

The variable `maxCount` should be a positive integer.

## Best Practices

### 1. Always Specify Language

```markdown
<!-- Good -->
\`\`\`javascript
console.log('Hello');
\`\`\`

<!-- Less good -->
\`\`\`
console.log('Hello');
\`\`\`
```

Specifying the language enables syntax highlighting.

### 2. Keep Code Blocks Focused

```markdown
<!-- Good: Focused example -->
\`\`\`javascript
// Calculate sum
const sum = numbers.reduce((a, b) => a + b, 0);
\`\`\`

<!-- Too long: Hard to follow -->
\`\`\`javascript
// 50+ lines of code...
\`\`\`
```

Break long code into smaller, focused examples.

### 3. Add Comments

```markdown
\`\`\`javascript
// Initialize the application
const app = express();

// Configure middleware
app.use(express.json());

// Define routes
app.get('/', (req, res) => {
  res.send('Hello, World!');
});
\`\`\`
```

Comments help readers understand the code.

### 4. Show Complete Examples

```markdown
<!-- Good: Complete and runnable -->
\`\`\`javascript
import React from 'react';

function Welcome({ name }) {
  return <h1>Hello, {name}!</h1>;
}

export default Welcome;
\`\`\`

<!-- Bad: Incomplete -->
\`\`\`javascript
function Welcome({ name }) {
  return <h1>Hello, {name}!</h1>;
}
\`\`\`
```

Include necessary imports and exports.

### 5. Use Consistent Formatting

```markdown
<!-- Good: Consistent indentation -->
\`\`\`javascript
function example() {
  if (condition) {
    doSomething();
  }
}
\`\`\`

<!-- Bad: Inconsistent -->
\`\`\`javascript
function example() {
if(condition){
doSomething();
}
}
\`\`\`
```

## Code Blocks in Admonitions

Code blocks work great inside admonitions:

```markdown
:::tip
To install dependencies, run:

\`\`\`bash
pnpm install
\`\`\`

This will install all packages defined in package.json.
:::
```

**Rendered:**

:::tip
To install dependencies, run:

```bash
pnpm install
```

This will install all packages defined in package.json.
:::

## Multiple Code Blocks

Show multiple related code blocks:

```markdown
First, create the configuration file:

\`\`\`yaml
# config.yaml
server:
  port: 3000
  host: localhost
\`\`\`

Then, load it in your application:

\`\`\`javascript
// app.js
const config = require('./config.yaml');
console.log(`Server running on ${config.server.host}:${config.server.port}`);
\`\`\`
```

## Diff-like Code Blocks

Show changes using comments:

```javascript
// Before
function oldWay() {
  return data.filter(x => x > 0);
}

// After
function newWay() {
  return data.filter(x => x > 0).map(x => x * 2);
}
```

:::note
BaiDocs doesn't have built-in diff highlighting yet. Use comments to indicate changes.
:::

## Common Language Aliases

Some languages have multiple aliases:

| Language | Aliases |
|----------|---------|
| JavaScript | `js`, `javascript` |
| TypeScript | `ts`, `typescript` |
| Bash | `bash`, `sh`, `shell` |
| Python | `py`, `python` |
| YAML | `yaml`, `yml` |
| Markdown | `md`, `markdown` |

## Troubleshooting

### No Syntax Highlighting

If syntax highlighting doesn't work:

1. ✅ Check language identifier spelling
2. ✅ Ensure language is supported
3. ✅ Check for proper backtick formatting
4. ✅ Try a common alias

### Code Not Displaying

If code doesn't display:

1. ✅ Check for proper fencing (three backticks)
2. ✅ Ensure backticks are on their own lines
3. ✅ Check for special characters that need escaping

## Next Steps

Learn about [Tables and Lists](tables-lists.md) to organize structured information!
