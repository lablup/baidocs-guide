---
title: Basic Markdown Syntax
description: Learn the fundamental markdown syntax supported in BaiDocs
---

# Basic Markdown Syntax

BaiDocs supports standard Markdown syntax with GitHub Flavored Markdown (GFM) extensions. This page covers all the basics you need to know.

## Headings

Create headings using `#` symbols:

```markdown
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6
```

**Rendered:**

# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6

:::tip
Use only one H1 (`#`) per page - it should be your page title. Start content headings from H2 (`##`).
:::

## Paragraphs

Paragraphs are created by separating text with blank lines:

```markdown
This is the first paragraph.

This is the second paragraph.
```

**Rendered:**

This is the first paragraph.

This is the second paragraph.

## Emphasis

### Bold

Use double asterisks or underscores:

```markdown
**This is bold text**
__This is also bold__
```

**Rendered:**

**This is bold text**
__This is also bold__

### Italic

Use single asterisks or underscores:

```markdown
*This is italic text*
_This is also italic_
```

**Rendered:**

*This is italic text*
_This is also italic_

### Bold and Italic

Combine them:

```markdown
***Bold and italic***
___Also bold and italic___
**Bold with *italic* inside**
```

**Rendered:**

***Bold and italic***
___Also bold and italic___
**Bold with *italic* inside**

### Strikethrough

Use double tildes (GFM):

```markdown
~~This text is struck through~~
```

**Rendered:**

~~This text is struck through~~

## Lists

### Unordered Lists

Use `-`, `*`, or `+`:

```markdown
- First item
- Second item
- Third item
  - Nested item
  - Another nested item
- Fourth item
```

**Rendered:**

- First item
- Second item
- Third item
  - Nested item
  - Another nested item
- Fourth item

### Ordered Lists

Use numbers followed by periods:

```markdown
1. First item
2. Second item
3. Third item
   1. Nested item
   2. Another nested item
4. Fourth item
```

**Rendered:**

1. First item
2. Second item
3. Third item
   1. Nested item
   2. Another nested item
4. Fourth item

:::info
The actual numbers don't matter - Markdown will automatically number them correctly. You can use `1.` for all items!
:::

### Task Lists

Use `- [ ]` for unchecked and `- [x]` for checked (GFM):

```markdown
- [x] Completed task
- [x] Another completed task
- [ ] Incomplete task
- [ ] Another incomplete task
```

**Rendered:**

- [x] Completed task
- [x] Another completed task
- [ ] Incomplete task
- [ ] Another incomplete task

## Links

### Inline Links

```markdown
[Link text](https://example.com)
[Link with title](https://example.com "Hover text")
```

**Rendered:**

[Link text](https://example.com)
[Link with title](https://example.com "Hover text")

### Reference Links

```markdown
[Link text][reference]

[reference]: https://example.com "Optional title"
```

**Rendered:**

[Link text][reference]

[reference]: https://example.com "Optional title"

### Automatic Links

URLs and emails are automatically linked (GFM):

```markdown
https://example.com
user@example.com
```

**Rendered:**

https://example.com
user@example.com

### Internal Links

Link to other pages in your book:

```markdown
[Installation guide](installation.md)
[Section on this page](#headings)
[Another book](../other-book/page.md)
```

:::tip
Use relative paths for internal links. BaiDocs will resolve them correctly.
:::

## Images

### Basic Image Syntax

```markdown
![Alt text](images/filename.png)
![Alt text with title](images/filename.png "Image title")
```

### Image with Link

```markdown
[![Alt text](images/filename.png)](https://example.com)
```

### HTML Image Tag

For more control:

```markdown
<img src="images/filename.png" alt="Alt text" width="500" />
```

:::info
Images should be placed in the `images/` directory of your book. See [Images and Media](images-media.md) for more details.
:::

## Inline Code

Use single backticks:

```markdown
Use the `git status` command to check status.
The `useState` hook is fundamental in React.
```

**Rendered:**

Use the `git status` command to check status.
The `useState` hook is fundamental in React.

## Code Blocks

### Fenced Code Blocks

Use triple backticks with optional language:

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

### Indented Code Blocks

Indent with 4 spaces:

```markdown
    def hello():
        print("Hello, World!")
```

**Rendered:**

    def hello():
        print("Hello, World!")

:::tip
Fenced code blocks with language tags are preferred because they enable syntax highlighting.
:::

## Blockquotes

Use `>` for blockquotes:

```markdown
> This is a blockquote.
> It can span multiple lines.
>
> And multiple paragraphs.

> Blockquotes can also be nested:
> > This is a nested blockquote.
```

**Rendered:**

> This is a blockquote.
> It can span multiple lines.
>
> And multiple paragraphs.

> Blockquotes can also be nested:
> > This is a nested blockquote.

## Horizontal Rules

Create with three or more hyphens, asterisks, or underscores:

```markdown
---

***

___
```

**Rendered:**

---

## Line Breaks

### Hard Break

End a line with two spaces or a backslash:

```markdown
First line
Second line

First line\
Second line
```

**Rendered:**

First line
Second line

First line\
Second line

### Paragraph Break

Use a blank line:

```markdown
First paragraph

Second paragraph
```

## Escaping Characters

Use backslash to escape special characters:

```markdown
\*Not italic\*
\[Not a link\]
\`Not code\`
```

**Rendered:**

\*Not italic\*
\[Not a link\]
\`Not code\`

## HTML

Markdown supports inline HTML:

```markdown
This is <span style="color: red;">red text</span>.

<div style="background: #f0f0f0; padding: 10px;">
This is a custom div.
</div>
```

**Rendered:**

This is <span style="color: red;">red text</span>.

<div style="background: #f0f0f0; padding: 10px;">
This is a custom div.
</div>

:::warning
While HTML is supported, use it sparingly. Prefer Markdown syntax for better maintainability and portability.
:::

## Comments

HTML comments won't appear in rendered output:

```markdown
<!-- This is a comment and won't be visible -->
```

<!-- This is a comment and won't be visible -->

## Special Characters

These characters have special meaning in Markdown:

```
\ ` * _ { } [ ] ( ) # + - . ! | ~
```

Escape them with backslash when you want them literal:

```markdown
\* Not a bullet point
\# Not a heading
```

## Frontmatter

Every page should start with YAML frontmatter:

```markdown
---
title: Page Title
description: Page description for search and SEO
author: Your Name
date: 2024-01-01
tags: [markdown, documentation, tutorial]
---

# Page content starts here
```

**Supported fields:**
- `title` - Page title (required)
- `description` - Brief description
- `author` - Content author
- `date` - Creation/update date
- `tags` - Array of tags
- Any custom fields you need

:::info
Frontmatter is parsed but currently only `title` and `description` are actively used by BaiDocs. Custom fields are available for future features.
:::

## Best Practices

### 1. Be Consistent

Choose one style and stick with it:
- Use `-` for unordered lists (not mixing with `*` or `+`)
- Use `**bold**` consistently (not `__bold__`)
- Consistent heading hierarchy

### 2. Use Semantic Structure

```markdown
# Page Title (H1) - Only one per page

## Major Section (H2)

### Subsection (H3)

#### Minor Section (H4)
```

### 3. Add Alt Text to Images

Always provide meaningful alt text:

```markdown
<!-- Good -->
![BaiDocs architecture diagram showing editor and viewer components](images/architecture.png)

<!-- Bad -->
![image](images/img1.png)
```

### 4. Use Descriptive Link Text

```markdown
<!-- Good -->
Learn more about [Markdown syntax](https://www.markdownguide.org)

<!-- Bad -->
Learn more [here](https://www.markdownguide.org)
```

### 5. Break Long Lines

Keep markdown source readable:

```markdown
<!-- Good -->
This is a long paragraph that should be broken into multiple lines
in the source for better readability. Each line should be around
80 characters or less.

<!-- Harder to read -->
This is a long paragraph that goes on and on without any line breaks which makes it harder to read in the source code and harder to track changes in version control.
```

## Quick Reference

| Element | Syntax |
|---------|--------|
| Heading 1 | `# H1` |
| Heading 2 | `## H2` |
| Bold | `**bold**` |
| Italic | `*italic*` |
| Strikethrough | `~~strikethrough~~` |
| Blockquote | `> quote` |
| Code | `` `code` `` |
| Link | `[text](url)` |
| Image | `![alt](url)` |
| Unordered List | `- item` |
| Ordered List | `1. item` |
| Task List | `- [ ] task` |
| Horizontal Rule | `---` |
| Code Block | ` ```language ` |

## Next Steps

Now that you know the basics, learn about [Extended Markdown Features](markdown-extended.md) available in BaiDocs!
