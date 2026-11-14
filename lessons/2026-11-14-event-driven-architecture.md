# Event-Driven Architecture

Exam-prep lesson on Event-Driven Architecture (EDA). About a 10-minute read.

## What is EDA?

In an event-driven architecture, components communicate by publishing and
reacting to **events**: immutable facts that something happened.

- **Producer:** emits an event, and does not know who consumes it.
- **Broker / channel:** transports events (queue, topic, log).
- **Consumer:** subscribes and reacts, independently of other consumers.
- **Outcome:** loose coupling in time, space and (often) technology.

## Events, Commands and Queries

| Concept | Meaning | Example |
|---------|---------|---------|
| Event | Something that already happened | `OrderPlaced` |
| Command | A request to do something | `PlaceOrder` |
| Query | A request for data | `GetOrder` |

- Name events in the past tense; a command can be rejected, an event cannot.
- Events are facts: do not edit them, publish a new compensating event.

## Common Patterns

- **Publish/subscribe:** one event, many independent consumers.
- **Work queue:** one message handled by exactly one of several workers.
- **Event notification:** a small event says "something changed"; consumers
  call back for details.
- **Event-carried state transfer:** the event holds the data consumers need,
  so they avoid calling the producer.
- **Event sourcing:** store state as an ordered log of events and rebuild
  current state by replaying them.
- **CQRS:** separate the write model from read models; often paired with
  event sourcing but usable on its own.

## Delivery Guarantees

| Guarantee | Trade-off |
|-----------|-----------|
| At-most-once | No duplicates, but messages can be lost |
| At-least-once | No loss, but duplicates are possible |
| Exactly-once | Hard in general; usually approximated end to end |

- Design consumers to be **idempotent** so duplicates are harmless
  (for example, track processed event IDs).
- Ordering is usually guaranteed only within a partition or key, not globally.

## Reliability Techniques

- **Transactional outbox:** write the event to an outbox table in the same
  database transaction as the state change, then publish it separately. This
  avoids the "saved to DB but event never sent" gap.
- **Dead-letter queue (DLQ):** park messages that repeatedly fail so they do
  not block the stream; monitor and reprocess them.
- **Retries with backoff:** retry transient failures with increasing delays.
- **Saga:** coordinate a multi-step business process across services using
  events and compensating actions instead of a distributed transaction.

## Architectural Considerations

- **Eventual consistency:** consumers lag behind producers; the UI and the
  business must tolerate it.
- **Schema evolution:** version events and prefer backward-compatible
  changes (add optional fields, avoid removing or repurposing fields).
- **Observability:** use correlation IDs to trace a flow across services;
  see the observability lesson.
- **Debugging difficulty:** flow is implicit, so document event contracts
  and keep an event catalogue.
- **Backpressure:** decide what happens when consumers fall behind
  (scale out, buffer, drop, or slow producers).
- **Security:** authenticate producers and consumers, and avoid putting
  sensitive data in events that many consumers can read.

## When to Use / When to Avoid

- **Good fit:** decoupled services, spikes smoothed by buffering, fan-out to
  many consumers, audit trails, integration across teams.
- **Poor fit:** simple CRUD apps, flows needing an immediate synchronous
  answer, small teams where the added moving parts outweigh the benefits.

## Quick Review Questions

1. How does an event differ from a command?
2. Why must consumers be idempotent under at-least-once delivery?
3. What problem does the transactional outbox solve?
4. What is a saga, and why use it instead of a distributed transaction?
5. Name two costs EDA adds compared with synchronous calls.
