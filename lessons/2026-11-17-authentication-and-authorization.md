# Authentication and Authorization

Study lesson on proving who a caller is and deciding what they may do.

## Two Different Questions

- **Authentication (AuthN):** who are you? Verify an identity.
- **Authorization (AuthZ):** what are you allowed to do? Decide on access.

Authentication comes first, but passing it says nothing about permissions.
Mixing the two up is a classic design mistake.

## Identities Are Not Only People

- **Users:** employees, customers, administrators.
- **Workloads:** services, jobs and pipelines calling other services.
- **Devices:** laptops, IoT hardware, build runners.

Prefer short-lived, automatically issued credentials for workloads over
long-lived shared keys (see the secrets management lesson).

## Common Authentication Approaches

| Approach | Idea | Trade-off |
|----------|------|-----------|
| Password + MFA | Something known plus something held | Familiar, but phishable depending on the factor |
| Single sign-on (SSO) | One central identity provider issues proof of login | Central control; the provider becomes critical |
| Token-based (e.g. OAuth 2.0 / OpenID Connect) | Client presents a signed, time-limited token | Stateless checks; token theft and expiry need handling |
| Mutual TLS | Both sides present certificates | Strong service identity; certificate lifecycle overhead |

## Authorization Models

- **RBAC (role-based):** permissions attach to roles; users get roles.
  Simple to audit, can lead to role explosion.
- **ABAC (attribute-based):** decisions use attributes of user, resource
  and context (such as department or time). Flexible, harder to reason about.
- **ACLs:** per-resource lists of who may do what. Fine-grained, hard to
  manage at scale.

## Where to Enforce

- **At the edge:** the gateway authenticates and rejects obvious bad calls.
- **In every service:** each service checks authorization itself; do not
  assume a request is safe because it came from "inside".
- **Central policy, local enforcement:** keep rules in one place, apply them
  at each service.

## Architectural Considerations

- **Use a proven identity provider** rather than building your own.
- **Least privilege:** grant minimal access and review it periodically.
- **Deny by default:** access exists only when explicitly granted.
- **Short-lived tokens with scopes:** limit the damage of a leaked token.
- **Propagate identity** across service calls so audits show the real actor.
- **Log access decisions** to support investigation (ties to observability).
- **Plan for revocation:** know how a compromised credential is cut off.

## Example: This Repository

Using the setup in `notes/ci-notes.md`:

- **Authentication:** contributors sign in to GitHub; the workflow runs
  under a token issued automatically for each run.
- **Authorization:** the workflow token has `contents: read` only, and
  merging to `main` requires a pull request and a passing check.

## Common Pitfalls

- Treating a valid login as permission to do anything.
- Checking access only in the UI, not in the API.
- Long-lived, broadly scoped service credentials.
- Hard-coding roles in code so they cannot be reviewed or changed.
- No tested process for revoking access quickly.

## Quick Review Questions

1. What is the difference between authentication and authorization?
2. Why should every service check authorization, not only the gateway?
3. Compare RBAC and ABAC with one strength of each.
4. Why prefer short-lived tokens for workloads?
5. What does "deny by default" mean in practice?
