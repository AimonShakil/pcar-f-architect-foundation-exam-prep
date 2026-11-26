# Evolutionary Architecture and Technical Debt

Study lesson on keeping an architecture changeable over time.
Roughly a 10-minute read.

> These are general study notes, not a statement of the official exam objectives.

## Why it matters

Requirements, traffic and teams change, so an architecture is never "done".
The goal is not to predict every change but to make change **cheap and safe**.
Neglected design shortcuts (technical debt) make every later change slower.

## Technical debt

Technical debt is the future cost created by choosing a quick or imperfect
solution now. Like financial debt, it can be a reasonable choice if it is
deliberate and repaid.

| Type | Example | Typical response |
|------|---------|------------------|
| Deliberate, prudent | Ship a simple design to hit a deadline, with a plan to revisit | Record it and schedule repayment |
| Deliberate, reckless | "No time for design" with no follow-up | Avoid; surface the risk |
| Accidental | Team learns a better approach after building | Refactor as understanding grows |
| Environmental | Dependencies, runtimes or platforms age out | Budget for regular upgrades |

- **Interest:** the extra effort paid on every change while the debt remains.
- **Make it visible:** keep a debt register or backlog items; record
  deliberate shortcuts in an ADR.
- **Prioritise by interest, not by ugliness:** fix debt in code that changes
  often first.

## Principles of evolutionary design

- **Small, reversible steps** beat large irreversible ones.
- **Loose coupling and clear boundaries** let parts change independently.
- **Defer decisions** that are costly to reverse until you have information
  (the "last responsible moment"), but do not defer forever.
- **Automate verification** so change is safe: tests, CI and deployment
  pipelines.

## Fitness functions

A fitness function is an automated check that an architectural property still
holds. It turns an architecture goal into something testable.

- No module in the domain layer may import from the UI layer (dependency rule test).
- p95 latency of a key endpoint stays under an agreed target in a load test.
- No dependency with a known critical vulnerability is allowed in the build.
- Service-to-service calls stay within agreed boundaries.

Run them in the CI pipeline so drift is caught at the pull request, not in
production.

## Migration patterns

- **Strangler fig:** put a routing layer in front of the old system and move
  functionality to the new one piece by piece until the old one can be retired.
- **Branch by abstraction:** introduce an interface over the old
  implementation, build the new one behind it, then switch over.
- **Parallel run:** run old and new together and compare results before
  cutting over.
- **Expand and contract (for schemas and APIs):** add the new form, migrate
  consumers, then remove the old form.
- **Feature flags:** merge incomplete work safely and switch behaviour on
  gradually.

## Trade-offs

- **Rewrite vs refactor:** rewrites carry high risk and often lose hidden
  behaviour; incremental migration delivers value sooner.
- **Flexibility vs simplicity:** building extension points "just in case" is
  itself a cost. Add flexibility where change is likely.
- **Standardisation vs autonomy:** shared platforms reduce effort, but
  too much central control slows teams.
- **Deprecation discipline:** every old version kept alive is ongoing cost;
  set and communicate retirement dates.

## Common pitfalls

- Treating debt as purely a developer concern, not a business risk.
- Big-bang rewrites with no incremental delivery.
- Documenting architecture once and never revisiting it.
- Having no automated checks, so erosion goes unnoticed.
- Never retiring old systems, so the estate only grows.

## Quick review questions

1. What does "interest" mean in the technical debt metaphor?
2. Why might deliberate debt be acceptable, and what makes it responsible?
3. What is a fitness function and where should it run?
4. How does the strangler fig pattern reduce migration risk?
5. When is a rewrite more justified than incremental refactoring?
