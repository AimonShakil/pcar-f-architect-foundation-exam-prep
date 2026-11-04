# Lesson: Secrets Management

A 10-minute study lesson. This is general architecture background written for
exam preparation; it is not taken from the official exam objectives.

## What is a Secret?

A secret is a value that grants access or proves identity: passwords, API
keys, tokens, private keys and certificates, database credentials.

- **Goal:** let systems authenticate without exposing credentials to people or
  to places they should not be.
- **Risk:** a leaked secret is often a full compromise of what it protects.
- **Outcome:** secrets are stored, delivered, rotated and revoked in a
  controlled way.

## Where Secrets Should Not Live

| Location | Problem |
|----------|---------|
| Source code / Git history | Copied to every clone; history keeps it after deletion |
| Container images | Anyone who can pull the image can read the layers |
| Plain config files | Easy to copy, hard to audit |
| Logs and error messages | Spread widely and retained for a long time |
| Chat and tickets | Broad audience, no access control |

## Where Secrets Should Live

- **Dedicated secret store:** a vault or cloud secret manager with access
  control, encryption at rest and an audit trail.
- **CI/CD secret storage:** pipeline-level secrets, injected only into the jobs
  that need them (see the CI notes on least privilege).
- **Injected at runtime:** delivered as environment variables or mounted files
  when the workload starts, not baked into the artifact.

## Key Concepts

- **Rotation:** replacing a secret on a schedule or after suspected exposure.
- **Revocation:** invalidating a secret immediately.
- **Short-lived credentials:** tokens that expire quickly, so a leak has a
  limited window.
- **Workload identity:** the platform proves who a workload is, so no
  long-lived static secret is needed at all.
- **Separation per environment:** dev, test and production use different
  secrets.
- **Audit logging:** record who read which secret and when.

## Design Guidance

1. **Prefer not having a secret.** Use workload identity or federated
   authentication where available.
2. **Prefer short-lived over long-lived.** Dynamic credentials limit blast
   radius.
3. **Apply least privilege.** Each secret grants the minimum access, and only
   the services that need it can read it.
4. **Plan for rotation from day one.** If rotating causes an outage, the design
   is fragile.
5. **Scan for leaks.** Add secret scanning to commits and pipelines.
6. **Assume exposure will happen.** Know how to revoke, rotate and investigate.

## If a Secret Leaks

1. Revoke or rotate it first; do not rely on deleting the commit.
2. Check audit logs for use of the secret.
3. Find and fix how it leaked.
4. Review whether the secret had more privilege than needed.

## Trade-offs

- A central secret store is a critical dependency: plan for its availability.
- Short-lived credentials add moving parts, such as renewal logic.
- Environment variables are convenient but can leak through process listings,
  crash dumps or logs if handled carelessly.

## Quick Review Questions

1. Why is deleting a committed secret from the latest commit not enough?
2. What advantage do short-lived credentials have over static ones?
3. How does workload identity reduce secrets risk?
4. Why should dev and production use different secrets?
5. What is the first step when a secret is exposed?
