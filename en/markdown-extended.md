---
title: Extended Markdown Features
description: Advanced markdown features supported in BaiDocs beyond the basics
---

# Extended Markdown Features

BaiDocs supports GitHub Flavored Markdown (GFM) and additional extensions that make your documentation more powerful and expressive.

## Tables

Create tables using pipes and hyphens:

```markdown
| Header 1 | Header 2 | Header 3 |
|----------|----------|----------|
| Row 1, Col 1 | Row 1, Col 2 | Row 1, Col 3 |
| Row 2, Col 1 | Row 2, Col 2 | Row 2, Col 3 |
```

**Rendered:**

| Header 1 | Header 2 | Header 3 |
|----------|----------|----------|
| Row 1, Col 1 | Row 1, Col 2 | Row 1, Col 3 |
| Row 2, Col 1 | Row 2, Col 2 | Row 2, Col 3 |

### Column Alignment

Use colons to align columns:

```markdown
| Left Aligned | Center Aligned | Right Aligned |
|:-------------|:--------------:|--------------:|
| Left         | Center         | Right         |
| Text         | Text           | Text          |
```

**Rendered:**

| Left Aligned | Center Aligned | Right Aligned |
|:-------------|:--------------:|--------------:|
| Left         | Center         | Right         |
| Text         | Text           | Text          |

### Tables with Formatting

You can use inline formatting in tables:

```markdown
| Feature | Status | Description |
|---------|--------|-------------|
| **Bold** | ✅ Complete | *Works great* |
| `Code` | 🚧 In Progress | ~~Old way~~ |
| [Links](/) | ✅ Complete | External links |
```

**Rendered:**

| Feature | Status | Description |
|---------|--------|-------------|
| **Bold** | ✅ Complete | *Works great* |
| `Code` | 🚧 In Progress | ~~Old way~~ |
| [Links](/) | ✅ Complete | External links |

:::tip
Keep tables simple. For complex data, consider using a different format or breaking into multiple tables.
:::

## Task Lists

Create interactive checkboxes (GFM):

```markdown
- [x] Design the feature
- [x] Implement core functionality
- [x] Write tests
- [ ] Write documentation
- [ ] Release to production
```

**Rendered:**

- [x] Design the feature
- [x] Implement core functionality
- [x] Write tests
- [ ] Write documentation
- [ ] Release to production

### Nested Task Lists

Task lists can be nested:

```markdown
- [x] Phase 1: Planning
  - [x] Requirements gathering
  - [x] Architecture design
- [ ] Phase 2: Development
  - [x] Set up environment
  - [ ] Implement features
  - [ ] Code review
- [ ] Phase 3: Testing
  - [ ] Unit tests
  - [ ] Integration tests
```

**Rendered:**

- [x] Phase 1: Planning
  - [x] Requirements gathering
  - [x] Architecture design
- [ ] Phase 2: Development
  - [x] Set up environment
  - [ ] Implement features
  - [ ] Code review
- [ ] Phase 3: Testing
  - [ ] Unit tests
  - [ ] Integration tests

## Strikethrough

Mark text as deleted or outdated (GFM):

```markdown
~~This feature is deprecated.~~

Use ~~the old API~~ the new API instead.
```

**Rendered:**

~~This feature is deprecated.~~

Use ~~the old API~~ the new API instead.

## Autolinks

URLs and email addresses automatically become clickable (GFM):

```markdown
Visit https://github.com/lablup/baidocs

Contact us at support@example.com
```

**Rendered:**

Visit https://github.com/lablup/baidocs

Contact us at support@example.com

## Footnotes

Add footnotes to provide additional context:

```markdown
Here's a statement that needs a reference.[^1]

Here's another one.[^2]

[^1]: This is the first footnote with detailed information.
[^2]: This is the second footnote with more details.
```

**Rendered:**

Here's a statement that needs a reference.[^1]

Here's another one.[^2]

[^1]: This is the first footnote with detailed information.
[^2]: This is the second footnote with more details.

:::info
Footnotes are automatically numbered and linked. Click a footnote number to jump to its definition.
:::

## Definition Lists

Create definition lists with terms and descriptions:

```markdown
Term 1
: Definition for term 1

Term 2
: First definition for term 2
: Second definition for term 2
```

:::note
Definition lists may have limited support depending on the markdown renderer. They work in most cases but test them in your specific setup.
:::

## Emoji

Use emoji shortcodes or direct emoji characters:

```markdown
:smile: :heart: :rocket: :check_mark:

Or use direct emoji: 😀 ❤️ 🚀 ✅
```

**Rendered:**

😀 ❤️ 🚀 ✅

Common emoji for documentation:

| Icon | Usage |
|------|-------|
| ✅ | Completed, working, yes |
| ❌ | Failed, not working, no |
| ⚠️ | Warning, caution |
| ℹ️ | Information |
| 🚧 | In progress, under construction |
| 📝 | Note, documentation |
| 💡 | Tip, idea |
| 🔗 | Link, reference |
| 📦 | Package, release |
| 🐛 | Bug |

## Abbreviations

Define abbreviations that show tooltips on hover:

```markdown
*[HTML]: Hyper Text Markup Language
*[CSS]: Cascading Style Sheets

HTML and CSS are fundamental web technologies.
```

:::note
Abbreviation support depends on the markdown processor. BaiDocs may not fully support interactive tooltips for abbreviations in the current version.
:::

## Keyboard Keys

Show keyboard shortcuts using `<kbd>` tags:

```markdown
Press <kbd>Ctrl</kbd> + <kbd>C</kbd> to copy.

Use <kbd>Cmd</kbd> + <kbd>K</kbd> to open search.

Press <kbd>Esc</kbd> to cancel.
```

**Rendered:**

Press <kbd>Ctrl</kbd> + <kbd>C</kbd> to copy.

Use <kbd>Cmd</kbd> + <kbd>K</kbd> to open search.

Press <kbd>Esc</kbd> to cancel.

## Superscript and Subscript

Use HTML tags for superscript and subscript:

```markdown
E = mc<sup>2</sup>

H<sub>2</sub>O is water.

The answer is x<sup>2</sup> + y<sup>2</sup>.
```

**Rendered:**

E = mc<sup>2</sup>

H<sub>2</sub>O is water.

The answer is x<sup>2</sup> + y<sup>2</sup>.

## Mark (Highlight)

Highlight text with the `<mark>` tag:

```markdown
This is <mark>highlighted text</mark>.

<mark>Important information</mark> should be highlighted.
```

**Rendered:**

This is <mark>highlighted text</mark>.

<mark>Important information</mark> should be highlighted.

## Details and Summary (Collapsible Content)

Create collapsible sections:

```markdown
<details>
<summary>Click to expand</summary>

This content is hidden by default and can be expanded by clicking the summary.

You can include any markdown here:
- Lists
- **Bold text**
- `Code`
- And more!

</details>
```

**Rendered:**

<details>
<summary>Click to expand</summary>

This content is hidden by default and can be expanded by clicking the summary.

You can include any markdown here:
- Lists
- **Bold text**
- `Code`
- And more!

</details>

:::tip
Use collapsible sections for optional or advanced content that might clutter the page if always visible.
:::

## Block Quotes with Citation

Add citations to blockquotes:

```markdown
> The best way to predict the future is to invent it.
>
> — Alan Kay

> Documentation is a love letter that you write to your future self.
>
> — Damian Conway
```

**Rendered:**

> The best way to predict the future is to invent it.
>
> — Alan Kay

> Documentation is a love letter that you write to your future self.
>
> — Damian Conway

## Nested Lists with Mixed Types

Combine ordered and unordered lists:

```markdown
1. First main item
   - Unordered sub-item
   - Another sub-item
     1. Ordered sub-sub-item
     2. Another one
2. Second main item
   - Mixed list item
     - Deeply nested
       - Even deeper
```

**Rendered:**

1. First main item
   - Unordered sub-item
   - Another sub-item
     1. Ordered sub-sub-item
     2. Another one
2. Second main item
   - Mixed list item
     - Deeply nested
       - Even deeper

## Math Equations (LaTeX)

:::note
Math equation rendering is not currently enabled in BaiDocs by default. If you need LaTeX support, you'll need to configure a math rendering plugin like KaTeX or MathJax.
:::

If math rendering is enabled, you can use inline or block math:

```markdown
Inline math: $E = mc^2$

Block math:
$$
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
$$
```

## Alerts (GitHub-style)

GitHub-style alerts using blockquotes with prefixes:

```markdown
> [!NOTE]
> Useful information that users should know.

> [!TIP]
> Helpful advice for doing things better.

> [!IMPORTANT]
> Key information users need to know.

> [!WARNING]
> Urgent info that needs immediate attention.

> [!CAUTION]
> Advises about risks or negative outcomes.
```

:::info
GitHub-style alerts are shown as regular blockquotes in BaiDocs. Use the [Admonitions](admonitions.md) syntax instead for styled callout boxes.
:::

## Tables with Empty Cells

Tables don't need content in every cell:

```markdown
| Name | Value | Description |
|------|-------|-------------|
| foo  | 1     | First item  |
| bar  |       | No value    |
|      | 3     | No name     |
```

**Rendered:**

| Name | Value | Description |
|------|-------|-------------|
| foo  | 1     | First item  |
| bar  |       | No value    |
|      | 3     | No name     |

## Links with Attributes

Add custom attributes to links (if supported):

```markdown
[Link](https://example.com){target="_blank" rel="noopener"}
```

:::note
Link attribute syntax support varies. BaiDocs may not support all markdown extensions for link attributes.
:::

## Multi-paragraph List Items

List items can contain multiple paragraphs:

```markdown
1. First item with multiple paragraphs.

   This is the second paragraph of the first item.

   And here's a third paragraph.

2. Second item.

   With its own additional paragraph.
```

**Rendered:**

1. First item with multiple paragraphs.

   This is the second paragraph of the first item.

   And here's a third paragraph.

2. Second item.

   With its own additional paragraph.

## Code Blocks in Lists

Include code blocks within list items:

```markdown
1. First, install the dependencies:

   ```bash
   pnpm install
   ```

2. Then, start the development server:

   ```bash
   pnpm dev
   ```

3. Finally, open your browser.
```

**Rendered:**

1. First, install the dependencies:

   ```bash
   pnpm install
   ```

2. Then, start the development server:

   ```bash
   pnpm dev
   ```

3. Finally, open your browser.

## Automatic URL Encoding

URLs with special characters are automatically encoded:

```markdown
[Search for "documentation"](https://example.com/search?q=documentation&lang=en)
```

## Best Practices

### 1. Choose the Right Feature

Don't use advanced features just because you can:
- ✅ Use tables for tabular data
- ❌ Don't use tables for layout
- ✅ Use task lists for checklists
- ❌ Don't use them for feature status (use tables)

### 2. Keep Tables Readable

```markdown
<!-- Good: Aligned and readable -->
| Name    | Age | City     |
|---------|-----|----------|
| Alice   | 30  | New York |
| Bob     | 25  | London   |

<!-- Bad: Hard to read in source -->
|Name|Age|City|
|-|-|-|
|Alice|30|New York|
|Bob|25|London|
```

### 3. Use Semantic HTML Sparingly

Prefer Markdown syntax when possible:

```markdown
<!-- Prefer this -->
**Bold text**

<!-- Over this -->
<strong>Bold text</strong>
```

### 4. Test Advanced Features

Not all markdown processors support all features:
- Test in your BaiDocs instance
- Have fallbacks for unsupported features
- Document which features you're using

### 5. Accessibility

Make content accessible:
- Use semantic HTML
- Add alt text to images
- Provide text alternatives for complex tables
- Use proper heading hierarchy

## Feature Support Matrix

| Feature | Markdown | GFM | BaiDocs |
|---------|----------|-----|---------|
| Basic syntax | ✅ | ✅ | ✅ |
| Tables | ❌ | ✅ | ✅ |
| Task lists | ❌ | ✅ | ✅ |
| Strikethrough | ❌ | ✅ | ✅ |
| Autolinks | ❌ | ✅ | ✅ |
| Footnotes | ❌ | ❌ | ✅ |
| Admonitions | ❌ | ❌ | ✅ |
| Syntax highlighting | ❌ | ✅ | ✅ |
| Math (LaTeX) | ❌ | ❌ | ⚠️ (configurable) |

## Next Steps

Ready to learn about one of BaiDocs' most powerful features? Check out [Admonitions](admonitions.md) for beautiful callout boxes!
