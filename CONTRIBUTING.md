# Contributing

## Development Setup

Use the pinned Rust toolchain in `rust-toolchain.toml` unless you are actively working on a migration.

Typical workflow:

```bash
cargo build -p node-cli
cargo test -p node-cli
```

## Scope of Changes

- Keep chain state compatibility in mind before changing the runtime, chain spec, or pallet configuration.
- Separate metadata and documentation cleanup from consensus or storage-impacting changes whenever possible.
- Prefer small pull requests with a clear upgrade or maintenance goal.

## Review Notes

- Document whether a change affects runtime behavior, node behavior, or repository metadata only.
- Include the commands you used for validation in the pull request description.
- If a change updates chain configuration, call that out explicitly.
