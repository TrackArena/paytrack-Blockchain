# Contributing to paytrack-blockchain

Thank you for your interest in contributing to PayTrack's smart contract layer.

## Prerequisites

Before contributing here, please read the main [paytrack-backend CONTRIBUTING.md](https://github.com/PayTrack/paytrack-backend/blob/main/CONTRIBUTING.md) for the overall project workflow.

Smart contract contributions additionally require:
- Solid Rust experience
- Familiarity with the Soroban SDK
- Understanding of the PayTrack payment reconciliation flow

## Getting Started

1. Fork this repository
2. Clone your fork: `git clone https://github.com/YOUR_USERNAME/paytrack-blockchain`
3. Read the README thoroughly — especially the "When contracts will be developed" section
4. Open an issue describing the contract feature you want to build before writing code

## Branching

- `main` — stable, reviewed contracts only  
- `feat/contract-name` — new contract development  
- `fix/issue-description` — bug fixes  

## Contract Standards

All contracts must:
- Include comprehensive unit tests using `soroban-sdk` test utilities
- Document every public function with Rust doc comments
- Handle all error cases explicitly — never panic in contract code
- Be tested on Stellar testnet before any PR is opened
- Include gas/fee estimates in the PR description

## Testing

```bash
# Run all tests
cargo test

# Run a specific contract's tests
cargo test -p contract-name

# Run with testutils enabled
cargo test -- --features testutils
```

## Pull Requests

- Keep PRs focused on a single contract or feature
- Include testnet transaction IDs demonstrating the contract works
- Update README with any new contract interfaces
- Request review from at least one maintainer before merging

## Security

Report vulnerabilities privately via [SECURITY.md](SECURITY.md). Never open a public issue for security bugs in contract code.
