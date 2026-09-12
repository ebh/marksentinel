---
status: accepted
date: 2026-09-12
deciders: [ebh]
---

# Structure the engine as a library crate with a staged pipeline

## Context and Problem Statement

marksentinel needs to ship progressively — one check at a time, each independently testable
and independently mergeable — rather than as a single large rewrite of `src/main.rs`. The
engine also has natural stages implied by its own core vocabulary (`Bundle`, `Doc`,
`Finding`) and by the shape of the problem itself: discovering documents, reading one,
running checks against it, producing output. The internal structure needs to make that
progression possible instead of accidental.

## Decision Drivers

* Each check should be addable, and testable, on its own — without wiring up the CLI,
  spawning a subprocess, or touching unrelated checks.
* Progress should be externally visible: closing a check should correspond directly to
  flipping a row in README's Implementation status table, not to an internal refactor nobody
  outside the project can see.
* Keep the module split aligned with the vocabulary the project already uses for its core
  types (`Bundle`, `Doc`, `Finding`).
* Don't over-architect a project this size — no workspace, no crate-per-stage, until there's
  a demonstrated need for one.

## Considered Options

* A single `src/main.rs` that grows organically as checks are added.
* A library crate (`src/lib.rs`) + thin CLI binary (`src/main.rs`), with the library organized
  as a staged pipeline of modules: `discovery`, `document`, `checks/*` (one file per rule
  family), `report`.
* The same lib+bin split, but with all rule logic in one flat `checks.rs`/`rules.rs` file
  rather than one file per family.
* A Cargo workspace of multiple crates (e.g. `marksentinel-core`, `marksentinel-cli`).

## Decision Outcome

Chosen option: library crate + thin CLI binary, with the library split into staged modules —
`discovery`, `document`, `checks/{filename,frontmatter,links,dependency_block,log,tags,index,
orphan}`, `report` — and `src/main.rs` doing nothing but clap wiring and calling into the
library. `dependency_block` also owns `h1-missing`: finding the H1 is a prerequisite for
locating the dependency block beneath it, so the two share one parse step rather than
`h1-missing` getting a module of its own. `frontmatter` and `tags` both touch tags but at
different scopes: `frontmatter` owns the per-document shape checks (`frontmatter-tags`,
`tags-one-line`), while `tags` owns the bundle-wide vocabulary checks
(`tag-vocabulary-missing`, `tag-undeclared`, `tag-unused`) — the two never need each other's
state, so splitting by scope keeps each module's tests independent.

* Every check becomes a unit test against the library directly — a `Doc`/`Bundle` in, a
  `Vec<Finding>` out — with no CLI parsing or subprocess in the loop, which matters most while
  still learning Rust.
* One file per rule family lets a check ship, review, and merge independently of every other
  check, in the order README's Implementation status table already lists them.
* Module names track the project's own core types (`Doc`, `Bundle`, `Finding`) directly, so a
  contributor isn't translating vocabulary between design discussion and code.
* A flat single-file `checks.rs` was rejected: it would grow past readability quickly and
  forces every check's tests to share one file's compile/test cycle.
* A multi-crate workspace was rejected as premature — nothing here needs independent
  versioning or publishing yet, and it adds Cargo ceremony this project doesn't need until it
  does.

### Consequences

* Good, because a new check is: add a file under `checks/`, write its tests against
  `document`'s types, wire it into the pipeline — a repeatable, low-risk unit of work.
* Good, because the module boundary matches the rule groups documented in README, so "is this
  check implemented" is answerable by looking at one file, not by searching the whole crate.
* Bad, because there's directory/module ceremony up front (`discovery`, `document`, `report`,
  an empty `checks/` tree) before a single check exists — a small delay before anything is
  visibly working.

This doesn't revisit [ADR 0004](0004-use-pulldown-cmark-for-document-parsing.md) —
`pulldown-cmark` stays as the parsing engine `document` is built on.
