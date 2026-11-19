# Containers and Orchestration

Study lesson on packaging and running workloads with containers. About a
10-minute read.

## Why It Matters

Containers are a common way to package and run services consistently across
environments. Architects need to know what they isolate, what they do not,
and what an orchestrator adds on top.

## Definitions

- **Container image:** an immutable, layered package of an application and its
  dependencies.
- **Container:** a running instance of an image, isolated by the host kernel
  (namespaces and cgroups) while sharing that kernel.
- **Registry:** a store for versioned images.
- **Orchestrator:** a system that schedules, connects, scales and heals
  containers across many machines (Kubernetes is the best-known example).

## Containers vs Virtual Machines

| Aspect | Container | Virtual machine |
|--------|-----------|-----------------|
| Isolation | Process-level, shared host kernel | Hardware-level, own guest OS |
| Startup | Typically fast | Typically slower |
| Footprint | Usually smaller | Usually larger |
| Isolation strength | Weaker boundary | Stronger boundary |

Rule of thumb: containers trade some isolation strength for density and speed.

## Image Practices

- Build from a small, trusted base image.
- Pin versions; avoid mutable tags such as `latest` in production.
- Keep one main process per container.
- Run as a non-root user; drop unneeded capabilities.
- Never bake secrets into images (see the secrets management lesson).
- Scan images for known vulnerabilities in the CI pipeline.
- Keep images immutable: change by rebuilding, not by patching a running one.

## What an Orchestrator Provides

- **Scheduling:** place workloads on nodes based on resource requests.
- **Self-healing:** restart failed containers; replace unhealthy ones.
- **Scaling:** add or remove replicas manually or by load metrics.
- **Service discovery and load balancing:** stable names for changing instances.
- **Rolling updates and rollbacks:** see the deployment strategies lesson.
- **Configuration and secrets injection:** keep them outside the image.

## Key Kubernetes Concepts

| Concept | Role |
|---------|------|
| Pod | Smallest deployable unit; one or more tightly coupled containers |
| Deployment | Declares desired replicas and manages rolling updates |
| Service | Stable network endpoint in front of a set of pods |
| ConfigMap / Secret | Configuration and sensitive data supplied at runtime |
| Namespace | Logical grouping for isolation and quotas |
| Probes | Liveness (restart me?) and readiness (send me traffic?) checks |

The model is **declarative**: you state the desired state and the control
loop works to make reality match it.

## Stateless vs Stateful Workloads

- Stateless services fit containers naturally: any instance can be replaced.
- Stateful workloads need persistent storage, stable identity and careful
  backup and recovery planning.
- Often the better choice is a managed data service outside the cluster.

## Architectural Trade-offs

- **Portability:** images run similarly across environments, but surrounding
  services (networking, storage, IAM) may still differ.
- **Operational complexity:** running an orchestrator is real work; managed
  offerings reduce it but do not remove it.
- **Right-sizing:** set resource requests and limits so scheduling and
  scaling behave predictably.
- **Security:** a shared kernel means a compromised container is a concern;
  use least privilege, network policies and patched hosts.
- **Fit:** a few simple services may not need an orchestrator.

## Common Pitfalls

- Using `latest` tags, making deployments non-reproducible.
- Missing readiness probes, so traffic hits containers that are not ready.
- No resource limits, letting one workload starve its neighbours.
- Storing data on a container's local filesystem and losing it on restart.
- Treating the orchestrator as a substitute for observability.

## Quick Review Questions

1. What do containers share with the host that virtual machines do not?
2. Why avoid mutable image tags in production?
3. What is the difference between a liveness and a readiness probe?
4. What does "declarative" mean in the context of an orchestrator?
5. When might an orchestrator be more than a system needs?
