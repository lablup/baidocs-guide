# Admonitions and Callouts

Admonitions provide visually distinct ways to highlight important information, warnings, and supplementary content. This section covers the comprehensive admonition system available in BaiDocs.

## Admonition Types

### Informational Admonitions

#### Info
Used for supplementary information that enhances understanding:

```markdown
::: info
Additional context or background information that supports the main content
without being critical to the primary workflow.
:::
```

#### Note
For important details that readers should be aware of:

```markdown
::: note
Important details that provide clarification or additional context
for the current topic or procedure.
:::
```

#### Tip
For helpful suggestions and best practices:

```markdown
::: tip
Helpful suggestions, shortcuts, or best practices that can improve
user experience or efficiency.
:::
```

### Warning Admonitions

#### Warning
For potentially problematic situations:

```markdown
::: warning
Information about potential issues or situations that require
careful attention to avoid problems.
:::
```

#### Caution
For situations requiring careful consideration:

```markdown
::: caution
Situations that require careful consideration or specific precautions
to prevent unintended consequences.
:::
```

#### Danger
For critical warnings about serious risks:

```markdown
::: danger
Critical warnings about actions that could result in data loss,
security vulnerabilities, or system damage.
:::
```

### Specialized Admonitions

#### Example
For demonstrating concepts through practical examples:

```markdown
::: example
Practical demonstrations showing how concepts apply in real-world
scenarios or specific use cases.
:::
```

#### Quote
For significant citations or quotations:

```markdown
::: quote
"Significant quotations from authoritative sources or important
statements that support the documentation content."
:::
```

## Custom Titles

### Basic Custom Titles

Override default titles with specific, contextual headings:

```markdown
::: warning Database Migration Required
Before proceeding with the upgrade, ensure all database migrations
have been applied and tested in a staging environment.
:::

::: tip Performance Optimization
For improved performance with large datasets, consider implementing
query optimization and appropriate indexing strategies.
:::
```

### Multi-word and Special Character Titles

```markdown
::: info API Version 2.0 Changes
The new API version introduces breaking changes that require
code modifications for existing integrations.
:::

::: note Implementation Notes (Advanced)
Advanced implementation details for developers working with
complex integration scenarios.
:::
```

## Content Structure Within Admonitions

### Simple Content

```markdown
::: warning
Single paragraph warnings with straightforward information that
requires immediate attention from readers.
:::
```

### Multi-paragraph Content

```markdown
::: info Project Setup Requirements

The project setup requires several components to be configured properly.
Each component serves a specific purpose in the overall architecture.

Configuration files must be updated to reflect your environment settings.
Testing procedures should verify all components function correctly
before deploying to production environments.
:::
```

### Lists and Structured Content

```markdown
::: tip Best Practices Checklist

Before deploying your documentation:

- **Content review**: Verify all content is accurate and up-to-date
- **Link validation**: Test all internal and external links
- **Image optimization**: Ensure images are properly compressed
- **Accessibility check**: Confirm content meets accessibility standards

Additional considerations:
- Performance testing on various devices
- Cross-browser compatibility verification
- Search engine optimization review
:::
```

### Code Examples in Admonitions

```markdown
::: example Database Configuration

Configure your database connection using environment variables:

```env
DATABASE_URL=postgresql://user:password@localhost:5432/mydb
DATABASE_POOL_SIZE=20
DATABASE_TIMEOUT=5000
```

Implement the connection in your application:

```javascript
const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
  max: process.env.DATABASE_POOL_SIZE,
  idleTimeoutMillis: process.env.DATABASE_TIMEOUT,
});
```
:::
```

### Tables Within Admonitions

```markdown
::: note Version Compatibility Matrix

| BaiDocs Version | Node.js | npm | Operating System |
|----------------|---------|-----|------------------|
| 1.0.x | 16+ | 8+ | macOS, Linux, Windows |
| 1.1.x | 18+ | 9+ | macOS, Linux, Windows |
| 2.0.x | 20+ | 10+ | macOS, Linux, Windows |

Ensure your environment meets these requirements before installation.
:::
```

## Nested and Complex Admonitions

### Sequential Admonitions

```markdown
::: info Prerequisites
Ensure Node.js version 18 or higher is installed on your system.
:::

::: warning Backup Recommendation
Create a backup of your existing configuration before proceeding
with the installation process.
:::

::: tip Installation Optimization
For faster installation, use npm cache and consider using yarn
as an alternative package manager.
:::
```

### Nested Content Structures

```markdown
::: warning Critical Update Procedure

This update requires careful execution of the following steps:

::: info Step 1: Preparation
1. Stop all running services
2. Create database backup
3. Document current configuration
:::

::: info Step 2: Update Process
1. Download latest release
2. Apply configuration changes
3. Test in staging environment
:::

::: danger Final Warning
Do not proceed to production deployment without successful
staging environment testing and validation.
:::

:::
```

## Advanced Styling and Customization

### Custom CSS Classes

```markdown
::: info custom-styling
Content with custom CSS classes applied for specialized formatting
and visual presentation requirements.
:::
```

### HTML Integration

```markdown
::: note
<div class="highlight-box">
<strong>Important:</strong> This procedure cannot be reversed once completed.
Ensure all prerequisites are met before proceeding.
</div>
:::
```

## Accessibility Considerations

### Screen Reader Compatibility

- **Semantic markup**: Admonitions use proper ARIA labels and semantic HTML
- **Clear hierarchy**: Admonition types provide context for assistive technologies
- **Descriptive titles**: Custom titles enhance understanding for screen readers

### Visual Accessibility

- **Color contrast**: All admonition types meet WCAG color contrast requirements
- **Icon usage**: Icons supplement, not replace, textual information
- **Responsive design**: Admonitions adapt to different screen sizes and zoom levels

## Best Practices

### Content Guidelines

#### Appropriate Usage

- **Selective application**: Use admonitions for truly important information
- **Consistent terminology**: Maintain consistent language across similar admonitions
- **Clear context**: Ensure admonitions enhance rather than interrupt content flow
- **Logical placement**: Position admonitions at relevant points in the content

#### Content Quality

```markdown
✅ Good Example:
::: warning Database Connection
Ensure database credentials are configured before starting the application.
Connection failures will prevent proper system initialization.
:::

❌ Poor Example:
::: warning
Something might go wrong with the database if you don't set it up right.
:::
```

### Performance Considerations

#### Efficient Implementation

- **Minimal nesting**: Limit deep nesting of admonitions within admonitions
- **Reasonable length**: Keep admonition content focused and concise
- **Image optimization**: Optimize any images included within admonitions
- **Code formatting**: Use syntax highlighting judiciously in admonitions

### Maintenance and Updates

#### Content Review Process

1. **Regular auditing**: Review admonitions for accuracy and relevance
2. **Version updates**: Update admonitions when software versions change
3. **User feedback**: Monitor user feedback about admonition clarity and usefulness
4. **Consistency checks**: Ensure consistent formatting across all documents

#### Quality Assurance

- **Link validation**: Verify all links within admonitions function correctly
- **Code testing**: Test all code examples included in admonitions
- **Cross-reference verification**: Confirm references to other sections remain accurate
- **Accessibility testing**: Regularly test admonitions with assistive technologies

## Integration with Documentation Workflow

### Editorial Guidelines

#### Review Checklist

- [ ] Admonition type appropriate for content importance
- [ ] Custom title adds clarity and context
- [ ] Content is concise yet comprehensive
- [ ] Formatting enhances readability
- [ ] Links and references are functional

#### Translation Considerations

- **Title translation**: Translate custom titles while maintaining meaning
- **Cultural adaptation**: Consider cultural differences in warning interpretation
- **Length adjustment**: Account for text expansion or contraction in translations
- **Consistency maintenance**: Ensure admonition usage consistency across languages

Effective use of admonitions significantly enhances documentation usability by drawing attention to critical information and providing clear visual hierarchy for different types of content importance.