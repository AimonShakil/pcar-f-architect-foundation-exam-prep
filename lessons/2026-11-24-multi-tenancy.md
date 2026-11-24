# Multi-Tenancy

Study lesson on designing systems that serve multiple customers (tenants).
Roughly a 10-minute read.

> These are general study notes, not a statement of the official exam objectives.

## Why it matters

A tenant is a customer or organisation whose data and configuration must be
kept separate from others'. Multi-tenancy means one system serves many tenants.
The core tension is **sharing** (lower cost, simpler operations) versus
**isolation** (security, performance, compliance).

## Isolation models

| Model | Description | Strength | Trade-off |
|-------|-------------|----------|-----------|
| Shared everything | All tenants share app and database; rows carry a tenant ID | Cheapest, easiest to operate | Weakest isolation; one bug can leak data |
| Schema per tenant | Shared database server, separate schema per tenant | Better data separation | Migrations run many times; limits on schema count |
| Database per tenant | Each tenant has its own database | Strong data isolation, per-tenant restore | Higher cost and operational overhead |
| Stack per tenant | Separate deployment of the whole system | Strongest isolation, custom versions possible | Most expensive; fleet management is hard |

Many systems mix models, for example a shared stack for small tenants and
dedicated resources for large or regulated ones (a "tiered" approach).

## Key concerns

- **Tenant identification:** resolve the tenant early (token claim, subdomain,
  header) and carry it through every layer. Never trust a tenant ID supplied
  by the client without checking it against the authenticated identity.
- **Data isolation:** enforce at more than one layer, such as query filters,
  database row-level policies and separate encryption keys.
- **Noisy neighbours:** one tenant's heavy usage can degrade others. Mitigate
  with rate limits, quotas, queues and per-tenant resource limits.
- **Per-tenant customisation:** prefer configuration over custom code, or the
  system turns into many forks.
- **Onboarding and offboarding:** automate tenant creation; plan data export
  and deletion when a tenant leaves.
- **Observability and cost:** tag metrics, logs and costs with the tenant ID
  to find noisy tenants and support billing or chargeback.

## Common pitfalls

- Forgetting the tenant filter in one query (a classic data leak).
- Shared caches or queues whose keys do not include the tenant.
- Background jobs that run without tenant context.
- Picking database-per-tenant, then discovering thousands of tenants make
  migrations and monitoring painful.
- Assuming one tenant's size predicts all tenants; a few large tenants often
  dominate load.

## Decision guide

- **Strict compliance or residency needs?** Lean toward stronger isolation.
- **Many small tenants and tight margins?** Lean toward shared resources.
- **Wide variation in tenant size?** Consider tiers.
- **Need per-tenant backup and restore?** Separate databases make it easy.
- Remember that moving a tenant between models later is possible but costly,
  so build tenant context into the design from the start.

## Quick review questions

1. What is the central trade-off in multi-tenant design?
2. Why enforce isolation at several layers rather than only in application code?
3. What is a noisy neighbour, and how can you limit it?
4. When might a tiered isolation approach be useful?
5. Why should the tenant ID come from the authenticated identity?
