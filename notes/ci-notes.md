# CI Notes

Exam-prep notes on Continuous Integration (CI).

## What is CI?

Continuous Integration is the practice of merging small changes into a shared
branch frequently, with every change automatically built and verified.

- **Goal:** find integration problems early, while changes are small.
- **Trigger:** a push or pull request starts an automated pipeline.
- **Outcome:** a pass/fail signal that gates merging.

## Core Practices

- Keep a single shared source repository.
- Integrate small changes often (at least daily).
- Automate the build and make it self-testing.
- Keep the pipeline fast; fix a broken build immediately.
- Make results visible to the whole team.

## Typical Pipeline Stages

| Stage | Purpose |
|-------|---------|
| Checkout | Fetch the source at the commit under test |
| Build | Compile / package the artifact |
| Test | Unit, integration and other automated tests |
| Static analysis | Linting, security and quality scans |
| Publish | Store the build artifact for later delivery |

## CI vs CD

- **Continuous Integration:** automatically build and test every change.
- **Continuous Delivery:** every passing change is *releasable*; deployment
  to production is a manual decision.
- **Continuous Deployment:** every passing change is deployed automatically.

## Architectural Considerations

- **Pipeline as code:** define pipelines in version-controlled files.
- **Reproducibility:** use clean, ephemeral runners and pinned versions.
- **Least privilege:** grant the pipeline only the permissions it needs.
- **Fast feedback:** run cheap checks first; parallelise independent jobs.
- **Branch protection:** require status checks to pass before merging.

## Example: This Repository

This repo uses GitHub Actions (`.github/workflows/markdown-only.yml`).

- **Triggers:** every pull request, and every push to `main`.
- **Job:** `check-file-types` on `ubuntu-latest`.
- **Permissions:** `contents: read` only (least privilege).
- **Logic:** lists tracked files with `git ls-files` and fails if any is not
  `.md`, except `.gitignore` and files under `.github/`.
- **Gate:** `check-file-types` must pass before a PR can be merged.

## Quick Review Questions

1. What problem does CI primarily solve?
2. How do Continuous Delivery and Continuous Deployment differ?
3. Why should pipeline permissions be minimal?
4. Which check gates merges in this repository?
