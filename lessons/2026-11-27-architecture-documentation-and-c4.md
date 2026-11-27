# Architecture Documentation and the C4 Model

Study lesson on communicating an architecture clearly to different audiences.
Roughly a 10-minute read.

> These are general study notes, not a statement of the official exam objectives.

## Why it matters

An architecture that only lives in a few heads cannot be reviewed, onboarded
into, or safely changed. Documentation is a communication tool: its value is
measured by whether the intended reader can make a decision with it.

- **Know the audience:** executives, developers, operators and security
  reviewers need different views of the same system.
- **Keep it just enough:** documents that are never updated mislead more than
  they help.
- **Prefer a few clear diagrams** over one diagram that tries to show everything.

## The C4 model

C4 describes a system at four zoom levels, like a map.

| Level | Shows | Typical audience |
|-------|-------|------------------|
| 1. System context | The system as one box, its users and external systems | Everyone, including non-technical people |
| 2. Container | Separately deployable or runnable parts: web app, API, database, queue | Developers, operators |
| 3. Component | Major building blocks inside one container | Developers on that container |
| 4. Code | Classes or modules, often generated from code | Rarely drawn by hand |

- A "container" here is a runtime unit (an app or data store), **not** necessarily
  a Docker container.
- Most teams get the most value from levels 1 and 2.
- Zoom in only where the detail helps a decision.

## Making diagrams readable

- **Title every diagram** and say which level or view it is.
- **Label every box** with a name, its type and a short responsibility.
- **Label every arrow** with direction and purpose (for example "reads orders
  over HTTPS").
- **Include a legend** if shapes or colours carry meaning.
- **One level of detail per diagram;** do not mix a database table with a
  whole external system.
- Avoid unexplained abbreviations and decorative icons.

## Views for different concerns

Beyond structure, some concerns need their own view.

- **Deployment view:** where containers run (regions, networks, clusters).
- **Data flow view:** how sensitive data moves, useful for threat modeling.
- **Runtime or sequence view:** how components interact for one key scenario.
- **Decision records:** the *why* behind choices (see the ADR lesson).

## Docs as code

- Keep diagrams and text in **version control** next to the system, written as
  text where possible (Markdown, or a diagram-as-code tool).
- Review documentation changes in **pull requests**, like code.
- Update docs in the same change that alters the architecture.
- Generate what can be generated (API specs, dependency graphs) so it cannot
  drift from reality.

## Common pitfalls

- A single "big picture" diagram nobody can read.
- Boxes and arrows with no meaning attached.
- Documentation written once and never revisited.
- Describing *what* the system is but never *why* it is that way.
- Treating documentation as a separate phase instead of part of the work.

## Architect's checklist

- Who is the reader, and what decision should this help them make?
- Is there a context diagram a newcomer can understand in two minutes?
- Are arrows labelled and is the level of detail consistent?
- Where do the docs live, and who updates them?
- Do decision records explain the trade-offs behind the structure?

## Quick review questions

1. What are the four levels of the C4 model, and which two give the most value
   for most teams?
2. Does a C4 "container" mean a Docker container? Why or why not?
3. Name three things that make a diagram easier to read.
4. Why keep documentation in version control and review it in pull requests?
5. Why are decision records a useful companion to diagrams?
