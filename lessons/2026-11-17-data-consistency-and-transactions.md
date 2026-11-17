# Data Consistency and Transactions

Study lesson on keeping data correct across services and stores. About a
10-minute read.

## Why It Matters

Once data lives in more than one place (replicas, caches, several services),
architects must decide how correct and how current it must be, and what it
costs to guarantee that. This is a trade-off, not a default.

## ACID Transactions (Single Database)

| Property | Meaning |
|----------|---------|
| Atomicity | All steps of a transaction apply, or none do |
| Consistency | A transaction moves data from one valid state to another (constraints hold) |
| Isolation | Concurrent transactions do not see each other's partial work |
| Durability | Once committed, the change survives crashes |

- Simple to reason about, but usually scoped to one database.
- Stronger isolation levels reduce anomalies but can reduce concurrency.

## Strong vs Eventual Consistency

- **Strong consistency:** after a write completes, every read sees it.
  Easier for users and code; costs latency and availability under partitions.
- **Eventual consistency:** replicas converge over time if writes stop.
  Scales and stays available; readers may see stale data.
- **Read-your-writes:** a middle ground where a user always sees their own
  updates, even if others lag.

## The CAP Trade-off

During a network partition a distributed store must choose between
**consistency** (reject or delay some requests) and **availability**
(answer, possibly with stale data). Outside partitions, the everyday
trade-off is usually latency versus consistency.

## Transactions Across Services

Distributed two-phase commit is often avoided in microservices because it
couples services and blocks on failures. Common alternatives:

- **Saga:** a sequence of local transactions; if one fails, run
  **compensating actions** to undo earlier steps.
  - *Orchestration:* a coordinator directs each step.
  - *Choreography:* services react to each other's events.
- **Transactional outbox:** write the data change and an outgoing event in
  the same local transaction; a relay publishes the event afterwards.
  This avoids "saved but never announced" failures.
- **Idempotent consumers:** messages may be delivered more than once, so
  handling the same message twice must be safe (e.g. deduplicate by ID).

## Concurrency Control

- **Pessimistic locking:** lock data while working. Safe under high
  contention, but can block and deadlock.
- **Optimistic concurrency:** read a version, write only if it is unchanged,
  else retry. Good when conflicts are rare.

## Architectural Considerations

- Keep strongly consistent operations inside one service boundary where
  possible; draw boundaries around data that must change together.
- Decide per use case: a payment balance and a "likes" counter need
  different guarantees.
- Design for failure: retries, timeouts, idempotency and compensation.
- Make staleness visible and bounded where users can notice it.
- Cache invalidation and replica lag are consistency decisions too.

## Quick Review Questions

1. What does atomicity guarantee?
2. When might eventual consistency be acceptable, and when not?
3. How does a saga undo work after a partial failure?
4. What problem does the transactional outbox solve?
5. Why must consumers of at-least-once messages be idempotent?
