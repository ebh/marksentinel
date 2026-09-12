# marksentinel

[![CI](https://github.com/ebh/marksentinel/actions/workflows/ci.yml/badge.svg)](https://github.com/ebh/marksentinel/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

A mechanical conformance checker for Markdown documentation. It enforces the
[OKF (Open Knowledge Format)](https://okf.md/) documentation profile against any `docs/`
directory in a repo: YAML frontmatter shape, cross-document link integrity, heading-anchor
correctness, tag-vocabulary closure, index completeness, and log ordering.

It exists because these rules are invisible to a type checker, a general-purpose linter, or a
formatter — they're specific to how a repo's documentation is organized, and nothing else in
the toolchain checks them.

marksentinel deliberately does not check general Markdown formatting (bullet markers, table
padding, heading style) — that's a formatter's job, not a documentation-profile checker's.
Run a dedicated Markdown formatter such as [`oxfmt`](https://oxc.rs/docs/guide/usage/formatter.html)
first; marksentinel checks the things a formatter can't.

## Getting started

Install instructions are TBC until packaging/publishing is settled. For now, build from
source — see [Development](#development).

## Using

```
marksentinel [PATHS]... [OPTIONS]
```

With no paths, marksentinel checks every `docs/` bundle in the repo. Pass one or more
file/directory paths to limit which documents are *reported* — bundle-wide checks (tag
vocabulary, index completeness, orphans) always evaluate the whole bundle a document belongs
to, even when only part of it was selected.

### Options

| Flag | Description |
|---|---|
| `--review` | Also print [review material](#review-material) — things worth a human or model's judgement, not a pass/fail verdict |
| `--json` | Emit findings as JSON instead of the human-readable report |
| `--root <DIR>` | Treat `<DIR>` as the repo root instead of the current checkout |

### Exit codes

| Code | Meaning |
|---|---|
| `0` | Clean run — no `ERROR`-level findings among the reported set (warnings alone don't fail a run) |
| `1` | At least one `ERROR`-level finding among the reported set |
| `2` | Couldn't run at all — no `docs/` bundle found, bad arguments, or a given path matched no document |

### Review material

Some things aren't a pass/fail check — they need a human or model's judgement. `--review`
prints these separately, alongside (never mixed into) the findings: where an index's
description of a document diverges from that document's own frontmatter description, every
document's stated dependency chain, and every link a document makes out of the documentation
into source code.

## Rules

Every check below produces zero or more **findings** against a document. A finding is either
`ERROR` (the document doesn't conform, and the run fails) or `WARN` (worth a human's
attention, but never fails the run on its own).

### Filenames

- Every Markdown filename must be lower-kebab-case (`word-word.md`) — a filename names a
  concept, not a person, a date, or free text.

### Frontmatter

- Every non-reserved document needs a YAML frontmatter block.
- `type` is required and non-empty.
- `title` is recommended.
- `description` is recommended, and when present must be a single sentence ending in
  terminal punctuation.
- `tags`, when present, must be an array, written as a single-line flow sequence
  (`tags: [a, b, c]`) — a `grep` across the bundle is how tags get discovered, and anything
  else prints as a bare `tags:`.
- `timestamp`, when present, must be a real calendar date in `YYYY-MM-DD` form.
- A reserved file (`index.md`, `log.md`) must carry no frontmatter at all — except the
  canonical root's `index.md`, which must declare a non-empty `okf_version`.

### Links

- Every internal link must be an absolute, repo-rooted path starting with `/`, never
  relative.
- It must resolve to a file that exists (a dead link from a log file is a warning, not an
  error — a log is a historical record).
- An anchored link (`#heading` or `/path#heading`) must resolve to a heading that actually
  exists, using the same slug algorithm GitHub uses to render anchors.
- External links (anything with a URL scheme, or `//`) are exempt.

### Dependency blocks

- Every document needs an H1 heading.
- A document may declare a dependency block (a `Read first:` / `Read with:` blockquote)
  directly under the H1 — at most one per document, and it must come first.
- A document a `Read first:` clause names as a hard prerequisite must not also be linked
  later as a bare, unanchored see-also.

### Reserved files

- **`log.md`** — every `##` heading must be a `YYYY-MM-DD` date, in descending order.
- **`index.md`** (every bundle, not just the canonical root) must link to every non-reserved
  document in its own directory, plus the index of every immediate child sub-bundle.

### Bundle-wide

- **Tag vocabulary closure** — a sub-bundle index may declare a controlled vocabulary (under
  a "Tag vocabulary" heading). When it does, every tag a member document uses must appear in
  it, and every declared tag should be used by at least one document.
- **Orphan detection** — every non-reserved document must be reachable by following links
  from within its own bundle.

### Implementation status

| Check | Severity | Rule | Implemented |
|---|---|---|---|
| [`filename`](docs/checks/filename.md) | ERROR | Lower-kebab-case filename | ❌ |
| [`frontmatter-missing`](docs/checks/frontmatter-missing.md) | ERROR | Non-reserved document needs frontmatter | ❌ |
| [`frontmatter-parse`](docs/checks/frontmatter-parse.md) | ERROR | Frontmatter is well-formed YAML | ❌ |
| [`frontmatter-type`](docs/checks/frontmatter-type.md) | ERROR | `type` is required and non-empty | ❌ |
| [`frontmatter-title`](docs/checks/frontmatter-title.md) | WARN | `title` is recommended | ❌ |
| [`frontmatter-description`](docs/checks/frontmatter-description.md) | WARN | `description` is recommended and a full sentence | ❌ |
| [`frontmatter-tags`](docs/checks/frontmatter-tags.md) | WARN/ERROR | `tags` is recommended and must be an array | ❌ |
| [`tags-one-line`](docs/checks/tags-one-line.md) | ERROR | `tags:` is a single-line flow array | ❌ |
| [`timestamp`](docs/checks/timestamp.md) | ERROR | `timestamp` is a real `YYYY-MM-DD` date | ❌ |
| [`frontmatter-reserved`](docs/checks/frontmatter-reserved.md) | ERROR | Reserved files carry no frontmatter (except the canonical root index) | ❌ |
| [`link-relative`](docs/checks/link-relative.md) | ERROR | Internal links are absolute, repo-rooted paths | ❌ |
| [`link-dead`](docs/checks/link-dead.md) | ERROR (WARN in `log.md`) | Internal links resolve to a real file | ❌ |
| [`link-anchor`](docs/checks/link-anchor.md) | ERROR | Anchored links resolve to a real heading | ❌ |
| [`h1-missing`](docs/checks/h1-missing.md) | ERROR | Document has an H1 heading | ❌ |
| [`read-block-duplicate`](docs/checks/read-block-duplicate.md) | ERROR | At most one dependency block | ❌ |
| [`read-block-position`](docs/checks/read-block-position.md) | ERROR | Dependency block comes first under the H1 | ❌ |
| [`read-first-repeated`](docs/checks/read-first-repeated.md) | WARN | A stated hard prerequisite isn't also a later see-also | ❌ |
| [`log-heading`](docs/checks/log-heading.md) | ERROR | Every `log.md` `##` heading is a date | ❌ |
| [`log-order`](docs/checks/log-order.md) | ERROR | `log.md` dates are descending | ❌ |
| [`index-missing-entry`](docs/checks/index-missing-entry.md) | ERROR | Index links every document and child sub-bundle | ❌ |
| [`tag-vocabulary-missing`](docs/checks/tag-vocabulary-missing.md) | WARN | Sub-bundle index declares a tag vocabulary | ❌ |
| [`tag-undeclared`](docs/checks/tag-undeclared.md) | ERROR | A used tag is in the bundle's vocabulary | ❌ |
| [`tag-unused`](docs/checks/tag-unused.md) | WARN | A declared tag is used by some document | ❌ |
| [`orphan`](docs/checks/orphan.md) | WARN | Every document is reachable from within its bundle | ❌ |

## Architecture decisions

Significant architectural decisions are recorded as lightweight ADRs in [`/adr/`](adr/), using
the [MADR](https://adr.github.io/madr/) template — not `docs/`, so they stay outside the OKF
profile this tool itself enforces. Each is numbered sequentially and never renumbered or
rewritten in place; a superseded decision gets a new ADR, with the old one's status updated to
point at it.

To add one: scaffold with [adr-tools](https://github.com/npryce/adr-tools) pointed at `/adr/`
and the MADR template, or copy the structure of the most recent file by hand.

## Development

- Install stable Rust via [rustup](https://rustup.rs).
- Install [mdbook-lint](https://github.com/joshrotenberg/mdbook-lint): `cargo install mdbook-lint`
- Build: `cargo build`
- Test: `cargo test`
- Format: `cargo fmt`
- Lint: `cargo clippy -- -D warnings`
- Lint ADRs: `mdbook-lint lint adr/*.md`

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[MIT](LICENSE)
