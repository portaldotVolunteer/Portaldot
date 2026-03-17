# Portaldot Architecture

## Overview

Portaldot is a Layer-0 meta-protocol built on the Substrate framework. It enables secure cross-chain data transmission between any blockchain protocol, targeting physical industries and Web3 infrastructure.

## Core Components

### 1. Consensus — LAO NPoS
Portaldot uses a Nominated Proof-of-Stake (NPoS) consensus mechanism called LAO NPoS.

- **Validators** produce and finalize blocks
- **Nominators** back validators with POT stake
- **Block time:** ~6 seconds
- **Finality:** GRANDPA (deterministic finality)
- **Block production:** BABE (blind assignment for blockchain extension)

### 2. Runtime
The runtime is the state transition function — it defines all on-chain logic.
```
bin/node-template/runtime/src/lib.rs  ← main runtime
frame/                                 ← all pallets
```

Key pallets included:
- `frame/balances` — POT token transfers
- `frame/staking` — NPoS validator/nominator logic
- `frame/session` — validator session management
- `frame/grandpa` — finality gadget
- `frame/babe` — block production

### 3. Networking — libp2p
Portaldot uses libp2p for peer-to-peer networking.

- **P2P port:** 30333 (default)
- **Discovery:** mDNS + Kademlia DHT
- **Protocol:** `/sup/1` (default)

### 4. RPC Layer
Two RPC interfaces are exposed:

| Interface | Port | Protocol |
|-----------|------|----------|
| HTTP RPC | 9933 | HTTP POST |
| WebSocket RPC | 9944 | WebSocket |
| Prometheus metrics | 9615 | HTTP GET |

### 5. POT Token
- **Decimals:** 12
- **SS58 prefix:** Substrate default (42 for dev)
- **Total issuance:** defined in genesis

## Node Architecture
```
portaldot_dev
├── CLI (bin/node/cli)
│   ├── chain_spec.rs     ← genesis configuration
│   ├── service.rs        ← node services setup
│   └── rpc.rs            ← RPC extensions
├── Runtime (bin/node-template/runtime)
│   └── lib.rs            ← all pallets wired together
└── Pallets (frame/)
    └── [50+ pallets]
```

## Cross-Chain Architecture (Planned)

Portaldot's vision is to enable cross-chain data transfer. The planned architecture includes:

- **Relay layer** — coordinates between chains
- **Bridge pallets** — connect to external networks
- **Message passing** — XCM-compatible messaging

## Development vs Production

| Setting | Development | Production |
|---------|-------------|------------|
| Flag | `--dev` | custom chain spec |
| Accounts | Alice, Bob pre-funded | real validator keys |
| Consensus | instant-seal or BABE | BABE + GRANDPA |
| POT supply | test tokens | real POT |
