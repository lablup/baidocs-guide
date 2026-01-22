# Theme Customization

BaiDocs provides comprehensive theme customization capabilities to align documentation appearance with organizational branding and user experience requirements. This section covers theme development, customization options, and implementation strategies.

## Theme Architecture

### Theme Structure Overview

```
themes/
├── default/
│   ├── components/
│   │   ├── Header.tsx
│   │   ├── Navigation.tsx
│   │   ├── Content.tsx
│   │   └── Footer.tsx
│   ├── styles/
│   │   ├── globals.css
│   │   ├── variables.css
│   │   └── components.css
│   ├── assets/
│   │   ├── images/
│   │   ├── icons/
│   │   └── fonts/
│   └── theme.config.js
└── custom/
    ├── components/
    ├── styles/
    ├── assets/
    └── theme.config.js
```

### Theme Configuration

#### Basic Theme Configuration

```javascript
// theme.config.js
module.exports = {
  name: 'Corporate Theme',
  version: '1.0.0',
  description: 'Professional corporate documentation theme',

  // Color scheme
  colors: {
    primary: '#0066cc',
    secondary: '#f0f8ff',
    accent: '#ff6b35',
    background: '#ffffff',
    text: '#333333',
    muted: '#666666',
    border: '#e1e5e9',
    success: '#28a745',
    warning: '#ffc107',
    error: '#dc3545',
    info: '#17a2b8'
  },

  // Typography
  typography: {
    fontFamily: {
      sans: ['Inter', 'system-ui', 'sans-serif'],
      mono: ['JetBrains Mono', 'Consolas', 'monospace'],
      serif: ['Georgia', 'Times New Roman', 'serif']
    },
    fontSize: {
      xs: '0.75rem',
      sm: '0.875rem',
      base: '1rem',
      lg: '1.125rem',
      xl: '1.25rem',
      '2xl': '1.5rem',
      '3xl': '1.875rem',
      '4xl': '2.25rem'
    },
    lineHeight: {
      tight: 1.25,
      normal: 1.5,
      relaxed: 1.75
    }
  },

  // Layout configuration
  layout: {
    maxWidth: '1200px',
    sidebarWidth: '280px',
    headerHeight: '64px',
    footerHeight: '120px',
    contentPadding: '2rem',
    borderRadius: '8px',
    boxShadow: '0 2px 8px rgba(0, 0, 0, 0.1)'
  },

  // Component overrides
  components: {
    Header: './components/CustomHeader.tsx',
    Navigation: './components/CustomNavigation.tsx',
    Footer: './components/CustomFooter.tsx'
  }
};
```

## CSS Customization

### CSS Variables and Custom Properties

```css
/* styles/variables.css */
:root {
  /* Color palette */
  --color-primary: #0066cc;
  --color-primary-dark: #004499;
  --color-primary-light: #3385d6;
  --color-secondary: #f0f8ff;
  --color-accent: #ff6b35;

  /* Background colors */
  --bg-primary: #ffffff;
  --bg-secondary: #f8f9fa;
  --bg-tertiary: #e9ecef;
  --bg-code: #f6f8fa;
  --bg-sidebar: #ffffff;

  /* Text colors */
  --text-primary: #333333;
  --text-secondary: #666666;
  --text-muted: #999999;
  --text-inverse: #ffffff;

  /* Border and divider colors */
  --border-light: #e1e5e9;
  --border-medium: #d1d9e0;
  --border-dark: #adb5bd;

  /* Typography */
  --font-family-sans: 'Inter', system-ui, sans-serif;
  --font-family-mono: 'JetBrains Mono', Consolas, monospace;
  --font-size-base: 1rem;
  --line-height-base: 1.5;

  /* Spacing system */
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 1.5rem;
  --spacing-xl: 2rem;
  --spacing-2xl: 3rem;

  /* Layout dimensions */
  --layout-max-width: 1200px;
  --sidebar-width: 280px;
  --header-height: 64px;
  --content-padding: 2rem;

  /* Animation and transitions */
  --transition-fast: 0.15s ease;
  --transition-normal: 0.3s ease;
  --transition-slow: 0.5s ease;

  /* Border radius */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;

  /* Box shadows */
  --shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.1);
  --shadow-md: 0 2px 8px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 4px 16px rgba(0, 0, 0, 0.15);
}
```

### Component-specific Styling

```css
/* styles/components.css */

/* Header customization */
.baidocs-header {
  background-color: var(--bg-primary);
  border-bottom: 1px solid var(--border-light);
  height: var(--header-height);
  padding: 0 var(--spacing-lg);
  box-shadow: var(--shadow-sm);
}

.baidocs-header .logo {
  height: 32px;
  width: auto;
}

.baidocs-header .navigation {
  display: flex;
  align-items: center;
  gap: var(--spacing-md);
}

/* Sidebar customization */
.baidocs-sidebar {
  width: var(--sidebar-width);
  background-color: var(--bg-sidebar);
  border-right: 1px solid var(--border-light);
  padding: var(--spacing-lg);
  overflow-y: auto;
}

.baidocs-sidebar .navigation-item {
  display: block;
  padding: var(--spacing-sm) var(--spacing-md);
  color: var(--text-primary);
  text-decoration: none;
  border-radius: var(--radius-sm);
  transition: background-color var(--transition-fast);
}

.baidocs-sidebar .navigation-item:hover {
  background-color: var(--bg-secondary);
}

.baidocs-sidebar .navigation-item.active {
  background-color: var(--color-primary);
  color: var(--text-inverse);
}

/* Content area styling */
.baidocs-content {
  max-width: var(--layout-max-width);
  margin: 0 auto;
  padding: var(--content-padding);
}

.baidocs-content h1 {
  font-size: var(--font-size-3xl);
  font-weight: 700;
  color: var(--text-primary);
  margin-bottom: var(--spacing-lg);
  line-height: var(--line-height-tight);
}

.baidocs-content h2 {
  font-size: var(--font-size-2xl);
  font-weight: 600;
  color: var(--text-primary);
  margin-top: var(--spacing-xl);
  margin-bottom: var(--spacing-md);
  border-bottom: 2px solid var(--border-light);
  padding-bottom: var(--spacing-sm);
}

/* Code block styling */
.baidocs-code-block {
  background-color: var(--bg-code);
  border: 1px solid var(--border-light);
  border-radius: var(--radius-md);
  padding: var(--spacing-lg);
  font-family: var(--font-family-mono);
  overflow-x: auto;
  margin: var(--spacing-lg) 0;
}

.baidocs-code-block pre {
  margin: 0;
  padding: 0;
  background: none;
  border: none;
}

/* Table styling */
.baidocs-table {
  width: 100%;
  border-collapse: collapse;
  margin: var(--spacing-lg) 0;
}

.baidocs-table th,
.baidocs-table td {
  padding: var(--spacing-md);
  text-align: left;
  border-bottom: 1px solid var(--border-light);
}

.baidocs-table th {
  background-color: var(--bg-secondary);
  font-weight: 600;
  color: var(--text-primary);
}

/* Admonition styling */
.baidocs-admonition {
  border-left: 4px solid var(--color-primary);
  background-color: var(--bg-secondary);
  padding: var(--spacing-lg);
  margin: var(--spacing-lg) 0;
  border-radius: 0 var(--radius-md) var(--radius-md) 0;
}

.baidocs-admonition.warning {
  border-left-color: var(--color-warning);
}

.baidocs-admonition.danger {
  border-left-color: var(--color-error);
}

.baidocs-admonition.info {
  border-left-color: var(--color-info);
}
```

## Dark Mode Implementation

### Dark Theme Variables

```css
/* Dark mode color scheme */
[data-theme="dark"] {
  /* Background colors */
  --bg-primary: #1a1a1a;
  --bg-secondary: #2d2d2d;
  --bg-tertiary: #404040;
  --bg-code: #2d2d2d;
  --bg-sidebar: #1a1a1a;

  /* Text colors */
  --text-primary: #ffffff;
  --text-secondary: #cccccc;
  --text-muted: #999999;
  --text-inverse: #000000;

  /* Border colors */
  --border-light: #404040;
  --border-medium: #555555;
  --border-dark: #666666;

  /* Adjust primary colors for dark mode */
  --color-primary: #4d9eff;
  --color-primary-dark: #0066cc;
  --color-primary-light: #80b3ff;
}
```

### Theme Toggle Implementation

```javascript
// Theme toggle functionality
const ThemeToggle = () => {
  const [isDark, setIsDark] = useState(false);

  useEffect(() => {
    const savedTheme = localStorage.getItem('theme');
    const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
    const shouldUseDark = savedTheme === 'dark' || (!savedTheme && prefersDark);

    setIsDark(shouldUseDark);
    document.documentElement.setAttribute('data-theme', shouldUseDark ? 'dark' : 'light');
  }, []);

  const toggleTheme = () => {
    const newTheme = isDark ? 'light' : 'dark';
    setIsDark(!isDark);
    localStorage.setItem('theme', newTheme);
    document.documentElement.setAttribute('data-theme', newTheme);
  };

  return (
    <button
      onClick={toggleTheme}
      className="theme-toggle"
      aria-label={`Switch to ${isDark ? 'light' : 'dark'} mode`}
    >
      {isDark ? '☀️' : '🌙'}
    </button>
  );
};
```

## Custom Component Development

### Header Component Customization

```tsx
// components/CustomHeader.tsx
import React from 'react';
import { ThemeToggle } from './ThemeToggle';
import { LanguageSelector } from './LanguageSelector';

interface HeaderProps {
  title: string;
  logo?: string;
  navigation?: Array<{
    title: string;
    href: string;
  }>;
}

export const CustomHeader: React.FC<HeaderProps> = ({
  title,
  logo,
  navigation = []
}) => {
  return (
    <header className="baidocs-header">
      <div className="header-content">
        <div className="header-left">
          {logo && (
            <img src={logo} alt={title} className="logo" />
          )}
          <h1 className="title">{title}</h1>
        </div>

        <nav className="header-navigation">
          {navigation.map((item, index) => (
            <a
              key={index}
              href={item.href}
              className="navigation-link"
            >
              {item.title}
            </a>
          ))}
        </nav>

        <div className="header-right">
          <LanguageSelector />
          <ThemeToggle />
        </div>
      </div>
    </header>
  );
};
```

### Navigation Component Enhancement

```tsx
// components/CustomNavigation.tsx
import React, { useState } from 'react';
import { ChevronDownIcon, ChevronRightIcon } from '@heroicons/react/24/outline';

interface NavigationItem {
  title: string;
  path?: string;
  items?: NavigationItem[];
}

interface NavigationProps {
  items: NavigationItem[];
  currentPath: string;
}

export const CustomNavigation: React.FC<NavigationProps> = ({
  items,
  currentPath
}) => {
  const [expandedItems, setExpandedItems] = useState<Set<string>>(new Set());

  const toggleExpanded = (path: string) => {
    const newExpanded = new Set(expandedItems);
    if (newExpanded.has(path)) {
      newExpanded.delete(path);
    } else {
      newExpanded.add(path);
    }
    setExpandedItems(newExpanded);
  };

  const renderNavigationItem = (item: NavigationItem, level: number = 0) => {
    const hasChildren = item.items && item.items.length > 0;
    const isExpanded = expandedItems.has(item.path || '');
    const isActive = currentPath === item.path;

    return (
      <div key={item.path || item.title} className={`navigation-item level-${level}`}>
        <div className="navigation-item-content">
          {hasChildren ? (
            <button
              onClick={() => toggleExpanded(item.path || '')}
              className={`navigation-button ${isActive ? 'active' : ''}`}
            >
              <span className="navigation-title">{item.title}</span>
              {hasChildren && (
                <span className="navigation-icon">
                  {isExpanded ? (
                    <ChevronDownIcon className="w-4 h-4" />
                  ) : (
                    <ChevronRightIcon className="w-4 h-4" />
                  )}
                </span>
              )}
            </button>
          ) : (
            <a
              href={item.path}
              className={`navigation-link ${isActive ? 'active' : ''}`}
            >
              {item.title}
            </a>
          )}
        </div>

        {hasChildren && isExpanded && (
          <div className="navigation-children">
            {item.items!.map(child => renderNavigationItem(child, level + 1))}
          </div>
        )}
      </div>
    );
  };

  return (
    <nav className="baidocs-navigation">
      {items.map(item => renderNavigationItem(item))}
    </nav>
  );
};
```

## Responsive Design

### Mobile-first Approach

```css
/* Mobile-first responsive design */
.baidocs-layout {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.baidocs-main {
  display: flex;
  flex: 1;
}

.baidocs-sidebar {
  display: none; /* Hidden on mobile */
}

.baidocs-content {
  flex: 1;
  padding: var(--spacing-md);
}

/* Tablet breakpoint */
@media (min-width: 768px) {
  .baidocs-sidebar {
    display: block;
    position: fixed;
    top: var(--header-height);
    left: 0;
    height: calc(100vh - var(--header-height));
    transform: translateX(-100%);
    transition: transform var(--transition-normal);
    z-index: 100;
  }

  .baidocs-sidebar.open {
    transform: translateX(0);
  }

  .baidocs-content {
    padding: var(--spacing-lg);
  }
}

/* Desktop breakpoint */
@media (min-width: 1024px) {
  .baidocs-sidebar {
    position: static;
    transform: none;
    height: auto;
  }

  .baidocs-content {
    margin-left: var(--sidebar-width);
    padding: var(--content-padding);
  }
}

/* Large desktop optimization */
@media (min-width: 1200px) {
  .baidocs-content {
    max-width: calc(var(--layout-max-width) - var(--sidebar-width));
  }
}
```

## Brand Integration

### Logo and Brand Assets

```tsx
// components/BrandAssets.tsx
export const BrandAssets = {
  logo: {
    default: '/assets/images/logo.svg',
    dark: '/assets/images/logo-dark.svg',
    icon: '/assets/images/icon.svg',
    wordmark: '/assets/images/wordmark.svg'
  },

  colors: {
    brand: {
      primary: '#0066cc',
      secondary: '#004499',
      accent: '#ff6b35'
    },
    semantic: {
      success: '#28a745',
      warning: '#ffc107',
      error: '#dc3545',
      info: '#17a2b8'
    }
  },

  typography: {
    headings: 'Inter',
    body: 'Inter',
    code: 'JetBrains Mono'
  }
};
```

### Custom Branding Implementation

```css
/* Brand-specific customizations */
.brand-header {
  background: linear-gradient(90deg, var(--color-primary), var(--color-primary-dark));
  color: var(--text-inverse);
}

.brand-logo {
  height: 40px;
  width: auto;
  filter: brightness(0) invert(1); /* Make logo white */
}

.brand-accent {
  border-left: 4px solid var(--color-accent);
}

/* Custom brand patterns */
.brand-pattern {
  background-image: url('/assets/images/pattern.svg');
  background-size: 200px 200px;
  background-repeat: repeat;
  opacity: 0.05;
}
```

## Theme Development Best Practices

### Performance Optimization

- **CSS Variables**: Use CSS custom properties for dynamic theming
- **Critical CSS**: Inline critical styling for above-the-fold content
- **Lazy Loading**: Load theme assets progressively
- **Caching**: Implement efficient caching strategies for theme resources

### Accessibility Compliance

- **Color Contrast**: Ensure WCAG compliance for all color combinations
- **Focus Indicators**: Provide clear focus states for interactive elements
- **Screen Readers**: Use semantic HTML and ARIA labels appropriately
- **Reduced Motion**: Respect user preferences for reduced motion

### Maintenance and Updates

- **Version Control**: Track theme changes with semantic versioning
- **Documentation**: Maintain comprehensive theme documentation
- **Testing**: Implement automated testing for theme consistency
- **Backward Compatibility**: Ensure themes work across BaiDocs versions

Theme customization in BaiDocs provides extensive flexibility for creating branded, accessible, and performant documentation experiences that align with organizational design systems and user expectations.