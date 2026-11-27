# Software Supply Chain Security

Study lesson on protecting what goes into your software and how it is built and
delivered. Roughly a 10-minute read.

> These are general study notes, not a statement of the official exam objectives.

## Why it matters

Modern systems are mostly assembled from code you did not write: libraries,
base images, build tools, CI actions and cloud services. An attacker who
compromises any of these can reach your production environment without ever
attacking your code directly.

- **Attack surface is wider than your repo:** dependencies, build systems and
  distribution channels all count.
- **Trust is transitive:** a dependency's dependencies are also in your system.
- **Goal:** know what you ship, prove where it came from, and limit the damage
  if one piece is compromised.

## Where the chain can break

| Stage | Example risk |
|-------|--------------|
| Source | Stolen developer credentials; unreviewed or malicious commits |
| Dependencies | Vulnerable library; typosquatted or hijacked package; dependency confusion (a public package shadows an internal name) |
| Build | Compromised CI runner or plugin; build output that differs from the source |
| Artifact storage | Tampered image or package in a registry |
| Deployment | Unverified artifacts accepted by the runtime |

## Core controls

- **Pin and lock dependencies:** use lock files and exact versions or digests
  so builds are repeatable and changes are deliberate.
- **Use trusted sources:** prefer a private proxy or mirror of approved
  packages; scope internal package names to your own registry.
- **Scan continuously:** check dependencies and images for known
  vulnerabilities, in CI and on already-deployed artifacts.
- **Code review and branch protection:** require review and passing checks
  before merging.
- **Harden the build:** ephemeral, isolated runners; least-privilege and
  short-lived credentials; pin third-party CI actions to a specific version
  or commit.
- **Sign and verify artifacts:** sign images/packages at build time and have the
  deploy platform reject anything unsigned or from an unexpected builder.

## SBOM and provenance

- **SBOM (Software Bill of Materials):** a machine-readable inventory of the
  components in an artifact. It lets you answer "are we affected?" quickly when
  a new vulnerability is announced.
- **Provenance:** a signed record of how an artifact was built: which source,
  which builder, which steps.
- **Reproducible builds:** the same source yields the same output, so
  independent parties can verify an artifact was not altered.
- An SBOM says *what* is inside; provenance says *how it got there*. They
  complement each other and neither proves the code is free of bugs.

## Architectural trade-offs

| Decision | Benefit | Cost |
|----------|---------|------|
| Pin everything | Predictable, auditable builds | Must actively update to receive security fixes |
| Internal mirror of packages | Control and availability | Operating and curating the mirror |
| Fewer dependencies | Smaller attack surface | More code to write and maintain |
| Strict signature enforcement | Blocks tampered artifacts | Added pipeline complexity; risk of blocking urgent fixes if poorly managed |
| Frequent automated updates | Faster patching | More change volume to test |

## Common pitfalls

- Scanning only at build time, so newly disclosed vulnerabilities in deployed
  software go unnoticed.
- Floating tags such as `latest` for base images.
- CI secrets available to every job, including those from untrusted pull requests.
- Generating an SBOM but never using it.
- Treating "signed" as "safe": a signature proves origin, not quality.

## Connections to other topics

- **CI notes:** least privilege, pinned versions and ephemeral runners are
  supply chain controls.
- **Secrets management:** short-lived build credentials limit what a
  compromised pipeline can leak.
- **Containers:** base image choice and registry policy matter.
- **Deployment strategies:** canary rollouts limit blast radius if a bad
  artifact slips through.
- **Threat modeling:** treat the build pipeline as part of the system under
  analysis.

## Quick review questions

1. Why is a dependency's own dependency list part of your risk?
2. What is dependency confusion and how can scoping to a private registry help?
3. What is the difference between an SBOM and provenance?
4. Why is scanning only in CI insufficient?
5. What trade-off comes with pinning all versions?
6. What does signature verification at deploy time protect against, and what
   does it not?
