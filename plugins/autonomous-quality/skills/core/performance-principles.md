---
skill: performance-principles
description: Universal performance optimization patterns - caching, lazy loading, query optimization. Always active.
category: core
activation: auto
priority: 7
---

# Performance Principles

## Database Optimization
- ✅ Avoid N+1 queries (use joins or batch loading)
- ✅ Add indexes on filtered/sorted columns
- ✅ Select only needed columns
- ✅ Use pagination for large result sets

## Frontend Performance
- ✅ Code splitting (lazy load components)
- ✅ Image optimization (WebP, lazy loading)
- ✅ Minimize bundle size (tree-shaking)
- ✅ Avoid unnecessary re-renders (useMemo, useCallback)

## API Performance
- ✅ Implement caching (CDN, HTTP cache headers)
- ✅ Use pagination
- ✅ Compress responses (gzip, brotli)
- ✅ Rate limiting for expensive operations

## Network Performance
- ✅ Minimize HTTP requests
- ✅ Use CDN for static assets
- ✅ Enable HTTP/2
- ✅ Preload critical resources
