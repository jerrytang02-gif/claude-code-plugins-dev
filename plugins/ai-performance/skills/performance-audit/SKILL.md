---
name: Performance Audit
description: Guide for analyzing and improving application performance including identifying bottlenecks, implementing caching, and optimizing queries. This skill should be used when reviewing performance issues or optimizing code.
---

# Performance Audit Skill

This skill provides elite performance engineering expertise for making applications lightning-fast through systematic optimization.

## When to Use This Skill

Invoke this skill when:
- Analyzing slow page loads or response times
- Identifying performance bottlenecks in code execution
- Designing and implementing caching strategies
- Optimizing database queries and preventing N+1 problems
- Reducing memory consumption or investigating memory leaks
- Improving asset delivery (compression, minification, bundling)
- Implementing lazy loading or code splitting
- Profiling and benchmarking code performance
- Reviewing new features for performance implications
- Establishing performance budgets for critical user journeys

## Core Performance Expertise

### 1. Performance Analysis Methodology

To analyze performance issues effectively:

**Measure First**: Always establish baseline metrics before optimization. Use profiling tools, timing measurements, and performance monitoring to identify actual bottlenecks rather than assumed ones.

**Prioritize Impact**: Focus on optimizations that provide the greatest performance improvement relative to implementation effort. Target the critical path and high-traffic code paths first.

**Consider Trade-offs**: Evaluate each optimization for its impact on code maintainability, complexity, and resource usage. Sometimes a 10% performance gain isn't worth a 50% increase in code complexity.

**Validate Improvements**: After implementing optimizations, measure again to confirm actual performance gains. Be prepared to roll back changes that don't deliver meaningful improvements.

### 2. Caching Strategies

To implement effective caching:

- Choose the appropriate caching layer (browser cache, CDN, application cache, database query cache, computed result cache)
- Implement proper cache invalidation strategies to prevent stale data issues
- Use cache keys that are specific enough to avoid collisions but general enough to maximize hit rates
- Set appropriate TTLs based on data volatility and business requirements
- Implement cache warming for predictable high-traffic scenarios
- Use cache-aside, write-through, or write-behind patterns as appropriate
- Monitor cache hit rates and adjust strategies based on real usage patterns

**Key Rules:**
- Never cache without considering invalidation strategy
- Always measure cache hit rates to validate effectiveness
- Balance cache complexity against actual performance benefits

### 3. Frontend Performance Optimization

To optimize frontend performance, focus on:

**Critical Rendering Path:**
- Minimize render-blocking resources (CSS, JavaScript)
- Prioritize above-the-fold content loading
- Use resource hints (preload, prefetch, preconnect)

**Asset Optimization:**
- Compress and minify JavaScript and CSS
- Optimize images (format, compression, responsive sizes)
- Implement lazy loading for images and off-screen content
- Use code splitting to reduce initial bundle size

**Runtime Performance:**
- Debounce and throttle user interaction handlers
- Use virtual scrolling for large lists
- Offload CPU-intensive tasks to Web Workers
- Implement efficient React re-render patterns (memoization, useMemo, useCallback)

**Key Rules:**
- Always measure with real-world conditions (throttled network, low-end devices)
- Focus on First Contentful Paint (FCP) and Time to Interactive (TTI)
- Avoid premature optimization of rarely-executed code

### 4. Backend Performance Optimization

To optimize backend performance, address:

**Database Performance:**
- Add indexes on frequently queried columns
- Prevent N+1 query problems with eager loading
- Use query explain plans to identify slow operations
- Implement connection pooling for database connections
- Consider read replicas for high-traffic read operations

**Request Processing:**
- Implement pagination and filtering for large datasets
- Use asynchronous processing for long-running tasks
- Batch similar operations to reduce overhead
- Implement request/response compression

**Resource Management:**
- Use connection pooling for external services
- Implement circuit breakers for failing dependencies
- Set appropriate timeouts to prevent resource exhaustion

**Key Rules:**
- Database queries should use indexes, not full table scans
- Long-running operations belong in background jobs, not HTTP requests
- Always implement pagination for unbounded result sets

### 5. Infrastructure Performance

To optimize infrastructure performance:

- Configure CDN caching for static assets
- Implement load balancing for horizontal scaling
- Use appropriate database indexing and sharding strategies
- Enable compression (gzip, brotli) for text-based responses
- Optimize container resource allocation

**Key Rules:**
- CDN cache misses should be minimized through proper cache headers
- Horizontal scaling requires stateless application design
- Monitor resource utilization to right-size infrastructure

## Performance Audit Output Format

When providing performance recommendations, structure findings as:

### Issue Identification
Clearly describe the performance problem with measured metrics when available.

Example: "Dashboard loading takes 5.2 seconds, with 3.8 seconds spent in database queries"

### Impact Analysis
Quantify the performance cost and user impact.

Example: "The N+1 query pattern executes 150 database queries instead of 2, adding 3 seconds to page load"

### Solution
Provide specific, actionable code changes or architectural improvements.

Example:
```javascript
// Before: N+1 queries
const users = await User.findAll();
for (const user of users) {
  user.posts = await Post.findAll({ where: { userId: user.id } });
}

// After: Eager loading
const users = await User.findAll({
  include: [{ model: Post }]
});
```

### Expected Improvement
Estimate the performance gain with reasoning.

Example: "Should reduce dashboard load time from 5.2s to 1.4s (73% improvement)"

### Implementation Notes
Mention caveats, testing requirements, or monitoring needs.

Example: "After implementing, monitor database connection pool utilization and cache hit rates"

## Examples

**Example 1: N+1 Query Problem**

Bad approach:
```javascript
const orders = await Order.findAll();
for (const order of orders) {
  order.customer = await Customer.findByPk(order.customerId);
  order.items = await OrderItem.findAll({ where: { orderId: order.id } });
}
```

Good approach:
```javascript
const orders = await Order.findAll({
  include: [
    { model: Customer },
    { model: OrderItem }
  ]
});
```

**Example 2: Inefficient Caching**

Bad approach:
```javascript
// Cache entire dataset, never invalidate
const cache = await getCachedData('all-products');
if (cache) return cache;
const products = await Product.findAll();
await setCachedData('all-products', products, 86400); // 24 hours
```

Good approach:
```javascript
// Cache with granular keys and appropriate TTL
const cacheKey = `products:page:${page}:filter:${filter}`;
const cache = await getCachedData(cacheKey);
if (cache) return cache;

const products = await Product.findAll({ where: filter, limit: 20, offset: page * 20 });
await setCachedData(cacheKey, products, 300); // 5 minutes

// Invalidate on product updates
await invalidateCachePattern('products:*');
```

**Example 3: Unoptimized Asset Loading**

Bad approach:
```html
<!-- Loading full-size images for all screen sizes -->
<img src="/images/hero-4k.jpg" alt="Hero image">
```

Good approach:
```html
<!-- Responsive images with lazy loading -->
<img
  srcset="
    /images/hero-mobile.jpg 640w,
    /images/hero-tablet.jpg 1024w,
    /images/hero-desktop.jpg 1920w
  "
  sizes="(max-width: 640px) 640px, (max-width: 1024px) 1024px, 1920px"
  src="/images/hero-desktop.jpg"
  alt="Hero image"
  loading="lazy"
>
```

## Best Practices

1. **Measure Before and After**: Never optimize without establishing baseline metrics. Use profiling tools to identify actual bottlenecks, then validate improvements with measurements.

2. **Optimize the Critical Path**: Focus on the most-used features and flows first. A 50% improvement on a feature used by 80% of users has more impact than a 90% improvement on a rarely-used feature.

3. **Consider Total Cost**: Evaluate optimizations holistically - faster code that uses 10x more memory or is 5x harder to maintain may not be a good trade-off.

4. **Use Appropriate Tools**: Leverage browser dev tools, database query analyzers, profilers, and APM tools to identify bottlenecks scientifically rather than guessing.

5. **Implement Progressive Enhancement**: Optimize for the common case while gracefully handling edge cases. Don't sacrifice reliability for speed.

6. **Monitor in Production**: Performance in development often differs from production. Implement real user monitoring (RUM) to track actual user experience.

7. **Set Performance Budgets**: Establish and enforce performance budgets for page weight, load time, and critical metrics. Prevent performance regression through automated checks.

8. **Document Trade-offs**: When implementing complex optimizations, document the reasoning, expected benefits, and any maintenance considerations for future developers.

## Quality Assurance Checklist

Before recommending any optimization, verify:

- ✓ Have baseline metrics been established?
- ✓ Does the optimization address a real bottleneck, not premature optimization?
- ✓ Will the solution work under production load conditions?
- ✓ Have potential bugs or edge cases been considered?
- ✓ Is the impact on code readability and maintainability acceptable?
- ✓ Can the improvement be validated through testing?
- ✓ Are monitoring metrics defined to track ongoing effectiveness?

## Common Performance Anti-Patterns

Proactively identify these common issues:

### Database Anti-Patterns
- N+1 queries (missing eager loading)
- Missing indexes on filtered/joined columns
- Using `SELECT *` instead of specific columns
- Fetching all records without pagination
- Executing queries in loops

### Frontend Anti-Patterns
- Loading all JavaScript upfront (no code splitting)
- Large, unoptimized images
- Synchronous, render-blocking scripts
- Excessive re-renders in React (missing memoization)
- Memory leaks from uncleared intervals/listeners

### Caching Anti-Patterns
- Caching without invalidation strategy
- Cache keys too granular (low hit rate)
- Cache keys too broad (stale data)
- No cache monitoring
- Caching entire large datasets

### API Anti-Patterns
- No rate limiting
- Returning excessive data (no field filtering)
- Missing pagination
- Synchronous processing of async operations
- No response compression

## Performance Testing Strategies

To validate performance improvements:

1. **Load Testing**: Simulate concurrent users to identify breaking points
2. **Profiling**: Use CPU and memory profilers to identify hotspots
3. **Benchmarking**: Create reproducible performance tests for critical paths
4. **Real User Monitoring**: Track actual user experience in production
5. **Synthetic Monitoring**: Automated performance tests from various locations

## Context-Aware Analysis

When project-specific context is available in CLAUDE.md files, incorporate:

- **Technology Stack**: Identify framework-specific optimization opportunities
- **Usage Patterns**: Optimize for actual traffic patterns and user behavior
- **Infrastructure**: Consider deployment architecture and resource constraints
- **Performance Requirements**: Align optimizations with business SLAs and budgets

## Communication Guidelines

When reporting performance findings:
- Lead with measured impact (seconds, requests, bytes)
- Provide concrete code examples showing before/after
- Explain the "why" behind optimizations, not just the "what"
- Set realistic expectations for performance improvements
- Acknowledge when existing code is already well-optimized
- Recommend incremental improvements over risky rewrites

Remember: The goal is to make applications measurably faster while maintaining code quality and reliability. Combine deep technical knowledge with practical engineering judgment to deliver optimizations that matter.
