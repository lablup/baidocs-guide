# 다국어 지원

BaiDocs는 동기화된 네비게이션, 콘텐츠 조정 및 번역 관리 도구를 통한 포괄적인 다국어 문서 기능을 제공합니다. 이 섹션에서는 다국어 설정, 콘텐츠 관리 및 지역화 모범 사례를 다룹니다.

## 다국어 아키텍처

### 언어 지원 프레임워크

BaiDocs 다국어 시스템은 다음을 포함합니다:

- **동기화된 네비게이션**: 모든 언어 버전에서 일관된 구조
- **콘텐츠 조정**: 누락된 번역의 자동 감지
- **언어 전환**: 지속적인 네비게이션 상태로 원활한 사용자 경험
- **번역 워크플로**: 번역 프로세스 관리 도구
- **지역화 지원**: 단순한 번역을 넘어선 문화적 적응
- **RTL 언어 지원**: 우측에서 좌측으로 텍스트 렌더링 기능

### 디렉터리 구조

```
content/
└── documentation-book/
    ├── book.config.yaml
    ├── en/                     # 영어 콘텐츠
    │   ├── introduction/
    │   │   ├── index.md
    │   │   └── overview.md
    │   └── user-guide/
    │       └── getting-started.md
    ├── ko/                     # 한국어 콘텐츠
    │   ├── introduction/
    │   │   ├── index.md
    │   │   └── overview.md
    │   └── user-guide/
    │       └── getting-started.md
    ├── ja/                     # 일본어 콘텐츠
    │   ├── introduction/
    │   │   ├── index.md
    │   │   └── overview.md
    │   └── user-guide/
    │       └── getting-started.md
    ├── images/                 # 공유 이미지
    └── assets/                 # 공유 자산
```

## 언어 설정

### 기본 다국어 설정

```yaml
# book.config.yaml
id: multilingual-documentation
title: Multi-language Documentation
description: Comprehensive guide supporting multiple languages
emoji: 🌐
type: standalone

# 언어 설정
languages:
  - en      # 영어 (기본값)
  - ko      # 한국어
  - ja      # 일본어
  - de      # 독일어
  - fr      # 프랑스어
  - es      # 스페인어
  - zh-CN   # 간체 중국어
  - zh-TW   # 번체 중국어
  - ar      # 아랍어

defaultLanguage: en

# 언어별 메타데이터
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

### 동기화된 네비게이션 구조

```yaml
# 모든 언어에서 일관된 네비게이션
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

## 콘텐츠 관리 전략

### 번역 조정

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

    // 번역 파일에서 메타데이터 추출
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

### 번역 워크플로 관리

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
    if (!task) throw new Error(`작업 ${taskId}를 찾을 수 없습니다`);

    task.assignedTo = translator;
    task.status = 'in_progress';
    await this.saveTaskState(task);

    // 번역자에게 알림
    await this.sendTaskAssignmentNotification(task, translator);
  }

  async submitForReview(taskId: string, translatedContent: string): Promise<void> {
    const task = this.tasks.get(taskId);
    if (!task) throw new Error(`작업 ${taskId}를 찾을 수 없습니다`);

    // 메타데이터와 함께 번역된 콘텐츠 저장
    const contentWithMetadata = this.addTranslationMetadata(translatedContent, task);
    await fs.writeFile(task.targetFile, contentWithMetadata);

    task.status = 'review';
    await this.saveTaskState(task);

    // 검토자에게 알림
    await this.sendReviewNotification(task);
  }

  async approveTranslation(taskId: string, reviewerId: string): Promise<void> {
    const task = this.tasks.get(taskId);
    if (!task) throw new Error(`작업 ${taskId}를 찾을 수 없습니다`);

    task.status = 'completed';
    task.reviewedBy = reviewerId;
    task.completedAt = new Date();
    await this.saveTaskState(task);

    // 필요시 네비게이션 업데이트
    await this.updateNavigationTranslation(task);

    // 완료 알림
    await this.sendCompletionNotification(task);
  }

  private estimateTranslationTime(wordCount: number, targetLanguage: string): number {
    const baseRate = 250; // 시간당 단어 수
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

## 언어 전환 구현

### 클라이언트 측 언어 전환

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
    // URL 또는 localStorage에서 언어 감지
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

    // 언어 변경을 반영하도록 URL 업데이트
    const currentPath = router.asPath;
    const newPath = getLocalizedPath(currentPath, lang);
    router.push(newPath);

    // RTL 언어에 대한 문서 방향 업데이트
    const langInfo = availableLanguages.find(l => l.code === lang);
    if (langInfo) {
      document.dir = langInfo.direction;
      document.documentElement.lang = lang;
    }
  };

  const getLocalizedPath = (path: string, targetLang?: string): string => {
    const lang = targetLang || currentLanguage;

    // 경로에서 기존 언어 제거
    const pathWithoutLang = path.replace(/^\/[a-z]{2}(-[A-Z]{2})?\//, '/');

    // 새 언어 접두사 추가
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
    throw new Error('useLanguage는 LanguageProvider 내에서 사용되어야 합니다');
  }
  return context;
};

// 언어 선택기 컴포넌트
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

### RTL 언어 지원

```css
/* RTL 언어 지원 */
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

/* 텍스트 방향 유틸리티 */
.text-ltr {
  direction: ltr;
  text-align: left;
}

.text-rtl {
  direction: rtl;
  text-align: right;
}

/* 언어별 타이포그래피 */
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

## 번역 품질 보증

### 자동화된 품질 검사

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

    // 누락된 번역 확인
    const missingTranslations = await this.checkMissingTranslations(sourceContent, translatedContent);
    report.issues.push(...missingTranslations);

    // 형식 일관성 확인
    const formattingIssues = await this.checkFormatting(sourceContent, translatedContent);
    report.issues.push(...formattingIssues);

    // 링크 무결성 확인
    const linkIssues = await this.checkLinks(sourceContent, translatedContent);
    report.issues.push(...linkIssues);

    // 용어 일관성 확인
    const terminologyIssues = await this.checkTerminology(translatedContent, targetLanguage);
    report.issues.push(...terminologyIssues);

    // 품질 점수 계산
    report.score = this.calculateQualityScore(report.issues);

    // 제안 생성
    report.suggestions = await this.generateSuggestions(report.issues, targetLanguage);

    return report;
  }

  private async checkMissingTranslations(source: string, translation: string): Promise<QualityIssue[]> {
    const issues: QualityIssue[] = [];

    // 소스에서 제목 추출
    const sourceHeadings = this.extractHeadings(source);
    const translationHeadings = this.extractHeadings(translation);

    if (sourceHeadings.length !== translationHeadings.length) {
      issues.push({
        type: 'missing_headings',
        severity: 'high',
        message: `제목 수 불일치: 소스 ${sourceHeadings.length}개, 번역 ${translationHeadings.length}개`,
        line: 0
      });
    }

    // 번역되지 않은 코드 주석 확인
    const untranslatedComments = this.findUntranslatedComments(translation);
    untranslatedComments.forEach(comment => {
      issues.push({
        type: 'untranslated_comment',
        severity: 'medium',
        message: `번역되지 않은 주석: ${comment}`,
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

## 지역화 모범 사례

### 문화적 적응 가이드라인

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
        red: '위험을 나타내므로 텍스트에는 피하십시오',
        white: '죽음과 애도와 관련됨',
        yellow: '왕족의 전통 색상'
      },
      imagery: ['가리키는 제스처 피하기', '제시할 때 양손 사용'],
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
        white: '순수함과 청결함과 관련됨',
        black: '격식 있고 우아함',
        red: '생명과 활력을 상징'
      },
      imagery: ['절하는 제스처 사용', '직접적인 시선 접촉 최소화'],
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
        green: '이슬람에서 신성한 색상',
        blue: '보호와 관련됨',
        yellow: '금과 번영의 색상'
      },
      imagery: ['왼손 이미지 피하기', '일러스트에서 보수적인 복장 사용'],
      textDirection: 'rtl',
      formalityLevel: 'formal'
    }
  }
};
```

### 번역 메모리 및 일관성

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
    // 문자열 유사도 알고리즘 구현 (예: 편집 거리)
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

## 유지보수 및 업데이트

### 번역 동기화

```bash
#!/bin/bash
# 번역 동기화 스크립트

echo "번역 동기화 시작..."

# 소스 콘텐츠 변경 확인
changed_files=$(git diff --name-only HEAD~1 HEAD -- "**/en/**/*.md")

if [ -z "$changed_files" ]; then
  echo "소스 콘텐츠 변경이 감지되지 않았습니다."
  exit 0
fi

echo "소스 콘텐츠 변경이 감지되었습니다:"
echo "$changed_files"

# 변경된 파일에 대한 번역 작업 생성
for file in $changed_files; do
  echo "$file 처리 중..."

  # 상대 경로 추출
  rel_path=${file#*/en/}

  # 각 대상 언어에 대한 번역 작업 생성
  for lang in ko ja de fr es; do
    target_file="${file/\/en\//\/$lang\/}"

    # 번역이 존재하고 오래되었는지 확인
    if [ -f "$target_file" ]; then
      source_time=$(stat -c %Y "$file")
      target_time=$(stat -c %Y "$target_file")

      if [ $source_time -gt $target_time ]; then
        echo "$target_file에 대한 업데이트 작업 생성"
        npm run translation:create-task -- \
          --source "$file" \
          --target "$target_file" \
          --language "$lang" \
          --type "update"
      fi
    else
      echo "$target_file에 대한 새 번역 작업 생성"
      npm run translation:create-task -- \
        --source "$file" \
        --target "$target_file" \
        --language "$lang" \
        --type "new"
    fi
  done
done

echo "번역 동기화 완료."
```

BaiDocs의 다국어 지원은 포괄적인 번역 관리, 문화적 적응 및 지원되는 모든 언어에서 동기화된 콘텐츠 유지보수를 통한 글로벌 문서 제공을 가능하게 합니다.
