# Portaldot Runtime Pallets

## Overview

Portaldot's runtime is composed of Substrate pallets. Each pallet handles a specific area of on-chain functionality.

## Core Pallets

### frame/system
The base layer for all other pallets. Handles accounts, block execution, and events.

**Key storage:**
- `Account` — account info including nonce and balance references
- `BlockHash` — mapping of block number to hash
- `Number` — current block number

### frame/balances
Handles POT token balances and transfers.

**Key extrinsics:**
- `transfer(dest, value)` — transfer POT to another account
- `transfer_keep_alive(dest, value)` — transfer, keeping sender alive
- `force_transfer(source, dest, value)` — sudo-only forced transfer

**Key storage:**
- `TotalIssuance` — total POT supply
- `Account` — per-account balance data (free, reserved, frozen)

### frame/staking
Implements NPoS validator/nominator staking.

**Key extrinsics:**
- `bond(controller, value, payee)` — bond POT as stake
- `nominate(targets)` — nominate validators
- `validate(prefs)` — declare intent to validate
- `unbond(value)` — schedule unbonding
- `withdraw_unbonded()` — withdraw after unbonding period

### frame/session
Manages validator sessions and key rotation.

**Key concepts:**
- Sessions rotate every N blocks
- Validators must have session keys registered
- Keys: BABE + GRANDPA + ImOnline + AuthorityDiscovery

### frame/babe
Block production using BABE (Blind Assignment for Blockchain Extension).

- Slot-based block production
- VRF-based randomness
- Epoch duration configurable in chain spec

### frame/grandpa
Deterministic finality using GRANDPA.

- Votes on chains, not blocks
- Requires 2/3+ of validators to finalize
- Byzantine fault tolerant

### frame/sudo
Privileged access for development. **Should be removed in production.**

**Key extrinsics:**
- `sudo(call)` — execute call as root
- `sudo_unchecked_weight(call, weight)` — bypass weight checks

### frame/treasury
On-chain treasury funded by transaction fees and slashing.

### frame/democracy
On-chain governance for protocol upgrades and parameter changes.

### frame/collective
Multi-signature governance council.

### frame/assets
Multi-asset support for creating and managing tokens beyond POT.

### frame/contracts
Smart contract support (ink! contracts).

## Pallet Interactions
```
User Transaction
      ↓
frame/executive  ← dispatches calls
      ↓
frame/system     ← validates, charges fees
      ↓
target pallet    ← executes logic
      ↓
frame/events     ← emits events
```

## Adding a Custom Pallet

See [CONTRIBUTING.md](../CONTRIBUTING.md) for the full guide. Quick steps:

1. Copy `bin/node-template/pallets/template` to `frame/your-pallet`
2. Define storage, events, errors, and extrinsics in `src/lib.rs`
3. Add to `bin/node-template/runtime/Cargo.toml`
4. Register in `bin/node-template/runtime/src/lib.rs`
5. Write tests in `src/tests.rs`
6. Add benchmarks in `src/benchmarking.rs`
