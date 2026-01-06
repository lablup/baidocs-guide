---
title: Theme Customization
description: Customize the appearance of your BaiDocs documentation
---

# Theme Customization

BaiDocs includes a powerful theme system that lets you customize the look and feel of your documentation. You can use built-in themes or create your own.

## Available Themes

### Default Theme

The default theme is based on Ant Design with a clean, professional look:

- Light background
- Blue primary color
- Simple, readable typography
- Minimal distractions

### Backend.AI Theme

A colorful theme matching Backend.AI branding:

- Vibrant colors
- Colorful card-based book navigation
- Modern design
- Eye-catching accents

## Selecting a Theme

Configure the theme in `baidocs.config.yaml`:

```yaml
theme:
  name: "default"  # or "backend-ai"
  path: "themes"
```

After changing the theme:

```bash
# Rebuild to apply changes
pnpm build

# Or restart dev server
pnpm dev
```

:::info
Theme selection happens at build time. You need to rebuild after changing themes.
:::

## How Themes Work

### Theme Structure

Themes are CSS files located in `apps/viewer/themes/`:

```
apps/viewer/themes/
├── default/
│   └── theme.css
└── backend-ai/
    └── theme.css
```

### Theme Loading

During build:

1. Theme CSS is copied to `public/themes/`
2. Theme is loaded in the HTML `<head>`
3. CSS custom properties are applied
4. Styles override defaults

## Creating a Custom Theme

Let's create a custom theme step by step:

### Step 1: Create Theme Directory

```bash
mkdir -p apps/viewer/themes/my-theme
```

### Step 2: Create Theme CSS

Create `apps/viewer/themes/my-theme/theme.css`:

```css
/**
 * My Custom Theme
 * A unique theme for my documentation
 */

/* Color Palette */
:root {
  /* Primary Colors */
  --primary-color: #6366f1;
  --primary-light: #818cf8;
  --primary-dark: #4f46e5;

  /* Text Colors */
  --text-color: #1f2937;
  --text-secondary: #6b7280;
  --text-muted: #9ca3af;

  /* Background Colors */
  --background: #ffffff;
  --background-secondary: #f9fafb;
  --background-tertiary: #f3f4f6;

  /* Border Colors */
  --border-color: #e5e7eb;
  --border-light: #f3f4f6;

  /* Status Colors */
  --success-color: #10b981;
  --warning-color: #f59e0b;
  --error-color: #ef4444;
  --info-color: #3b82f6;

  /* Code Block */
  --code-background: #1e293b;
  --code-text: #e2e8f0;

  /* Link Colors */
  --link-color: var(--primary-color);
  --link-hover: var(--primary-dark);

  /* Shadow */
  --shadow-sm: 0 1px 2px 0 rgb(0 0 0 / 0.05);
  --shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.1);
  --shadow-lg: 0 10px 15px -3px rgb(0 0 0 / 0.1);

  /* Border Radius */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;

  /* Spacing */
  --spacing-xs: 4px;
  --spacing-sm: 8px;
  --spacing-md: 16px;
  --spacing-lg: 24px;
  --spacing-xl: 32px;

  /* Typography */
  --font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  --font-family-code: 'Monaco', 'Menlo', 'Ubuntu Mono', monospace;
  --font-size-base: 16px;
  --line-height-base: 1.6;
}

/* Custom Styles */

/* Navigation */
.sidebar {
  background: var(--background-secondary);
  border-right: 1px solid var(--border-color);
}

.nav-item {
  border-radius: var(--radius-sm);
  transition: background-color 0.2s;
}

.nav-item:hover {
  background: var(--background-tertiary);
}

.nav-item.active {
  background: var(--primary-color);
  color: white;
}

/* Headers */
h1, h2, h3, h4, h5, h6 {
  color: var(--text-color);
  font-weight: 600;
}

h1 {
  border-bottom: 2px solid var(--primary-color);
  padding-bottom: 0.5rem;
}

/* Links */
a {
  color: var(--link-color);
  text-decoration: none;
  transition: color 0.2s;
}

a:hover {
  color: var(--link-hover);
  text-decoration: underline;
}

/* Code Blocks */
pre {
  background: var(--code-background);
  border-radius: var(--radius-md);
  padding: var(--spacing-md);
  overflow-x: auto;
}

code {
  font-family: var(--font-family-code);
}

/* Inline Code */
:not(pre) > code {
  background: var(--background-tertiary);
  color: var(--primary-color);
  padding: 2px 6px;
  border-radius: var(--radius-sm);
  font-size: 0.9em;
}

/* Tables */
table {
  border-collapse: collapse;
  width: 100%;
  margin: var(--spacing-lg) 0;
}

th, td {
  border: 1px solid var(--border-color);
  padding: var(--spacing-sm) var(--spacing-md);
  text-align: left;
}

th {
  background: var(--background-secondary);
  font-weight: 600;
}

tr:hover {
  background: var(--background-secondary);
}

/* Blockquotes */
blockquote {
  border-left: 4px solid var(--primary-color);
  margin: var(--spacing-lg) 0;
  padding-left: var(--spacing-lg);
  color: var(--text-secondary);
  font-style: italic;
}

/* Admonitions */
.admonition {
  border-left: 4px solid;
  border-radius: var(--radius-sm);
  padding: var(--spacing-md);
  margin: var(--spacing-lg) 0;
}

.admonition-note {
  border-color: var(--info-color);
  background: #dbeafe;
}

.admonition-tip {
  border-color: var(--success-color);
  background: #d1fae5;
}

.admonition-warning {
  border-color: var(--warning-color);
  background: #fed7aa;
}

.admonition-danger {
  border-color: var(--error-color);
  background: #fecaca;
}

/* Search */
.search-modal {
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-lg);
}

.search-input {
  border: 2px solid var(--border-color);
  border-radius: var(--radius-md);
  padding: var(--spacing-sm) var(--spacing-md);
  font-size: var(--font-size-base);
}

.search-input:focus {
  border-color: var(--primary-color);
  outline: none;
}

/* Buttons */
.button-primary {
  background: var(--primary-color);
  color: white;
  border: none;
  border-radius: var(--radius-sm);
  padding: var(--spacing-sm) var(--spacing-lg);
  cursor: pointer;
  transition: background-color 0.2s;
}

.button-primary:hover {
  background: var(--primary-dark);
}

/* Card */
.card {
  background: var(--background);
  border: 1px solid var(--border-color);
  border-radius: var(--radius-md);
  padding: var(--spacing-lg);
  box-shadow: var(--shadow-sm);
  transition: box-shadow 0.2s;
}

.card:hover {
  box-shadow: var(--shadow-md);
}

/* Dark Mode Support */
@media (prefers-color-scheme: dark) {
  :root {
    --text-color: #f9fafb;
    --text-secondary: #d1d5db;
    --background: #111827;
    --background-secondary: #1f2937;
    --background-tertiary: #374151;
    --border-color: #374151;
  }
}
```

### Step 3: Update Configuration

Edit `baidocs.config.yaml`:

```yaml
theme:
  name: "my-theme"
  path: "themes"
```

### Step 4: Build and Test

```bash
# Build with new theme
pnpm build

# Or restart dev server
pnpm dev
```

Visit http://localhost:3000 to see your custom theme!

## Theme Variables

### Essential Variables

These are the most important variables to customize:

```css
:root {
  /* Brand Colors */
  --primary-color: #1890ff;      /* Main brand color */
  --primary-light: #40a9ff;      /* Lighter variant */
  --primary-dark: #096dd9;       /* Darker variant */

  /* Text */
  --text-color: #000000;         /* Main text */
  --text-secondary: #666666;     /* Secondary text */

  /* Background */
  --background: #ffffff;         /* Main background */
  --background-secondary: #f5f5f5; /* Secondary bg */

  /* UI Elements */
  --border-color: #d9d9d9;       /* Borders */
  --link-color: #1890ff;         /* Links */
}
```

## Advanced Customization

### Typography

Customize fonts:

```css
:root {
  /* Font Families */
  --font-family: 'Inter', -apple-system, sans-serif;
  --font-family-mono: 'Fira Code', monospace;

  /* Font Sizes */
  --font-size-xs: 12px;
  --font-size-sm: 14px;
  --font-size-base: 16px;
  --font-size-lg: 18px;
  --font-size-xl: 20px;

  /* Line Heights */
  --line-height-tight: 1.25;
  --line-height-normal: 1.5;
  --line-height-relaxed: 1.75;

  /* Font Weights */
  --font-weight-normal: 400;
  --font-weight-medium: 500;
  --font-weight-bold: 700;
}

/* Apply custom fonts */
body {
  font-family: var(--font-family);
  font-size: var(--font-size-base);
  line-height: var(--line-height-normal);
}

code, pre {
  font-family: var(--font-family-mono);
}

h1, h2, h3 {
  font-weight: var(--font-weight-bold);
}
```

### Spacing System

Define consistent spacing:

```css
:root {
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-5: 20px;
  --space-6: 24px;
  --space-8: 32px;
  --space-10: 40px;
  --space-12: 48px;
  --space-16: 64px;
}
```

### Animation

Add smooth transitions:

```css
:root {
  --transition-fast: 150ms ease;
  --transition-base: 200ms ease;
  --transition-slow: 300ms ease;
}

/* Apply to elements */
a, button, .nav-item {
  transition: all var(--transition-base);
}
```

## Responsive Design

Make your theme responsive:

```css
/* Mobile */
@media (max-width: 768px) {
  :root {
    --font-size-base: 14px;
    --spacing-md: 12px;
  }

  .sidebar {
    display: none; /* Hide on mobile */
  }
}

/* Tablet */
@media (min-width: 769px) and (max-width: 1024px) {
  .content {
    max-width: 90%;
  }
}

/* Desktop */
@media (min-width: 1025px) {
  .content {
    max-width: 1200px;
  }
}
```

## Dark Mode

Implement dark mode:

```css
/* Default (Light Mode) */
:root {
  --background: #ffffff;
  --text: #000000;
}

/* Dark Mode */
@media (prefers-color-scheme: dark) {
  :root {
    --background: #1a1a1a;
    --text: #ffffff;
    --background-secondary: #2d2d2d;
    --border-color: #404040;
  }

  /* Adjust code blocks for dark mode */
  pre {
    background: #0d1117;
  }

  /* Adjust images for dark mode */
  img {
    opacity: 0.9;
  }
}

/* Manual dark mode toggle */
[data-theme="dark"] {
  --background: #1a1a1a;
  --text: #ffffff;
}
```

## Theme Best Practices

### 1. Use CSS Custom Properties

```css
/* Good: Flexible and maintainable */
.button {
  background: var(--primary-color);
  color: var(--button-text);
}

/* Bad: Hard-coded values */
.button {
  background: #1890ff;
  color: white;
}
```

### 2. Maintain Accessibility

```css
/* Ensure sufficient contrast */
:root {
  --text-color: #000000;    /* AA compliant */
  --background: #ffffff;
}

/* Not accessible */
:root {
  --text-color: #cccccc;    /* Low contrast */
  --background: #ffffff;
}
```

### 3. Test Across Devices

Test your theme on:
- Desktop browsers (Chrome, Firefox, Safari)
- Mobile devices (iOS, Android)
- Different screen sizes
- Light and dark modes

### 4. Document Your Theme

Include comments in your CSS:

```css
/**
 * Primary Navigation
 *
 * Styles for the main navigation sidebar
 * including active states and hover effects
 */
.sidebar {
  /* styles here */
}
```

## Troubleshooting

### Theme Not Applying

If your theme doesn't apply:

1. ✅ Check theme name in `baidocs.config.yaml`
2. ✅ Verify theme file exists in `apps/viewer/themes/`
3. ✅ Rebuild the project: `pnpm build`
4. ✅ Clear browser cache
5. ✅ Check browser console for errors

### Styles Conflicting

If styles conflict:

1. Use more specific selectors
2. Check CSS specificity
3. Use `!important` sparingly
4. Ensure correct load order

## Next Steps

Learn about [PDF Generation](pdf-generation.md) to create beautiful PDFs of your documentation!
