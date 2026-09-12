---
status: accepted
date: 2026-09-12
deciders: [ebh]
---

# Use pulldown-cmark for document parsing

*Recorded retroactively — see [ADR 0001](0001-record-architecture-decisions-with-madr.md).*

## Context and Problem Statement

The engine needs to parse Markdown to evaluate structural rules, needing high execution speed
and precise source-position mapping for diagnostics.

## Decision Drivers

* Maximum execution speed and low memory overhead for CI environments.
* Precise byte-offset tracking, to report a lint finding's exact file position.
* Ecosystem stability and battle-testing.

## Considered Options

* `pulldown-cmark` — zero-allocation streaming event parser.
* `comrak` — full arena AST tree.
* `markdown` — JS-style AST with line maps.
* `tree-sitter-markdown` — concrete syntax tree.

## Decision Outcome

Chosen option: `pulldown-cmark` as the core parsing engine.

* Zero-allocation streaming event model delivers maximum performance for CI environments.
* Native byte-offset tracking via `.into_offset_iter()` enables precise file-position
  reporting for lint findings.
* Battle-tested ecosystem stability (used by `rustdoc`).

### Consequences

* Good, because it's extremely fast with low memory overhead when scanning large
  documentation sets.
* Bad, because the event-based pull-parsing model requires maintaining state manually in code
  for rules that inspect deep document hierarchies — unlike tree-traversal libraries such as
  `comrak`.
