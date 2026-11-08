# API Design and Integration Patterns

Study lesson on designing APIs and connecting systems. About a 10-minute read.

## Why It Matters

APIs are the contracts between components and teams. Architects choose how
systems talk to each other, and those choices are hard to change once
consumers depend on them.

## Synchronous vs Asynchronous Integration

| Style | How it works | Strengths | Trade-offs |
|-------|--------------|-----------|------------|
| Synchronous (request/response) | Caller waits for the reply | Simple, immediate result | Temporal coupling, failures propagate to the caller |
| Asynchronous (messaging/events) | Caller sends a message and moves on | Loose coupling, absorbs bursts | Eventual consistency, harder to trace and debug |

## Common API Styles

- **REST / resource-oriented HTTP:** resources identified by URLs, standard
  verbs and status codes. Widely understood and cache-friendly.
- **RPC-style (for example gRPC):** call remote functions with a typed
  contract. Efficient for service-to-service traffic.
- **GraphQL:** the client asks for exactly the fields it needs. Flexible, but
  needs care over query cost and caching.
- **Event-driven:** publish events to a broker; consumers subscribe.

No style is best everywhere; match it to consumers, latency needs and team
skills.

## Designing a Good Contract

- **Be consistent:** naming, error format and pagination behave the same
  everywhere.
- **Describe it formally:** keep a machine-readable specification in version
  control and review changes like code.
- **Use clear errors:** correct status codes plus a structured body.
- **Paginate collections:** never return unbounded lists.
- **Design for evolution:** adding optional fields is usually safe; removing
  or changing meaning is breaking.

## Versioning and Compatibility

- Prefer backward-compatible changes so most updates need no new version.
- When a breaking change is needed, publish a new version (in the path or a
  header) and run old and new side by side.
- Announce deprecations with a timeline and monitor who still uses the old
  version before removing it.
- Consumer-driven contract tests help detect breakage before release.

## Idempotency and Retries

An operation is **idempotent** if repeating it has the same effect as doing it
once.

- Reads and replace-style updates are naturally idempotent.
- For operations like "create payment", accept a client-supplied idempotency
  key and return the original result on a repeat.
- This makes retries safe, which matters because networks fail in ambiguous
  ways (did the request arrive or not?).
- Messaging systems often deliver at least once, so consumers should handle
  duplicates.

## Integration Patterns

| Pattern | Idea |
|---------|------|
| API gateway | Single entry point for routing, authentication and rate limiting |
| Message queue | Point-to-point buffer; one consumer handles each message |
| Publish/subscribe | Many consumers receive each event |
| Backend for frontend | Separate API tailored to each client type |
| Anti-corruption layer | Translate between a legacy model and a new one |
| Webhook | Provider calls a consumer URL when something happens |

## Protecting an API

- **Authentication and authorisation:** verify who the caller is and what
  they may do; apply least privilege.
- **Rate limiting and quotas:** stop one consumer exhausting capacity.
- **Input validation:** treat all input as untrusted.
- **TLS:** encrypt traffic in transit.
- **Timeouts and retries with backoff:** see the resilience lesson.

## Common Pitfalls

- Chatty APIs that need many calls for one screen or task.
- Leaking internal data models through the public contract.
- Shared databases used as an integration mechanism, creating hidden coupling.
- Breaking changes released without notice.
- Retries without idempotency, causing duplicate side effects.

## Architectural Considerations

- Treat the API as a product with owners, documentation and a lifecycle.
- Choose sync or async per interaction, based on whether the caller needs the
  answer now.
- Define compatibility rules and deprecation policy up front.
- Record significant API decisions in an ADR.
- Observe API traffic: latency, error rate and per-consumer usage.

## Quick Review Questions

1. When is asynchronous integration preferable to synchronous?
2. Which kinds of change are usually backward compatible?
3. What is an idempotency key and why is it useful?
4. What does an anti-corruption layer do?
5. Why is a shared database a risky integration approach?
