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
* [Recommended Node Configuration](doc/node-config.md)
* [Authentication](#authentication)
* [JSON-RPC Request Format](#json-rpc-request-format)
* [Common RPC Parameters](#common-rpc-parameters)
* [Block / Height Filter Parameters](#block--height-filter-parameters)
* [Safety Labels](#safety-labels)
* [Blockchain](doc/blockchain.md)
  * [callcontract](doc/blockchain.md#callcontract)
  * [getaccountinfo](doc/blockchain.md#getaccountinfo)
  * [getbestblockhash](doc/blockchain.md#getbestblockhash)
  * [getblock](doc/blockchain.md#getblock)
  * [getblockchaininfo](doc/blockchain.md#getblockchaininfo)
  * [getblockcount](doc/blockchain.md#getblockcount)
  * [getblockhash](doc/blockchain.md#getblockhash)
  * [getblockheader](doc/blockchain.md#getblockheader)
  * [getblockstats](doc/blockchain.md#getblockstats)
  * [getchaintips](doc/blockchain.md#getchaintips)
  * [getchaintxstats](doc/blockchain.md#getchaintxstats)
  * [getcontractcode](doc/blockchain.md#getcontractcode)
  * [getdifficulty](doc/blockchain.md#getdifficulty)
  * [getmempoolancestors](doc/blockchain.md#getmempoolancestors)
  * [getmempooldescendants](doc/blockchain.md#getmempooldescendants)
  * [getmempoolentry](doc/blockchain.md#getmempoolentry)
  * [getmempoolinfo](doc/blockchain.md#getmempoolinfo)
  * [getrawmempool](doc/blockchain.md#getrawmempool)
  * [getstorage](doc/blockchain.md#getstorage)
  * [gettransactionreceipt](doc/blockchain.md#gettransactionreceipt)
  * [gettxout](doc/blockchain.md#gettxout)
  * [gettxoutproof](doc/blockchain.md#gettxoutproof)
  * [gettxoutsetinfo](doc/blockchain.md#gettxoutsetinfo)
  * [listcontracts](doc/blockchain.md#listcontracts)
  * [preciousblock](doc/blockchain.md#preciousblock)
  * [pruneblockchain](doc/blockchain.md#pruneblockchain)
  * [savemempool](doc/blockchain.md#savemempool)
  * [scantxoutset](doc/blockchain.md#scantxoutset)
  * [searchlogs](doc/blockchain.md#searchlogs)
  * [verifychain](doc/blockchain.md#verifychain)
  * [verifytxoutproof](doc/blockchain.md#verifytxoutproof)
  * [waitforlogs](doc/blockchain.md#waitforlogs)
* [Control](doc/control.md)
  * [getdgpinfo](doc/control.md#getdgpinfo)
  * [getmemoryinfo](doc/control.md#getmemoryinfo)
  * [getrpcinfo](doc/control.md#getrpcinfo)
  * [help](doc/control.md#help)
  * [logging](doc/control.md#logging)
  * [stop](doc/control.md#stop)
  * [uptime](doc/control.md#uptime)
* [Generating](doc/generating.md)
  * [generate](doc/generating.md#generate)
  * [generatetoaddress](doc/generating.md#generatetoaddress)
* [Mining](doc/mining.md)
  * [getblocktemplate](doc/mining.md#getblocktemplate)
  * [getmininginfo](doc/mining.md#getmininginfo)
  * [getnetworkhashps](doc/mining.md#getnetworkhashps)
  * [getstakinginfo](doc/mining.md#getstakinginfo)
  * [getsubsidy](doc/mining.md#getsubsidy)
  * [prioritisetransaction](doc/mining.md#prioritisetransaction)
  * [submitblock](doc/mining.md#submitblock)
  * [submitheader](doc/mining.md#submitheader)
* [Network](doc/network.md)
  * [addnode](doc/network.md#addnode)
  * [clearbanned](doc/network.md#clearbanned)
  * [disconnectnode](doc/network.md#disconnectnode)
  * [getaddednodeinfo](doc/network.md#getaddednodeinfo)
  * [getconnectioncount](doc/network.md#getconnectioncount)
  * [getnettotals](doc/network.md#getnettotals)
  * [getnetworkinfo](doc/network.md#getnetworkinfo)
  * [getnodeaddresses](doc/network.md#getnodeaddresses)
  * [getpeerinfo](doc/network.md#getpeerinfo)
  * [listbanned](doc/network.md#listbanned)
  * [ping](doc/network.md#ping)
  * [setban](doc/network.md#setban)
  * [setnetworkactive](doc/network.md#setnetworkactive)
* [Rawtransactions](doc/rawtransactions.md)
  * [analyzepsbt](doc/rawtransactions.md#analyzepsbt)
  * [combinepsbt](doc/rawtransactions.md#combinepsbt)
  * [combinerawtransaction](doc/rawtransactions.md#combinerawtransaction)
  * [converttopsbt](doc/rawtransactions.md#converttopsbt)
  * [createpsbt](doc/rawtransactions.md#createpsbt)
  * [createrawtransaction](doc/rawtransactions.md#createrawtransaction)
  * [decodepsbt](doc/rawtransactions.md#decodepsbt)
  * [decoderawtransaction](doc/rawtransactions.md#decoderawtransaction)
  * [decodescript](doc/rawtransactions.md#decodescript)
  * [finalizepsbt](doc/rawtransactions.md#finalizepsbt)
  * [fromhexaddress](doc/rawtransactions.md#fromhexaddress)
  * [fundrawtransaction](doc/rawtransactions.md#fundrawtransaction)
  * [gethexaddress](doc/rawtransactions.md#gethexaddress)
  * [getrawtransaction](doc/rawtransactions.md#getrawtransaction)
  * [joinpsbts](doc/rawtransactions.md#joinpsbts)
  * [sendrawtransaction](doc/rawtransactions.md#sendrawtransaction)
  * [signrawtransactionwithkey](doc/rawtransactions.md#signrawtransactionwithkey)
  * [testmempoolaccept](doc/rawtransactions.md#testmempoolaccept)
  * [utxoupdatepsbt](doc/rawtransactions.md#utxoupdatepsbt)
* [Util](doc/util.md)
  * [createmultisig](doc/util.md#createmultisig)
  * [deriveaddresses](doc/util.md#deriveaddresses)
  * [estimatesmartfee](doc/util.md#estimatesmartfee)
  * [getdescriptorinfo](doc/util.md#getdescriptorinfo)
  * [signmessagewithprivkey](doc/util.md#signmessagewithprivkey)
  * [validateaddress](doc/util.md#validateaddress)
  * [verifymessage](doc/util.md#verifymessage)
* [Wallet](doc/wallet.md)
  * [abandontransaction](doc/wallet.md#abandontransaction)
  * [abortrescan](doc/wallet.md#abortrescan)
  * [addmultisigaddress](doc/wallet.md#addmultisigaddress)
  * [backupwallet](doc/wallet.md#backupwallet)
  * [bumpfee](doc/wallet.md#bumpfee)
  * [createcontract](doc/wallet.md#createcontract)
  * [createwallet](doc/wallet.md#createwallet)
  * [dumpprivkey](doc/wallet.md#dumpprivkey)
  * [dumpwallet](doc/wallet.md#dumpwallet)
  * [encryptwallet](doc/wallet.md#encryptwallet)
  * [getaddressesbylabel](doc/wallet.md#getaddressesbylabel)
  * [getaddressinfo](doc/wallet.md#getaddressinfo)
  * [getbalance](doc/wallet.md#getbalance)
  * [getnewaddress](doc/wallet.md#getnewaddress)
  * [getrawchangeaddress](doc/wallet.md#getrawchangeaddress)
  * [getreceivedbyaddress](doc/wallet.md#getreceivedbyaddress)
  * [getreceivedbylabel](doc/wallet.md#getreceivedbylabel)
  * [gettransaction](doc/wallet.md#gettransaction)
  * [getunconfirmedbalance](doc/wallet.md#getunconfirmedbalance)
  * [getwalletinfo](doc/wallet.md#getwalletinfo)
  * [importaddress](doc/wallet.md#importaddress)
  * [importmulti](doc/wallet.md#importmulti)
  * [importprivkey](doc/wallet.md#importprivkey)
  * [importprunedfunds](doc/wallet.md#importprunedfunds)
  * [importpubkey](doc/wallet.md#importpubkey)
  * [importwallet](doc/wallet.md#importwallet)
  * [keypoolrefill](doc/wallet.md#keypoolrefill)
  * [listaddressgroupings](doc/wallet.md#listaddressgroupings)
  * [listlabels](doc/wallet.md#listlabels)
  * [listlockunspent](doc/wallet.md#listlockunspent)
  * [listreceivedbyaddress](doc/wallet.md#listreceivedbyaddress)
  * [listreceivedbylabel](doc/wallet.md#listreceivedbylabel)
  * [listsinceblock](doc/wallet.md#listsinceblock)
  * [listtransactions](doc/wallet.md#listtransactions)
  * [listunspent](doc/wallet.md#listunspent)
  * [listwalletdir](doc/wallet.md#listwalletdir)
  * [listwallets](doc/wallet.md#listwallets)
  * [loadwallet](doc/wallet.md#loadwallet)
  * [lockunspent](doc/wallet.md#lockunspent)
  * [removeprunedfunds](doc/wallet.md#removeprunedfunds)
  * [rescanblockchain](doc/wallet.md#rescanblockchain)
  * [reservebalance](doc/wallet.md#reservebalance)
  * [sendmany](doc/wallet.md#sendmany)
  * [sendmanywithdupes](doc/wallet.md#sendmanywithdupes)
  * [sendtoaddress](doc/wallet.md#sendtoaddress)
  * [sendtocontract](doc/wallet.md#sendtocontract)
  * [sethdseed](doc/wallet.md#sethdseed)
  * [setlabel](doc/wallet.md#setlabel)
  * [settxfee](doc/wallet.md#settxfee)
  * [signmessage](doc/wallet.md#signmessage)
  * [signrawtransactionwithwallet](doc/wallet.md#signrawtransactionwithwallet)
  * [unloadwallet](doc/wallet.md#unloadwallet)
  * [walletcreatefundedpsbt](doc/wallet.md#walletcreatefundedpsbt)
  * [walletlock](doc/wallet.md#walletlock)
  * [walletpassphrase](doc/wallet.md#walletpassphrase)
  * [walletpassphrasechange](doc/wallet.md#walletpassphrasechange)
  * [walletprocesspsbt](doc/wallet.md#walletprocesspsbt)
* [Zmq](doc/zmq.md)
  * [getzmqnotifications](doc/zmq.md#getzmqnotifications)

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
