# Monolith vs Microservices

Study lesson on choosing a system's deployment and module boundaries. About a
10-minute read.

## Why It Matters

How a system is split into deployable units shapes team autonomy, release
speed, operational burden and failure behaviour. Architects should choose
deliberately, based on context, rather than by fashion.

## Definitions

- **Monolith:** the application is built and deployed as a single unit.
- **Modular monolith:** one deployable unit, with strict internal module
  boundaries and clear interfaces between modules.
- **Microservices:** many small, independently deployable services, each
  owning a focused capability and its own data.

## Comparison

| Aspect | Monolith | Microservices |
|--------|----------|---------------|
| Deployment | One artifact, all-or-nothing | Per service, independent |
| Scaling | Scale the whole app | Scale only the hot services |
| Data | Usually one shared database | Database per service |
| Calls between parts | In-process, fast, reliable | Network calls, can fail or be slow |
| Testing | Simpler end-to-end | Needs contract and integration testing |
| Operations | One thing to run | Many things: discovery, tracing, deployment pipelines |
| Team fit | Small or single team | Several teams working in parallel |

## Benefits of Microservices

- Teams release independently, with a smaller blast radius per change.
- Technology choices can differ per service where it truly helps.
- Faults can be isolated, if the design handles partial failure.

## Costs of Microservices

- Distributed-systems problems: latency, partial failure, retries.
- Data consistency across services is harder (see the data consistency
  lesson).
- More observability and automation is required to operate safely.
- Poor boundaries create a "distributed monolith": services that must be
  deployed together and call each other in long chains.

## Choosing Boundaries

- Align services with business capabilities, not technical layers.
- Keep data ownership clear: one service owns each piece of data.
- Prefer loose coupling and stable, versioned interfaces.
- Services that always change together probably belong together.

## Decision Guide

- Start with a (modular) monolith when the domain is unclear, the team is
  small, or speed of learning matters most.
- Consider splitting when teams block each other, parts need very different
  scaling, or release cadence differs sharply.
- Extract gradually (for example, the strangler approach: route functionality
  piece by piece to new services) rather than rewriting everything.
- Record the decision and its trade-offs in an ADR.

## Common Pitfalls

- Splitting too early, before boundaries are understood.
- Sharing one database between "independent" services.
- Ignoring the operational maturity (CI/CD, monitoring) services demand.
- Treating microservices as a goal instead of a means.

## Quick Review Questions

1. How does a modular monolith differ from a plain monolith?
2. What is a distributed monolith and how does it arise?
3. Name two signals that it may be time to split a monolith.
4. Why should each service own its data?
5. What operational capabilities should exist before adopting microservices?
