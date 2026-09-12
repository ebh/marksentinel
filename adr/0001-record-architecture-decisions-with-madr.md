# Record architecture decisions with MADR

* Status: Accepted
* Deciders: ebh
* Date: 2026-09-12

## Context and Problem Statement

marksentinel is expected to stay small, but the decisions behind it (language, core
dependencies, rule design) aren't obvious from the code alone. Later contributors — including
the current maintainer, revisiting a choice months on — need a record of what was decided and
why, without that record turning into a maintenance burden of its own.

## Decision Drivers

* Keep the footprint minimal — this project won't accumulate a large decision log, so the
  format and tooling shouldn't assume one will.
* No new runtime or build-time dependency for the project itself.
* Must not collide with, or be mistaken for, the `docs/` OKF profile this tool enforces on
  *other* repos — an ADR is a different kind of document with different rules.

## Considered Options

* Nygard-style ADRs authored/numbered with `adr-tools`.
* MADR template, scaffolded with `adr-tools` pointed at a custom template.
* `log4brains` (MADR-based, with a browsable static site and search).

## Decision Outcome

Chosen option: MADR template, scaffolded with `adr-tools`, stored under `/adr/` at the repo
root (not `docs/`).

* MADR's structured fields (status, drivers, considered options, consequences) give more to
  work with later than Nygard's freer-form template, at no extra tooling cost.
* `/adr/` sits outside every directory literally named `docs`, so marksentinel's own bundle
  discovery never picks these files up — no OKF frontmatter, no exemption logic needed.
* `log4brains` was rejected: it pulls in a Node toolchain and a static-site build for a
  decision log this project doesn't expect to outgrow plain files in git.

### Consequences

* Good, because ADRs stay plain Markdown, readable and diffable in git with no build step.
* Good, because the MADR fields double as a lightweight checklist when writing one — context,
  drivers, alternatives, consequences all have a place.
* Bad, because `adr-tools`' default template is Nygard's, not MADR's — scaffolding a new ADR
  requires pointing it at a MADR template explicitly rather than working out of the box.
