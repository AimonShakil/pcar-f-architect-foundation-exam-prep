# Data Storage Selection

Study lesson on choosing the right data store for a workload. Roughly a
10-minute read.

> These are general study notes, not a statement of the official exam objectives.

## Why it matters

Storage choices are among the hardest decisions to reverse. Data has gravity:
once a store holds years of records and many services depend on its shape,
migrating is slow and risky. Choose based on access patterns, not fashion.

- **Start from the questions:** how is the data read, written and queried?
- **Then pick the store:** not the other way round.

## Storage families

| Family | Good for | Typical trade-off |
|--------|----------|-------------------|
| Relational (SQL) | Structured data, joins, strong transactions | Harder to scale writes horizontally |
| Key-value | Lookups by key, sessions, caches | Limited querying beyond the key |
| Document | Self-contained records with flexible schema | Cross-document joins and transactions are weaker |
| Wide-column | Very high write volume, known query patterns | Queries must be designed up front |
| Graph | Highly connected data, relationship traversal | Narrower tooling and use cases |
| Search index | Full-text search, filtering, ranking | Usually not the source of truth |
| Object storage | Files, media, backups, data lakes | No in-place edits, higher latency |
| Time-series | Metrics and events ordered by time | Specialised; less suited to general queries |

Specific products vary in how closely they match these categories, so check
each product's actual guarantees.

## Questions to ask

1. **Access pattern:** read-heavy, write-heavy, or mixed? Point lookups or ad hoc queries?
2. **Consistency:** is stale data acceptable, or must every read see the latest write?
3. **Structure:** is the schema stable, or does it change often?
4. **Volume and growth:** what is the data size now and expected later?
5. **Latency:** what response time does the user or caller need?
6. **Durability and recovery:** what are the backup, retention and recovery needs?
7. **Compliance:** are there residency, encryption or deletion requirements?
8. **Team skills:** can the team operate this store reliably?

## Key concepts

- **OLTP vs OLAP:** transactional workloads handle many small reads and writes;
  analytical workloads scan large volumes. They often belong in different stores.
- **Normalisation vs denormalisation:** normalised data avoids duplication;
  denormalised data speeds up reads at the cost of keeping copies in sync.
- **Scaling reads:** replicas and caches. **Scaling writes:** partitioning
  (sharding), which needs a well-chosen partition key to avoid hot spots.
- **Indexes:** speed up reads, slow down writes and use space.
- **Hot, warm and cold tiers:** place data on cheaper storage as it ages.

## Polyglot persistence

Using several stores, each fitted to one job, is common. For example: a
relational database as the source of truth, a cache for hot reads, a search
index for text queries, and object storage for files.

- **Benefit:** each workload gets a store that suits it.
- **Cost:** more to operate, secure, back up and monitor.
- **Risk:** copies drift apart. Name one source of truth and derive the rest
  from it, for instance through events.

## Common pitfalls

- Picking a store for one feature and forcing every other workload onto it.
- Using a search index or cache as the only copy of important data.
- Ignoring the partition key until traffic is already uneven.
- Adding stores without owners, backups or monitoring.
- Choosing a distributed store when a single relational database would do.

## Architectural considerations

- **Default to simple:** a well-run relational database fits many systems.
- **Isolate data access:** hide the store behind an interface so change stays possible.
- **Own your data:** in microservices, each service owns its data.
- **Record the decision:** write an ADR that lists the alternatives considered.

## Quick Review Questions

1. Why is a storage decision hard to reverse?
2. Name two workloads where a relational database is a good fit and one where it may not be.
3. What is the main risk of polyglot persistence, and how do you reduce it?
4. Why is a search index usually not the source of truth?
5. What makes a good partition key?
