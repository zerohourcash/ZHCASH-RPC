# Raw Transactions RPC

Raw transaction RPC methods are used by wallets, exchanges, indexers, and
advanced tooling to create, decode, sign, test, and broadcast transactions.

## Menu

* [Raw Transaction Creation](#raw-transaction-creation)
* [Raw Transaction Decode / Broadcast](#raw-transaction-decode--broadcast)
* [Transaction Signing](#transaction-signing)
* [PSBT](#psbt)
* [Address Encoding Helpers](#address-encoding-helpers)
* [Full Method Index](#full-method-index)

## Raw Transaction Creation

| Method | Safety | Signature |
| --- | --- | --- |
| `createrawtransaction` | Read-only | `createrawtransaction [{"txid":"hex","vout":n,"sequence":n},...] [{"address":amount},{"data":"hex"},{"contractAddress":"hex","data":"hex","amount":amount,"gasLimit":n,"gasPrice":n},...] ( locktime replaceable )` |
| `fundrawtransaction` | Wallet | `fundrawtransaction "hexstring" ( options iswitness )` |

Checked read-only example:

```bash
RAW="$(zerohour-cli createrawtransaction '[]' \
  '{"ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr":0.001}')"
```

Checked raw transaction hex:

```text
020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000
```

## Raw Transaction Decode / Broadcast

| Method | Safety | Signature |
| --- | --- | --- |
| `decoderawtransaction` | Read-only | `decoderawtransaction "hexstring" ( iswitness )` |
| `decodescript` | Read-only | `decodescript "hexstring"` |
| `getrawtransaction` | Read-only | `getrawtransaction "txid" ( verbose "blockhash" )` |
| `sendrawtransaction` | Broadcast | `sendrawtransaction "hexstring" ( allowhighfees )` |
| `testmempoolaccept` | Read-only | `testmempoolaccept ["rawtx",...] ( allowhighfees )` |

Examples:

```bash
zerohour-cli decoderawtransaction "$RAW"
zerohour-cli testmempoolaccept "[\"$RAW\"]"
```

`sendrawtransaction` broadcasts to the network. Use it only after validating
the transaction.

## Transaction Signing

The active signing RPCs are `signrawtransactionwithwallet` and
`signrawtransactionwithkey`.

| Method | Safety | Signature |
| --- | --- | --- |
| `signrawtransactionwithkey` | Sensitive | `signrawtransactionwithkey "hexstring" ["privatekey",...] ( [{"txid":"hex","vout":n,"scriptPubKey":"hex","redeemScript":"hex","witnessScript":"hex","amount":amount},...] "sighashtype" )` |
| `signrawtransactionwithwallet` | Wallet | `signrawtransactionwithwallet "hexstring" ( [{"txid":"hex","vout":n,"scriptPubKey":"hex","redeemScript":"hex","witnessScript":"hex","amount":amount},...] "sighashtype" )` |

Checked example:

```bash
zerohour-cli signrawtransactionwithwallet "$RAW"
zerohour-cli signrawtransactionwithkey "$RAW" '[]'
```

JSON-RPC form:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"signrawtransactionwithwallet","params":["020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Do not log or expose private keys used with `signrawtransactionwithkey`.

## PSBT

PSBT methods are useful for multi-step and offline signing flows.

| Method | Safety | Signature |
| --- | --- | --- |
| `analyzepsbt` | Read-only | `analyzepsbt "psbt"` |
| `combinepsbt` | Read-only | `combinepsbt ["psbt",...]` |
| `combinerawtransaction` | Read-only | `combinerawtransaction ["hexstring",...]` |
| `converttopsbt` | Read-only | `converttopsbt "hexstring" ( permitsigdata iswitness )` |
| `createpsbt` | Read-only | `createpsbt [{"txid":"hex","vout":n,"sequence":n},...] [{"address":amount},{"data":"hex"},...] ( locktime replaceable )` |
| `decodepsbt` | Read-only | `decodepsbt "psbt"` |
| `finalizepsbt` | Read-only | `finalizepsbt "psbt" ( extract )` |
| `joinpsbts` | Read-only | `joinpsbts ["psbt",...]` |
| `utxoupdatepsbt` | Read-only | `utxoupdatepsbt "psbt"` |

Example:

```bash
zerohour-cli createpsbt '[]' '{}'
```

## Address Encoding Helpers

| Method | Safety | Signature |
| --- | --- | --- |
| `fromhexaddress` | Read-only | `fromhexaddress "hexaddress"` |
| `gethexaddress` | Read-only | `gethexaddress "address"` |

Checked conversion:

```bash
zerohour-cli gethexaddress ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr
zerohour-cli fromhexaddress 25ceedce21c41b53b8266d6f224ed7acfa046d96
```

Result:

```text
25ceedce21c41b53b8266d6f224ed7acfa046d96
ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr
```

## Full Method Index

| Method | Safety | Signature |
| --- | --- | --- |
| `analyzepsbt` | Read-only | `analyzepsbt "psbt"` |
| `combinepsbt` | Read-only | `combinepsbt ["psbt",...]` |
| `combinerawtransaction` | Read-only | `combinerawtransaction ["hexstring",...]` |
| `converttopsbt` | Read-only | `converttopsbt "hexstring" ( permitsigdata iswitness )` |
| `createpsbt` | Read-only | `createpsbt [{"txid":"hex","vout":n,"sequence":n},...] [{"address":amount},{"data":"hex"},...] ( locktime replaceable )` |
| `createrawtransaction` | Read-only | `createrawtransaction [{"txid":"hex","vout":n,"sequence":n},...] [{"address":amount},{"data":"hex"},{"contractAddress":"hex","data":"hex","amount":amount,"gasLimit":n,"gasPrice":n},...] ( locktime replaceable )` |
| `decodepsbt` | Read-only | `decodepsbt "psbt"` |
| `decoderawtransaction` | Read-only | `decoderawtransaction "hexstring" ( iswitness )` |
| `decodescript` | Read-only | `decodescript "hexstring"` |
| `finalizepsbt` | Read-only | `finalizepsbt "psbt" ( extract )` |
| `fromhexaddress` | Read-only | `fromhexaddress "hexaddress"` |
| `fundrawtransaction` | Wallet | `fundrawtransaction "hexstring" ( options iswitness )` |
| `gethexaddress` | Read-only | `gethexaddress "address"` |
| `getrawtransaction` | Read-only | `getrawtransaction "txid" ( verbose "blockhash" )` |
| `joinpsbts` | Read-only | `joinpsbts ["psbt",...]` |
| `sendrawtransaction` | Broadcast | `sendrawtransaction "hexstring" ( allowhighfees )` |
| `signrawtransactionwithkey` | Sensitive | `signrawtransactionwithkey "hexstring" ["privatekey",...] ( [{"txid":"hex","vout":n,"scriptPubKey":"hex","redeemScript":"hex","witnessScript":"hex","amount":amount},...] "sighashtype" )` |
| `signrawtransactionwithwallet` | Wallet | `signrawtransactionwithwallet "hexstring" ( [{"txid":"hex","vout":n,"scriptPubKey":"hex","redeemScript":"hex","witnessScript":"hex","amount":amount},...] "sighashtype" )` |
| `testmempoolaccept` | Read-only | `testmempoolaccept ["rawtx",...] ( allowhighfees )` |
| `utxoupdatepsbt` | Read-only | `utxoupdatepsbt "psbt"` |
