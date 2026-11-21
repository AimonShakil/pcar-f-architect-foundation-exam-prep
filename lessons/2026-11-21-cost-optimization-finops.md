# Cost Optimization and FinOps

Study lesson on designing for cost and managing cloud spend. Roughly a
10-minute read.

> These are general study notes, not a statement of the official exam objectives.

## Why architects care about cost

Cost is a quality attribute like any other. In the cloud, architecture
decisions (instance sizes, data placement, chattiness between services)
translate directly into a recurring bill, and the bill can change daily.

- **Goal:** deliver the required quality at an acceptable, predictable cost.
- **Not the goal:** the lowest possible cost. Cheap but unavailable is a failure.

## FinOps in one paragraph

FinOps is the practice of bringing engineering, finance and product together
to make spending visible and to make trade-offs deliberately. It is usually
described as a loop: **inform** (see the spend), **optimize** (act on it),
**operate** (make it routine).

## Cost drivers to recognise

| Driver | Typical cause |
|--------|---------------|
| Compute | Over-provisioned or idle instances, always-on dev environments |
| Storage | Old snapshots, unneeded copies, hot tier for cold data |
| Data transfer | Cross-region or cross-zone traffic, egress to the internet |
| Managed services | Premium tiers chosen "just in case" |
| Observability | High-cardinality metrics, unlimited log retention |
| People/operations | Manual toil that automation would remove |

## Core optimization levers

- **Right-size:** match resources to measured usage, not guesses.
- **Scale to demand:** autoscale, and shut down non-production at night.
- **Pricing models:** commit to steady baseline load for a discount; use
  interruptible capacity only for work that tolerates being stopped.
- **Storage tiering and lifecycle rules:** move or expire data as it ages.
- **Reduce transfer:** keep chatty components close together; use caching.
- **Serverless / managed:** pay per use for spiky workloads, but check that
  steady high load is not cheaper on fixed capacity.
- **Delete waste:** unattached volumes, unused IPs, abandoned environments.

## Make cost visible

- **Tagging / labelling:** tag resources by team, service and environment so
  spend can be attributed.
- **Budgets and alerts:** notify owners when spend deviates from forecast.
- **Unit economics:** track cost per customer, request or transaction, not
  only the total. Total spend rising can be healthy if unit cost falls.
- **Showback / chargeback:** show or bill teams for what they use so they
  feel the consequences of their design.

## Trade-offs

- **Cost vs availability:** multi-region active-active costs far more than
  backup-and-restore; choose based on recovery targets
  (see the resilience lesson).
- **Cost vs performance:** caching and larger instances cost money but can
  reduce other costs or meet latency goals.
- **Cost vs agility:** heavy commitments lower unit price but reduce
  flexibility if the architecture changes.
- **Cost vs engineering time:** a small saving is not worth weeks of effort.
  Compare the saving with the people cost of achieving it.

## Common pitfalls

- Optimizing once, then never revisiting; usage drifts.
- Cutting cost without measuring the effect on reliability or latency.
- No ownership: spend that belongs to nobody is never reduced.
- Ignoring data transfer until it appears on the invoice.
- Treating cost as a finance concern only, instead of a design input.

## Example: This Repository

This repo's CI keeps cost low by design: one small job, read-only
permissions, and a cheap file-type check that runs on every pull request. It
shows the principle of running inexpensive checks first.

## Quick Review Questions

1. Why is cost considered a quality attribute?
2. Name three common cloud cost drivers and one lever for each.
3. Why track unit cost rather than only total spend?
4. How do tags support cost attribution?
5. Give an example where lowering cost would hurt another quality attribute.
