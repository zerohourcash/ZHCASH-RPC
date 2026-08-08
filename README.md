# ZHCASH RPC API

Current baseline: ZHCASH Core v1.0.0 (`/Evolution:1.0.0/`, protocol `70018`).

This repository documents the ZHCASH node JSON-RPC interface. The original public
source page is:

https://zh.cash/docs/en/ZHCash-RPC-API/index.html

The method names below were checked against the current node with:

```bash
zerohour-cli help
```

The public page is older than the current node. For v1.0.0, use this repository
as the corrected method index.

## Connection

Default mainnet RPC port:

```text
3889
```

Recommended local CLI form:

```bash
/path/to/zerohour-cli getblockcount
```

Recommended local JSON-RPC form with cookie authentication:

```bash
COOKIE="$(tr -d '\n' < ~/.zerohour/.cookie)"

curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getblockcount","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Expected successful response shape:

```json
{"result":1671192,"error":null,"id":"curltest"}
```

The height changes over time; the important part is that `error` is `null`.

## Working read-only examples

These examples were checked on a running ZHCASH Core v1.0.0 node.

### Basic chain status

```bash
zerohour-cli getblockcount
zerohour-cli getbestblockhash
zerohour-cli getblockchaininfo
zerohour-cli getnetworkinfo
```

### Get a block by height

```bash
HASH="$(zerohour-cli getblockhash 1)"
zerohour-cli getblock "$HASH" 1
```

Known block 1 hash on the checked mainnet node:

```text
0000751feb032e4d1993d7852130092a6f95d529c44e425b06add2c79aa4a6c7
```

### PoS/staking and subsidy

```bash
zerohour-cli getstakinginfo
zerohour-cli getsubsidy
zerohour-cli getsubsidy 1700000
zerohour-cli getsubsidy 1700000 1700005
```

Checked outputs:

```text
zerohour-cli getsubsidy
80000000000

zerohour-cli getsubsidy 1700000
40000000000
```

`getsubsidy` returns satoshis. `80000000000` = `800 ZHC`,
`40000000000` = `400 ZHC`.

Range mode:

```json
{
  "startheight": 1700000,
  "endheight": 1700005,
  "blocks": 6,
  "firstsubsidy": 40000000000,
  "lastsubsidy": 40000000000,
  "totalsubsidy": 240000000000,
  "totalsubsidy_zhc": 2400.00000000
}
```

### Dynamic gas parameters

```bash
zerohour-cli getdgpinfo
```

Checked output shape:

```json
{
  "maxblocksize": 2000000,
  "mingasprice": 40,
  "blockgaslimit": 40000000
}
```

### Address helpers

```bash
zerohour-cli validateaddress ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr
zerohour-cli gethexaddress ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr
zerohour-cli fromhexaddress 25ceedce21c41b53b8266d6f224ed7acfa046d96
```

Checked conversion:

```text
ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr
25ceedce21c41b53b8266d6f224ed7acfa046d96
```

### Raw transaction decode/sign flow

Create a simple unsigned raw transaction with no inputs:

```bash
RAW="$(zerohour-cli createrawtransaction '[]' \
  '{"ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr":0.001}')"

zerohour-cli decoderawtransaction "$RAW"
zerohour-cli signrawtransactionwithwallet "$RAW"
zerohour-cli signrawtransactionwithkey "$RAW" '[]'
```

Checked raw transaction hex:

```text
020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000
```

Supported signing RPCs:

```bash
zerohour-cli signrawtransactionwithwallet "$RAW"
zerohour-cli signrawtransactionwithkey "$RAW" '["private_key_1","private_key_2"]'
```

Correct JSON-RPC form:

```bash
COOKIE="$(tr -d '\n' < ~/.zerohour/.cookie)"

curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"signrawtransactionwithwallet","params":["020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

## Smart contract RPC examples

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

Send to contract:

```bash
zerohour-cli sendtocontract \
  "c6ca2697719d00446d4ea51f6fac8fd1e9310214" \
  "54f6127f" \
  0 \
  250000 \
  0.0000004
```

Create contract:

```bash
zerohour-cli createcontract "bytecode_hex" 2500000 0.0000004
```

These contract examples are syntax-correct. They require a real contract
address/bytecode and, for broadcasting transactions, an unlocked funded wallet.

## Method index

### Blockchain

| Method | Signature |
| --- | --- |
| `callcontract` | `callcontract "address" "data" ( "senderAddress" gasLimit )` |
| `getaccountinfo` | `getaccountinfo "address"` |
| `getbestblockhash` | `getbestblockhash` |
| `getblock` | `getblock "blockhash" ( verbosity )` |
| `getblockchaininfo` | `getblockchaininfo` |
| `getblockcount` | `getblockcount` |
| `getblockhash` | `getblockhash height` |
| `getblockheader` | `getblockheader "blockhash" ( verbose )` |
| `getblockstats` | `getblockstats hash_or_height ( stats )` |
| `getchaintips` | `getchaintips` |
| `getchaintxstats` | `getchaintxstats ( nblocks "blockhash" )` |
| `getcontractcode` | `getcontractcode "address"` |
| `getdifficulty` | `getdifficulty` |
| `getmempoolancestors` | `getmempoolancestors "txid" ( verbose )` |
| `getmempooldescendants` | `getmempooldescendants "txid" ( verbose )` |
| `getmempoolentry` | `getmempoolentry "txid"` |
| `getmempoolinfo` | `getmempoolinfo` |
| `getrawmempool` | `getrawmempool ( verbose )` |
| `getstorage` | `getstorage "address" ( blockNum index )` |
| `gettransactionreceipt` | `gettransactionreceipt "hash"` |
| `gettxout` | `gettxout "txid" n ( include_mempool )` |
| `gettxoutproof` | `gettxoutproof ["txid",...] ( "blockhash" )` |
| `gettxoutsetinfo` | `gettxoutsetinfo` |
| `listcontracts` | `listcontracts ( start maxDisplay )` |
| `preciousblock` | `preciousblock "blockhash"` |
| `pruneblockchain` | `pruneblockchain height` |
| `savemempool` | `savemempool` |
| `scantxoutset` | `scantxoutset "action" [scanobjects,...]` |
| `searchlogs` | `searchlogs fromBlock toBlock ( "address" "topics" minconf )` |
| `verifychain` | `verifychain ( checklevel nblocks )` |
| `verifytxoutproof` | `verifytxoutproof "proof"` |
| `waitforlogs` | `waitforlogs ( fromBlock toBlock "filter" minconf )` |

### Control

| Method | Signature |
| --- | --- |
| `getdgpinfo` | `getdgpinfo` |
| `getmemoryinfo` | `getmemoryinfo ( "mode" )` |
| `getrpcinfo` | `getrpcinfo` |
| `help` | `help ( "command" )` |
| `logging` | `logging ( ["include_category",...] ["exclude_category",...] )` |
| `stop` | `stop` |
| `uptime` | `uptime` |

### Generating

| Method | Signature |
| --- | --- |
| `generate` | `generate nblocks ( maxtries )` |
| `generatetoaddress` | `generatetoaddress nblocks "address" ( maxtries )` |

### Mining

| Method | Signature |
| --- | --- |
| `getblocktemplate` | `getblocktemplate "template_request"` |
| `getmininginfo` | `getmininginfo` |
| `getnetworkhashps` | `getnetworkhashps ( nblocks height )` |
| `getstakinginfo` | `getstakinginfo` |
| `getsubsidy` | `getsubsidy ( height endheight )` |
| `prioritisetransaction` | `prioritisetransaction "txid" ( dummy ) fee_delta` |
| `submitblock` | `submitblock "hexdata" ( "dummy" )` |
| `submitheader` | `submitheader "hexdata"` |

### Network

| Method | Signature |
| --- | --- |
| `addnode` | `addnode "node" "command"` |
| `clearbanned` | `clearbanned` |
| `disconnectnode` | `disconnectnode ( "address" nodeid )` |
| `getaddednodeinfo` | `getaddednodeinfo ( "node" )` |
| `getconnectioncount` | `getconnectioncount` |
| `getnettotals` | `getnettotals` |
| `getnetworkinfo` | `getnetworkinfo` |
| `getnodeaddresses` | `getnodeaddresses ( count )` |
| `getpeerinfo` | `getpeerinfo` |
| `listbanned` | `listbanned` |
| `ping` | `ping` |
| `setban` | `setban "subnet" "command" ( bantime absolute )` |
| `setnetworkactive` | `setnetworkactive state` |

### Rawtransactions

| Method | Signature |
| --- | --- |
| `analyzepsbt` | `analyzepsbt "psbt"` |
| `combinepsbt` | `combinepsbt ["psbt",...]` |
| `combinerawtransaction` | `combinerawtransaction ["hexstring",...]` |
| `converttopsbt` | `converttopsbt "hexstring" ( permitsigdata iswitness )` |
| `createpsbt` | `createpsbt [{"txid":"hex","vout":n,"sequence":n},...] [{"address":amount},{"data":"hex"},...] ( locktime replaceable )` |
| `createrawtransaction` | `createrawtransaction [{"txid":"hex","vout":n,"sequence":n},...] [{"address":amount},{"data":"hex"},{"contractAddress":"hex","data":"hex","amount":amount,"gasLimit":n,"gasPrice":n},...] ( locktime replaceable )` |
| `decodepsbt` | `decodepsbt "psbt"` |
| `decoderawtransaction` | `decoderawtransaction "hexstring" ( iswitness )` |
| `decodescript` | `decodescript "hexstring"` |
| `finalizepsbt` | `finalizepsbt "psbt" ( extract )` |
| `fromhexaddress` | `fromhexaddress "hexaddress"` |
| `fundrawtransaction` | `fundrawtransaction "hexstring" ( options iswitness )` |
| `gethexaddress` | `gethexaddress "address"` |
| `getrawtransaction` | `getrawtransaction "txid" ( verbose "blockhash" )` |
| `joinpsbts` | `joinpsbts ["psbt",...]` |
| `sendrawtransaction` | `sendrawtransaction "hexstring" ( allowhighfees )` |
| `signrawtransactionwithkey` | `signrawtransactionwithkey "hexstring" ["privatekey",...] ( prevtxs "sighashtype" )` |
| `signrawtransactionwithwallet` | `signrawtransactionwithwallet "hexstring" ( prevtxs "sighashtype" )` |
| `testmempoolaccept` | `testmempoolaccept ["rawtx",...] ( allowhighfees )` |
| `utxoupdatepsbt` | `utxoupdatepsbt "psbt"` |

### Util

| Method | Signature |
| --- | --- |
| `createmultisig` | `createmultisig nrequired ["key",...] ( "address_type" )` |
| `deriveaddresses` | `deriveaddresses "descriptor" ( range )` |
| `estimatesmartfee` | `estimatesmartfee conf_target ( "estimate_mode" )` |
| `getdescriptorinfo` | `getdescriptorinfo "descriptor"` |
| `signmessagewithprivkey` | `signmessagewithprivkey "privkey" "message"` |
| `validateaddress` | `validateaddress "address"` |
| `verifymessage` | `verifymessage "address" "signature" "message"` |

### Wallet

| Method | Signature |
| --- | --- |
| `abandontransaction` | `abandontransaction "txid"` |
| `abortrescan` | `abortrescan` |
| `addmultisigaddress` | `addmultisigaddress nrequired ["key",...] ( "label" "address_type" )` |
| `backupwallet` | `backupwallet "destination"` |
| `bumpfee` | `bumpfee "txid" ( options )` |
| `createcontract` | `createcontract "bytecode" ( gasLimit gasPrice "senderaddress" broadcast changeToSender )` |
| `createwallet` | `createwallet "wallet_name" ( disable_private_keys blank )` |
| `dumpprivkey` | `dumpprivkey "address"` |
| `dumpwallet` | `dumpwallet "filename"` |
| `encryptwallet` | `encryptwallet "passphrase"` |
| `getaddressesbylabel` | `getaddressesbylabel "label"` |
| `getaddressinfo` | `getaddressinfo "address"` |
| `getbalance` | `getbalance ( "dummy" minconf include_watchonly )` |
| `getnewaddress` | `getnewaddress ( "label" "address_type" )` |
| `getrawchangeaddress` | `getrawchangeaddress ( "address_type" )` |
| `getreceivedbyaddress` | `getreceivedbyaddress "address" ( minconf )` |
| `getreceivedbylabel` | `getreceivedbylabel "label" ( minconf )` |
| `gettransaction` | `gettransaction "txid" ( include_watchonly waitconf )` |
| `getunconfirmedbalance` | `getunconfirmedbalance` |
| `getwalletinfo` | `getwalletinfo` |
| `importaddress` | `importaddress "address" ( "label" rescan p2sh )` |
| `importmulti` | `importmulti "requests" ( "options" )` |
| `importprivkey` | `importprivkey "zerohourprivkey" ( "label" rescan )` |
| `importprunedfunds` | `importprunedfunds "rawtransaction" "txoutproof"` |
| `importpubkey` | `importpubkey "pubkey" ( "label" rescan )` |
| `importwallet` | `importwallet "filename"` |
| `keypoolrefill` | `keypoolrefill ( newsize )` |
| `listaddressgroupings` | `listaddressgroupings` |
| `listlabels` | `listlabels ( "purpose" )` |
| `listlockunspent` | `listlockunspent` |
| `listreceivedbyaddress` | `listreceivedbyaddress ( minconf include_empty include_watchonly "address_filter" )` |
| `listreceivedbylabel` | `listreceivedbylabel ( minconf include_empty include_watchonly )` |
| `listsinceblock` | `listsinceblock ( "blockhash" target_confirmations include_watchonly include_removed )` |
| `listtransactions` | `listtransactions ( "label" count skip include_watchonly )` |
| `listunspent` | `listunspent ( minconf maxconf ["address",...] include_unsafe query_options )` |
| `listwalletdir` | `listwalletdir` |
| `listwallets` | `listwallets` |
| `loadwallet` | `loadwallet "filename"` |
| `lockunspent` | `lockunspent unlock ( [{"txid":"hex","vout":n},...] )` |
| `removeprunedfunds` | `removeprunedfunds "txid"` |
| `rescanblockchain` | `rescanblockchain ( start_height stop_height )` |
| `reservebalance` | `reservebalance ( reserve amount )` |
| `sendmany` | `sendmany "" {"address":amount} ( minconf "comment" ["address",...] replaceable conf_target "estimate_mode" )` |
| `sendmanywithdupes` | `sendmanywithdupes "fromaccount" {"address":"hex"} ( minconf "comment" ["address",...] )` |
| `sendtoaddress` | `sendtoaddress "address" amount ( "comment" "comment_to" subtractfeefromamount replaceable conf_target "estimate_mode" "senderaddress" changeToSender )` |
| `sendtocontract` | `sendtocontract "contractaddress" "datahex" ( amount gasLimit gasPrice "senderaddress" broadcast changeToSender )` |
| `sethdseed` | `sethdseed ( newkeypool "seed" )` |
| `setlabel` | `setlabel "address" "label"` |
| `settxfee` | `settxfee amount` |
| `signmessage` | `signmessage "address" "message"` |
| `signrawtransactionwithwallet` | `signrawtransactionwithwallet "hexstring" ( prevtxs "sighashtype" )` |
| `unloadwallet` | `unloadwallet ( "wallet_name" )` |
| `walletcreatefundedpsbt` | `walletcreatefundedpsbt [{"txid":"hex","vout":n,"sequence":n},...] [{"address":amount},{"data":"hex"},...] ( locktime options bip32derivs )` |
| `walletlock` | `walletlock` |
| `walletpassphrase` | `walletpassphrase "passphrase" timeout ( staking only )` |
| `walletpassphrasechange` | `walletpassphrasechange "oldpassphrase" "newpassphrase"` |
| `walletprocesspsbt` | `walletprocesspsbt "psbt" ( sign "sighashtype" bip32derivs )` |

### ZMQ

| Method | Signature |
| --- | --- |
| `getzmqnotifications` | `getzmqnotifications` |

## Changes versus the old public HTML page

Added in current node help but missing from the older public page:

```text
getdgpinfo
getrpcinfo
getcontractcode
submitheader
getnodeaddresses
analyzepsbt
joinpsbts
utxoupdatepsbt
deriveaddresses
getdescriptorinfo
getreceivedbylabel
listreceivedbylabel
listwalletdir
setlabel
```
