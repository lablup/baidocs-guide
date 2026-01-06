---
title: Search Configuration
description: Configure and optimize search functionality in BaiDocs
---

# Search Configuration

BaiDocs includes a powerful client-side search system powered by Fuse.js. Search is built at compile-time and works without any backend server.

## How Search Works

### Build-Time Indexing

During build, BaiDocs:

1. **Crawls** all markdown files
2. **Extracts** text content and headings
3. **Cleans** markdown syntax
4. **Creates** search index JSON
5. **Saves** to `public/search-index.json`

### Client-Side Search

When users search:

1. Search index loads in browser
2. Fuse.js performs fuzzy matching
3. Results ranked by relevance
4. Instant, no server required

:::tip
Search works completely offline once the page is loaded!
:::

## Enabling Search

Search is enabled by default. To rebuild the search index:

```bash
# Build search index only
pnpm build:search

# Full build (includes search)
pnpm build
```

## Search Features

### Fuzzy Matching

Fuse.js provides intelligent fuzzy search:

- **Typo tolerance**: "documnetation" finds "documentation"
- **Partial matching**: "react comp" finds "React Components"
- **Word order**: Flexible word order matching

### Search Modal

Access search with:
- Keyboard: <kbd>Cmd</kbd>/<kbd>Ctrl</kbd> + <kbd>K</kbd>
- Click: Search icon in navigation
- Touch: Tap search icon on mobile

### Result Highlighting

Search results show:
- Matching text with highlights
- Book and chapter context
- Page title and description
- Direct links to content

## Configuration

Configure search in `baidocs.config.yaml`:

```yaml
search:
  enabled: true
  threshold: 0.3          # Match threshold (0-1, lower = stricter)
  distance: 100           # Maximum distance for matches
  minMatchCharLength: 2   # Minimum characters to search
  keys:                   # Fields to search
    - title
    - content
    - headings
  maxResults: 50          # Maximum results to show
```

## Search Options

### Threshold

Controls match strictness (0.0 to 1.0):

```yaml
search:
  threshold: 0.0  # Exact match only
  threshold: 0.3  # Balanced (recommended)
  threshold: 0.6  # Fuzzy, more results
```

**Examples:**

| Threshold | Search: "react" | Results Include |
|-----------|-----------------|-----------------|
| 0.0 | "react" | Exact matches only |
| 0.3 | "react" | "react", "React", "reactor" |
| 0.6 | "react" | Above + "create", "reach" |

:::tip
Start with 0.3 and adjust based on user feedback.
:::

### Distance

Maximum character distance for matches:

```yaml
search:
  distance: 50   # Strict proximity
  distance: 100  # Balanced (recommended)
  distance: 200  # Loose proximity
```

### Search Keys

Fields to include in search:

```yaml
search:
  keys:
    - title        # Page titles (highest weight)
    - content      # Page content
    - headings     # Section headings
    - description  # Page descriptions
    - tags         # Page tags
```

### Weights

Prioritize certain fields:

```yaml
search:
  keys:
    - name: title
      weight: 0.5        # 50% importance
    - name: headings
      weight: 0.3        # 30% importance
    - name: content
      weight: 0.2        # 20% importance
```

## Optimizing Search

### Index Size

Search index size depends on content:

| Content Size | Index Size |
|--------------|------------|
| 50 pages | ~500 KB |
| 100 pages | ~1 MB |
| 300 pages | ~2 MB |
| 500 pages | ~3-4 MB |

### Reducing Index Size

If index is too large:

1. **Exclude content**: Search titles and headings only
   ```yaml
   search:
     keys:
       - title
       - headings
     # Don't include full content
   ```

2. **Limit depth**: Don't index all content
   ```yaml
   search:
     maxContentLength: 200  # Characters per page
   ```

3. **Exclude books**: Skip certain books
   ```yaml
   search:
     excludeBooks:
       - archived-docs
       - old-version
   ```

### Improving Performance

Speed up search:

1. **Lazy loading**: Load index when needed
2. **Debounce input**: Wait for user to finish typing
3. **Limit results**: Show fewer results initially
4. **Paginate**: Load more results on scroll

## Search UX

### Keyboard Shortcuts

Default shortcuts:

| Key | Action |
|-----|--------|
| <kbd>Cmd/Ctrl</kbd> + <kbd>K</kbd> | Open search |
| <kbd>↓</kbd> / <kbd>↑</kbd> | Navigate results |
| <kbd>Enter</kbd> | Go to selected result |
| <kbd>Esc</kbd> | Close search modal |

### Search Placeholder

Customize placeholder text:

```yaml
search:
  placeholder:
    en: "Search documentation..."
    ko: "문서 검색..."
```

### Empty State

Customize no results message:

```yaml
search:
  emptyState:
    en: "No results found. Try different keywords."
    ko: "결과가 없습니다. 다른 키워드를 시도해보세요."
```

## Advanced Features

### Multi-language Search

Search works across languages:

```yaml
search:
  multiLanguage: true
  languageField: "language"
```

Users can:
- Search all languages at once
- Filter by specific language
- See language indicator in results

### Book Filtering

Filter results by book:

```yaml
search:
  bookFilter: true
```

Search UI shows:
- Dropdown to select book
- "All Books" option
- Result counts per book

### Recent Searches

Show recent search history:

```yaml
search:
  recentSearches: true
  maxRecent: 5
```

### Popular Searches

Display popular searches:

```yaml
search:
  popularSearches:
    - "getting started"
    - "installation"
    - "configuration"
```

## Customizing Search UI

### Search Modal Styling

Customize in your theme CSS:

```css
/* Search modal */
.search-modal {
  background: white;
  border-radius: 12px;
  box-shadow: 0 10px 40px rgba(0,0,0,0.1);
  max-width: 600px;
}

/* Search input */
.search-input {
  border: 2px solid #e5e7eb;
  padding: 12px 16px;
  font-size: 16px;
}

.search-input:focus {
  border-color: var(--primary-color);
  outline: none;
}

/* Search results */
.search-results {
  max-height: 400px;
  overflow-y: auto;
}

.search-result-item {
  padding: 12px 16px;
  border-bottom: 1px solid #f3f4f6;
  cursor: pointer;
  transition: background 0.2s;
}

.search-result-item:hover,
.search-result-item.selected {
  background: #f9fafb;
}

/* Highlight matches */
.search-highlight {
  background: yellow;
  font-weight: bold;
  padding: 0 2px;
}
```

### Search Result Template

Customize result display:

```javascript
// Customize in apps/viewer/src/components/search/SearchResult.tsx
function SearchResult({ result }) {
  return (
    <div className="search-result-item">
      <div className="result-title">{result.title}</div>
      <div className="result-breadcrumb">
        {result.book} › {result.chapter}
      </div>
      <div className="result-excerpt">
        {highlightMatches(result.content, result.matches)}
      </div>
    </div>
  );
}
```

## Troubleshooting

### Search Not Working

If search doesn't work:

1. ✅ Check if `public/search-index.json` exists
2. ✅ Rebuild search: `pnpm build:search`
3. ✅ Check browser console for errors
4. ✅ Verify search is enabled in config
5. ✅ Clear browser cache

### No Results

If search returns no results:

1. ✅ Check threshold setting (try 0.4-0.6)
2. ✅ Verify content is indexed
3. ✅ Check search keys configuration
4. ✅ Try simpler search terms

### Slow Search

If search is slow:

1. **Reduce index size**: Limit content indexed
2. **Debounce input**: Add delay before searching
3. **Limit results**: Show fewer results
4. **Optimize threshold**: Lower for faster matching

### Large Index File

If search index is too large:

1. **Exclude content**: Index titles only
2. **Exclude books**: Skip certain books
3. **Truncate content**: Limit characters per page
4. **Split indexes**: Separate index per book

## Analytics

Track search usage:

```javascript
// Add to your analytics
function trackSearch(query, resultsCount) {
  analytics.track('search', {
    query: query,
    results: resultsCount,
    timestamp: Date.now()
  });
}
```

Useful metrics:
- Popular search terms
- Queries with no results
- Average results per query
- Search→click conversion

## Best Practices

### 1. Keep Index Updated

Rebuild search after content changes:

```bash
# After updating content
pnpm build:search
```

### 2. Test Search Quality

Regularly test search with:
- Common queries
- Typos and variations
- Technical terms
- Multi-word queries

### 3. Monitor Performance

Track:
- Index file size
- Search response time
- Result relevance
- User behavior

### 4. Provide Fallbacks

If search fails:
- Show navigation menu
- Link to site map
- Provide contact options

## Next Steps

Learn about [Multi-language Support](multi-language.md) to create documentation in multiple languages!
