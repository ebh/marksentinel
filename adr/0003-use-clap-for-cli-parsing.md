# Use clap for CLI parsing

* Status: Accepted (recorded retroactively — see [ADR 0001](0001-record-architecture-decisions-with-madr.md))
* Deciders: ebh
* Date: 2026-09-12

## Context and Problem Statement

marksentinel requires command-line interface parsing for target paths, configuration flags,
and formatting options. Priority is minimizing CLI setup overhead to focus effort on the
linter's rule logic instead.

## Decision Drivers

* Minimize time spent on argument-parsing boilerplate.
* Get `--help`, `--version`, and input validation without hand-rolling them.
* Room to add future subcommands or flags (e.g. JSON output) without an architecture change.

## Considered Options

* `clap` (v4, with the `derive` feature).
* `lexopt` — a minimalist pull parser.
* `pico-args` — a simple argument extractor.

## Decision Outcome

Chosen option: `clap` (v4) with the `derive` macro feature enabled.

* Declarative, struct-based definitions eliminate manual parsing loops and argument mapping.
* Generates `--help`, `--version`, and input validation automatically.
* Ecosystem standard — adding future subcommands or flags won't require an architecture
  change.

### Consequences

* Good, because it enables faster feature delivery and a clean separation between CLI parsing
  and core linter logic, once the latter moves out of `src/main.rs` into its own module or
  library crate.
* Bad, because derive-macro expansion adds to cold compilation times — judged an acceptable
  trade-off for the developer ergonomics gained.
