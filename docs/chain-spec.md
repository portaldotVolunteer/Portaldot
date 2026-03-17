# Chain Specification Guide

## What is a Chain Spec?

A chain specification defines the genesis state of a Portaldot network — the initial accounts, balances, validators, and configuration that every node must agree on to participate.

## Chain Types

| Type | Flag | Use Case |
|------|------|----------|
| Development | `--dev` | Single node, instant-seal, Alice funded |
| Local Testnet | `--chain local` | Two+ nodes on same machine |
| Custom | `--chain spec.json` | Production or public testnet |

## Development Mode (Quickstart)
```bash
./portaldot_dev --dev --rpc-external --ws-external --rpc-cors all
```

Pre-funded accounts in dev mode:
- **Alice** — `5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY`
- **Bob** — `5FHneW46xGXgs5mUiveU4sbTyGBzmstUspZC92UhjJM694ty`
- **Charlie** — `5FLSigC9HGRKVhB9FiEo4Y3koPsNmBmLJbpXg2mp1hXcS59Y`
- **Dave, Eve, Ferdie** — also pre-funded

## Generating a Custom Chain Spec

### Step 1 — Export the base spec
```bash
./portaldot_dev build-spec --disable-default-bootnode > spec.json
```

### Step 2 — Edit spec.json
Key fields to customize:
```json
{
  "name": "My Portaldot Network",
  "id": "my_network",
  "chainType": "Live",
  "bootNodes": [
    "/ip4/1.2.3.4/tcp/30333/p2p/12D3KooW..."
  ],
  "genesis": {
    "runtime": {
      "balances": {
        "balances": [
          ["5GrwvaEF...", 1000000000000000]
        ]
      },
      "sudo": {
        "key": "5GrwvaEF..."
      }
    }
  }
}
```

### Step 3 — Convert to raw format
```bash
./portaldot_dev build-spec --chain spec.json --raw > spec-raw.json
```

### Step 4 — Start nodes with custom spec
```bash
./portaldot_dev --chain spec-raw.json --validator --name MyNode
```

## SS58 Address Format

Portaldot uses Substrate SS58 encoding for addresses.
```javascript
const { encodeAddress, decodeAddress } = require('@polkadot/util-crypto')

// Encode to SS58
const address = encodeAddress(publicKey, 42) // 42 = default substrate prefix

// Decode from SS58
const publicKey = decodeAddress(address)
```

## Genesis Accounts

In development mode, accounts are derived from well-known seeds:
```bash
# Generate Alice's address
subkey inspect //Alice

# Generate Bob's address  
subkey inspect //Bob

# Generate a new random account
subkey generate
```

## Network Configuration

| Parameter | Default | Description |
|-----------|---------|-------------|
| P2P port | 30333 | Peer-to-peer networking |
| RPC port | 9933 | HTTP RPC |
| WS port | 9944 | WebSocket RPC |
| Prometheus | 9615 | Metrics endpoint |
| Block time | 6s | Target block production time |

## Connecting to the Network

### Via Polkadot.js Apps
```
https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:9944
```

### Via Developer Playground
```
https://portaldot-playground.vercel.app
```

### Via JavaScript SDK
```javascript
const { ApiPromise, WsProvider } = require('@polkadot/api')
const api = await ApiPromise.create({
  provider: new WsProvider('ws://127.0.0.1:9944')
})
```
