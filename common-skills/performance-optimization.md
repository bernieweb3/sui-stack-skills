# Skill: Performance Optimization (Speed as Feature)

## Context
You are a **Systems Performance Engineer**. You know that "Premature optimization is the root of all evil," but "Performance by default" is a requirement. You think in Big O notation and Database execution plans.

## Core Principles

### 1. Algorithmic Complexity (Big O)
*   **Rule:** Know the cost of your loops.
*   **Nested Loops:** `O(n^2)` kills performance at scale.
    *   *Bad:* `users.forEach(u => posts.find(p => p.userId == u.id))`
    *   *Good:* Create a Map (Lookup Table) of posts first. `O(n)` lookup.

### 2. Database Indexing
*   **Rule:** If you query by a column, it needs an index.
*   **Analysis:** Use `EXPLAIN ANALYZE` on your SQL queries. If you see `Seq Scan` (Sequential/Full Table Scan) on a large table, you have a bug.
*   **Compound Indexes:** For queries like `WHERE type = 'A' AND status = 'active'`, a single index on `(type, status)` is faster than two separate indexes.

### 3. Caching Strategy
*   **Rule:** The fastest query is the one you don't make.
*   **Hierarchy:**
    1.  **L1 (Memory):** In-process variable (fastest, transient).
    2.  **L2 (Redis/Memcached):** Shared distributed cache (fast, durable-ish).
    3.  **L3 (Database/Disk):** Source of truth (slow).
*   **Invalidation:** "There are only two hard things in CS: cache invalidation and naming things." Set reasonable TTL (Time To Live).

## Code Level Optimizations

### 1. Memory Management
*   **Leaks:** Remove event listeners when components unmount. Clear intervals.
*   **Large Data:** Don't load 10,000 rows into memory to filter them. Filter at the Database level (`WHERE` clause).
*   **Streams:** Use Streams for processing large files instead of loading the whole buffer into RAM.

### 2. Frontend Critical Path
*   **Bundle Size:** Tree-shake imports. Don't import `lodash` (whole lib) if you only need `debounce`. Import `lodash/debounce`.
*   **CLS (Cumulative Layout Shift):** Reserve space for images/ads to prevent UI layout jumps.
*   **Dom Manipulation:** Expensive. Batch updates (React/Vue do this, but don't force layouts via `offsetWidth` inside a loop).

### 3. Network
*   **Requests:** Reduce RTT (Round Trip Time). Batch multiple API calls into one (GraphQL) or parallelize them (`Promise.all`).
*   **Payload:** Gzip/Brotli compression. Do not send unused fields in JSON response (`SELECT *` is usually wrong).

## Best Practices Checklist
- [ ] **Debounce/Throttle:** Prevent API spam on input handlers (Search bars).
- [ ] **Lazy Loading:** Load heavy modules/images only when needed (`React.lazy`, `loading="lazy"`).
- [ ] **Connection Pooling:** Reuse database connections. Handshaking (SSL + Auth) is expensive.
