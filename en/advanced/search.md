# Search Configuration

BaiDocs provides advanced search capabilities with full-text indexing, faceted search, and intelligent result ranking. This section covers search system configuration, optimization strategies, and advanced search features.

## Search Architecture Overview

### Core Components

BaiDocs search system consists of:

- **Content Indexer**: Processes and indexes documentation content
- **Search Engine**: Handles query processing and result retrieval
- **Result Ranker**: Applies relevance scoring and result ordering
- **Search Interface**: Provides user-facing search functionality
- **Analytics Engine**: Tracks search usage and performance metrics

### Technology Stack

- **Lunr.js**: Client-side full-text search indexing
- **Fuse.js**: Fuzzy search capabilities for typo tolerance
- **Elasticsearch**: Advanced server-side search (optional)
- **Algolia**: Cloud-based search service integration (optional)
- **Custom Indexer**: BaiDocs native content processing

## Basic Search Configuration

### Search System Setup

```yaml
# book.config.yaml
search:
  enabled: true
  engine: "lunr"  # Options: lunr, fuse, elasticsearch, algolia

  # Index configuration
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

  # Language support
  languages:
    - en
    - ko
  stopWords:
    en: ["the", "a", "an", "and", "or", "but", "in", "on", "at", "to", "for", "of", "with", "by"]
    ko: ["그", "이", "저", "것", "들", "과", "와", "에", "의", "을", "를", "은", "는", "가"]

  # Search behavior
  behavior:
    minQueryLength: 2
    maxResults: 50
    highlightMatches: true
    fuzzyMatching: true
    stemming: true

  # Result display
  display:
    showSnippets: true
    snippetLength: 150
    maxSnippets: 3
    showBreadcrumbs: true
    groupBySection: false
```

### Advanced Search Configuration

```yaml
search:
  # Multi-engine configuration
  engines:
    primary: "elasticsearch"
    fallback: "lunr"

  # Elasticsearch configuration
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

  # Algolia configuration
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

  # Custom field processing
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

  # Advanced indexing options
  indexing:
    batchSize: 100
    updateStrategy: "incremental"  # Options: full, incremental
    scheduling:
      automatic: true
      interval: "1h"
      triggers:
        - content_change
        - navigation_update
        - configuration_change

  # Search analytics
  analytics:
    enabled: true
    trackQueries: true
    trackResults: true
    trackClicks: true
    retention: "90d"
```

## Content Indexing

### Indexing Process

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

      // Language support
      if (config.language === 'ko') {
        this.use(lunr.ko);
      }

      // Stemming support
      if (config.stemming) {
        this.use(lunr.stemmer);
      }
    });
  }

  async indexDocument(doc: IndexedContent): Promise<void> {
    // Process content
    const processedDoc = await this.processContent(doc);

    // Add to index
    this.documents.set(doc.id, processedDoc);

    // Rebuild index if necessary
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
    // Remove HTML tags
    const withoutHtml = content.replace(/<[^>]*>/g, ' ');

    // Normalize whitespace
    const normalized = withoutHtml.replace(/\s+/g, ' ').trim();

    // Remove code blocks for content indexing
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

### Incremental Indexing

```typescript
class IncrementalIndexer {
  private lastIndexTime: Date = new Date(0);
  private indexedDocuments: Set<string> = new Set();

  async updateIndex(books: Book[]): Promise<void> {
    const modifiedDocuments = await this.findModifiedDocuments(books);

    if (modifiedDocuments.length === 0) {
      console.log('No documents modified since last index');
      return;
    }

    console.log(`Indexing ${modifiedDocuments.length} modified documents`);

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

## Search Interface Development

### Basic Search Component

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
  placeholder = "Search documentation...",
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

      // Track search query
      if (searchQuery.trim() && !recentSearches.includes(searchQuery)) {
        const updatedRecent = [searchQuery, ...recentSearches.slice(0, 4)];
        setRecentSearches(updatedRecent);
        localStorage.setItem('recentSearches', JSON.stringify(updatedRecent));
      }

    } catch (error) {
      console.error('Search failed:', error);
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
            Searching...
          </div>
        )}

        {!query && recentSearches.length > 0 && (
          <div className="recent-searches">
            <h3 className="recent-title">Recent Searches</h3>
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
                    {Math.round(result.score * 100)}% match
                  </span>
                </div>
              </a>
            ))}
          </div>
        )}

        {query && !isLoading && results.length === 0 && (
          <div className="no-results">
            <p>No results found for "{query}"</p>
            <p className="no-results-suggestion">
              Try different keywords or check spelling
            </p>
          </div>
        )}
      </div>
    </div>
  );
};
```

### Advanced Search Features

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
          {isAdvancedMode ? 'Simple Search' : 'Advanced Search'}
        </button>
      </div>

      {isAdvancedMode && (
        <div className="advanced-filters">
          <div className="filter-group">
            <label>Content Type</label>
            <select
              value={filters.type || ''}
              onChange={(e) => setFilters(f => ({ ...f, type: e.target.value as any }))}
            >
              <option value="">All Types</option>
              <option value="page">Pages</option>
              <option value="section">Sections</option>
              <option value="heading">Headings</option>
            </select>
          </div>

          <div className="filter-group">
            <label>Section</label>
            <select
              value={filters.section || ''}
              onChange={(e) => setFilters(f => ({ ...f, section: e.target.value }))}
            >
              <option value="">All Sections</option>
              <option value="getting-started">Getting Started</option>
              <option value="api-reference">API Reference</option>
              <option value="tutorials">Tutorials</option>
            </select>
          </div>

          <div className="filter-group">
            <label>Language</label>
            <select
              value={filters.language || ''}
              onChange={(e) => setFilters(f => ({ ...f, language: e.target.value }))}
            >
              <option value="">All Languages</option>
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

## Search Analytics and Optimization

### Analytics Implementation

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

### Performance Optimization

```typescript
class SearchOptimizer {
  private cache: Map<string, CachedSearchResult> = new Map();
  private cacheTimeout = 5 * 60 * 1000; // 5 minutes

  async optimizedSearch(query: string): Promise<SearchResult[]> {
    // Check cache first
    const cached = this.cache.get(query);
    if (cached && Date.now() - cached.timestamp < this.cacheTimeout) {
      return cached.results;
    }

    // Perform search
    const results = await this.performSearch(query);

    // Cache results
    this.cache.set(query, {
      results,
      timestamp: Date.now()
    });

    // Clean old cache entries
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
    console.log(`Preloaded ${popularQueries.length} popular search queries`);
  }
}
```

## Best Practices and Maintenance

### Content Optimization for Search

```markdown
## Search Optimization Guidelines

### Content Structure
- Use descriptive, keyword-rich headings
- Include relevant tags and metadata
- Maintain consistent terminology across documents
- Structure content with clear hierarchies

### Keyword Strategy
- Research common user search terms
- Include synonyms and alternative phrasings
- Use keywords naturally in content
- Optimize titles and descriptions for searchability

### Technical Optimization
- Ensure all content is indexed
- Monitor search performance regularly
- Update stop words and synonyms based on usage
- Implement query suggestions and auto-complete

### User Experience
- Provide clear search feedback
- Show search progress and results count
- Implement faceted search for complex content
- Enable search within specific sections
```

### Search Index Maintenance

```bash
#!/bin/bash
# Search index maintenance script

echo "Starting search index maintenance..."

# Backup current index
cp search-index.json search-index.backup.json

# Rebuild full index
npm run search:rebuild

# Verify index integrity
npm run search:verify

# Update search analytics
npm run search:analytics:update

# Clean up old cached results
npm run search:cache:clean

echo "Search index maintenance completed."
```

BaiDocs search capabilities provide powerful, scalable content discovery with advanced customization options for optimal user experience and content accessibility.