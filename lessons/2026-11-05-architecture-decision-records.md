# Lesson: Architecture Decision Records (ADRs)

A 10-minute study lesson. This is general architecture background written for
exam preparation; it is not taken from the official exam objectives.

## What is an ADR?

An Architecture Decision Record is a short document that captures one
significant architectural decision, the context it was made in, and its
consequences.

- **Goal:** preserve the *why* behind a design, not just the *what*.
- **Trigger:** a decision that is costly to reverse or affects many teams.
- **Outcome:** newcomers and future maintainers can understand, challenge or
  supersede past decisions with full context.

## Why Decisions Get Lost

- People leave or change teams, and the reasoning goes with them.
- Chat threads and meeting notes are hard to find later.
- Diagrams show the result but not the rejected alternatives.
- Without records, teams re-debate settled questions or repeat old mistakes.

## Typical ADR Structure

| Section | Purpose |
|---------|---------|
| Title | Short, specific name (e.g. "Use a message queue for order events") |
| Status | Proposed, accepted, deprecated or superseded |
| Context | Forces at play: requirements, constraints, deadlines |
| Options considered | Realistic alternatives with brief pros and cons |
| Decision | The choice made, stated plainly |
| Consequences | Benefits, trade-offs and follow-up work, good and bad |

## Core Practices

- **One decision per record.** Keep each ADR small and focused.
- **Immutable once accepted.** Do not rewrite history; add a new ADR that
  supersedes the old one and link them.
- **Store beside the code.** Keep ADRs in version control, reviewed through
  pull requests like any other change.
- **Number or date them.** A sequence makes the timeline easy to follow.
- **Record trade-offs honestly.** List the downsides of the chosen option.
- **Keep it short.** One or two pages is usually enough.

## When to Write One

Good candidates:

- Choosing a database, messaging system or cloud service.
- Defining a service boundary or integration style.
- Setting a security, data retention or compliance approach.
- Adopting or dropping a framework or major library.

Usually not worth an ADR: routine implementation choices that are easy to
change later.

## Example (Abbreviated)

- **Title:** 0007 - Deploy with blue/green releases
- **Status:** Accepted
- **Context:** Releases need fast rollback; downtime windows are unacceptable.
- **Options:** In-place rolling update; blue/green; canary.
- **Decision:** Use blue/green for the customer-facing service.
- **Consequences:** Faster rollback; temporarily doubled capacity during
  releases; database changes must stay backward compatible.

## Architectural Considerations

- **Link to requirements:** tie each decision to the quality attributes it
  serves (performance, security, availability, cost).
- **Revisit on change:** when context shifts, review affected ADRs.
- **Lightweight governance:** review ADRs in pull requests instead of heavy
  approval boards.
- **Discoverability:** keep an index so records are easy to find.

## Quick Review Questions

1. What problem do ADRs solve that diagrams alone do not?
2. Why should an accepted ADR be superseded rather than edited?
3. Which sections capture the alternatives and the trade-offs?
4. Give two examples of decisions that deserve an ADR.
