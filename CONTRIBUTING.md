# Contributing to Portaldot

Thank you for contributing to Portaldot! Every contribution matters.

## Ways to Contribute
- Bug reports and fixes
- New pallets or features
- Documentation improvements
- Security disclosures (see SECURITY.md)
- Tests and benchmarks

## Getting Started

### Install Rust
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
rustup default stable
rustup update nightly
rustup target add wasm32-unknown-unknown --toolchain nightly
```

### Build
```bash
git clone https://github.com/portaldotVolunteer/Portaldot.git
cd Portaldot
cargo build --release
```

## Submitting a PR
1. Fork the repo
2. Create a branch: `git checkout -b feat/your-feature`
3. Make changes and test: `cargo test && cargo clippy`
4. Commit: `git commit -m "feat: description"`
5. Push and open a Pull Request

## Coding Standards
- Run `cargo fmt` before committing
- Fix all `cargo clippy` warnings
- Add `///` doc comments to public functions
- Include tests for new functionality

## Reporting Bugs
Use the [Portaldot Notion tracker](https://www.notion.so/Portaldot-Local-Development-Node-Installation-Guide-on-Windows-via-WSL2-2e06bdef409680618bf6dbcaa1423508)

## Community
- Website: [portaldot.io](https://www.portaldot.io)
- Developer docs: [portaldot-dev.readthedocs.io](https://portaldot-dev.readthedocs.io)
