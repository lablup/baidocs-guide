# Creating Your First Book

This section guides you through the process of creating your first documentation book using BaiDocs, from initial setup to content publication.

## Book Creation Workflow

### Accessing the Editor Interface

1. **Navigate to the editor**:
   ```
   http://localhost:3000
   ```

2. **Verify workspace status**:
   - Confirm the editor interface loads successfully
   - Check for any error messages in the browser console
   - Ensure the content directory is accessible

### Creating a New Book

#### Using the Editor Interface

1. **Initiate book creation**:
   - Click the "Create New Book" button on the main dashboard
   - Alternative: Use the "+" icon in the book listing sidebar

2. **Configure book metadata**:
   - **Book ID**: Enter a unique identifier (alphanumeric, hyphens allowed)
   - **Title**: Specify the display title for your documentation
   - **Description**: Provide a concise description of the book's content
   - **Languages**: Select supported languages (default: English)

3. **Set initial configuration**:
   - **Default Language**: Choose the primary language for content creation
   - **Emoji**: Select an emoji icon for visual identification (optional)
   - **Repository URL**: Configure if connecting to external Git repository

#### Manual Book Creation

Alternatively, create books manually by establishing the directory structure:

```bash
# Navigate to content directory
cd content

# Create book directory
mkdir my-first-book
cd my-first-book

# Create configuration file
touch book.config.yaml

# Create language directories
mkdir en ko
```

### Book Configuration

#### Configuration File Structure

Create `book.config.yaml` with the following structure:

```yaml
id: my-first-book
title: My First Documentation Book
description: A comprehensive guide to our product features
emoji: 📖
type: standalone
languages:
  - en
  - ko
defaultLanguage: en
navigation:
  en:
    - title: Introduction
      path: introduction
      items:
        - title: Welcome
          path: introduction/index.md
        - title: Overview
          path: introduction/overview.md
    - title: User Guide
      path: user-guide
      items:
        - title: Getting Started
          path: user-guide/getting-started.md
        - title: Advanced Features
          path: user-guide/advanced.md
  ko:
    - title: 소개
      path: introduction
      items:
        - title: 환영합니다
          path: introduction/index.md
        - title: 개요
          path: introduction/overview.md
    - title: 사용자 가이드
      path: user-guide
      items:
        - title: 시작하기
          path: user-guide/getting-started.md
        - title: 고급 기능
          path: user-guide/advanced.md
repoUrl: https://github.com/your-org/documentation
createdAt: 2026-01-20T00:00:00.000Z
updatedAt: 2026-01-20T00:00:00.000Z
```

#### Navigation Structure Guidelines

- **Sections**: Top-level containers with `path` but no file extension
- **Files**: Individual content pages with `.md` or `.mdx` extensions
- **Hierarchy**: Maintain consistent structure across all languages
- **Path Mapping**: Ensure paths correspond to actual file locations

### Content Creation

#### Creating Your First Page

1. **Navigate to the book** in the editor interface
2. **Create initial directories**:
   - Create `introduction` folder
   - Create `user-guide` folder

3. **Add welcome page**:
   ```markdown
   # Welcome to My Documentation

   This documentation provides comprehensive guidance for using our product.

   ## Quick Start

   Follow these steps to get started:

   1. Review the overview section
   2. Complete the initial setup
   3. Explore advanced features

   ## Support

   For additional assistance, contact our support team.
   ```

4. **Save and preview**:
   - Use Ctrl+S (Cmd+S on macOS) to save
   - Toggle preview mode to verify rendering

#### Adding Additional Content

1. **Create section pages**:
   - Add `overview.md` in the introduction folder
   - Add `getting-started.md` in the user-guide folder

2. **Maintain file naming consistency**:
   - Use identical filenames across all languages
   - Follow kebab-case naming conventions
   - Include appropriate file extensions

### Language Coordination

#### Multi-language Setup

1. **Create corresponding files** in each language directory:
   ```
   en/introduction/index.md
   ko/introduction/index.md
   en/introduction/overview.md
   ko/introduction/overview.md
   ```

2. **Maintain navigation synchronization**:
   - Ensure navigation structure matches across languages
   - Update `book.config.yaml` when adding new sections
   - Verify path consistency for all navigation items

#### Translation Workflow

1. **Primary language development**:
   - Create content in your default language first
   - Establish complete navigation structure
   - Finalize content organization

2. **Secondary language implementation**:
   - Translate navigation labels in `book.config.yaml`
   - Create corresponding content files
   - Maintain identical file and folder structure

### Testing and Validation

#### Content Verification

1. **Editor validation**:
   - Verify all navigation items are clickable
   - Confirm file creation through the interface
   - Check for any error indicators

2. **Viewer verification**:
   - Navigate to `http://localhost:3001`
   - Select your book from the available options
   - Test navigation across all languages
   - Verify content rendering and formatting

#### Quality Assurance

- **Link verification**: Ensure all internal links function correctly
- **Image validation**: Confirm all referenced images are accessible
- **Language consistency**: Verify translation completeness
- **Navigation accuracy**: Test all navigation paths

### Next Steps

After creating your first book:

1. **Explore project structure** to understand file organization
2. **Configure version control** for collaboration and backup
3. **Learn advanced content authoring** techniques
4. **Implement deployment strategies** for production environments

Your first BaiDocs book is now operational and ready for content development.