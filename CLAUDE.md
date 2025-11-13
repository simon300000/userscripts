# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a userscripts repository containing Tampermonkey/Greasemonkey browser scripts. Userscripts are JavaScript files that run in the browser to modify or enhance web pages.

## Code Style

- Use ESM-style imports (no semicolons)
- Plain vanilla JavaScript (no build system or transpilation)
- Scripts follow the Userscript metadata block format with headers like `@name`, `@match`, `@version`, etc.

## Userscript Structure

Each `.user.js` file must start with a metadata block:

```javascript
// ==UserScript==
// @name        Script Name
// @description Description
// @match       https://example.com/*
// @version     1.0.0
// @author      Author Name
// @license     MIT
// @supportURL  https://github.com/simon300000/userscripts/issues
// ==/UserScript==
```

Key metadata fields:
- `@match`: Specifies which URLs the script runs on
- `@version`: Version number (update when making changes)
- `@supportURL`: Points to GitHub issues

## Architecture Patterns

### Caching Strategy

The codebase implements a localStorage-based caching system with configurable TTL:

- `accessCache(key, param, func, cacheLife)`: Fetches data with cache, updates if stale
- `accessCurrentCache(key, param)`: Returns cached value without fetching
- Cache keys follow pattern: `mikan-cache-{key}-{param}`
- Cache stores both value and timestamp for TTL checks

### Rate Limiting

To avoid overwhelming external APIs:
- Sequential processing with `await wait(100)` delays between uncached requests
- Separate cache hit items (processed in batch) from cache misses (rate limited)

## Testing Userscripts

Userscripts cannot be tested with traditional test frameworks. To test:

1. Install Tampermonkey or Greasemonkey browser extension
2. Create new userscript and paste the code
3. Navigate to URLs matching the `@match` patterns
4. Check browser console for logs and errors

## Making Changes

When modifying existing userscripts:
1. Update the `@version` field in the metadata block (increment appropriately)
2. Test on actual target websites specified in `@match` directives
3. Verify caching behavior works correctly (check localStorage in DevTools)
