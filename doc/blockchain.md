# Blockchain RPC

Blockchain RPC methods read chain state, blocks, mempool entries, UTXO data,
contract state, and validation information.

## Menu

* [Blockchain Information](#blockchain-information)
* [Blocks](#blocks)
* [Mempool](#mempool)
* [UTXO / Proofs](#utxo--proofs)
* [Smart Contract Read RPC](#smart-contract-read-rpc)
* [Maintenance / Validation](#maintenance--validation)
* [Full Method Index](#full-method-index)

## Blockchain Information

Use these methods to inspect chain height, best block, difficulty, chain tips,
and chain transaction statistics.

| Method | Safety | Signature |
| --- | --- | --- |
| `getbestblockhash` | Read-only | `getbestblockhash` |
| `getblockchaininfo` | Read-only | `getblockchaininfo` |
| `getblockcount` | Read-only | `getblockcount` |
| `getchaintips` | Read-only | `getchaintips` |
| `getchaintxstats` | Read-only | `getchaintxstats ( nblocks "blockhash" )` |
| `getdifficulty` | Read-only | `getdifficulty` |

Example:

```bash
zerohour-cli getblockcount
zerohour-cli getbestblockhash
zerohour-cli getblockchaininfo
```

JSON-RPC:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getblockchaininfo","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

## Blocks

Use these methods to fetch block hashes, block headers, full block data, and
per-block statistics.

| Method | Safety | Signature |
| --- | --- | --- |
| `getblock` | Read-only | `getblock "blockhash" ( verbosity )` |
| `getblockhash` | Read-only | `getblockhash height` |
| `getblockheader` | Read-only | `getblockheader "blockhash" ( verbose )` |
| `getblockstats` | Read-only | `getblockstats hash_or_height ( stats )` |

Example:

```bash
HASH="$(zerohour-cli getblockhash 1)"
zerohour-cli getblock "$HASH" 1
zerohour-cli getblockheader "$HASH" true
```

Known block 1 hash:

```text
0000751feb032e4d1993d7852130092a6f95d529c44e425b06add2c79aa4a6c7
```

## Mempool

Use these methods to inspect unconfirmed transactions.

| Method | Safety | Signature |
| --- | --- | --- |
| `getmempoolancestors` | Read-only | `getmempoolancestors "txid" ( verbose )` |
| `getmempooldescendants` | Read-only | `getmempooldescendants "txid" ( verbose )` |
| `getmempoolentry` | Read-only | `getmempoolentry "txid"` |
| `getmempoolinfo` | Read-only | `getmempoolinfo` |
| `getrawmempool` | Read-only | `getrawmempool ( verbose )` |

Example:

```bash
zerohour-cli getmempoolinfo
zerohour-cli getrawmempool false
```

## UTXO / Proofs

Use these methods to inspect unspent outputs and build or verify UTXO proofs.

| Method | Safety | Signature |
| --- | --- | --- |
| `gettxout` | Read-only | `gettxout "txid" n ( include_mempool )` |
| `gettxoutproof` | Read-only | `gettxoutproof ["txid",...] ( "blockhash" )` |
| `gettxoutsetinfo` | Read-only | `gettxoutsetinfo` |
| `scantxoutset` | Advanced | `scantxoutset "action" [scanobjects,...]` |
| `verifytxoutproof` | Read-only | `verifytxoutproof "proof"` |

Example:

```bash
zerohour-cli gettxoutsetinfo
```

## Smart Contract Read RPC

These methods are used by explorers, wallets, and dApps to inspect contract
state without broadcasting a transaction.

| Method | Safety | Signature |
| --- | --- | --- |
| `callcontract` | Read-only | `callcontract "address" "data" ( "senderAddress" gasLimit )` |
| `getaccountinfo` | Read-only | `getaccountinfo "address"` |
| `getcontractcode` | Read-only | `getcontractcode "address"` |
| `getstorage` | Read-only | `getstorage "address" ( blockNum index )` |
| `gettransactionreceipt` | Read-only | `gettransactionreceipt "hash"` |
| `listcontracts` | Read-only | `listcontracts ( start maxDisplay )` |
| `searchlogs` | Read-only | `searchlogs fromBlock toBlock ( "address" "topics" minconf )` |
| `waitforlogs` | Advanced | `waitforlogs ( fromBlock toBlock "filter" minconf )` |

Read-only contract call:

```bash
zerohour-cli callcontract "contract_address_or_hex" "data_hex"
```

Correct JSON-RPC form:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"callcontract","params":["eb23c0b3e6042821da281a2e2364feb22dd543e3","06fdde03"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Log search by block range:

```bash
zerohour-cli searchlogs 1700000 1700100
```

## Maintenance / Validation

These methods affect validation priority, pruning, or mempool persistence.

| Method | Safety | Signature |
| --- | --- | --- |
| `preciousblock` | Advanced | `preciousblock "blockhash"` |
| `pruneblockchain` | Node-control | `pruneblockchain height` |
| `savemempool` | Node-control | `savemempool` |
| `verifychain` | Read-only | `verifychain ( checklevel nblocks )` |

Example:

```bash
zerohour-cli verifychain
```

## Full Method Index

| Method | Safety | Signature |
| --- | --- | --- |
| `callcontract` | Read-only | `callcontract "address" "data" ( "senderAddress" gasLimit )` |
| `getaccountinfo` | Read-only | `getaccountinfo "address"` |
| `getbestblockhash` | Read-only | `getbestblockhash` |
| `getblock` | Read-only | `getblock "blockhash" ( verbosity )` |
| `getblockchaininfo` | Read-only | `getblockchaininfo` |
| `getblockcount` | Read-only | `getblockcount` |
| `getblockhash` | Read-only | `getblockhash height` |
| `getblockheader` | Read-only | `getblockheader "blockhash" ( verbose )` |
| `getblockstats` | Read-only | `getblockstats hash_or_height ( stats )` |
| `getchaintips` | Read-only | `getchaintips` |
| `getchaintxstats` | Read-only | `getchaintxstats ( nblocks "blockhash" )` |
| `getcontractcode` | Read-only | `getcontractcode "address"` |
| `getdifficulty` | Read-only | `getdifficulty` |
| `getmempoolancestors` | Read-only | `getmempoolancestors "txid" ( verbose )` |
| `getmempooldescendants` | Read-only | `getmempooldescendants "txid" ( verbose )` |
| `getmempoolentry` | Read-only | `getmempoolentry "txid"` |
| `getmempoolinfo` | Read-only | `getmempoolinfo` |
| `getrawmempool` | Read-only | `getrawmempool ( verbose )` |
| `getstorage` | Read-only | `getstorage "address" ( blockNum index )` |
| `gettransactionreceipt` | Read-only | `gettransactionreceipt "hash"` |
| `gettxout` | Read-only | `gettxout "txid" n ( include_mempool )` |
| `gettxoutproof` | Read-only | `gettxoutproof ["txid",...] ( "blockhash" )` |
| `gettxoutsetinfo` | Read-only | `gettxoutsetinfo` |
| `listcontracts` | Read-only | `listcontracts ( start maxDisplay )` |
| `preciousblock` | Advanced | `preciousblock "blockhash"` |
| `pruneblockchain` | Node-control | `pruneblockchain height` |
| `savemempool` | Node-control | `savemempool` |
| `scantxoutset` | Advanced | `scantxoutset "action" [scanobjects,...]` |
| `searchlogs` | Read-only | `searchlogs fromBlock toBlock ( "address" "topics" minconf )` |
| `verifychain` | Read-only | `verifychain ( checklevel nblocks )` |
| `verifytxoutproof` | Read-only | `verifytxoutproof "proof"` |
| `waitforlogs` | Advanced | `waitforlogs ( fromBlock toBlock "filter" minconf )` |
