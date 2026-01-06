---
title: Multi-language Support
description: Create documentation in multiple languages with BaiDocs
---

# Multi-language Support

BaiDocs provides first-class support for multilingual documentation, making it easy to reach a global audience.

## Setting Up Languages

### Book Configuration

Define languages in `book.config.yaml`:

```yaml
id: my-book
title: My Documentation
languages:
  - en
  - ko
  - ja
defaultLanguage: en
```

### Directory Structure

Each language has its own directory:

```
content/my-book/
├── book.config.yaml
├── en/
│   ├── index.md
│   ├── guide.md
│   └── api.md
├── ko/
│   ├── index.md
│   ├── guide.md
│   └── api.md
├── ja/
│   ├── index.md
│   ├── guide.md
│   └── api.md
└── images/  # Shared across languages
```

:::info
The `images/` directory is shared across all languages to avoid duplicating assets.
:::

## Language Codes

Use standard ISO 639-1 codes:

| Language | Code |
|----------|------|
| English | `en` |
| Korean | `ko` |
| Japanese | `ja` |
| Chinese (Simplified) | `zh-CN` |
| Chinese (Traditional) | `zh-TW` |
| Spanish | `es` |
| French | `fr` |
| German | `de` |
| Portuguese | `pt` |
| Russian | `ru` |

## Navigation Per Language

Each language can have different navigation:

```yaml
navigation:
  en:
    - title: Getting Started
      items:
        - title: Introduction
          path: index.md
        - title: Installation
          path: installation.md

  ko:
    - title: 시작하기
      items:
        - title: 소개
          path: index.md
        - title: 설치
          path: installation.md

  ja:
    - title: はじめに
      items:
        - title: 紹介
          path: index.md
        - title: インストール
          path: installation.md
```

:::tip
Navigation structure can be different for each language to match cultural expectations and content organization.
:::

## Content Translation

### Creating Translated Pages

For each English page, create corresponding pages in other languages:

**English** (`en/index.md`):
```markdown
---
title: Welcome
description: Get started with BaiDocs
---

# Welcome to BaiDocs

BaiDocs is a modern documentation framework...
```

**Korean** (`ko/index.md`):
```markdown
---
title: 환영합니다
description: BaiDocs 시작하기
---

# BaiDocs에 오신 것을 환영합니다

BaiDocs는 현대적인 문서화 프레임워크입니다...
```

**Japanese** (`ja/index.md`):
```markdown
---
title: ようこそ
description: BaiDocsを始める
---

# BaiDocsへようこそ

BaiDocsは現代的なドキュメンテーションフレームワークです...
```

### Shared Images

Use the same image paths across languages:

```markdown
<!-- English -->
![Architecture diagram](images/architecture.png)

<!-- Korean -->
![아키텍처 다이어그램](images/architecture.png)

<!-- Japanese -->
![アーキテクチャ図](images/architecture.png)
```

## Language Switcher

BaiDocs automatically provides a language switcher UI:

- Dropdown menu in navigation
- Shows all available languages
- Preserves current page when switching
- Falls back to index if page doesn't exist

### Customizing Language Names

```yaml
languages:
  - code: en
    name: English
  - code: ko
    name: 한국어
  - code: ja
    name: 日本語
```

## Translation Workflow

### 1. Start with One Language

Create complete documentation in your primary language:

```yaml
languages:
  - en
defaultLanguage: en
```

### 2. Add Language Support

Add new language to configuration:

```yaml
languages:
  - en
  - ko  # Added
defaultLanguage: en
```

### 3. Create Language Directory

```bash
mkdir content/my-book/ko
```

### 4. Translate Content

Translate pages one by one:

```bash
# Start with most important pages
ko/index.md
ko/getting-started.md
ko/installation.md
```

### 5. Update Navigation

Add Korean navigation:

```yaml
navigation:
  ko:
    - title: 시작하기
      items:
        - title: 소개
          path: index.md
```

## Translation Tools

### Machine Translation

Use tools for initial translation:

- Google Translate API
- DeepL API
- Azure Translator
- AWS Translate

:::warning
Always review and edit machine translations. They're a starting point, not a final result.
:::

### Translation Management

Tools for managing translations:

- **Crowdin**: Collaborative translation platform
- **Transifex**: Translation management system
- **Lokalise**: Localization platform
- **Weblate**: Open-source translation tool

## Best Practices

### 1. Keep Structure Consistent

Match file structure across languages:

```
en/
├── index.md
├── guide.md
└── api.md

ko/
├── index.md
├── guide.md
└── api.md
```

### 2. Use Consistent Frontmatter

Keep frontmatter fields consistent:

```markdown
<!-- All languages should have -->
---
title: [Translated title]
description: [Translated description]
---
```

### 3. Translate All UI Text

Don't forget to translate:
- Page titles
- Navigation labels
- Admonition text
- Button labels
- Error messages

### 4. Consider Cultural Differences

Adapt content for each culture:
- Date formats (MM/DD vs DD/MM)
- Number formats (1,000 vs 1.000)
- Examples and references
- Color meanings
- Icons and symbols

### 5. Keep Translations In Sync

When updating English content:
- Update corresponding translations
- Mark outdated translations
- Track translation status

## Missing Translations

### Fallback Behavior

If a page doesn't exist in current language:

1. Show language switcher
2. Link to default language version
3. Display "Translation not available" message

### Translation Status

Track translation status in frontmatter:

```markdown
---
title: Installation Guide
translationStatus: complete  # complete, partial, outdated, missing
lastTranslated: 2024-01-01
---
```

## Multi-language Search

Search works across all languages:

```yaml
search:
  multiLanguage: true
  separateIndexes: false  # Combined index
```

Or create separate indexes:

```yaml
search:
  separateIndexes: true  # One index per language
```

## URLs and Routing

BaiDocs uses language-specific URLs:

```
/books/my-book/en/getting-started
/books/my-book/ko/getting-started
/books/my-book/ja/getting-started
```

### Language Detection

BaiDocs can detect user language:

```yaml
languageDetection:
  enabled: true
  sources:
    - url        # From URL parameter
    - cookie     # From saved preference
    - header     # From Accept-Language header
    - browser    # From browser settings
```

## PDF Generation

Generate PDFs for each language:

```yaml
pdf:
  perLanguage: true
```

Output:
```
public/pdfs/
├── my-book-en.pdf
├── my-book-ko.pdf
└── my-book-ja.pdf
```

## RTL Languages

Support right-to-left languages:

```yaml
languages:
  - code: ar
    name: العربية
    direction: rtl
  - code: he
    name: עברית
    direction: rtl
```

BaiDocs automatically:
- Flips layout for RTL
- Adjusts text alignment
- Mirrors navigation

## Translation Example

Complete example of multilingual book:

**book.config.yaml:**
```yaml
id: my-guide
title: Complete Guide
languages:
  - en
  - ko
  - ja
defaultLanguage: en

navigation:
  en:
    - title: Introduction
      items:
        - title: Welcome
          path: index.md
        - title: Getting Started
          path: getting-started.md

  ko:
    - title: 소개
      items:
        - title: 환영합니다
          path: index.md
        - title: 시작하기
          path: getting-started.md

  ja:
    - title: はじめに
      items:
        - title: ようこそ
          path: index.md
        - title: スタートガイド
          path: getting-started.md
```

## Testing Multi-language

Test your translations:

1. **Check all pages exist**
2. **Verify navigation works**
3. **Test language switcher**
4. **Check image references**
5. **Test search in each language**
6. **Generate PDFs**
7. **Test on different browsers**

## Next Steps

Ready to deploy? Learn about [Building for Production](building-production.md)!
