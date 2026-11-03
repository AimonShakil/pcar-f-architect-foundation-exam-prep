# Lesson: Deployment Strategies

A 10-minute study lesson. This is general architecture background written for
exam preparation; it is not taken from the official exam objectives.

## What is a Deployment Strategy?

A deployment strategy is the plan for getting a new version of a system into
production while controlling risk and downtime.

- **Goal:** release changes safely, with a fast way back if something breaks.
- **Input:** a tested, versioned artifact (see the CI notes).
- **Outcome:** the new version serves users, or the release is rolled back.

## Common Strategies

| Strategy | How it works | Strength | Trade-off |
|----------|--------------|----------|-----------|
| Recreate | Stop the old version, start the new one | Simple | Downtime during the switch |
| Rolling | Replace instances gradually in batches | No extra full environment | Two versions run side by side; rollback is slow |
| Blue/green | Run old (blue) and new (green) environments; switch traffic | Fast, clean rollback | Needs capacity for two environments |
| Canary | Send a small share of traffic to the new version first | Limits blast radius; real-traffic feedback | Needs good routing and observability |
| Shadow | Copy live traffic to the new version, ignore its responses | Tests under realistic load | Side effects (writes, emails) must be isolated |

## Key Concepts

- **Blast radius:** how many users a bad release can affect.
- **Rollback:** returning to the last known good version.
- **Feature flag:** a runtime switch that turns a feature on or off without a
  new deploy. It separates *deploying* code from *releasing* a feature.
- **Health check / readiness probe:** an automated check that an instance can
  take traffic before it receives any.
- **Immutable deployment:** replace instances with new ones rather than
  modifying them in place.

## The Database Problem

Application rollouts are easy to reverse; schema changes often are not.

- Use the **expand/contract** pattern: first add new structures in a way the
  old and new code both tolerate, migrate, then remove the old ones later.
- Keep versions **backward compatible** while two versions may run together.
- Treat destructive migrations as separate, deliberate steps.

## Architectural Considerations

- **Automate:** deployments should come from the pipeline, not manual steps.
- **Observe:** define what "healthy" means (error rate, latency) before
  releasing, and use it to decide whether to proceed or roll back.
- **Automate rollback** where the signal is clear, to shorten recovery time.
- **Match strategy to risk:** a low-risk internal tool may be fine with
  recreate; a critical user-facing service may justify canary or blue/green.
- **Least privilege:** the deploy identity should only reach the targets it
  needs.
- **Config as code:** keep environment configuration versioned alongside
  infrastructure definitions.

## Choosing Quickly

- Downtime is acceptable and simplicity matters: **recreate**.
- Limited spare capacity, tolerant of mixed versions: **rolling**.
- Need instant switch-back: **blue/green**.
- Want to validate with real users gradually: **canary**.
- Want to test without user impact: **shadow**.

## Quick Review Questions

1. How does blue/green differ from a rolling update?
2. Why does a canary release depend on good observability?
3. What does a feature flag separate?
4. Why are database changes harder to roll back than application code?
5. What is the expand/contract pattern for?
