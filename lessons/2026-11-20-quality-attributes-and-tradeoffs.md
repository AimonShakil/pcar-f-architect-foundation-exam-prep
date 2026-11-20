# Quality Attributes and Trade-offs

Study lesson on non-functional requirements (quality attributes) and how
architects balance them. Roughly a 10-minute read.

> These are general study notes, not a statement of the official exam objectives.

## What are quality attributes?

Functional requirements say *what* a system does. Quality attributes (also
called non-functional requirements) say *how well* it does it. Architecture is
shaped far more by these than by features.

## Common quality attributes

| Attribute | Question it answers |
|-----------|---------------------|
| Performance | How fast, and how much work per unit of time? |
| Scalability | Can it handle growth in load or data? |
| Availability | What fraction of time is it usable? |
| Reliability | Does it behave correctly over time? |
| Security | Are data and operations protected? |
| Maintainability | How easily can it be changed and fixed? |
| Testability | How easily can behaviour be verified? |
| Operability | How easily can it be deployed, monitored, run? |
| Cost | What does it cost to build and run? |

## Make them measurable

"The system must be fast" cannot be tested. Write each requirement as a
scenario with a stimulus, an environment and a measurable response:

- *Vague:* "The API should be quick."
- *Better:* "Under normal load, 95% of read requests complete within a stated
  target latency." (Pick the number with stakeholders; do not guess.)

Useful habits:

- Name a **metric** and a **target** for every attribute that matters.
- Separate **must-have** from **nice-to-have**.
- Define the **load and conditions** the target applies to.

## Trade-offs are unavoidable

Improving one attribute usually costs another. Typical tensions:

- **Availability vs consistency:** replicating data widely helps uptime but
  makes strong consistency harder.
- **Security vs usability:** more checks add friction.
- **Performance vs maintainability:** heavy optimisation can obscure code.
- **Flexibility vs simplicity:** extra abstraction layers add complexity.
- **Cost vs resilience:** redundancy in more zones or regions costs more.
- **Time to market vs everything else:** shortcuts become technical debt.

The architect's job is not to maximise every attribute but to choose a
balance that fits the business goals, and to make that choice explicit.

## Prioritising

1. Gather stakeholders and list candidate attributes.
2. Rank them for *this* system; only a few will be top drivers.
3. Turn the top drivers into measurable scenarios.
4. Pick tactics that serve them, and note what you gave up.
5. Record the decision (see the ADR lesson).

## Tactics: examples

- **Performance:** caching, async processing, efficient data access.
- **Availability:** redundancy, health checks, failover.
- **Security:** least privilege, defence in depth, input validation.
- **Maintainability:** modular boundaries, clear interfaces, automated tests.
- **Operability:** observability, automated deployment, runbooks.

## Validating the architecture

- Review designs against the scenarios.
- Test early: load tests, failure injection, security reviews.
- Monitor in production and compare against the targets.
- Revisit priorities when the business context changes.

## Common pitfalls

- Listing every attribute as "high priority".
- Leaving requirements unmeasured.
- Discovering quality needs only after the design is fixed.
- Optimising one attribute without noticing what it degraded.

## Quick review questions

1. How do functional and non-functional requirements differ?
2. Rewrite "the system should be secure" as a measurable scenario.
3. Name two attributes that commonly conflict and why.
4. Why should trade-off decisions be recorded?
5. How can you check that a quality target is actually being met?
