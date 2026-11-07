# Scalability and Caching

Study lesson on scaling systems and using caches. About a 10-minute read.

## Why It Matters

Architects must decide how a system copes as load grows, and where to trade
freshness or consistency for speed. Scaling and caching are two of the most
common levers.

## Vertical vs Horizontal Scaling

| Approach | What it means | Strengths | Limits |
|----------|---------------|-----------|--------|
| Vertical (scale up) | Give one node more CPU, memory or disk | Simple, no code changes | Hardware ceiling, single point of failure |
| Horizontal (scale out) | Add more nodes behind a load balancer | Better fault tolerance, grows incrementally | Needs stateless design or shared state, more operational complexity |

## Designing for Horizontal Scale

- **Stateless services:** keep session and request state out of the process
  (use a shared store or signed tokens) so any node can serve any request.
- **Load balancing:** spread requests across nodes; use health checks to
  remove unhealthy nodes.
- **Autoscaling:** add or remove capacity from a metric such as CPU, queue
  depth or request rate. Allow for warm-up time.
- **Asynchronous work:** move slow tasks onto queues so the request path
  stays short and bursts are absorbed.
- **Data partitioning (sharding):** split data by a key. Choose the key
  carefully to avoid hot spots.
- **Read replicas:** offload reads from the primary, accepting replication lag.

## Find the Bottleneck First

Scaling the wrong component wastes money. Measure first (see the
observability lesson), then address the constraint: CPU, memory, I/O,
network, a database, or a downstream dependency.

## Caching Basics

A cache stores the result of expensive work so repeat requests are cheaper.

- **Goal:** lower latency and reduce load on the origin.
- **Cost:** data may be stale, and there is one more component to run.

### Where Caches Live

- Client or browser cache
- CDN / edge cache
- Application or in-process cache
- Distributed cache (a shared in-memory store)
- Database-level caches

### Common Patterns

| Pattern | How it works | Trade-off |
|---------|--------------|-----------|
| Cache-aside | App checks cache, on a miss reads the source and fills the cache | Simple; first request is slow, app owns the logic |
| Read-through | Cache itself loads from the source on a miss | Cleaner app code; cache must know the source |
| Write-through | Writes go to cache and source together | Fresher cache; slower writes |
| Write-back | Write to cache, flush to source later | Fast writes; risk of data loss if cache fails |

### Invalidation and Expiry

- **TTL (time to live):** entries expire after a set time. Simple but allows
  staleness up to the TTL.
- **Explicit invalidation:** delete or update entries when the source changes.
  More accurate, harder to get right.
- **Eviction policy:** when full, the cache removes entries, for example
  least recently used (LRU).

## Common Pitfalls

- **Cache stampede:** many requests miss at once (for example after expiry)
  and hit the origin together. Mitigate with request coalescing, jittered
  TTLs or pre-warming.
- **Stale data:** decide per data type how much staleness is acceptable.
- **Caching sensitive data:** respect access control; do not serve one
  user's data to another.
- **Cold start:** an empty cache sends full load to the origin.
- **Treating the cache as the source of truth:** the system should still
  work, if slower, when the cache is unavailable.

## Architectural Considerations

- Cache read-heavy, rarely changing data first.
- Set an explicit freshness requirement before choosing a TTL.
- Monitor hit ratio, latency and evictions.
- Plan capacity so the origin can survive a cache outage.
- Combine with resilience practices: timeouts, retries and fallbacks.

## Quick Review Questions

1. What makes a service easy to scale horizontally?
2. Why measure before scaling?
3. How does cache-aside differ from write-through?
4. What is a cache stampede and how can it be reduced?
5. Why should the system tolerate a cache outage?
