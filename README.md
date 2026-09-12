# marksentinel

[![CI](https://github.com/ebh/marksentinel/actions/workflows/ci.yml/badge.svg)](https://github.com/ebh/marksentinel/actions/workflows/ci.yml)

A mechanical conformance checker for Markdown documentation. It enforces the
[OKF (Open Knowledge Format)](https://okf.md/) documentation profile against any `docs/`
directory in a repo: YAML frontmatter shape, cross-document link integrity, heading-anchor
correctness, tag-vocabulary closure, index completeness, and log ordering.

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
