# 검색 설정

BaiDocs는 전문 텍스트 인덱싱, 패싯 검색 및 지능형 결과 순위 지정을 통한 고급 검색 기능을 제공합니다. 이 섹션에서는 검색 시스템 설정, 최적화 전략 및 고급 검색 기능을 다룹니다.

## 검색 아키텍처 개요

### 핵심 구성 요소

BaiDocs 검색 시스템은 다음으로 구성됩니다:

- **콘텐츠 인덱서**: 문서 콘텐츠를 처리하고 인덱싱
- **검색 엔진**: 쿼리 처리 및 결과 검색 처리
- **결과 순위 지정기**: 관련성 점수 및 결과 정렬 적용
- **검색 인터페이스**: 사용자 대면 검색 기능 제공
- **분석 엔진**: 검색 사용량 및 성능 지표 추적

### 기술 스택

- **Lunr.js**: 클라이언트 측 전문 텍스트 검색 인덱싱
- **Fuse.js**: 오타 허용을 위한 퍼지 검색 기능
- **Elasticsearch**: 고급 서버 측 검색 (선택사항)
- **Algolia**: 클라우드 기반 검색 서비스 통합 (선택사항)
- **사용자 정의 인덱서**: BaiDocs 네이티브 콘텐츠 처리

## 기본 검색 설정

### 검색 시스템 설정

```yaml
# book.config.yaml
search:
  enabled: true
  engine: "lunr"  # 옵션: lunr, fuse, elasticsearch, algolia

  # 인덱스 설정
  index:
    fields:
      - title
      - content
      - headings
      - tags
      - description
    boost:
      title: 10
      headings: 5
      content: 1
      tags: 3

  # 언어 지원
  languages:
    - en
    - ko
  stopWords:
    en: ["the", "a", "an", "and", "or", "but", "in", "on", "at", "to", "for", "of", "with", "by"]
    ko: ["그", "이", "저", "것", "들", "과", "와", "에", "의", "을", "를", "은", "는", "가"]

  # 검색 동작
  behavior:
    minQueryLength: 2
    maxResults: 50
    highlightMatches: true
    fuzzyMatching: true
    stemming: true

  # 결과 표시
  display:
    showSnippets: true
    snippetLength: 150
    maxSnippets: 3
    showBreadcrumbs: true
    groupBySection: false
```

### 고급 검색 설정

```yaml
search:
  # 다중 엔진 설정
  engines:
    primary: "elasticsearch"
    fallback: "lunr"

  # Elasticsearch 설정
  elasticsearch:
    host: "localhost:9200"
    index: "baidocs"
    settings:
      number_of_shards: 1
      number_of_replicas: 0
      analysis:
        analyzer:
          custom_analyzer:
            type: custom
            tokenizer: standard
            filter:
              - lowercase
              - stop
              - stemmer

  # Algolia 설정
  algolia:
    applicationId: "YOUR_APP_ID"
    apiKey: "YOUR_API_KEY"
    indexName: "baidocs_search"
    settings:
      searchableAttributes:
        - title
        - headings
        - content
        - tags
      attributesToHighlight:
        - title
        - content
      attributesToSnippet:
        - content:150

  # 사용자 정의 필드 처리
  fieldProcessors:
    content:
      stripHtml: true
      normalizeWhitespace: true
      extractCodeBlocks: true
    title:
      boost: 10
      exactMatchBoost: 20
    headings:
      hierarchyWeight: true
      levelBoosts:
        h1: 8
        h2: 6
        h3: 4
        h4: 2

  # 고급 인덱싱 옵션
  indexing:
    batchSize: 100
    updateStrategy: "incremental"  # 옵션: full, incremental
    scheduling:
      automatic: true
      interval: "1h"
      triggers:
        - content_change
        - navigation_update
        - configuration_change

  # 검색 분석
  analytics:
    enabled: true
    trackQueries: true
    trackResults: true
    trackClicks: true
    retention: "90d"
```

## 콘텐츠 인덱싱

### 인덱싱 프로세스

```typescript
interface IndexedContent {
  id: string;
  type: 'page' | 'section' | 'heading';
  title: string;
  content: string;
  headings: string[];
  url: string;
  breadcrumbs: string[];
  tags: string[];
  language: string;
  lastModified: Date;
  boost?: number;
}

class ContentIndexer {
  private index: lunr.Index;
  private documents: Map<string, IndexedContent> = new Map();

  constructor(config: SearchConfig) {
    this.index = lunr(function() {
      this.ref('id');
      this.field('title', { boost: 10 });
      this.field('headings', { boost: 5 });
      this.field('content');
      this.field('tags', { boost: 3 });

      // 언어 지원
      if (config.language === 'ko') {
        this.use(lunr.ko);
      }

      // 어간 추출 지원
      if (config.stemming) {
        this.use(lunr.stemmer);
      }
    });
  }

  async indexDocument(doc: IndexedContent): Promise<void> {
    // 콘텐츠 처리
    const processedDoc = await this.processContent(doc);

    // 인덱스에 추가
    this.documents.set(doc.id, processedDoc);

    // 필요 시 인덱스 재구축
    await this.rebuildIndex();
  }

  private async processContent(doc: IndexedContent): Promise<IndexedContent> {
    return {
      ...doc,
      content: this.cleanContent(doc.content),
      headings: this.extractHeadings(doc.content),
      tags: this.extractTags(doc.content)
    };
  }

  private cleanContent(content: string): string {
    // HTML 태그 제거
    const withoutHtml = content.replace(/<[^>]*>/g, ' ');

    // 공백 정규화
    const normalized = withoutHtml.replace(/\s+/g, ' ').trim();

    // 콘텐츠 인덱싱을 위해 코드 블록 제거
    const withoutCode = normalized.replace(/```[\s\S]*?```/g, '');

    return withoutCode;
  }

  private extractHeadings(content: string): string[] {
    const headingRegex = /^#{1,6}\s+(.+)$/gm;
    const headings: string[] = [];
    let match;

    while ((match = headingRegex.exec(content)) !== null) {
      headings.push(match[1].trim());
    }

    return headings;
  }
}
```

### 점진적 인덱싱

```typescript
class IncrementalIndexer {
  private lastIndexTime: Date = new Date(0);
  private indexedDocuments: Set<string> = new Set();

  async updateIndex(books: Book[]): Promise<void> {
    const modifiedDocuments = await this.findModifiedDocuments(books);

    if (modifiedDocuments.length === 0) {
      console.log('마지막 인덱스 이후 수정된 문서가 없습니다');
      return;
    }

    console.log(`${modifiedDocuments.length}개의 수정된 문서를 인덱싱 중`);

    for (const doc of modifiedDocuments) {
      await this.indexDocument(doc);
    }

    this.lastIndexTime = new Date();
    await this.saveIndexMetadata();
  }

  private async findModifiedDocuments(books: Book[]): Promise<IndexedContent[]> {
    const modifiedDocs: IndexedContent[] = [];

    for (const book of books) {
      for (const language of book.languages) {
        const files = await this.getContentFiles(book, language);

        for (const file of files) {
          const stats = await fs.stat(file.path);

          if (stats.mtime > this.lastIndexTime || !this.indexedDocuments.has(file.id)) {
            const content = await this.processFile(file, book, language);
            modifiedDocs.push(content);
          }
        }
      }
    }

    return modifiedDocs;
  }
}
```

## 검색 인터페이스 개발

### 기본 검색 컴포넌트

```tsx
import React, { useState, useCallback, useEffect } from 'react';
import { Search, X, Clock, FileText } from 'lucide-react';
import { useDebounce } from './hooks/useDebounce';

interface SearchResult {
  id: string;
  title: string;
  excerpt: string;
  url: string;
  breadcrumbs: string[];
  type: 'page' | 'section' | 'heading';
  score: number;
}

interface SearchProps {
  onClose?: () => void;
  placeholder?: string;
  maxResults?: number;
}

export const SearchInterface: React.FC<SearchProps> = ({
  onClose,
  placeholder = "문서 검색...",
  maxResults = 10
}) => {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState<SearchResult[]>([]);
  const [isLoading, setIsLoading] = useState(false);
  const [recentSearches, setRecentSearches] = useState<string[]>([]);

  const debouncedQuery = useDebounce(query, 300);

  const performSearch = useCallback(async (searchQuery: string) => {
    if (!searchQuery.trim()) {
      setResults([]);
      return;
    }

    setIsLoading(true);

    try {
      const response = await fetch(`/api/search?q=${encodeURIComponent(searchQuery)}&limit=${maxResults}`);
      const searchResults = await response.json();

      setResults(searchResults.results || []);

      // 검색 쿼리 추적
      if (searchQuery.trim() && !recentSearches.includes(searchQuery)) {
        const updatedRecent = [searchQuery, ...recentSearches.slice(0, 4)];
        setRecentSearches(updatedRecent);
        localStorage.setItem('recentSearches', JSON.stringify(updatedRecent));
      }

    } catch (error) {
      console.error('검색 실패:', error);
      setResults([]);
    } finally {
      setIsLoading(false);
    }
  }, [maxResults, recentSearches]);

  useEffect(() => {
    performSearch(debouncedQuery);
  }, [debouncedQuery, performSearch]);

  useEffect(() => {
    const saved = localStorage.getItem('recentSearches');
    if (saved) {
      setRecentSearches(JSON.parse(saved));
    }
  }, []);

  const highlightMatch = (text: string, query: string): React.ReactNode => {
    if (!query) return text;

    const regex = new RegExp(`(${query})`, 'gi');
    const parts = text.split(regex);

    return parts.map((part, index) =>
      regex.test(part) ? (
        <mark key={index} className="bg-yellow-200 px-1 rounded">
          {part}
        </mark>
      ) : (
        part
      )
    );
  };

  return (
    <div className="search-interface">
      <div className="search-input-container">
        <Search className="search-icon" size={20} />
        <input
          type="text"
          value={query}
          onChange={(e) => setQuery(e.target.value)}
          placeholder={placeholder}
          className="search-input"
          autoFocus
        />
        {onClose && (
          <button onClick={onClose} className="close-button">
            <X size={20} />
          </button>
        )}
      </div>

      <div className="search-results">
        {isLoading && (
          <div className="loading-indicator">
            검색 중...
          </div>
        )}

        {!query && recentSearches.length > 0 && (
          <div className="recent-searches">
            <h3 className="recent-title">최근 검색</h3>
            {recentSearches.map((search, index) => (
              <button
                key={index}
                onClick={() => setQuery(search)}
                className="recent-search-item"
              >
                <Clock size={16} />
                {search}
              </button>
            ))}
          </div>
        )}

        {results.length > 0 && (
          <div className="search-results-list">
            {results.map((result) => (
              <a
                key={result.id}
                href={result.url}
                className="search-result-item"
                onClick={onClose}
              >
                <div className="result-header">
                  <FileText size={16} className="result-icon" />
                  <h4 className="result-title">
                    {highlightMatch(result.title, query)}
                  </h4>
                </div>

                <div className="result-breadcrumbs">
                  {result.breadcrumbs.join(' > ')}
                </div>

                <p className="result-excerpt">
                  {highlightMatch(result.excerpt, query)}
                </p>

                <div className="result-metadata">
                  <span className="result-type">{result.type}</span>
                  <span className="result-score">
                    {Math.round(result.score * 100)}% 일치
                  </span>
                </div>
              </a>
            ))}
          </div>
        )}

        {query && !isLoading && results.length === 0 && (
          <div className="no-results">
            <p>"{query}"에 대한 결과가 없습니다</p>
            <p className="no-results-suggestion">
              다른 키워드를 시도하거나 철자를 확인해 주세요
            </p>
          </div>
        )}
      </div>
    </div>
  );
};
```

### 고급 검색 기능

```tsx
interface AdvancedSearchFilters {
  type?: 'page' | 'section' | 'heading';
  section?: string;
  language?: string;
  tags?: string[];
  dateRange?: {
    start: Date;
    end: Date;
  };
}

export const AdvancedSearch: React.FC = () => {
  const [filters, setFilters] = useState<AdvancedSearchFilters>({});
  const [isAdvancedMode, setIsAdvancedMode] = useState(false);

  const buildSearchQuery = () => {
    const queryParts: string[] = [];

    if (filters.type) {
      queryParts.push(`type:${filters.type}`);
    }

    if (filters.section) {
      queryParts.push(`section:"${filters.section}"`);
    }

    if (filters.language) {
      queryParts.push(`lang:${filters.language}`);
    }

    if (filters.tags && filters.tags.length > 0) {
      queryParts.push(`tags:(${filters.tags.join(' OR ')})`);
    }

    return queryParts.join(' AND ');
  };

  return (
    <div className="advanced-search">
      <div className="search-mode-toggle">
        <button
          onClick={() => setIsAdvancedMode(!isAdvancedMode)}
          className="toggle-button"
        >
          {isAdvancedMode ? '간단한 검색' : '고급 검색'}
        </button>
      </div>

      {isAdvancedMode && (
        <div className="advanced-filters">
          <div className="filter-group">
            <label>콘텐츠 유형</label>
            <select
              value={filters.type || ''}
              onChange={(e) => setFilters(f => ({ ...f, type: e.target.value as any }))}
            >
              <option value="">모든 유형</option>
              <option value="page">페이지</option>
              <option value="section">섹션</option>
              <option value="heading">제목</option>
            </select>
          </div>

          <div className="filter-group">
            <label>섹션</label>
            <select
              value={filters.section || ''}
              onChange={(e) => setFilters(f => ({ ...f, section: e.target.value }))}
            >
              <option value="">모든 섹션</option>
              <option value="getting-started">시작하기</option>
              <option value="api-reference">API 참조</option>
              <option value="tutorials">튜토리얼</option>
            </select>
          </div>

          <div className="filter-group">
            <label>언어</label>
            <select
              value={filters.language || ''}
              onChange={(e) => setFilters(f => ({ ...f, language: e.target.value }))}
            >
              <option value="">모든 언어</option>
              <option value="en">English</option>
              <option value="ko">한국어</option>
            </select>
          </div>
        </div>
      )}
    </div>
  );
};
```

## 검색 분석 및 최적화

### 분석 구현

```typescript
class SearchAnalytics {
  private analytics: AnalyticsClient;

  constructor(config: AnalyticsConfig) {
    this.analytics = new AnalyticsClient(config);
  }

  trackSearch(query: string, results: SearchResult[]): void {
    this.analytics.track('search_performed', {
      query: query.toLowerCase(),
      resultCount: results.length,
      timestamp: new Date().toISOString(),
      hasResults: results.length > 0
    });
  }

  trackResultClick(query: string, result: SearchResult, position: number): void {
    this.analytics.track('search_result_clicked', {
      query: query.toLowerCase(),
      resultId: result.id,
      resultTitle: result.title,
      position: position,
      score: result.score,
      timestamp: new Date().toISOString()
    });
  }

  async generateSearchReport(period: '7d' | '30d' | '90d'): Promise<SearchReport> {
    const events = await this.analytics.getEvents('search_performed', period);

    const topQueries = this.getTopQueries(events);
    const noResultQueries = this.getNoResultQueries(events);
    const clickThroughRate = this.calculateClickThroughRate(events);

    return {
      period,
      totalSearches: events.length,
      uniqueQueries: new Set(events.map(e => e.query)).size,
      topQueries,
      noResultQueries,
      clickThroughRate,
      averageResultCount: this.calculateAverageResultCount(events)
    };
  }

  private getTopQueries(events: SearchEvent[]): QueryStat[] {
    const queryCount = new Map<string, number>();

    events.forEach(event => {
      queryCount.set(event.query, (queryCount.get(event.query) || 0) + 1);
    });

    return Array.from(queryCount.entries())
      .map(([query, count]) => ({ query, count }))
      .sort((a, b) => b.count - a.count)
      .slice(0, 20);
  }

  private getNoResultQueries(events: SearchEvent[]): string[] {
    return events
      .filter(event => event.resultCount === 0)
      .map(event => event.query)
      .slice(0, 50);
  }
}
```

### 성능 최적화

```typescript
class SearchOptimizer {
  private cache: Map<string, CachedSearchResult> = new Map();
  private cacheTimeout = 5 * 60 * 1000; // 5분

  async optimizedSearch(query: string): Promise<SearchResult[]> {
    // 먼저 캐시 확인
    const cached = this.cache.get(query);
    if (cached && Date.now() - cached.timestamp < this.cacheTimeout) {
      return cached.results;
    }

    // 검색 수행
    const results = await this.performSearch(query);

    // 결과 캐싱
    this.cache.set(query, {
      results,
      timestamp: Date.now()
    });

    // 오래된 캐시 항목 정리
    this.cleanupCache();

    return results;
  }

  private cleanupCache(): void {
    const now = Date.now();
    for (const [key, value] of this.cache.entries()) {
      if (now - value.timestamp > this.cacheTimeout) {
        this.cache.delete(key);
      }
    }
  }

  async preloadPopularQueries(): Promise<void> {
    const popularQueries = await this.getPopularQueries();

    const preloadPromises = popularQueries.map(query =>
      this.optimizedSearch(query)
    );

    await Promise.all(preloadPromises);
    console.log(`${popularQueries.length}개의 인기 검색 쿼리를 미리 로드했습니다`);
  }
}
```

## 모범 사례 및 유지보수

### 검색을 위한 콘텐츠 최적화

```markdown
## 검색 최적화 가이드라인

### 콘텐츠 구조
- 설명적이고 키워드가 풍부한 제목 사용
- 관련 태그 및 메타데이터 포함
- 문서 전체에서 일관된 용어 유지
- 명확한 계층구조로 콘텐츠 구조화

### 키워드 전략
- 일반적인 사용자 검색 용어 조사
- 동의어 및 대체 표현 포함
- 콘텐츠에서 자연스럽게 키워드 사용
- 검색 가능성을 위한 제목 및 설명 최적화

### 기술적 최적화
- 모든 콘텐츠가 인덱싱되는지 확인
- 검색 성능을 정기적으로 모니터링
- 사용량에 기반하여 불용어 및 동의어 업데이트
- 쿼리 제안 및 자동 완성 구현

### 사용자 경험
- 명확한 검색 피드백 제공
- 검색 진행 상황 및 결과 수 표시
- 복잡한 콘텐츠에 대한 패싯 검색 구현
- 특정 섹션 내 검색 활성화
```

### 검색 인덱스 유지보수

```bash
#!/bin/bash
# 검색 인덱스 유지보수 스크립트

echo "검색 인덱스 유지보수 시작..."

# 현재 인덱스 백업
cp search-index.json search-index.backup.json

# 전체 인덱스 재구축
npm run search:rebuild

# 인덱스 무결성 검증
npm run search:verify

# 검색 분석 업데이트
npm run search:analytics:update

# 오래된 캐시 결과 정리
npm run search:cache:clean

echo "검색 인덱스 유지보수 완료."
```

BaiDocs 검색 기능은 최적의 사용자 경험과 콘텐츠 접근성을 위한 고급 커스터마이징 옵션을 갖춘 강력하고 확장 가능한 콘텐츠 검색 기능을 제공합니다.
