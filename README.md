# ZHCASH RPC API Documentation

Current baseline: ZHCASH Core v1.0.0 (`/Evolution:1.0.0/`, protocol `70018`).

This documentation is for developers who need to call a ZHCASH full node through
`zerohour-cli` or JSON-RPC. Method names were checked against the current node
with `zerohour-cli help`.

Original public reference:
https://zh.cash/docs/en/ZHCash-RPC-API/index.html

The public reference is older than the current node. This repository keeps the
corrected v1.0.0 method list and beginner-safe examples.

## Tables of Contents

* [Quick Start](#quick-start)
* [API Endpoint](#api-endpoint)
* [Authentication](#authentication)
* [JSON-RPC Request Format](#json-rpc-request-format)
* [Common RPC Parameters](#common-rpc-parameters)
* [Block / Height Filter Parameters](#block--height-filter-parameters)
* [Safety Labels](#safety-labels)
* [Blockchain](doc/blockchain.md)
  * [callcontract](doc/blockchain.md#smart-contract-read-rpc)
  * [getaccountinfo](doc/blockchain.md#smart-contract-read-rpc)
  * [getbestblockhash](doc/blockchain.md#blockchain-information)
  * [getblock](doc/blockchain.md#blocks)
  * [getblockchaininfo](doc/blockchain.md#blockchain-information)
  * [getblockcount](doc/blockchain.md#blockchain-information)
  * [getblockhash](doc/blockchain.md#blocks)
  * [getblockheader](doc/blockchain.md#blocks)
  * [getblockstats](doc/blockchain.md#blocks)
  * [getchaintips](doc/blockchain.md#blockchain-information)
  * [getchaintxstats](doc/blockchain.md#blockchain-information)
  * [getcontractcode](doc/blockchain.md#smart-contract-read-rpc)
  * [getdifficulty](doc/blockchain.md#blockchain-information)
  * [getmempoolancestors](doc/blockchain.md#mempool)
  * [getmempooldescendants](doc/blockchain.md#mempool)
  * [getmempoolentry](doc/blockchain.md#mempool)
  * [getmempoolinfo](doc/blockchain.md#mempool)
  * [getrawmempool](doc/blockchain.md#mempool)
  * [getstorage](doc/blockchain.md#smart-contract-read-rpc)
  * [gettransactionreceipt](doc/blockchain.md#smart-contract-read-rpc)
  * [gettxout](doc/blockchain.md#utxo--proofs)
  * [gettxoutproof](doc/blockchain.md#utxo--proofs)
  * [gettxoutsetinfo](doc/blockchain.md#utxo--proofs)
  * [listcontracts](doc/blockchain.md#smart-contract-read-rpc)
  * [preciousblock](doc/blockchain.md#maintenance--validation)
  * [pruneblockchain](doc/blockchain.md#maintenance--validation)
  * [savemempool](doc/blockchain.md#maintenance--validation)
  * [scantxoutset](doc/blockchain.md#utxo--proofs)
  * [searchlogs](doc/blockchain.md#smart-contract-read-rpc)
  * [verifychain](doc/blockchain.md#maintenance--validation)
  * [verifytxoutproof](doc/blockchain.md#utxo--proofs)
  * [waitforlogs](doc/blockchain.md#smart-contract-read-rpc)
* [Control](doc/control.md)
  * [getdgpinfo](doc/control.md#dynamic-gas-parameters)
  * [getmemoryinfo](doc/control.md#diagnostics)
  * [getrpcinfo](doc/control.md#diagnostics)
  * [help](doc/control.md#node-control)
  * [logging](doc/control.md#node-control)
  * [stop](doc/control.md#node-control)
  * [uptime](doc/control.md#diagnostics)
* [Generating](doc/generating.md)
  * [generate](doc/generating.md#generating)
  * [generatetoaddress](doc/generating.md#generating)
* [Mining](doc/mining.md)
  * [getblocktemplate](doc/mining.md#mining)
  * [getmininginfo](doc/mining.md#mining)
  * [getnetworkhashps](doc/mining.md#mining)
  * [getstakinginfo](doc/mining.md#staking)
  * [getsubsidy](doc/mining.md#subsidy)
  * [prioritisetransaction](doc/mining.md#mining)
  * [submitblock](doc/mining.md#mining)
  * [submitheader](doc/mining.md#mining)
* [Network](doc/network.md)
  * [addnode](doc/network.md#peer-connections)
  * [clearbanned](doc/network.md#ban-list)
  * [disconnectnode](doc/network.md#peer-connections)
  * [getaddednodeinfo](doc/network.md#peer-connections)
  * [getconnectioncount](doc/network.md#peer-connections)
  * [getnettotals](doc/network.md#network-state)
  * [getnetworkinfo](doc/network.md#network-state)
  * [getnodeaddresses](doc/network.md#peer-connections)
  * [getpeerinfo](doc/network.md#peer-connections)
  * [listbanned](doc/network.md#ban-list)
  * [ping](doc/network.md#peer-connections)
  * [setban](doc/network.md#ban-list)
  * [setnetworkactive](doc/network.md#network-state)
* [Rawtransactions](doc/rawtransactions.md)
  * [analyzepsbt](doc/rawtransactions.md#psbt)
  * [combinepsbt](doc/rawtransactions.md#psbt)
  * [combinerawtransaction](doc/rawtransactions.md#psbt)
  * [converttopsbt](doc/rawtransactions.md#psbt)
  * [createpsbt](doc/rawtransactions.md#psbt)
  * [createrawtransaction](doc/rawtransactions.md#raw-transaction-creation)
  * [decodepsbt](doc/rawtransactions.md#psbt)
  * [decoderawtransaction](doc/rawtransactions.md#raw-transaction-decode--broadcast)
  * [decodescript](doc/rawtransactions.md#raw-transaction-decode--broadcast)
  * [finalizepsbt](doc/rawtransactions.md#psbt)
  * [fromhexaddress](doc/rawtransactions.md#address-encoding-helpers)
  * [fundrawtransaction](doc/rawtransactions.md#raw-transaction-creation)
  * [gethexaddress](doc/rawtransactions.md#address-encoding-helpers)
  * [getrawtransaction](doc/rawtransactions.md#raw-transaction-decode--broadcast)
  * [joinpsbts](doc/rawtransactions.md#psbt)
  * [sendrawtransaction](doc/rawtransactions.md#raw-transaction-decode--broadcast)
  * [signrawtransactionwithkey](doc/rawtransactions.md#transaction-signing)
  * [signrawtransactionwithwallet](doc/rawtransactions.md#transaction-signing)
  * [testmempoolaccept](doc/rawtransactions.md#raw-transaction-decode--broadcast)
  * [utxoupdatepsbt](doc/rawtransactions.md#psbt)
* [Util](doc/util.md)
  * [createmultisig](doc/util.md#descriptors--multisig)
  * [deriveaddresses](doc/util.md#descriptors--multisig)
  * [estimatesmartfee](doc/util.md#fee-estimation)
  * [getdescriptorinfo](doc/util.md#descriptors--multisig)
  * [signmessagewithprivkey](doc/util.md#address--message-validation)
  * [validateaddress](doc/util.md#address--message-validation)
  * [verifymessage](doc/util.md#address--message-validation)
* [Wallet](doc/wallet.md)
  * [abandontransaction](doc/wallet.md#wallet-files)
  * [abortrescan](doc/wallet.md#wallet-files)
  * [addmultisigaddress](doc/wallet.md#addresses--labels)
  * [backupwallet](doc/wallet.md#import--export)
  * [bumpfee](doc/wallet.md#sending-funds)
  * [createcontract](doc/wallet.md#smart-contract-write-rpc)
  * [createwallet](doc/wallet.md#wallet-files)
  * [dumpprivkey](doc/wallet.md#import--export)
  * [dumpwallet](doc/wallet.md#import--export)
  * [encryptwallet](doc/wallet.md#encryption--locking--staking)
  * [getaddressesbylabel](doc/wallet.md#addresses--labels)
  * [getaddressinfo](doc/wallet.md#addresses--labels)
  * [getbalance](doc/wallet.md#balances--utxo)
  * [getnewaddress](doc/wallet.md#addresses--labels)
  * [getrawchangeaddress](doc/wallet.md#addresses--labels)
  * [getreceivedbyaddress](doc/wallet.md#balances--utxo)
  * [getreceivedbylabel](doc/wallet.md#balances--utxo)
  * [gettransaction](doc/wallet.md#balances--utxo)
  * [getunconfirmedbalance](doc/wallet.md#balances--utxo)
  * [getwalletinfo](doc/wallet.md#wallet-information)
  * [importaddress](doc/wallet.md#import--export)
  * [importmulti](doc/wallet.md#import--export)
  * [importprivkey](doc/wallet.md#import--export)
  * [importprunedfunds](doc/wallet.md#import--export)
  * [importpubkey](doc/wallet.md#import--export)
  * [importwallet](doc/wallet.md#import--export)
  * [keypoolrefill](doc/wallet.md#encryption--locking--staking)
  * [listaddressgroupings](doc/wallet.md#balances--utxo)
  * [listlabels](doc/wallet.md#addresses--labels)
  * [listlockunspent](doc/wallet.md#balances--utxo)
  * [listreceivedbyaddress](doc/wallet.md#balances--utxo)
  * [listreceivedbylabel](doc/wallet.md#balances--utxo)
  * [listsinceblock](doc/wallet.md#balances--utxo)
  * [listtransactions](doc/wallet.md#balances--utxo)
  * [listunspent](doc/wallet.md#balances--utxo)
  * [listwalletdir](doc/wallet.md#wallet-information)
  * [listwallets](doc/wallet.md#wallet-information)
  * [loadwallet](doc/wallet.md#wallet-files)
  * [lockunspent](doc/wallet.md#balances--utxo)
  * [removeprunedfunds](doc/wallet.md#import--export)
  * [rescanblockchain](doc/wallet.md#wallet-files)
  * [reservebalance](doc/wallet.md#encryption--locking--staking)
  * [sendmany](doc/wallet.md#sending-funds)
  * [sendmanywithdupes](doc/wallet.md#sending-funds)
  * [sendtoaddress](doc/wallet.md#sending-funds)
  * [sendtocontract](doc/wallet.md#smart-contract-write-rpc)
  * [sethdseed](doc/wallet.md#encryption--locking--staking)
  * [setlabel](doc/wallet.md#addresses--labels)
  * [settxfee](doc/wallet.md#sending-funds)
  * [signmessage](doc/wallet.md#encryption--locking--staking)
  * [signrawtransactionwithwallet](doc/wallet.md#wallet-files)
  * [unloadwallet](doc/wallet.md#wallet-files)
  * [walletcreatefundedpsbt](doc/wallet.md#wallet-files)
  * [walletlock](doc/wallet.md#encryption--locking--staking)
  * [walletpassphrase](doc/wallet.md#encryption--locking--staking)
  * [walletpassphrasechange](doc/wallet.md#encryption--locking--staking)
  * [walletprocesspsbt](doc/wallet.md#wallet-files)
* [Zmq](doc/zmq.md)
  * [getzmqnotifications](doc/zmq.md#zmq-notifications)

## Quick Start

Use `zerohour-cli` for local development. It automatically reads RPC settings
from the standard ZHCASH data directory.

```bash
zerohour-cli getblockcount
zerohour-cli getblockchaininfo
zerohour-cli getnetworkinfo
```

Use JSON-RPC when calling the node from another process or language.

```bash
COOKIE="$(tr -d '\n' < ~/.zerohour/.cookie)"

curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getblockcount","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Successful response shape:

```json
{"result":1671192,"error":null,"id":"curltest"}
```

The exact height changes. A successful response has `error: null`.

## API Endpoint

Default local mainnet RPC endpoint:

```text
http://127.0.0.1:3889/
```

Default CLI binary:

```text
zerohour-cli
```

The RPC interface should normally stay private on localhost. Do not expose
`3889` to the public internet.

## Authentication

For local development, cookie authentication is the simplest option:

```bash
COOKIE="$(tr -d '\n' < ~/.zerohour/.cookie)"
```

For configured RPC credentials, set them in `zerohour.conf`:

```ini
server=1
rpcuser=your_rpc_user
rpcpassword=your_strong_rpc_password
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
```

Then call:

```bash
curl --user 'your_rpc_user:your_strong_rpc_password' \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getblockcount","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

## JSON-RPC Request Format

Every JSON-RPC request uses the same envelope:

```json
{
  "jsonrpc": "1.0",
  "id": "request-id",
  "method": "getblockcount",
  "params": []
}
```

Field meaning:

| Name | Type | Description |
| --- | --- | --- |
| `jsonrpc` | String | Use `"1.0"` for ZHCASH node RPC compatibility. |
| `id` | String | Client request id. Returned unchanged in the response. |
| `method` | String | RPC method name, for example `getblockchaininfo`. |
| `params` | Array | Positional parameters in the exact order shown in method help. |

## Common RPC Parameters

ZHCASH Core RPC uses positional parameters. Optional parameters are shown in
parentheses in the method signature.

Example:

```text
getblock "blockhash" ( verbosity )
```

Means:

| Position | Name | Required | Description |
| --- | --- | --- | --- |
| 1 | `blockhash` | Yes | Block hash as a hex string. |
| 2 | `verbosity` | No | Output detail level. Common values: `0`, `1`, `2`. |

CLI form:

```bash
HASH="$(zerohour-cli getblockhash 1)"
zerohour-cli getblock "$HASH" 1
```

JSON-RPC form:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getblock","params":["0000751feb032e4d1993d7852130092a6f95d529c44e425b06add2c79aa4a6c7",1]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

## Block / Height Filter Parameters

Some RPC methods accept block heights or height ranges.

| Name | Type | Description |
| --- | --- | --- |
| `height` | Number | Exact block height, for example `1700000`. |
| `start_height` | Number | First block height in a range. |
| `stop_height` | Number | Last block height in a range. |
| `fromBlock` | Number | First block searched by contract log methods. |
| `toBlock` | Number | Last block searched by contract log methods. |
| `minconf` | Number | Minimum confirmations for log/search results. |

Examples:

```bash
zerohour-cli getblockhash 1
zerohour-cli getsubsidy 1700000
zerohour-cli getsubsidy 1700000 1700005
zerohour-cli searchlogs 1700000 1700100
```

## Safety Labels

Each detailed section uses these labels:

| Label | Meaning |
| --- | --- |
| Read-only | Safe information query. Does not change wallet or chain state. |
| Wallet | Reads or changes local wallet state. Requires wallet availability. |
| Broadcast | Can broadcast transactions to the network. Use carefully. |
| Sensitive | Can expose private keys, seed material, or wallet backup data. |
| Node-control | Can stop or alter node/network behavior. |
| Advanced | Usually for infrastructure, indexers, explorers, or contract tooling. |

## Checked Working Examples

These read-only examples were checked on a running ZHCASH Core v1.0.0 node.

```bash
zerohour-cli getblockcount
zerohour-cli getblockchaininfo
zerohour-cli getnetworkinfo
zerohour-cli getstakinginfo
zerohour-cli getsubsidy
zerohour-cli getsubsidy 1700000
zerohour-cli getsubsidy 1700000 1700005
zerohour-cli getdgpinfo
```

Known block 1:

```bash
HASH="$(zerohour-cli getblockhash 1)"
zerohour-cli getblock "$HASH" 1
```

Known result:

```text
0000751feb032e4d1993d7852130092a6f95d529c44e425b06add2c79aa4a6c7
```

Address helper example:

```bash
zerohour-cli validateaddress ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr
zerohour-cli gethexaddress ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr
zerohour-cli fromhexaddress 25ceedce21c41b53b8266d6f224ed7acfa046d96
```

Raw transaction decode/sign example:

```bash
RAW="$(zerohour-cli createrawtransaction '[]' \
  '{"ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr":0.001}')"

zerohour-cli decoderawtransaction "$RAW"
zerohour-cli signrawtransactionwithwallet "$RAW"
zerohour-cli signrawtransactionwithkey "$RAW" '[]'
```

Supported signing methods:

```bash
zerohour-cli signrawtransactionwithwallet "$RAW"
zerohour-cli signrawtransactionwithkey "$RAW" '["private_key_1","private_key_2"]'
```

## Important Notes for Beginners

* `zerohour-cli` is easier for local work; JSON-RPC is better for applications.
* Amounts in wallet methods are usually ZHC. Raw subsidy values are satoshis.
* `getsubsidy` returns satoshis: `80000000000` means `800 ZHC`.
* Do not expose RPC to the internet.
* Do not paste private keys into shared terminals, logs, chats, or screenshots.
* Methods that broadcast transactions are irreversible once accepted by the network.
