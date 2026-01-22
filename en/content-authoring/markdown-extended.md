# Extended Markdown Syntax

BaiDocs supports enhanced Markdown features through MDX integration and custom extensions. This section covers advanced syntax options for creating rich, interactive documentation.

## MDX Integration

### Component Embedding

MDX allows embedding React components directly within Markdown content:

```mdx
import { Alert, Button } from '@/components'

# Document Title

Regular markdown content can include interactive components:

<Alert type="info">
This is an interactive alert component embedded in markdown.
</Alert>

<Button onClick={() => alert('Clicked!')}>
Interactive Button
</Button>
```

### Component Props and Data

```mdx
import { ApiReference } from '@/components'

<ApiReference
  endpoint="/api/users"
  method="GET"
  description="Retrieve user information"
  parameters={[
    { name: 'id', type: 'string', required: true },
    { name: 'include', type: 'array', required: false }
  ]}
/>
```

## Tables and Data Presentation

### Enhanced Table Syntax

```markdown
| Feature | Basic | Professional | Enterprise |
|---------|-------|---------------|------------|
| Users | 5 | 50 | Unlimited |
| Storage | 1GB | 10GB | Unlimited |
| Support | Email | Priority | 24/7 Phone |
| API Access | ❌ | ✅ | ✅ |
| Custom Themes | ❌ | ❌ | ✅ |
```

#### Table Alignment

```markdown
| Left Aligned | Center Aligned | Right Aligned |
|:-------------|:--------------:|--------------:|
| Text | Text | Text |
| More text | More text | More text |
```

#### Complex Table Content

```markdown
| Component | Syntax | Example |
|-----------|--------|---------|
| **Bold Text** | `**text**` | **Important** |
| *Italic Text* | `*text*` | *Emphasis* |
| `Code` | `` `code` `` | `function()` |
| [Link](url) | `[text](url)` | [GitHub](https://github.com) |
```

## Mathematical Expressions

### Inline Mathematics

Use single dollar signs for inline mathematical expressions:

```markdown
The formula for compound interest is $A = P(1 + r/n)^{nt}$ where:
- $P$ is the principal amount
- $r$ is the annual interest rate
- $n$ is the number of times interest compounds per year
- $t$ is the time in years
```

### Block Mathematics

Use double dollar signs for block-level mathematical expressions:

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

## Definition Lists

### Term and Definition Syntax

```markdown
Term 1
: Definition for term 1
: Additional definition for term 1

Term 2
: Definition for term 2 with multiple lines
  that continue on the next line
: Second definition for term 2

API Endpoint
: A specific URL where an API can receive requests
: RESTful APIs typically use standard HTTP methods

Authentication
: The process of verifying user identity
: Can be implemented using tokens, sessions, or certificates
```

## Footnotes and Citations

### Footnote Syntax

```markdown
This text contains a footnote reference[^1] and another footnote[^note].

Regular content continues here with additional footnote[^2].

[^1]: This is the first footnote with simple content.

[^note]: This footnote has a custom identifier and can contain:
    - Multiple paragraphs
    - Code blocks
    - Other markdown formatting

[^2]: Short footnote content.
```

### Citation References

```markdown
According to the documentation[^docs2024], the recommended approach
is to use semantic versioning[^semver] for API releases.

[^docs2024]: Smith, J. (2024). *Modern API Documentation Practices*.
Tech Publishers. https://example.com/docs

[^semver]: Semantic Versioning Specification. Retrieved from https://semver.org
```

## Custom Containers and Admonitions

### Basic Admonition Syntax

```markdown
::: info
This is an informational admonition that provides additional context
without being critical to the main content flow.
:::

::: warning
This warning highlights important information that users should
be aware of to avoid potential issues.
:::

::: danger
This danger admonition indicates critical information that could
result in data loss or system damage if ignored.
:::
```

### Titled Admonitions

```markdown
::: tip Custom Tip Title
You can provide custom titles for admonitions to make them more
specific and contextually relevant.
:::

::: note Performance Consideration
When processing large datasets, consider implementing pagination
to improve response times and reduce memory usage.
:::
```

### Nested Content in Admonitions

```markdown
::: warning Database Migration Required

Before upgrading to version 2.0, you must:

1. **Backup your database**
   ```bash
   pg_dump myapp > backup.sql
   ```

2. **Run migration scripts**
   ```bash
   npm run migrate
   ```

3. **Verify data integrity**
   - Check critical tables
   - Validate foreign key relationships
   - Test application functionality

See the [migration guide](./migration.md) for detailed instructions.
:::
```

## Interactive Elements

### Expandable Sections

```markdown
<details>
<summary>Advanced Configuration Options</summary>

This section contains detailed configuration options that are not
commonly needed but may be useful for specific use cases:

- **Database Connection Pooling**: Configure connection limits
- **Caching Strategies**: Redis vs. memory caching options
- **Logging Levels**: Debug, info, warning, error configurations

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

### Tabbed Content

```markdown
::: tabs
== Tab One
Content for the first tab including code examples and explanations.

```javascript
const config = {
  mode: 'production',
  optimization: true
};
```

== Tab Two
Content for the second tab with different implementation:

```python
config = {
    'mode': 'production',
    'optimization': True
}
```

== Tab Three
Additional implementation details and best practices.
:::
```

## Code Enhancement

### Syntax Highlighting with Metadata

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

### Code with Highlighting

````markdown
```javascript {2,4-6}
function processData(input) {
  const validated = validate(input); // highlighted line

  if (validated.isValid) {        // highlighted block start
    return transform(validated.data);
  }                               // highlighted block end

  throw new Error('Invalid input');
}
```
````

### Code Diffs

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

## Media Integration

### Advanced Image Syntax

```markdown
![Alt text](./images/diagram.png "Image title")

<!-- Image with custom attributes -->
<img src="./images/screenshot.png"
     alt="Application screenshot"
     width="600"
     height="400" />

<!-- Responsive images -->
<picture>
  <source media="(min-width: 800px)" srcset="./images/large.png">
  <source media="(min-width: 400px)" srcset="./images/medium.png">
  <img src="./images/small.png" alt="Responsive image">
</picture>
```

### Video Embedding

```markdown
<!-- Local video files -->
<video controls width="100%">
  <source src="./videos/demo.mp4" type="video/mp4">
  <source src="./videos/demo.webm" type="video/webm">
  Your browser does not support video playback.
</video>

<!-- External video embedding -->
<iframe width="560" height="315"
        src="https://www.youtube.com/embed/VIDEO_ID"
        title="Video title"
        frameborder="0"
        allowfullscreen>
</iframe>
```

## Cross-references and Navigation

### Internal Link Enhancement

```markdown
<!-- Standard internal links -->
[Getting Started Guide](../getting-started/installation.md)

<!-- Links with anchors -->
[Configuration Section](./config.md#database-settings)

<!-- Reference-style internal links -->
See the [installation guide][install] for setup instructions.

[install]: ../getting-started/installation.md "Installation Guide"
```

### Automatic Cross-references

```markdown
<!-- These patterns are automatically converted to links -->
See issue #123 for more details.
Refer to commit abc123f for implementation.
Check pull request !456 for code review.
```

## Best Practices for Extended Markdown

### Performance Considerations

- **Component usage**: Use interactive components sparingly for optimal performance
- **Image optimization**: Optimize images before embedding
- **Mathematical expressions**: Limit complex mathematical expressions for faster rendering

### Accessibility Guidelines

- **Alternative text**: Provide meaningful alt text for all images and media
- **Color contrast**: Ensure sufficient contrast in custom styling
- **Keyboard navigation**: Test interactive elements with keyboard navigation
- **Screen reader compatibility**: Verify content works with assistive technologies

### Maintenance and Updates

- **Version compatibility**: Test extended syntax with BaiDocs updates
- **Component dependencies**: Monitor custom component functionality
- **Link validation**: Regularly check internal and external links
- **Content review**: Periodically review and update enhanced content elements

Extended Markdown features in BaiDocs enable creation of rich, interactive documentation that engages users while maintaining professional presentation standards.