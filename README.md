# paytrack-blockchain

Soroban smart contract repository for the PayTrack payment reconciliation platform.

## Status

This repository is initialised and ready for Phase 7 development.

The PayTrack MVP uses the Stellar SDK directly in `paytrack-backend` for transaction verification, payment detection, and USDC asset validation. This repository exists for on-chain programmable logic that genuinely requires smart contracts.

## Proposed Contract Architecture

### When contracts will be developed

Soroban contracts are planned for these specific use cases:

| Use case | Justification |
|---|---|
| Programmable escrow | Funds held on-chain pending milestone completion |
| Milestone-based payments | Conditional fund release tied to verifiable events |
| Automated revenue distribution | Split payments to multiple recipients on-chain |

These will **not** be implemented until the off-chain reconciliation engine is production-stable, the use case is confirmed with real users, and the contract logic cannot be replicated off-chain securely.

### What will NOT be implemented as contracts

- Invoice management (database concern)
- Authentication (backend concern)
- Payment matching (reconciliation engine concern)
- Reporting and analytics (database concern)

## Technology

- **Language**: Rust
- **Framework**: Soroban SDK
- **Network**: Stellar testnet → mainnet
- **Toolchain**: Stellar CLI + Cargo

## Repository Structure (planned)

```
paytrack-blockchain/
├── contracts/
│   ├── escrow/              # Escrow contract (Phase 7)
│   │   ├── src/
│   │   │   └── lib.rs
│   │   └── Cargo.toml
│   └── milestone-release/   # Milestone payment release (Phase 7)
│       ├── src/
│       │   └── lib.rs
│       └── Cargo.toml
├── tests/
│   └── integration/         # Testnet integration tests
├── scripts/
│   ├── deploy-testnet.sh    # Testnet deployment
│   └── deploy-mainnet.sh    # Mainnet deployment (requires multisig approval)
├── Cargo.toml
└── README.md
```

## Prerequisites (when development begins)

- Rust (latest stable)
- Stellar CLI: `cargo install stellar-cli --features opt`
- Soroban SDK: added as Cargo dependency
- Stellar testnet account with XLM for deployment

## Getting Started (Phase 7)

```bash
# Install Rust
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Install Stellar CLI
cargo install stellar-cli --features opt

# Configure testnet
stellar network add testnet \
  --rpc-url https://soroban-testnet.stellar.org \
  --network-passphrase "Test SDF Network ; September 2015"

# Generate a test keypair
stellar keys generate --global alice --network testnet

# Build contracts (once implemented)
cargo build --release --target wasm32-unknown-unknown

# Run tests
cargo test
```

## Contract Interface (proposed — escrow)

```rust
// Proposed interface for the PayTrack escrow contract
pub trait PayTrackEscrow {
    // Deposit funds into escrow
    fn deposit(env: Env, invoice_id: Bytes, amount: i128, asset: Address) -> Result<(), Error>;
    
    // Release funds when invoice is verified paid
    fn release(env: Env, invoice_id: Bytes, recipient: Address) -> Result<(), Error>;
    
    // Refund depositor after timeout
    fn refund(env: Env, invoice_id: Bytes) -> Result<(), Error>;
    
    // Check escrow status
    fn status(env: Env, invoice_id: Bytes) -> EscrowStatus;
}
```

## Integration with paytrack-backend

When contracts are deployed, `paytrack-backend` will interact with them via:

1. Stellar SDK `contract.invoke()` calls
2. Contract events streamed via Horizon
3. Transaction simulation before submission

The backend remains the authoritative source of truth — contract state supplements, not replaces, the PostgreSQL database.

## Security Notes

- All contracts will undergo internal review before testnet deployment
- Mainnet deployment requires explicit team sign-off and audit
- Contract addresses are stored in `paytrack-backend` environment variables
- Never hardcode contract addresses in source code

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Contract contributions require:
1. Rust experience with Soroban SDK
2. Understanding of the PayTrack payment flow
3. Passing unit and integration tests
4. Documentation of all public interfaces

## Architecture

The full system architecture is documented in the backend repository:

→ [paytrack-backend/ARCHITECTURE.md](https://github.com/PayTrack/paytrack-backend/blob/main/ARCHITECTURE.md)

## Related Repositories

- [paytrack-backend](https://github.com/PayTrack/paytrack-backend) — API server, Stellar SDK integration
- [paytrack-frontend](https://github.com/PayTrack/paytrack-frontend) — React dashboard
