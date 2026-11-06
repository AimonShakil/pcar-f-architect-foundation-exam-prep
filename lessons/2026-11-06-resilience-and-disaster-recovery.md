# Resilience and Disaster Recovery

Exam-prep lesson on designing systems that tolerate failure and recover from
disasters. General architecture material, not taken from any official exam
objectives.

## Why It Matters

Every component eventually fails. Resilience is about deciding in advance how
much failure the system absorbs, and how quickly it recovers from the rest.

- **High availability (HA):** keep serving during common, small failures.
- **Disaster recovery (DR):** restore service after a large failure, such as
  loss of a data centre or region, or data corruption.
- HA reduces the *chance* of an outage; DR limits the *damage* of one.

## Key Terms

| Term | Meaning |
|------|---------|
| RTO (Recovery Time Objective) | Maximum acceptable time to restore service |
| RPO (Recovery Point Objective) | Maximum acceptable amount of data loss, measured in time |
| SLA / SLO | Availability promise to customers / internal target |
| Failure domain | A set of components that fail together (host, rack, zone, region) |
| Single point of failure (SPOF) | A component whose failure stops the whole system |

Simple reading: **RTO = how long we may be down; RPO = how much data we may
lose.** Both are business decisions; architecture then has to meet them.

## Core Resilience Patterns

- **Redundancy:** run more than one instance, spread across failure domains.
- **Health checks and failover:** detect a bad instance and route around it.
- **Timeouts and retries with backoff:** avoid hanging on a slow dependency;
  add jitter so retries do not arrive in synchronised waves.
- **Circuit breaker:** stop calling a failing dependency for a while so it can
  recover and callers fail fast.
- **Bulkheads:** isolate resources (pools, queues) so one failure does not
  consume everything.
- **Graceful degradation:** drop non-essential features instead of failing
  entirely.
- **Idempotent operations:** make retries safe.

## Disaster Recovery Strategies

Roughly ordered from cheapest and slowest to costliest and fastest:

| Strategy | Idea | Typical trade-off |
|----------|------|-------------------|
| Backup and restore | Keep backups elsewhere; rebuild after a disaster | Cheapest; longest RTO, RPO = backup age |
| Pilot light | Core data replicated; the rest is off until needed | Low cost; moderate RTO |
| Warm standby | Scaled-down copy always running | Higher cost; shorter RTO |
| Active-active | Multiple sites serve live traffic | Highest cost and complexity; lowest RTO/RPO |

Pick the cheapest strategy that still meets the agreed RTO and RPO.

## Architectural Considerations

- **Define RTO/RPO per system,** not globally; not everything is equally
  critical.
- **Eliminate SPOFs,** including less obvious ones: DNS, identity provider,
  shared database, a single pipeline or secrets store.
- **Replication is not backup.** Replicas copy corruption and accidental
  deletes too; keep point-in-time, ideally immutable, backups.
- **Rebuild from code:** infrastructure as code makes recreating an
  environment repeatable.
- **Test the recovery.** An untested backup or failover plan is an assumption.
  Run restore tests and failure drills regularly.
- **Document runbooks** and keep them current; people execute them under
  stress.
- **Watch the dependencies:** your availability is bounded by the services you
  rely on in the critical path.
- **Cost vs. risk:** each extra level of resilience adds cost and
  operational complexity; justify it with business impact.

## Example Scenario

An internal reporting tool can be down for a day and may lose up to a day of
data. Nightly backups restored into a fresh environment meet that RTO/RPO, so
active-active would be wasteful. A customer-facing payment service with an RTO
of minutes and near-zero RPO needs redundancy across zones and replicated
data, possibly across regions.

## Quick Review Questions

1. What is the difference between RTO and RPO?
2. Why is database replication not a substitute for backups?
3. When would you choose warm standby over backup and restore?
4. What problem does a circuit breaker solve?
5. Name three single points of failure that are easy to overlook.
