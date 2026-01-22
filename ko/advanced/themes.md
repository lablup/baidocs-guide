# 테마 커스터마이징

BaiDocs는 조직의 브랜딩과 사용자 경험 요구사항에 맞게 문서 외관을 조정할 수 있는 포괄적인 테마 커스터마이징 기능을 제공합니다. 이 섹션에서는 테마 개발, 커스터마이징 옵션 및 구현 전략을 다룹니다.

## 테마 아키텍처

### 테마 구조 개요

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

### 테마 설정

#### 기본 테마 설정

```javascript
// theme.config.js
module.exports = {
  name: 'Corporate Theme',
  version: '1.0.0',
  description: 'Professional corporate documentation theme',

  // 색상 스키마
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

  // 타이포그래피
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

  // 레이아웃 설정
  layout: {
    maxWidth: '1200px',
    sidebarWidth: '280px',
    headerHeight: '64px',
    footerHeight: '120px',
    contentPadding: '2rem',
    borderRadius: '8px',
    boxShadow: '0 2px 8px rgba(0, 0, 0, 0.1)'
  },

  // 컴포넌트 재정의
  components: {
    Header: './components/CustomHeader.tsx',
    Navigation: './components/CustomNavigation.tsx',
    Footer: './components/CustomFooter.tsx'
  }
};
```

## CSS 커스터마이징

### CSS 변수 및 사용자 정의 속성

```css
/* styles/variables.css */
:root {
  /* 색상 팔레트 */
  --color-primary: #0066cc;
  --color-primary-dark: #004499;
  --color-primary-light: #3385d6;
  --color-secondary: #f0f8ff;
  --color-accent: #ff6b35;

  /* 배경 색상 */
  --bg-primary: #ffffff;
  --bg-secondary: #f8f9fa;
  --bg-tertiary: #e9ecef;
  --bg-code: #f6f8fa;
  --bg-sidebar: #ffffff;

  /* 텍스트 색상 */
  --text-primary: #333333;
  --text-secondary: #666666;
  --text-muted: #999999;
  --text-inverse: #ffffff;

  /* 테두리 및 구분선 색상 */
  --border-light: #e1e5e9;
  --border-medium: #d1d9e0;
  --border-dark: #adb5bd;

  /* 타이포그래피 */
  --font-family-sans: 'Inter', system-ui, sans-serif;
  --font-family-mono: 'JetBrains Mono', Consolas, monospace;
  --font-size-base: 1rem;
  --line-height-base: 1.5;

  /* 간격 시스템 */
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 1.5rem;
  --spacing-xl: 2rem;
  --spacing-2xl: 3rem;

  /* 레이아웃 치수 */
  --layout-max-width: 1200px;
  --sidebar-width: 280px;
  --header-height: 64px;
  --content-padding: 2rem;

  /* 애니메이션 및 전환 */
  --transition-fast: 0.15s ease;
  --transition-normal: 0.3s ease;
  --transition-slow: 0.5s ease;

  /* 테두리 반경 */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;

  /* 박스 그림자 */
  --shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.1);
  --shadow-md: 0 2px 8px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 4px 16px rgba(0, 0, 0, 0.15);
}
```

### 컴포넌트별 스타일링

```css
/* styles/components.css */

/* 헤더 커스터마이징 */
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

/* 사이드바 커스터마이징 */
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

/* 콘텐츠 영역 스타일링 */
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

/* 코드 블록 스타일링 */
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

/* 테이블 스타일링 */
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

/* 경고문 스타일링 */
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

## 다크 모드 구현

### 다크 테마 변수

```css
/* 다크 모드 색상 스키마 */
[data-theme="dark"] {
  /* 배경 색상 */
  --bg-primary: #1a1a1a;
  --bg-secondary: #2d2d2d;
  --bg-tertiary: #404040;
  --bg-code: #2d2d2d;
  --bg-sidebar: #1a1a1a;

  /* 텍스트 색상 */
  --text-primary: #ffffff;
  --text-secondary: #cccccc;
  --text-muted: #999999;
  --text-inverse: #000000;

  /* 테두리 색상 */
  --border-light: #404040;
  --border-medium: #555555;
  --border-dark: #666666;

  /* 다크 모드에 맞게 기본 색상 조정 */
  --color-primary: #4d9eff;
  --color-primary-dark: #0066cc;
  --color-primary-light: #80b3ff;
}
```

### 테마 전환 구현

```javascript
// 테마 전환 기능
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

## 사용자 정의 컴포넌트 개발

### 헤더 컴포넌트 커스터마이징

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

### 네비게이션 컴포넌트 향상

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

## 반응형 디자인

### 모바일 우선 접근법

```css
/* 모바일 우선 반응형 디자인 */
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
  display: none; /* 모바일에서 숨김 */
}

.baidocs-content {
  flex: 1;
  padding: var(--spacing-md);
}

/* 태블릿 브레이크포인트 */
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

/* 데스크톱 브레이크포인트 */
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

/* 대형 데스크톱 최적화 */
@media (min-width: 1200px) {
  .baidocs-content {
    max-width: calc(var(--layout-max-width) - var(--sidebar-width));
  }
}
```

## 브랜드 통합

### 로고 및 브랜드 자산

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

### 사용자 정의 브랜딩 구현

```css
/* 브랜드별 커스터마이징 */
.brand-header {
  background: linear-gradient(90deg, var(--color-primary), var(--color-primary-dark));
  color: var(--text-inverse);
}

.brand-logo {
  height: 40px;
  width: auto;
  filter: brightness(0) invert(1); /* 로고를 흰색으로 만들기 */
}

.brand-accent {
  border-left: 4px solid var(--color-accent);
}

/* 사용자 정의 브랜드 패턴 */
.brand-pattern {
  background-image: url('/assets/images/pattern.svg');
  background-size: 200px 200px;
  background-repeat: repeat;
  opacity: 0.05;
}
```

## 테마 개발 모범 사례

### 성능 최적화

- **CSS 변수**: 동적 테마 적용을 위해 CSS 사용자 정의 속성 사용
- **중요 CSS**: 최초 화면 렌더링을 위한 중요 스타일 인라인 처리
- **지연 로딩**: 테마 자산을 점진적으로 로딩
- **캐싱**: 테마 리소스에 대한 효율적인 캐싱 전략 구현

### 접근성 준수

- **색상 대비**: 모든 색상 조합에 대해 WCAG 준수 보장
- **포커스 표시**: 상호 작용 요소에 대한 명확한 포커스 상태 제공
- **스크린 리더**: 의미론적 HTML과 ARIA 레이블 적절히 사용
- **움직임 감소**: 움직임 감소에 대한 사용자 기본 설정 존중

### 유지보수 및 업데이트

- **버전 관리**: 의미 버전 관리로 테마 변경 사항 추적
- **문서화**: 포괄적인 테마 문서 유지
- **테스트**: 테마 일관성에 대한 자동화된 테스트 구현
- **하위 호환성**: 모든 BaiDocs 버전에서 테마 작동 보장

BaiDocs의 테마 커스터마이징은 조직의 디자인 시스템 및 사용자 기대와 일치하는 브랜딩되고 접근 가능하며 성능이 우수한 문서 경험을 생성할 수 있는 광범위한 유연성을 제공합니다.
