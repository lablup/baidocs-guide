---
title: Tables and Lists
description: Master tables and lists for organizing content in BaiDocs
---

# Tables and Lists

Tables and lists are essential for organizing information. BaiDocs provides full support for GitHub Flavored Markdown tables and various list types.

## Tables

### Basic Table Syntax

Create tables using pipes `|` and hyphens `-`:

```markdown
| Header 1 | Header 2 | Header 3 |
|----------|----------|----------|
| Cell 1   | Cell 2   | Cell 3   |
| Cell 4   | Cell 5   | Cell 6   |
```

**Rendered:**

| Header 1 | Header 2 | Header 3 |
|----------|----------|----------|
| Cell 1   | Cell 2   | Cell 3   |
| Cell 4   | Cell 5   | Cell 6   |

### Column Alignment

Use colons to align columns:

```markdown
| Left | Center | Right |
|:-----|:------:|------:|
| L1   | C1     | R1    |
| L2   | C2     | R2    |
```

**Rendered:**

| Left | Center | Right |
|:-----|:------:|------:|
| L1   | C1     | R1    |
| L2   | C2     | R2    |

- `:---` - Left aligned (default)
- `:---:` - Center aligned
- `---:` - Right aligned

### Inline Formatting

Tables support markdown formatting in cells:

```markdown
| Feature | Status | Notes |
|---------|--------|-------|
| **Search** | ✅ | *Working great* |
| `PDF Export` | ✅ | Ready to use |
| ~~Old Feature~~ | ❌ | Deprecated |
| [Documentation](/docs) | 📝 | In progress |
```

**Rendered:**

| Feature | Status | Notes |
|---------|--------|-------|
| **Search** | ✅ | *Working great* |
| `PDF Export` | ✅ | Ready to use |
| ~~Old Feature~~ | ❌ | Deprecated |
| [Documentation](/docs) | 📝 | In progress |

### Wide Tables

For wide tables, they automatically scroll horizontally:

```markdown
| Column 1 | Column 2 | Column 3 | Column 4 | Column 5 | Column 6 | Column 7 |
|----------|----------|----------|----------|----------|----------|----------|
| Data 1   | Data 2   | Data 3   | Data 4   | Data 5   | Data 6   | Data 7   |
| More 1   | More 2   | More 3   | More 4   | More 5   | More 6   | More 7   |
```

**Rendered:**

| Column 1 | Column 2 | Column 3 | Column 4 | Column 5 | Column 6 | Column 7 |
|----------|----------|----------|----------|----------|----------|----------|
| Data 1   | Data 2   | Data 3   | Data 4   | Data 5   | Data 6   | Data 7   |
| More 1   | More 2   | More 3   | More 4   | More 5   | More 6   | More 7   |

### Empty Cells

Cells can be empty:

```markdown
| Name | Value | Description |
|------|-------|-------------|
| foo  | 123   | First item  |
| bar  |       | No value    |
|      | 456   | No name     |
```

**Rendered:**

| Name | Value | Description |
|------|-------|-------------|
| foo  | 123   | First item  |
| bar  |       | No value    |
|      | 456   | No name     |

## Lists

### Unordered Lists

Use `-`, `*`, or `+` for bullets:

```markdown
- First item
- Second item
- Third item
```

**Rendered:**

- First item
- Second item
- Third item

### Ordered Lists

Use numbers followed by periods:

```markdown
1. First item
2. Second item
3. Third item
```

**Rendered:**

1. First item
2. Second item
3. Third item

:::tip
You can use `1.` for all items - Markdown will auto-number them:

```markdown
1. First
1. Second
1. Third
```
:::

### Nested Lists

Indent with 2 or 4 spaces to nest:

```markdown
- Level 1
  - Level 2
    - Level 3
      - Level 4
```

**Rendered:**

- Level 1
  - Level 2
    - Level 3
      - Level 4

### Mixed Lists

Combine ordered and unordered:

```markdown
1. First ordered item
   - Unordered sub-item
   - Another sub-item
2. Second ordered item
   1. Ordered sub-item
   2. Another ordered sub-item
3. Third ordered item
```

**Rendered:**

1. First ordered item
   - Unordered sub-item
   - Another sub-item
2. Second ordered item
   1. Ordered sub-item
   2. Another ordered sub-item
3. Third ordered item

### Task Lists

Create checkboxes with `- [ ]` and `- [x]`:

```markdown
- [x] Completed task
- [x] Another done task
- [ ] Pending task
- [ ] Another pending task
```

**Rendered:**

- [x] Completed task
- [x] Another done task
- [ ] Pending task
- [ ] Another pending task

### Nested Task Lists

Task lists can be nested:

```markdown
- [x] Phase 1: Planning
  - [x] Requirements
  - [x] Design
  - [x] Approval
- [ ] Phase 2: Development
  - [x] Set up project
  - [ ] Implement features
    - [x] Feature A
    - [ ] Feature B
    - [ ] Feature C
  - [ ] Write tests
- [ ] Phase 3: Deployment
  - [ ] Build for production
  - [ ] Deploy to staging
  - [ ] Deploy to production
```

**Rendered:**

- [x] Phase 1: Planning
  - [x] Requirements
  - [x] Design
  - [x] Approval
- [ ] Phase 2: Development
  - [x] Set up project
  - [ ] Implement features
    - [x] Feature A
    - [ ] Feature B
    - [ ] Feature C
  - [ ] Write tests
- [ ] Phase 3: Deployment
  - [ ] Build for production
  - [ ] Deploy to staging
  - [ ] Deploy to production

## Advanced List Features

### Multi-paragraph List Items

Add paragraphs to list items with proper indentation:

```markdown
1. First item with multiple paragraphs.

   This is the second paragraph of the first item. It must be indented
   to be part of the same list item.

   And here's a third paragraph.

2. Second item.

   With its own additional paragraph.
```

**Rendered:**

1. First item with multiple paragraphs.

   This is the second paragraph of the first item. It must be indented
   to be part of the same list item.

   And here's a third paragraph.

2. Second item.

   With its own additional paragraph.

### Code Blocks in Lists

Include code blocks within list items:

```markdown
1. Install dependencies:

   \`\`\`bash
   pnpm install
   \`\`\`

2. Start the server:

   \`\`\`bash
   pnpm dev
   \`\`\`

3. Open browser to http://localhost:3000
```

**Rendered:**

1. Install dependencies:

   ```bash
   pnpm install
   ```

2. Start the server:

   ```bash
   pnpm dev
   ```

3. Open browser to http://localhost:3000

### Blockquotes in Lists

Add blockquotes to list items:

```markdown
- Regular list item

- List item with quote:

  > This is a blockquote inside a list item.
  > It can have multiple lines.

- Another regular item
```

**Rendered:**

- Regular list item

- List item with quote:

  > This is a blockquote inside a list item.
  > It can have multiple lines.

- Another regular item

## Table Examples

### Feature Comparison

```markdown
| Feature | Free | Pro | Enterprise |
|---------|:----:|:---:|:----------:|
| Users | 5 | 50 | Unlimited |
| Storage | 10 GB | 100 GB | Unlimited |
| Support | Email | Email + Chat | 24/7 Dedicated |
| API Access | ❌ | ✅ | ✅ |
| Custom Domain | ❌ | ✅ | ✅ |
| SSO | ❌ | ❌ | ✅ |
```

**Rendered:**

| Feature | Free | Pro | Enterprise |
|---------|:----:|:---:|:----------:|
| Users | 5 | 50 | Unlimited |
| Storage | 10 GB | 100 GB | Unlimited |
| Support | Email | Email + Chat | 24/7 Dedicated |
| API Access | ❌ | ✅ | ✅ |
| Custom Domain | ❌ | ✅ | ✅ |
| SSO | ❌ | ❌ | ✅ |

### Command Reference

```markdown
| Command | Description | Example |
|---------|-------------|---------|
| `pnpm install` | Install dependencies | `pnpm install` |
| `pnpm dev` | Start development | `pnpm dev` |
| `pnpm build` | Build for production | `pnpm build` |
| `pnpm start` | Start production server | `pnpm start` |
```

**Rendered:**

| Command | Description | Example |
|---------|-------------|---------|
| `pnpm install` | Install dependencies | `pnpm install` |
| `pnpm dev` | Start development | `pnpm dev` |
| `pnpm build` | Build for production | `pnpm build` |
| `pnpm start` | Start production server | `pnpm start` |

### API Endpoints

```markdown
| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/users` | List all users |
| `GET` | `/api/users/:id` | Get user by ID |
| `POST` | `/api/users` | Create new user |
| `PUT` | `/api/users/:id` | Update user |
| `DELETE` | `/api/users/:id` | Delete user |
```

**Rendered:**

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/users` | List all users |
| `GET` | `/api/users/:id` | Get user by ID |
| `POST` | `/api/users` | Create new user |
| `PUT` | `/api/users/:id` | Update user |
| `DELETE` | `/api/users/:id` | Delete user |

## List Examples

### Installation Steps

```markdown
1. **Prerequisites**
   - Node.js 18 or higher
   - pnpm package manager
   - Git

2. **Clone Repository**
   ```bash
   git clone https://github.com/lablup/baidocs.git
   cd baidocs
   ```

3. **Install Dependencies**
   ```bash
   pnpm install
   ```

4. **Start Development**
   ```bash
   pnpm dev
   ```
```

**Rendered:**

1. **Prerequisites**
   - Node.js 18 or higher
   - pnpm package manager
   - Git

2. **Clone Repository**
   ```bash
   git clone https://github.com/lablup/baidocs.git
   cd baidocs
   ```

3. **Install Dependencies**
   ```bash
   pnpm install
   ```

4. **Start Development**
   ```bash
   pnpm dev
   ```

### Project Checklist

```markdown
- [x] **Setup**
  - [x] Install Node.js
  - [x] Install pnpm
  - [x] Clone repository
- [x] **Development**
  - [x] Create first book
  - [x] Write content
  - [ ] Add images
- [ ] **Testing**
  - [ ] Test in viewer
  - [ ] Test PDF generation
  - [ ] Test search
- [ ] **Deployment**
  - [ ] Build for production
  - [ ] Deploy to server
  - [ ] Configure domain
```

**Rendered:**

- [x] **Setup**
  - [x] Install Node.js
  - [x] Install pnpm
  - [x] Clone repository
- [x] **Development**
  - [x] Create first book
  - [x] Write content
  - [ ] Add images
- [ ] **Testing**
  - [ ] Test in viewer
  - [ ] Test PDF generation
  - [ ] Test search
- [ ] **Deployment**
  - [ ] Build for production
  - [ ] Deploy to server
  - [ ] Configure domain

## Best Practices

### Tables

#### 1. Keep Tables Simple

```markdown
<!-- Good: Simple and readable -->
| Name | Age |
|------|-----|
| Alice | 30 |
| Bob | 25 |

<!-- Bad: Too complex -->
| Name | Age | Address | Phone | Email | Company | Position | Start Date | End Date |
|------|-----|---------|-------|-------|---------|----------|------------|----------|
```

#### 2. Align for Readability

```markdown
<!-- Good: Aligned in source -->
| Name    | Status  | Notes       |
|---------|---------|-------------|
| Feature | Done    | Working     |
| Bug     | Fixed   | Resolved    |

<!-- Bad: Hard to read -->
|Name|Status|Notes|
|-|-|-|
|Feature|Done|Working|
```

#### 3. Use Appropriate Alignment

- Left: Text, names, descriptions
- Center: Status indicators, icons
- Right: Numbers, dates, metrics

### Lists

#### 1. Be Consistent

```markdown
<!-- Good: Consistent markers -->
- Item 1
- Item 2
- Item 3

<!-- Bad: Mixed markers -->
- Item 1
* Item 2
+ Item 3
```

#### 2. Use Proper Indentation

```markdown
<!-- Good: Clear hierarchy -->
- Level 1
  - Level 2
    - Level 3

<!-- Bad: Inconsistent -->
- Level 1
   - Level 2
      - Level 3
```

#### 3. Choose the Right List Type

- **Unordered**: When order doesn't matter
- **Ordered**: For steps, rankings, sequences
- **Task**: For checklists, TODOs

## Accessibility

### Tables

- Use header rows (`| Header |`)
- Keep tables simple
- Provide table captions when needed
- Don't use tables for layout

### Lists

- Use semantic list types
- Keep list items concise
- Use proper nesting
- Don't use lists for non-list content

## Next Steps

Learn about [Images and Media](images-media.md) to add visual content to your documentation!
