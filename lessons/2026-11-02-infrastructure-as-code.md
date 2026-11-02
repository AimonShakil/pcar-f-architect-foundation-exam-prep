# Lesson: Infrastructure as Code (IaC)

A 10-minute study lesson. This is general architecture background written for
exam preparation; it is not taken from the official exam objectives.

## What is IaC?

Infrastructure as Code means describing servers, networks, and platform
services in text files that are version-controlled and applied by tooling,
instead of configuring them by hand in a console.

- **Goal:** make environments repeatable, reviewable, and recoverable.
- **Input:** declarative or scripted definition files stored in a repository.
- **Outcome:** real infrastructure that matches what the files say.

## Declarative vs Imperative

| Approach | You describe | Tool figures out | Typical consequence |
|----------|--------------|------------------|---------------------|
| Declarative | The desired end state | The steps to get there | Easier to reason about; re-applying is safe |
| Imperative | The exact steps to run | Nothing; it runs your steps | More control; harder to keep idempotent |

## Key Concepts

- **Idempotence:** applying the same definition twice gives the same result.
- **State:** many tools record what they created so they can compute the
  difference between "desired" and "actual". Protect and back up this state.
- **Drift:** the real environment diverges from the code, usually through
  manual changes. Detect it with a plan/diff step and fix it by re-applying
  the code or updating the code.
- **Immutable infrastructure:** replace components with new versions rather
  than patching them in place.
- **Modules / reuse:** package common patterns once and parameterise them.

## Typical Workflow

1. Change the definition files on a branch.
2. Open a pull request; CI validates syntax and runs static checks.
3. A preview ("plan") shows what would change; reviewers read it.
4. After approval and merge, an automated job applies the change.
5. Periodically check for drift.

This is the same pull-request and CI gating pattern described in
`notes/ci-notes.md`, applied to infrastructure rather than application code.

## Architectural Considerations

- **Environment parity:** create dev, test, and production from the same
  definitions with different parameters.
- **Least privilege:** the account that applies changes should have only the
  permissions the definitions need.
- **Secrets:** never commit secrets to the repository; reference a secrets
  manager instead.
- **Blast radius:** split large definitions into smaller units so one mistake
  affects less.
- **Review and audit:** version history gives a record of who changed what
  and why.
- **Trade-off:** IaC adds up-front effort and a learning curve; the benefit
  grows with the number of environments and the rate of change.

## Common Pitfalls

- Making "quick fixes" in the console, which creates drift.
- Losing or corrupting the state file with no backup.
- Putting everything in one giant definition.
- Applying changes without reviewing the preview first.

## Quick Review Questions

1. How does declarative IaC differ from imperative IaC?
2. What is drift, and how can you detect it?
3. Why is idempotence useful when re-running a definition?
4. Why should secrets stay out of IaC repositories?
5. How does the CI pull-request gate apply to infrastructure changes?
