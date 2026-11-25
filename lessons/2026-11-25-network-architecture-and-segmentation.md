# Network Architecture and Segmentation

Study lesson on structuring networks to limit exposure and control traffic.
Roughly a 10-minute read.

> These are general study notes, not a statement of the official exam objectives.

## Why it matters

Network design decides what can talk to what. Good structure limits the
**blast radius** of a compromise, keeps internal services off the public
internet and makes traffic easier to monitor. Poor structure (a "flat"
network) lets one breached host reach everything.

## Building blocks

| Concept | Purpose | Notes |
|---------|---------|-------|
| Subnet | Slice of an address range | Group resources with similar exposure |
| Public vs private subnet | Reachable from the internet or not | Keep databases and internal services private |
| Firewall / security group | Allow or deny traffic by rule | Default deny, then allow what is needed |
| Load balancer | Spread traffic across instances | Also terminates TLS and runs health checks |
| Reverse proxy / API gateway | Single entry point for requests | Central place for auth, rate limits, routing |
| NAT gateway | Outbound-only internet access for private hosts | Inbound connections are not allowed |
| DNS | Maps names to addresses | Enables failover and service discovery |
| Private connectivity (VPN, private link) | Reach services without public exposure | Reduces public attack surface |

## Common layered layout

1. **Edge:** DNS, CDN, web application firewall, load balancer.
2. **Application tier:** private subnets running stateless services.
3. **Data tier:** most restricted subnets; accept connections only from the
   application tier.
4. **Management path:** controlled admin access (bastion or session manager),
   never open administrative ports to the internet.

## Segmentation and zero trust

- **Segmentation:** split the network into zones (by environment, sensitivity
  or tenant) and control the traffic between them.
- **East-west vs north-south:** north-south traffic crosses the network
  boundary; east-west moves between internal services. Attackers often move
  east-west after getting in, so filter it too.
- **Zero trust:** do not treat being "inside the network" as proof of
  trustworthiness. Authenticate and authorise each request, ideally with
  mutual TLS or signed tokens between services.
- **Micro-segmentation:** per-workload rules, for example allowing only the
  checkout service to reach the payments service on one port.

## Design considerations

- **Least privilege for traffic:** open specific ports to specific sources,
  not broad ranges.
- **Encrypt in transit:** inside the network as well as at the edge.
- **Separate environments:** keep production apart from dev and test.
- **Egress control:** restrict outbound traffic to limit data exfiltration
  and command-and-control.
- **Availability:** spread across several zones; make DNS and load balancer
  health checks part of the failover design.
- **Observability:** collect flow logs and firewall logs to see actual traffic.
- **Cost:** cross-zone and cross-region traffic and NAT often carry charges,
  so factor them into the layout.

## Common pitfalls

- Rules like "allow all from anywhere" added during debugging and never removed.
- A flat network where every host can reach the database.
- Trusting internal traffic and skipping authentication between services.
- Long DNS cache lifetimes (TTLs) that slow failover.
- Overlapping address ranges that block later connection of networks.
- Forgetting egress rules, leaving outbound traffic wide open.

## Decision guide

- **Does this component need to be reachable from the internet?** If not,
  place it in a private subnet.
- **Who legitimately calls this service?** Allow only those sources.
- **Is the data sensitive or regulated?** Use a dedicated, tighter segment.
- **Many services talking to each other?** Consider a service mesh or
  consistent mutual TLS rather than relying on network location alone.

## Quick review questions

1. What is blast radius, and how does segmentation reduce it?
2. Which tiers belong in private subnets, and why?
3. What is the difference between north-south and east-west traffic?
4. What does zero trust change about "inside the network"?
5. Why control egress traffic as well as ingress?
