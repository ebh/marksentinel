# marksentinel

[![CI](https://github.com/ebh/marksentinel/actions/workflows/ci.yml/badge.svg)](https://github.com/ebh/marksentinel/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

A mechanical conformance checker for Markdown documentation. It enforces the
[OKF (Open Knowledge Format)](https://okf.md/) documentation profile against any `docs/`
directory in a repo: YAML frontmatter shape, cross-document link integrity, heading-anchor
correctness, tag-vocabulary closure, index completeness, and log ordering.

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
- Build: `cargo build`
- Test: `cargo test`
- Format: `cargo fmt`
- Lint: `cargo clippy -- -D warnings`

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[MIT](LICENSE)
