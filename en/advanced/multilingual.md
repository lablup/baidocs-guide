# Multi-language Support

BaiDocs provides comprehensive multi-language documentation capabilities with synchronized navigation, content coordination, and translation management tools. This section covers multi-language setup, content management, and localization best practices.

## Multi-language Architecture

### Language Support Framework

BaiDocs multi-language system includes:

- **Synchronized Navigation**: Consistent structure across all language versions
- **Content Coordination**: Automatic detection of missing translations
- **Language Switching**: Seamless user experience with persistent navigation state
- **Translation Workflow**: Tools for managing translation processes
- **Localization Support**: Cultural adaptation beyond simple translation
- **RTL Language Support**: Right-to-left text rendering capabilities

### Directory Structure

```
content/
└── documentation-book/
    ├── book.config.yaml
    ├── en/                     # English content
    │   ├── introduction/
    │   │   ├── index.md
    │   │   └── overview.md
    │   └── user-guide/
    │       └── getting-started.md
    ├── ko/                     # Korean content
    │   ├── introduction/
    │   │   ├── index.md
    │   │   └── overview.md
    │   └── user-guide/
    │       └── getting-started.md
    ├── ja/                     # Japanese content
    │   ├── introduction/
    │   │   ├── index.md
    │   │   └── overview.md
    │   └── user-guide/
    │       └── getting-started.md
    ├── images/                 # Shared images
    └── assets/                 # Shared assets
```

## Language Configuration

### Basic Multi-language Setup

```yaml
# book.config.yaml
id: multilingual-documentation
title: Multi-language Documentation
description: Comprehensive guide supporting multiple languages
emoji: 🌐
type: standalone

# Language configuration
languages:
  - en      # English (default)
  - ko      # Korean
  - ja      # Japanese
  - de      # German
  - fr      # French
  - es      # Spanish
  - zh-CN   # Simplified Chinese
  - zh-TW   # Traditional Chinese
  - ar      # Arabic

defaultLanguage: en

# Language-specific metadata
languageConfig:
  en:
    name: "English"
    nativeName: "English"
    direction: "ltr"
    locale: "en-US"
    dateFormat: "MM/DD/YYYY"
    numberFormat: "1,234.56"

  ko:
    name: "Korean"
    nativeName: "한국어"
    direction: "ltr"
    locale: "ko-KR"
    dateFormat: "YYYY년 MM월 DD일"
    numberFormat: "1,234.56"

  ja:
    name: "Japanese"
    nativeName: "日本語"
    direction: "ltr"
    locale: "ja-JP"
    dateFormat: "YYYY年MM月DD日"
    numberFormat: "1,234.56"

  ar:
    name: "Arabic"
    nativeName: "العربية"
    direction: "rtl"
    locale: "ar-SA"
    dateFormat: "DD/MM/YYYY"
    numberFormat: "١٬٢٣٤٫٥٦"
```

### Synchronized Navigation Structure

```yaml
# Consistent navigation across all languages
navigation:
  en:
    - title: "Introduction"
      path: introduction
      items:
        - title: "Welcome"
          path: introduction/index.md
        - title: "Overview"
          path: introduction/overview.md
    - title: "User Guide"
      path: user-guide
      items:
        - title: "Getting Started"
          path: user-guide/getting-started.md

  ko:
    - title: "소개"
      path: introduction
      items:
        - title: "환영합니다"
          path: introduction/index.md
        - title: "개요"
          path: introduction/overview.md
    - title: "사용자 가이드"
      path: user-guide
      items:
        - title: "시작하기"
          path: user-guide/getting-started.md

  ja:
    - title: "はじめに"
      path: introduction
      items:
        - title: "ようこそ"
          path: introduction/index.md
        - title: "概要"
          path: introduction/overview.md
    - title: "ユーザーガイド"
      path: user-guide
      items:
        - title: "スタートガイド"
          path: user-guide/getting-started.md
```

## Content Management Strategies

### Translation Coordination

```typescript
interface TranslationStatus {
  language: string;
  filePath: string;
  exists: boolean;
  lastModified?: Date;
  wordCount?: number;
  translationStatus: 'missing' | 'outdated' | 'current' | 'pending';
  translator?: string;
  reviewedBy?: string;
  reviewedAt?: Date;
}

class TranslationManager {
  private baseLanguage: string = 'en';
  private supportedLanguages: string[];

  constructor(languages: string[], defaultLang: string) {
    this.supportedLanguages = languages;
    this.baseLanguage = defaultLang;
  }

  async analyzeTranslationStatus(bookPath: string): Promise<TranslationStatus[]> {
    const baseFiles = await this.getContentFiles(bookPath, this.baseLanguage);
    const statuses: TranslationStatus[] = [];

    for (const baseFile of baseFiles) {
      for (const language of this.supportedLanguages) {
        if (language === this.baseLanguage) continue;

        const translationPath = this.getTranslationPath(baseFile, language);
        const status = await this.checkTranslationStatus(baseFile, translationPath, language);
        statuses.push(status);
      }
    }

    return statuses;
  }

  private async checkTranslationStatus(
    baseFile: string,
    translationPath: string,
    language: string
  ): Promise<TranslationStatus> {
    const exists = await fs.pathExists(translationPath);

    if (!exists) {
      return {
        language,
        filePath: translationPath,
        exists: false,
        translationStatus: 'missing'
      };
    }

    const baseStat = await fs.stat(baseFile);
    const translationStat = await fs.stat(translationPath);

    const translationStatus = translationStat.mtime >= baseStat.mtime ? 'current' : 'outdated';

    // Extract metadata from translation file
    const translationContent = await fs.readFile(translationPath, 'utf-8');
    const metadata = this.extractTranslationMetadata(translationContent);

    return {
      language,
      filePath: translationPath,
      exists: true,
      lastModified: translationStat.mtime,
      wordCount: this.countWords(translationContent),
      translationStatus,
      translator: metadata.translator,
      reviewedBy: metadata.reviewedBy,
      reviewedAt: metadata.reviewedAt ? new Date(metadata.reviewedAt) : undefined
    };
  }

  async generateTranslationReport(bookPath: string): Promise<TranslationReport> {
    const statuses = await this.analyzeTranslationStatus(bookPath);

    const report: TranslationReport = {
      timestamp: new Date(),
      languages: this.supportedLanguages,
      summary: {
        totalFiles: 0,
        missingTranslations: 0,
        outdatedTranslations: 0,
        currentTranslations: 0
      },
      details: {}
    };

    this.supportedLanguages.forEach(lang => {
      if (lang === this.baseLanguage) return;

      const langStatuses = statuses.filter(s => s.language === lang);
      report.details[lang] = {
        total: langStatuses.length,
        missing: langStatuses.filter(s => s.translationStatus === 'missing').length,
        outdated: langStatuses.filter(s => s.translationStatus === 'outdated').length,
        current: langStatuses.filter(s => s.translationStatus === 'current').length,
        files: langStatuses
      };
    });

    return report;
  }
}
```

### Translation Workflow Management

```typescript
interface TranslationTask {
  id: string;
  sourceFile: string;
  targetLanguage: string;
  targetFile: string;
  status: 'pending' | 'in_progress' | 'review' | 'completed';
  assignedTo?: string;
  reviewedBy?: string;
  deadline?: Date;
  priority: 'low' | 'medium' | 'high' | 'urgent';
  wordCount: number;
  estimatedHours: number;
  completedAt?: Date;
}

class TranslationWorkflow {
  private tasks: Map<string, TranslationTask> = new Map();

  async createTranslationTask(
    sourceFile: string,
    targetLanguage: string,
    priority: TranslationTask['priority'] = 'medium'
  ): Promise<TranslationTask> {
    const sourceContent = await fs.readFile(sourceFile, 'utf-8');
    const wordCount = this.countWords(sourceContent);

    const task: TranslationTask = {
      id: this.generateTaskId(),
      sourceFile,
      targetLanguage,
      targetFile: this.getTargetFilePath(sourceFile, targetLanguage),
      status: 'pending',
      priority,
      wordCount,
      estimatedHours: this.estimateTranslationTime(wordCount, targetLanguage)
    };

    this.tasks.set(task.id, task);
    await this.saveTaskState(task);

    return task;
  }

  async assignTask(taskId: string, translator: string): Promise<void> {
    const task = this.tasks.get(taskId);
    if (!task) throw new Error(`Task ${taskId} not found`);

    task.assignedTo = translator;
    task.status = 'in_progress';
    await this.saveTaskState(task);

    // Notify translator
    await this.sendTaskAssignmentNotification(task, translator);
  }

  async submitForReview(taskId: string, translatedContent: string): Promise<void> {
    const task = this.tasks.get(taskId);
    if (!task) throw new Error(`Task ${taskId} not found`);

    // Save translated content with metadata
    const contentWithMetadata = this.addTranslationMetadata(translatedContent, task);
    await fs.writeFile(task.targetFile, contentWithMetadata);

    task.status = 'review';
    await this.saveTaskState(task);

    // Notify reviewers
    await this.sendReviewNotification(task);
  }

  async approveTranslation(taskId: string, reviewerId: string): Promise<void> {
    const task = this.tasks.get(taskId);
    if (!task) throw new Error(`Task ${taskId} not found`);

    task.status = 'completed';
    task.reviewedBy = reviewerId;
    task.completedAt = new Date();
    await this.saveTaskState(task);

    // Update navigation if needed
    await this.updateNavigationTranslation(task);

    // Notify completion
    await this.sendCompletionNotification(task);
  }

  private estimateTranslationTime(wordCount: number, targetLanguage: string): number {
    const baseRate = 250; // words per hour
    const languageMultipliers: Record<string, number> = {
      'ko': 1.2,
      'ja': 1.3,
      'zh-CN': 1.4,
      'zh-TW': 1.4,
      'ar': 1.5,
      'de': 0.9,
      'fr': 0.9,
      'es': 0.9
    };

    const multiplier = languageMultipliers[targetLanguage] || 1.0;
    return Math.ceil((wordCount / baseRate) * multiplier);
  }

  private addTranslationMetadata(content: string, task: TranslationTask): string {
    const metadata = `---
translator: ${task.assignedTo}
translatedAt: ${new Date().toISOString()}
sourceFile: ${task.sourceFile}
wordCount: ${task.wordCount}
estimatedHours: ${task.estimatedHours}
---

${content}`;

    return metadata;
  }
}
```

## Language Switching Implementation

### Client-side Language Switching

```tsx
import React, { createContext, useContext, useState, useEffect } from 'react';
import { useRouter } from 'next/router';

interface LanguageContextType {
  currentLanguage: string;
  availableLanguages: LanguageInfo[];
  setLanguage: (lang: string) => void;
  getLocalizedPath: (path: string, targetLang?: string) => string;
  isRTL: boolean;
}

interface LanguageInfo {
  code: string;
  name: string;
  nativeName: string;
  direction: 'ltr' | 'rtl';
  flag?: string;
}

const LanguageContext = createContext<LanguageContextType | null>(null);

export const LanguageProvider: React.FC<{
  children: React.ReactNode;
  availableLanguages: LanguageInfo[];
  defaultLanguage: string;
}> = ({ children, availableLanguages, defaultLanguage }) => {
  const router = useRouter();
  const [currentLanguage, setCurrentLanguage] = useState(defaultLanguage);

  useEffect(() => {
    // Detect language from URL or localStorage
    const urlLanguage = router.query.lang as string;
    const savedLanguage = localStorage.getItem('preferred-language');
    const browserLanguage = navigator.language.split('-')[0];

    const detectedLanguage = urlLanguage || savedLanguage || browserLanguage || defaultLanguage;

    if (availableLanguages.some(l => l.code === detectedLanguage)) {
      setCurrentLanguage(detectedLanguage);
    }
  }, [router.query.lang, availableLanguages, defaultLanguage]);

  const setLanguage = (lang: string) => {
    if (!availableLanguages.some(l => l.code === lang)) return;

    setCurrentLanguage(lang);
    localStorage.setItem('preferred-language', lang);

    // Update URL to reflect language change
    const currentPath = router.asPath;
    const newPath = getLocalizedPath(currentPath, lang);
    router.push(newPath);

    // Update document direction for RTL languages
    const langInfo = availableLanguages.find(l => l.code === lang);
    if (langInfo) {
      document.dir = langInfo.direction;
      document.documentElement.lang = lang;
    }
  };

  const getLocalizedPath = (path: string, targetLang?: string): string => {
    const lang = targetLang || currentLanguage;

    // Remove existing language from path
    const pathWithoutLang = path.replace(/^\/[a-z]{2}(-[A-Z]{2})?\//, '/');

    // Add new language prefix
    if (lang === defaultLanguage) {
      return pathWithoutLang;
    }

    return `/${lang}${pathWithoutLang}`;
  };

  const isRTL = availableLanguages.find(l => l.code === currentLanguage)?.direction === 'rtl';

  return (
    <LanguageContext.Provider value={{
      currentLanguage,
      availableLanguages,
      setLanguage,
      getLocalizedPath,
      isRTL
    }}>
      {children}
    </LanguageContext.Provider>
  );
};

export const useLanguage = () => {
  const context = useContext(LanguageContext);
  if (!context) {
    throw new Error('useLanguage must be used within LanguageProvider');
  }
  return context;
};

// Language selector component
export const LanguageSelector: React.FC = () => {
  const { currentLanguage, availableLanguages, setLanguage } = useLanguage();

  return (
    <div className="language-selector">
      <select
        value={currentLanguage}
        onChange={(e) => setLanguage(e.target.value)}
        className="language-select"
      >
        {availableLanguages.map((lang) => (
          <option key={lang.code} value={lang.code}>
            {lang.flag && <span className="flag">{lang.flag}</span>}
            {lang.nativeName}
          </option>
        ))}
      </select>
    </div>
  );
};
```

### RTL Language Support

```css
/* RTL language support */
[dir="rtl"] {
  text-align: right;
}

[dir="rtl"] .sidebar {
  right: 0;
  left: auto;
  border-right: none;
  border-left: 1px solid #e1e5e9;
}

[dir="rtl"] .content {
  margin-right: var(--sidebar-width);
  margin-left: 0;
}

[dir="rtl"] .breadcrumbs {
  direction: rtl;
}

[dir="rtl"] .breadcrumbs::before {
  content: "«";
}

[dir="rtl"] .navigation-arrow {
  transform: scaleX(-1);
}

/* Text direction utilities */
.text-ltr {
  direction: ltr;
  text-align: left;
}

.text-rtl {
  direction: rtl;
  text-align: right;
}

/* Language-specific typography */
[lang="ar"] {
  font-family: 'Noto Sans Arabic', 'Cairo', sans-serif;
  line-height: 1.8;
}

[lang="ko"] {
  font-family: 'Noto Sans KR', 'Malgun Gothic', sans-serif;
  line-height: 1.7;
}

[lang="ja"] {
  font-family: 'Noto Sans JP', 'Yu Gothic', sans-serif;
  line-height: 1.7;
}

[lang="zh-CN"] {
  font-family: 'Noto Sans SC', 'PingFang SC', sans-serif;
  line-height: 1.7;
}

[lang="zh-TW"] {
  font-family: 'Noto Sans TC', 'PingFang TC', sans-serif;
  line-height: 1.7;
}
```

## Translation Quality Assurance

### Automated Quality Checks

```typescript
class TranslationQualityChecker {
  async validateTranslation(
    sourceContent: string,
    translatedContent: string,
    targetLanguage: string
  ): Promise<QualityReport> {
    const report: QualityReport = {
      language: targetLanguage,
      issues: [],
      score: 100,
      suggestions: []
    };

    // Check for missing translations
    const missingTranslations = await this.checkMissingTranslations(sourceContent, translatedContent);
    report.issues.push(...missingTranslations);

    // Check formatting consistency
    const formattingIssues = await this.checkFormatting(sourceContent, translatedContent);
    report.issues.push(...formattingIssues);

    // Check link integrity
    const linkIssues = await this.checkLinks(sourceContent, translatedContent);
    report.issues.push(...linkIssues);

    // Check terminology consistency
    const terminologyIssues = await this.checkTerminology(translatedContent, targetLanguage);
    report.issues.push(...terminologyIssues);

    // Calculate quality score
    report.score = this.calculateQualityScore(report.issues);

    // Generate suggestions
    report.suggestions = await this.generateSuggestions(report.issues, targetLanguage);

    return report;
  }

  private async checkMissingTranslations(source: string, translation: string): Promise<QualityIssue[]> {
    const issues: QualityIssue[] = [];

    // Extract headings from source
    const sourceHeadings = this.extractHeadings(source);
    const translationHeadings = this.extractHeadings(translation);

    if (sourceHeadings.length !== translationHeadings.length) {
      issues.push({
        type: 'missing_headings',
        severity: 'high',
        message: `Heading count mismatch: source has ${sourceHeadings.length}, translation has ${translationHeadings.length}`,
        line: 0
      });
    }

    // Check for untranslated code comments
    const untranslatedComments = this.findUntranslatedComments(translation);
    untranslatedComments.forEach(comment => {
      issues.push({
        type: 'untranslated_comment',
        severity: 'medium',
        message: `Untranslated comment: ${comment}`,
        line: this.getLineNumber(translation, comment)
      });
    });

    return issues;
  }

  private async checkTerminology(content: string, language: string): Promise<QualityIssue[]> {
    const issues: QualityIssue[] = [];
    const glossary = await this.loadTerminologyGlossary(language);

    for (const [term, correctTranslation] of glossary) {
      const incorrectUsage = this.findIncorrectTerminology(content, term, correctTranslation);
      issues.push(...incorrectUsage);
    }

    return issues;
  }

  private calculateQualityScore(issues: QualityIssue[]): number {
    let score = 100;

    issues.forEach(issue => {
      switch (issue.severity) {
        case 'high':
          score -= 10;
          break;
        case 'medium':
          score -= 5;
          break;
        case 'low':
          score -= 2;
          break;
      }
    });

    return Math.max(0, score);
  }
}

interface QualityReport {
  language: string;
  issues: QualityIssue[];
  score: number;
  suggestions: string[];
}

interface QualityIssue {
  type: string;
  severity: 'high' | 'medium' | 'low';
  message: string;
  line: number;
  suggestion?: string;
}
```

## Localization Best Practices

### Cultural Adaptation Guidelines

```typescript
interface LocalizationGuidelines {
  language: string;
  dateFormat: string;
  numberFormat: string;
  currencyFormat: string;
  addressFormat: string[];
  phoneFormat: string;
  nameFormat: 'first-last' | 'last-first';
  culturalConsiderations: {
    colors: Record<string, string>;
    imagery: string[];
    textDirection: 'ltr' | 'rtl';
    formalityLevel: 'formal' | 'informal';
  };
}

const localizationGuidelines: Record<string, LocalizationGuidelines> = {
  'ko': {
    language: 'Korean',
    dateFormat: 'YYYY년 MM월 DD일',
    numberFormat: '#,###.##',
    currencyFormat: '₩#,###',
    addressFormat: ['postal-code', 'city', 'street'],
    phoneFormat: '010-####-####',
    nameFormat: 'last-first',
    culturalConsiderations: {
      colors: {
        red: 'Avoid for text as it represents danger',
        white: 'Associated with death and mourning',
        yellow: 'Traditional color of royalty'
      },
      imagery: ['Avoid pointing gestures', 'Use both hands when presenting'],
      textDirection: 'ltr',
      formalityLevel: 'formal'
    }
  },

  'ja': {
    language: 'Japanese',
    dateFormat: 'YYYY年MM月DD日',
    numberFormat: '#,###.##',
    currencyFormat: '¥#,###',
    addressFormat: ['postal-code', 'prefecture', 'city', 'street'],
    phoneFormat: '03-####-####',
    nameFormat: 'last-first',
    culturalConsiderations: {
      colors: {
        white: 'Associated with purity and cleanliness',
        black: 'Formal and elegant',
        red: 'Signifies life and vitality'
      },
      imagery: ['Use bowing gestures', 'Minimize direct eye contact'],
      textDirection: 'ltr',
      formalityLevel: 'formal'
    }
  },

  'ar': {
    language: 'Arabic',
    dateFormat: 'DD/MM/YYYY',
    numberFormat: '###,###.##',
    currencyFormat: '### ر.س',
    addressFormat: ['country', 'city', 'district', 'street'],
    phoneFormat: '+966-##-###-####',
    nameFormat: 'first-last',
    culturalConsiderations: {
      colors: {
        green: 'Sacred color in Islam',
        blue: 'Associated with protection',
        yellow: 'Color of gold and prosperity'
      },
      imagery: ['Avoid left hand imagery', 'Use modest clothing in illustrations'],
      textDirection: 'rtl',
      formalityLevel: 'formal'
    }
  }
};
```

### Translation Memory and Consistency

```typescript
class TranslationMemory {
  private memory: Map<string, TranslationEntry> = new Map();

  async addTranslation(
    source: string,
    target: string,
    language: string,
    context?: string
  ): Promise<void> {
    const key = this.generateKey(source, language, context);
    const entry: TranslationEntry = {
      source,
      target,
      language,
      context,
      createdAt: new Date(),
      usageCount: 1
    };

    if (this.memory.has(key)) {
      const existing = this.memory.get(key)!;
      existing.usageCount++;
      existing.lastUsed = new Date();
    } else {
      this.memory.set(key, entry);
    }

    await this.persistMemory();
  }

  async findSimilarTranslations(
    source: string,
    language: string,
    threshold: number = 0.8
  ): Promise<TranslationSuggestion[]> {
    const suggestions: TranslationSuggestion[] = [];

    for (const [key, entry] of this.memory) {
      if (entry.language !== language) continue;

      const similarity = this.calculateSimilarity(source, entry.source);
      if (similarity >= threshold) {
        suggestions.push({
          source: entry.source,
          target: entry.target,
          similarity,
          context: entry.context,
          usageCount: entry.usageCount
        });
      }
    }

    return suggestions.sort((a, b) => b.similarity - a.similarity);
  }

  private calculateSimilarity(text1: string, text2: string): number {
    // Implement string similarity algorithm (e.g., Levenshtein distance)
    const distance = this.levenshteinDistance(text1, text2);
    const maxLength = Math.max(text1.length, text2.length);
    return 1 - (distance / maxLength);
  }

  private levenshteinDistance(str1: string, str2: string): number {
    const matrix = Array(str2.length + 1).fill(null).map(() =>
      Array(str1.length + 1).fill(null));

    for (let i = 0; i <= str1.length; i++) matrix[0][i] = i;
    for (let j = 0; j <= str2.length; j++) matrix[j][0] = j;

    for (let j = 1; j <= str2.length; j++) {
      for (let i = 1; i <= str1.length; i++) {
        const indicator = str1[i - 1] === str2[j - 1] ? 0 : 1;
        matrix[j][i] = Math.min(
          matrix[j][i - 1] + 1,
          matrix[j - 1][i] + 1,
          matrix[j - 1][i - 1] + indicator
        );
      }
    }

    return matrix[str2.length][str1.length];
  }
}
```

## Maintenance and Updates

### Translation Synchronization

```bash
#!/bin/bash
# Translation synchronization script

echo "Starting translation synchronization..."

# Check for source content changes
changed_files=$(git diff --name-only HEAD~1 HEAD -- "**/en/**/*.md")

if [ -z "$changed_files" ]; then
  echo "No source content changes detected."
  exit 0
fi

echo "Source content changes detected in:"
echo "$changed_files"

# Generate translation tasks for changed files
for file in $changed_files; do
  echo "Processing $file..."

  # Extract relative path
  rel_path=${file#*/en/}

  # Create translation tasks for each target language
  for lang in ko ja de fr es; do
    target_file="${file/\/en\//\/$lang\/}"

    # Check if translation exists and is outdated
    if [ -f "$target_file" ]; then
      source_time=$(stat -c %Y "$file")
      target_time=$(stat -c %Y "$target_file")

      if [ $source_time -gt $target_time ]; then
        echo "Creating update task for $target_file"
        npm run translation:create-task -- \
          --source "$file" \
          --target "$target_file" \
          --language "$lang" \
          --type "update"
      fi
    else
      echo "Creating new translation task for $target_file"
      npm run translation:create-task -- \
        --source "$file" \
        --target "$target_file" \
        --language "$lang" \
        --type "new"
    fi
  done
done

echo "Translation synchronization completed."
```

Multi-language support in BaiDocs enables global documentation delivery with comprehensive translation management, cultural adaptation, and synchronized content maintenance across all supported languages.