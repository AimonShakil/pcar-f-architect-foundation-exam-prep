# Threat Modeling

Study lesson on identifying and addressing security risks during design.

## What is Threat Modeling?

Threat modeling is a structured way to ask, early in design, "what can go
wrong, and what will we do about it?"

- **Goal:** find security weaknesses while they are cheap to fix.
- **When:** at design time, and again whenever the architecture changes.
- **Output:** a prioritised list of threats with chosen mitigations.

## The Four Guiding Questions

1. What are we building? (diagram the system)
2. What can go wrong? (enumerate threats)
3. What are we going to do about it? (mitigate, accept, transfer, avoid)
4. Did we do a good enough job? (review and validate)

## Step 1: Model the System

Draw a data flow diagram showing:

- **Actors:** users, external systems, administrators.
- **Processes:** services and components that handle data.
- **Data stores:** databases, queues, object storage, caches.
- **Data flows:** how data moves between the above.
- **Trust boundaries:** lines where the level of trust changes (internet to
  DMZ, service to database, tenant to tenant).

Most threats appear where data crosses a trust boundary.

## Step 2: Find Threats with STRIDE

STRIDE is a common checklist, one category per kind of threat.

| Category | Threat | Example | Typical mitigation |
|----------|--------|---------|--------------------|
| Spoofing | Pretending to be someone else | Stolen session token | Strong authentication, MFA |
| Tampering | Modifying data or code | Altered request payload | Integrity checks, signing, input validation |
| Repudiation | Denying an action took place | No record of an admin change | Audit logging |
| Information disclosure | Exposing data to the wrong party | Verbose error leaks internals | Encryption, access control |
| Denial of service | Making a system unavailable | Request flood | Rate limiting, autoscaling |
| Elevation of privilege | Gaining rights not granted | Missing authorization check | Least privilege, authorization on every call |

## Step 3: Decide on a Response

For each threat, choose one:

- **Mitigate:** add a control that reduces likelihood or impact.
- **Accept:** knowingly live with the risk (record who accepted it).
- **Transfer:** shift it, for example through insurance or a provider.
- **Avoid:** remove the feature or component that causes it.

Prioritise by likelihood and impact. Simple High / Medium / Low ratings are
fine; precision matters less than consistent, reasoned decisions.

## Architectural Considerations

- **Defence in depth:** layer controls so one failure is not fatal.
- **Least privilege:** give each identity only the access it needs.
- **Secure defaults:** deny by default; make the safe option the easy one.
- **Reduce attack surface:** expose fewer endpoints, ports and features.
- **Validate at trust boundaries:** never trust input from outside them.
- **Record decisions:** capture outcomes in ADRs so they can be revisited.
- **Iterate:** treat the model as a living document, not a one-off.

## Example: This Repository

A small walk-through using the CI setup in `notes/ci-notes.md`:

- **Trust boundary:** contributor pull request into the protected `main`.
- **Tampering:** an unreviewed change lands on `main`. Mitigation: no direct
  pushes, required status check.
- **Elevation of privilege:** a compromised workflow abuses broad token
  rights. Mitigation: `contents: read` permissions only.
- **Repudiation:** unclear who changed what. Mitigation: history kept in
  version control via pull requests.

## Common Pitfalls

- Doing it once and never updating it.
- Modeling only the happy path and ignoring third-party integrations.
- Producing a long list with no prioritisation or owner.
- Treating it as a security-team-only activity; include the builders.

## Quick Review Questions

1. Why is threat modeling most valuable at design time?
2. What is a trust boundary, and why does it matter?
3. Name the six STRIDE categories and one mitigation for each.
4. What are the four possible responses to an identified threat?
5. Why should a threat model be revisited after architecture changes?
