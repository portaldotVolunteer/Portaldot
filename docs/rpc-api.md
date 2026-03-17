# Portaldot RPC API Reference

## Connection

| Endpoint | URL |
|----------|-----|
| Local HTTP | `http://127.0.0.1:9933` |
| Local WebSocket | `ws://127.0.0.1:9944` |

All RPC calls use JSON-RPC 2.0 format:
```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "method_name",
  "params": []
}
```

## System Methods

### system_chain
Returns the chain name.
```bash
curl -H "Content-Type: application/json" \
  -d '{"id":1,"jsonrpc":"2.0","method":"system_chain","params":[]}' \
  http://127.0.0.1:9933
# Response: {"jsonrpc":"2.0","result":"Development","id":1}
```

### system_name
Returns the node implementation name.
```bash
curl -d '{"id":1,"jsonrpc":"2.0","method":"system_name","params":[]}' \
  -H "Content-Type: application/json" http://127.0.0.1:9933
# Response: {"jsonrpc":"2.0","result":"Portaldot Node","id":1}
```

### system_version
Returns the node version.
```bash
curl -d '{"id":1,"jsonrpc":"2.0","method":"system_version","params":[]}' \
  -H "Content-Type: application/json" http://127.0.0.1:9933
```

### system_localPeerId
Returns this node's peer ID.
```bash
curl -d '{"id":1,"jsonrpc":"2.0","method":"system_localPeerId","params":[]}' \
  -H "Content-Type: application/json" http://127.0.0.1:9933
# Response: {"jsonrpc":"2.0","result":"12D3KooW...","id":1}
```

### system_peers
Returns connected peers.
```bash
curl -d '{"id":1,"jsonrpc":"2.0","method":"system_peers","params":[]}' \
  -H "Content-Type: application/json" http://127.0.0.1:9933
```

### system_health
Returns node health status.
```bash
curl -d '{"id":1,"jsonrpc":"2.0","method":"system_health","params":[]}' \
  -H "Content-Type: application/json" http://127.0.0.1:9933
# Response: {"jsonrpc":"2.0","result":{"peers":1,"isSyncing":false,"shouldHavePeers":true},"id":1}
```

## Chain Methods

### chain_getBlockHash
Get block hash by block number.
```bash
curl -d '{"id":1,"jsonrpc":"2.0","method":"chain_getBlockHash","params":[0]}' \
  -H "Content-Type: application/json" http://127.0.0.1:9933
```

### chain_getBlock
Get a block by hash.
```bash
curl -d '{"id":1,"jsonrpc":"2.0","method":"chain_getBlock","params":["0x..."]}' \
  -H "Content-Type: application/json" http://127.0.0.1:9933
```

### chain_getFinalizedHead
Get the latest finalized block hash.
```bash
curl -d '{"id":1,"jsonrpc":"2.0","method":"chain_getFinalizedHead","params":[]}' \
  -H "Content-Type: application/json" http://127.0.0.1:9933
```

## State Methods

### state_getStorage
Get raw storage value at a key.
```bash
curl -d '{"id":1,"jsonrpc":"2.0","method":"state_getStorage","params":["0x..."]}' \
  -H "Content-Type: application/json" http://127.0.0.1:9933
```

### state_call
Call a runtime API method.
```bash
curl -d '{"id":1,"jsonrpc":"2.0","method":"state_call","params":["AccountNonceApi_account_nonce","0x...",null]}' \
  -H "Content-Type: application/json" http://127.0.0.1:9933
```

## Author Methods

### author_submitExtrinsic
Submit a signed transaction.
```bash
curl -d '{"id":1,"jsonrpc":"2.0","method":"author_submitExtrinsic","params":["0x..."]}' \
  -H "Content-Type: application/json" http://127.0.0.1:9933
```

## WebSocket Subscriptions

Connect via WebSocket (`ws://127.0.0.1:9944`) for real-time subscriptions:

### chain_subscribeNewHeads
Subscribe to new block headers.
```javascript
const { ApiPromise, WsProvider } = require('@polkadot/api')
const api = await ApiPromise.create({ provider: new WsProvider('ws://127.0.0.1:9944') })
await api.rpc.chain.subscribeNewHeads((header) => {
  console.log(`Block #${header.number}: ${header.hash}`)
})
```

### state_subscribeStorage
Subscribe to storage changes.
```javascript
await api.rpc.state.subscribeStorage([key], (changes) => {
  console.log('Storage changed:', changes)
})
```

## JavaScript SDK Examples
```javascript
const { ApiPromise, WsProvider } = require('@polkadot/api')

async function main() {
  const api = await ApiPromise.create({
    provider: new WsProvider('ws://127.0.0.1:9944')
  })

  // Get chain info
  const chain = await api.rpc.system.chain()
  const version = await api.rpc.system.version()
  console.log(`Connected to ${chain} (${version})`)

  // Get account balance
  const { data: { free } } = await api.query.system.account('5GrwvaEF...')
  console.log(`Balance: ${free}`)

  // Get latest block
  const header = await api.rpc.chain.getHeader()
  console.log(`Latest block: #${header.number}`)
}

main()
```

## Error Codes

| Code | Meaning |
|------|---------|
| -32700 | Parse error |
| -32600 | Invalid request |
| -32601 | Method not found |
| -32602 | Invalid params |
| -32603 | Internal error |
