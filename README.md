# Portaldot

Portaldot is a Substrate-based blockchain project focused on building a Layer0 protocol and public chain infrastructure for Web3 applications.

## Repository Status

This repository currently contains the chain node, runtime, and a vendored Substrate codebase snapshot used by Portaldot.

Known maintenance gaps:

- The codebase is based on an older Substrate generation and will require a broader SDK migration.
- The checked-in chain configuration includes both development/staging defaults and a raw mainnet chain spec.
- Modern CI, security policy, and pinned toolchain metadata are being added incrementally.

## Links

- Homepage: <https://www.portaldot.io>
- Developer docs: <https://portaldot-dev.readthedocs.io>

## Quick Start

Clone the repository and build the node binary:

```bash
git clone https://github.com/portaldotVolunteer/Portaldot.git
cd Portaldot
cargo build --release -p node-cli
```

Run a local development chain:

```bash
./target/release/portaldot --dev
```

Use the checked-in raw chain spec when you need to inspect or boot the main network configuration:

```bash
./target/release/portaldot --chain ./pot_mainnet_spec_raw.json
```

## Chain Configuration Notes

- `--chain portaldot` currently resolves to the built-in Portaldot chain spec compiled into the node.
- `pot_mainnet_spec_raw.json` contains a separate raw mainnet chain specification.
- Review both before using this repository for production node deployment.

## Development Notes

- The project currently targets an older Rust/Substrate stack. A pinned toolchain file is included to make local builds more reproducible.
- Some crates still carry upstream Substrate metadata. Cleanup is ongoing.
- If you are making runtime or chain spec changes, validate them against your deployment environment before release.

## Contributing

Contributions are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for development and review expectations.

## Security

Please report security issues privately as described in [SECURITY.md](SECURITY.md).

## License

Portaldot is released under the terms of the Apache 2.0 license.

See [LICENSE-APACHE2](LICENSE-APACHE2) for details.
