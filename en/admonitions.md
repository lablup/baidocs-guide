---
title: Admonitions
description: Learn how to use admonitions (callout boxes) in BaiDocs
---

# Admonitions

Admonitions (also called callouts or alerts) are special styled boxes that highlight important information. BaiDocs supports beautiful, Obsidian-inspired admonitions.

## What are Admonitions?

Admonitions draw attention to important information that deserves special emphasis. They're perfect for:

- Notes and tips
- Warnings and cautions
- Success messages
- Important information
- Errors and dangers

## Basic Syntax

Admonitions use the **directive** syntax with three colons:

```markdown
:::type
Content goes here
:::
```

The `type` determines the style and color of the admonition.

## Available Types

### Note

Use `note` for general information and clarifications:

```markdown
:::note
This is a note admonition. Use it for supplementary information.
:::
```

**Rendered:**

:::note
This is a note admonition. Use it for supplementary information.
:::

### Info

Use `info` for informational content:

```markdown
:::info
This is an info admonition. Use it for informational messages.
:::
```

**Rendered:**

:::info
This is an info admonition. Use it for informational messages.
:::

### Tip

Use `tip` for helpful suggestions:

```markdown
:::tip
This is a tip admonition. Use it for helpful advice and best practices.
:::
```

**Rendered:**

:::tip
This is a tip admonition. Use it for helpful advice and best practices.
:::

### Hint

`hint` is an alias for `tip`:

```markdown
:::hint
This is a hint admonition. It looks the same as a tip.
:::
```

**Rendered:**

:::hint
This is a hint admonition. It looks the same as a tip.
:::

### Important

Use `important` for critical information:

```markdown
:::important
This is an important admonition. Use it for critical information users must know.
:::
```

**Rendered:**

:::important
This is an important admonition. Use it for critical information users must know.
:::

### Success

Use `success` for positive outcomes and confirmations:

```markdown
:::success
This is a success admonition. Use it to confirm successful operations.
:::
```

**Rendered:**

:::success
This is a success admonition. Use it to confirm successful operations.
:::

### Warning

Use `warning` for important cautions:

```markdown
:::warning
This is a warning admonition. Use it for important warnings and cautions.
:::
```

**Rendered:**

:::warning
This is a warning admonition. Use it for important warnings and cautions.
:::

### Caution

`caution` is an alias for `warning`:

```markdown
:::caution
This is a caution admonition. It looks the same as a warning.
:::
```

**Rendered:**

:::caution
This is a caution admonition. It looks the same as a warning.
:::

### Attention

`attention` is another alias for `warning`:

```markdown
:::attention
This is an attention admonition. Use it to grab the reader's attention.
:::
```

**Rendered:**

:::attention
This is an attention admonition. Use it to grab the reader's attention.
:::

### Danger

Use `danger` for critical warnings:

```markdown
:::danger
This is a danger admonition. Use it for critical warnings about dangerous operations.
:::
```

**Rendered:**

:::danger
This is a danger admonition. Use it for critical warnings about dangerous operations.
:::

### Error

`error` is an alias for `danger`:

```markdown
:::error
This is an error admonition. Use it to highlight errors and critical issues.
:::
```

**Rendered:**

:::error
This is an error admonition. Use it to highlight errors and critical issues.
:::

## Color Reference

Each admonition type has its own color scheme:

| Type | Color | Use Case |
|------|-------|----------|
| **note** | Light Blue | General notes and information |
| **info** | Cyan | Informational content |
| **tip, hint, success** | Green | Helpful advice, successful outcomes |
| **important** | Purple | Critical information |
| **warning, caution, attention** | Orange | Important warnings |
| **danger, error** | Red | Critical warnings, errors |

## Advanced Usage

### Multiple Paragraphs

Admonitions can contain multiple paragraphs:

```markdown
:::tip
This is the first paragraph with some helpful advice.

This is a second paragraph with more details.

And here's a third paragraph!
:::
```

**Rendered:**

:::tip
This is the first paragraph with some helpful advice.

This is a second paragraph with more details.

And here's a third paragraph!
:::

### Formatted Content

Use any markdown formatting inside admonitions:

```markdown
:::note
You can use **bold**, *italic*, and `code` inside admonitions.

- Lists work too
- Multiple items
- With sub-items
  - Like this
  - And this

Even [links](https://example.com) work!
:::
```

**Rendered:**

:::note
You can use **bold**, *italic*, and `code` inside admonitions.

- Lists work too
- Multiple items
- With sub-items
  - Like this
  - And this

Even [links](https://example.com) work!
:::

### Code Blocks

Include code blocks in admonitions:

```markdown
:::tip
Here's how to start the development server:

\`\`\`bash
pnpm dev
\`\`\`

This will start both the editor and viewer.
:::
```

**Rendered:**

:::tip
Here's how to start the development server:

```bash
pnpm dev
```

This will start both the editor and viewer.
:::

### Tables

Even tables work inside admonitions:

```markdown
:::info
Here are the available commands:

| Command | Description |
|---------|-------------|
| `pnpm dev` | Start development |
| `pnpm build` | Build for production |
| `pnpm start` | Start production server |
:::
```

**Rendered:**

:::info
Here are the available commands:

| Command | Description |
|---------|-------------|
| `pnpm dev` | Start development |
| `pnpm build` | Build for production |
| `pnpm start` | Start production server |
:::

### Task Lists

Task lists work great in admonitions:

```markdown
:::success
Completed features:

- [x] Admonition support
- [x] Multiple types
- [x] Syntax highlighting
- [x] Nested content
- [ ] Custom titles (coming soon)
:::
```

**Rendered:**

:::success
Completed features:

- [x] Admonition support
- [x] Multiple types
- [x] Syntax highlighting
- [x] Nested content
- [ ] Custom titles (coming soon)
:::

### Images

You can include images in admonitions:

```markdown
:::tip
Here's what the interface looks like:

![BaiDocs interface](images/interface.png)
:::
```

:::note
Make sure images are in the `images/` directory and use relative paths.
:::

## Best Practices

### 1. Choose the Right Type

Use the appropriate admonition type for your content:

```markdown
<!-- Good -->
:::tip
Use Ctrl+K to open search quickly!
:::

:::warning
Deleting this action cannot be undone.
:::

<!-- Less clear -->
:::note
Use Ctrl+K to open search quickly!
:::

:::info
Deleting this action cannot be undone.
:::
```

### 2. Don't Overuse

Too many admonitions make your page cluttered:

```markdown
<!-- Bad: Too many admonitions -->
:::note
First note
:::

Some text

:::tip
A tip
:::

More text

:::warning
A warning
:::

<!-- Better: Use sparingly -->
Regular paragraph with important information.

:::warning
Critical warning about a dangerous operation.
:::

Continue with regular content.
```

### 3. Keep Content Focused

Each admonition should have one clear message:

```markdown
<!-- Good: Focused message -->
:::tip
Use `pnpm` instead of `npm` for faster installs.
:::

<!-- Bad: Too much information -->
:::tip
Use `pnpm` instead of `npm` for faster installs. Also, make sure
you have Node.js 18 or higher. You might also want to consider
using Docker for deployment. Speaking of which, don't forget to
set up your environment variables...
:::
```

### 4. Use Appropriate Length

Keep admonitions concise:

- ✅ 1-3 sentences for simple notes
- ✅ Up to a short paragraph for tips
- ✅ Multiple paragraphs only when necessary

### 5. Consider Placement

Place admonitions where they're most relevant:

```markdown
<!-- Good: Warning before dangerous action -->
:::warning
This action will delete all your data. Make sure you have a backup!
:::

Run the following command:
\`\`\`bash
rm -rf data/
\`\`\`

<!-- Bad: Warning after the action -->
Run the following command:
\`\`\`bash
rm -rf data/
\`\`\`

:::warning
This action will delete all your data. Make sure you have a backup!
:::
```

## Common Use Cases

### Installation Instructions

```markdown
:::note
This guide assumes you have Node.js 18 or higher installed.
:::

To install BaiDocs, run:
\`\`\`bash
pnpm install
\`\`\`
```

**Rendered:**

:::note
This guide assumes you have Node.js 18 or higher installed.
:::

To install BaiDocs, run:
```bash
pnpm install
```

### Configuration Tips

```markdown
:::tip
For better performance in development, disable PDF generation:
\`\`\`bash
ENABLE_PDF_GENERATION=false pnpm dev
\`\`\`
:::
```

**Rendered:**

:::tip
For better performance in development, disable PDF generation:
```bash
ENABLE_PDF_GENERATION=false pnpm dev
```
:::

### Breaking Changes

```markdown
:::danger
**Breaking Change in v2.0**

The API endpoint has changed from `/api/v1/` to `/api/v2/`.
Update your code accordingly.
:::
```

**Rendered:**

:::danger
**Breaking Change in v2.0**

The API endpoint has changed from `/api/v1/` to `/api/v2/`.
Update your code accordingly.
:::

### Prerequisites

```markdown
:::important
Before proceeding, ensure you have:
- [x] Node.js 18+
- [x] pnpm installed
- [x] Git configured
:::
```

**Rendered:**

:::important
Before proceeding, ensure you have:
- [x] Node.js 18+
- [x] pnpm installed
- [x] Git configured
:::

### Troubleshooting

```markdown
:::error
**Error: Port 3000 already in use**

If you see this error, another process is using port 3000.

Solution:
\`\`\`bash
lsof -ti:3000 | xargs kill -9
\`\`\`
:::
```

**Rendered:**

:::error
**Error: Port 3000 already in use**

If you see this error, another process is using port 3000.

Solution:
```bash
lsof -ti:3000 | xargs kill -9
```
:::

## Accessibility

Admonitions are designed to be accessible:

- **Visual distinction**: Different colors for different types
- **Semantic HTML**: Proper HTML structure
- **Screen reader friendly**: Clear content hierarchy
- **Keyboard navigation**: Fully accessible via keyboard

## Comparison with Other Tools

Different documentation tools call them different things:

| Tool | Name | Syntax |
|------|------|--------|
| **BaiDocs** | Admonitions | `:::type content :::` |
| **Docusaurus** | Admonitions | `:::type content :::` |
| **MkDocs** | Admonitions | `!!! type "Title"` |
| **GitHub** | Alerts | `> [!NOTE]` |
| **Obsidian** | Callouts | `> [!type]` |

BaiDocs uses the **remark-directive** syntax, which is widely supported and clean to write.

## Quick Reference

```markdown
:::note
General information
:::

:::info
Informational content
:::

:::tip
Helpful advice
:::

:::success
Positive outcomes
:::

:::warning
Important cautions
:::

:::danger
Critical warnings
:::
```

## Next Steps

Now let's explore [Code Blocks](code-blocks.md) and learn how to showcase code with beautiful syntax highlighting!
