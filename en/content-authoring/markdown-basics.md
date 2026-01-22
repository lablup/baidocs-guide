# Markdown Fundamentals

Markdown serves as the foundation for all content creation in BaiDocs. This section covers essential Markdown syntax and best practices for technical documentation.

## Document Structure

### Headers and Hierarchy

Use headers to establish document structure and navigation hierarchy:

```markdown
# H1 - Document Title
## H2 - Major Sections
### H3 - Subsections
#### H4 - Detailed Topics
##### H5 - Specific Points
###### H6 - Fine Details
```

#### Header Guidelines

- **Single H1**: Use only one H1 per document for the main title
- **Logical progression**: Follow sequential header levels without skipping
- **Descriptive titles**: Use clear, descriptive header text
- **Consistent formatting**: Maintain uniform header style across documents

### Paragraph Structure

```markdown
Regular paragraphs are created by separating text with blank lines.
This text continues the same paragraph.

This text starts a new paragraph after the blank line.

For better readability, keep paragraphs focused on a single concept
or idea. Break long paragraphs into smaller, digestible sections.
```

## Text Formatting

### Basic Text Styling

```markdown
**Bold text** for emphasis and importance
*Italic text* for subtle emphasis or technical terms
***Bold and italic*** for maximum emphasis
~~Strikethrough~~ for deprecated or incorrect information
```

### Inline Code and Technical Terms

```markdown
Use `inline code` for:
- Function names: `getUserData()`
- File names: `config.yaml`
- Command line tools: `npm install`
- Variable names: `API_KEY`
- Short code snippets: `const result = true`
```

## Lists and Organization

### Unordered Lists

```markdown
- Primary item
- Secondary item
  - Nested item
  - Another nested item
    - Deeply nested item
- Back to primary level

Alternative syntax:
* Primary item
* Secondary item
+ Mixed bullet styles (not recommended)
```

### Ordered Lists

```markdown
1. First step in process
2. Second step in sequence
   1. Substep one
   2. Substep two
3. Third step continues
4. Final step

Alternative numbering:
1. First item
1. Second item (auto-numbered)
1. Third item (auto-numbered)
```

### Task Lists

```markdown
- [x] Completed task
- [ ] Incomplete task
- [x] Another completed task
- [ ] Future task
```

## Links and References

### Basic Link Syntax

```markdown
[Link text](https://example.com)
[Link with title](https://example.com "Hover title")
[Relative link](../other-section/page.md)
[Internal anchor](#section-header)
```

### Reference-style Links

```markdown
This paragraph contains [reference link][ref1] and [another reference][ref2].

[ref1]: https://example.com "Reference title"
[ref2]: ./local-file.md "Local file reference"
```

### Auto-linking

```markdown
Direct URLs: https://example.com
Email addresses: contact@company.com
```

## Blockquotes and Callouts

### Basic Blockquotes

```markdown
> This is a blockquote for important information
> that spans multiple lines and provides
> additional context or citations.

> Nested blockquotes
>> Can be created with additional
>> greater-than symbols.
```

### Citation Format

```markdown
> "Documentation is a love letter that you write to your future self."
>
> — Damian Conway, Software Developer
```

## Line Breaks and Spacing

### Hard Line Breaks

```markdown
First line with two trailing spaces
Second line with hard break

Alternative method using backslash\
Creates hard break without spaces
```

### Horizontal Rules

```markdown
Three or more hyphens:
---

Three or more asterisks:
***

Three or more underscores:
___
```

## Escaping Special Characters

### Character Escaping

```markdown
\*Not italic text\*
\[Not a link\]
\`Not inline code\`
\# Not a header

Use backslash (\) to escape:
* Asterisks
_ Underscores
# Hash symbols
` Backticks
[ ] Square brackets
( ) Parentheses
```

### HTML Character Entities

```markdown
&lt; Less than
&gt; Greater than
&amp; Ampersand
&quot; Quote marks
&copy; Copyright symbol
```

## Document Metadata

### Front Matter (YAML)

```markdown
---
title: Document Title
description: Brief document description
author: Author Name
date: 2026-01-20
tags:
  - documentation
  - guide
  - markdown
---

# Document Content Begins Here
```

### Inline Metadata

```markdown
<!-- Document metadata as comments -->
<!-- Last updated: 2026-01-20 -->
<!-- Reviewer: John Doe -->
<!-- Version: 1.2 -->
```

## Best Practices

### Consistency Guidelines

#### Formatting Standards

- **Headers**: Use sentence case for headers (not title case)
- **Lists**: Use hyphens (-) for unordered lists consistently
- **Emphasis**: Reserve bold for critical information, italic for definitions
- **Links**: Use descriptive link text, avoid "click here" or "read more"

#### Content Organization

- **Single topic per section**: Keep sections focused on specific concepts
- **Logical flow**: Organize content from general to specific
- **Clear transitions**: Use connecting sentences between sections
- **Scannable structure**: Use headers and lists for easy scanning

### Accessibility Considerations

#### Screen Reader Compatibility

- **Semantic headers**: Use proper header hierarchy for screen readers
- **Alt text**: Provide meaningful alt text for images (covered in media section)
- **Link context**: Ensure link text is meaningful without surrounding context
- **List structure**: Use proper list markup for navigation aids

#### International Considerations

- **Language specification**: Include language codes in document metadata
- **Cultural sensitivity**: Consider cultural differences in examples and references
- **Translation preparation**: Write clear, unambiguous text for translation
- **Unicode support**: Use proper Unicode characters for international content

### Performance Optimization

#### Document Length

- **Reasonable size**: Keep individual documents under 10,000 words
- **Logical breaks**: Split long content into multiple related documents
- **Cross-references**: Use links to connect related content across documents

#### File Organization

- **Descriptive names**: Use clear, descriptive file names
- **Consistent structure**: Maintain uniform directory organization
- **Version control**: Follow version control best practices for content changes

### Quality Assurance

#### Content Review Process

1. **Grammar and spelling**: Use automated tools and manual review
2. **Link validation**: Verify all internal and external links function correctly
3. **Format consistency**: Ensure uniform formatting across documents
4. **Technical accuracy**: Verify all code examples and instructions

#### Maintenance Procedures

- **Regular updates**: Schedule periodic content reviews and updates
- **Link checking**: Implement automated link validation
- **Deprecation notices**: Mark outdated content clearly
- **Version tracking**: Maintain change logs for significant updates

Mastering these Markdown fundamentals provides the foundation for creating clear, professional technical documentation in BaiDocs.