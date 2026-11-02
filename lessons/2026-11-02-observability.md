# Lesson: Observability

A 10-minute study lesson. This is general architecture background written for
exam preparation; it is not taken from the official exam objectives.

## What is Observability?

Observability is the ability to understand what a system is doing internally
by examining the data it emits. Monitoring tells you *that* something is
wrong; observability helps you ask *why*.

- **Goal:** detect, diagnose, and fix problems quickly.
- **Input:** telemetry emitted by applications and infrastructure.
- **Outcome:** dashboards, alerts, and evidence for troubleshooting.

## The Three Common Signals

| Signal | What it is | Good for | Watch out for |
|--------|------------|----------|---------------|
| Logs | Timestamped event records | Detail about a specific event | Volume, cost, unstructured text |
| Metrics | Numeric measurements over time | Trends, alerting, capacity | Loses per-request detail |
| Traces | The path of one request across services | Finding where latency or errors arise | Needs context propagation everywhere |

## Key Concepts

- **Structured logging:** emit logs as key/value (e.g. JSON) so they can be
  searched and filtered.
- **Correlation ID / trace ID:** an identifier passed between services so
  events from one request can be joined.
- **Cardinality:** the number of distinct label values on a metric. Very high
  cardinality (e.g. a user ID label) can make metrics expensive.
- **Instrumentation:** code or agents that produce telemetry, added manually
  or through libraries.
- **Health checks:** simple endpoints that report whether a service can
  serve traffic.

## Alerting and Objectives

- **SLI (service level indicator):** a measured aspect of service quality,
  such as the fraction of successful requests.
- **SLO (service level objective):** the target for an SLI over a period.
- **SLA (service level agreement):** a contractual commitment, often with
  consequences if missed.
- **Alert on symptoms** users feel (errors, latency) rather than only on
  causes (CPU usage), to reduce noisy alerts.
- Every alert should be actionable and have an owner.

## Architectural Considerations

- **Design it in:** decide what to emit while designing, not after an outage.
- **Consistency:** use common formats, naming, and field names across services.
- **Protect sensitive data:** never log secrets or unnecessary personal data.
- **Retention and cost:** keep detailed data briefly, aggregates longer.
- **Separation:** ship telemetry off the host so it survives a failure.
- **Sampling:** trace a fraction of requests when full tracing is too costly,
  accepting that rare events may be missed.

## Example: Investigating Slow Checkouts

1. An alert fires: the checkout error-rate SLI is below its SLO.
2. Metrics show latency rising in one service only.
3. A trace of a slow request shows a database call taking most of the time.
4. Logs for that trace ID reveal repeated timeouts on one query.
5. Fix the query, then confirm on the dashboard that the SLI recovers.

## Quick Review Questions

1. How do logs, metrics, and traces differ in what they are best used for?
2. Why is a correlation or trace ID important in a multi-service system?
3. What is the difference between an SLI, an SLO, and an SLA?
4. Why can high metric cardinality be a problem?
5. Why alert on symptoms rather than only on causes?
