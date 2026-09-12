---
status: accepted
date: 2026-09-12
deciders: [ebh]
---

# Use Rust as the implementation language

*Recorded retroactively — this decision predates the project's adoption of ADRs; see
[ADR 0001](0001-record-architecture-decisions-with-madr.md).*

## Context and Problem Statement

marksentinel needed an implementation language before any code was written. This ADR captures
that choice after the fact, to the extent it can be reconstructed.

## Decision Drivers

* Rust is increasingly the language of choice for new developer tooling (e.g. `oxlint`),
  giving confidence in the ecosystem and toolchain maturity for a CLI linter.
* Additional considerations factored into the choice that aren't recorded here.

## Considered Options

* Not formally evaluated against alternatives before deciding — recorded as a single option
  chosen outright, not a comparison.

## Decision Outcome

Chosen option: Rust.

### Consequences

* Good, because it produces a single, dependency-free binary well suited to a CLI tool
  invoked from CI.
* Good, because it gives access to a growing ecosystem of Rust-based linting and tooling
  crates to draw on.
* Bad, because the pool of contributors comfortable with Rust is smaller than for, say,
  TypeScript or Python — a consideration for anyone weighing future contribution friction.
