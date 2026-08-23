# Blockchain RPC

Blockchain RPC methods read chain state, blocks, mempool entries, UTXO data, contract state, and validation information.

## Menu

* [callcontract](#callcontract)
* [getaccountinfo](#getaccountinfo)
* [getbestblockhash](#getbestblockhash)
* [getblock](#getblock)
* [getblockchaininfo](#getblockchaininfo)
* [getblockcount](#getblockcount)
* [getblockhash](#getblockhash)
* [getblockheader](#getblockheader)
* [getblockstats](#getblockstats)
* [getchaintips](#getchaintips)
* [getchaintxstats](#getchaintxstats)
* [getcontractcode](#getcontractcode)
* [getdifficulty](#getdifficulty)
* [getmempoolancestors](#getmempoolancestors)
* [getmempooldescendants](#getmempooldescendants)
* [getmempoolentry](#getmempoolentry)
* [getmempoolinfo](#getmempoolinfo)
* [getrawmempool](#getrawmempool)
* [getstorage](#getstorage)
* [gettransactionreceipt](#gettransactionreceipt)
* [gettxout](#gettxout)
* [gettxoutproof](#gettxoutproof)
* [gettxoutsetinfo](#gettxoutsetinfo)
* [listcontracts](#listcontracts)
* [preciousblock](#preciousblock)
* [pruneblockchain](#pruneblockchain)
* [savemempool](#savemempool)
* [scantxoutset](#scantxoutset)
* [searchlogs](#searchlogs)
* [verifychain](#verifychain)
* [verifytxoutproof](#verifytxoutproof)
* [waitforlogs](#waitforlogs)
* [Full Method Index](#full-method-index)

## Methods

### callcontract

Safety: Read-only

Call contract methods offline.

Signature:

```text
callcontract "address" "data" ( "senderAddress" gasLimit )
```

Example call:

```bash
zerohour-cli callcontract 'eb23c0b3e6042821da281a2e2364feb22dd543e3' '06fdde03'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"callcontract","params":["eb23c0b3e6042821da281a2e2364feb22dd543e3","06fdde03"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "address": "contract address",             (string)  address of the contract
  "executionResult": {                       (object)  method execution result
    "gasUsed": n,                            (numeric) gas used
    "excepted": "exception",                 (string)  thrown exception
    "newAddress": "contract address",        (string)  new address of the contract
    "output": "data",                        (string)  returned data from the method
    "codeDeposit": n,                        (numeric) code deposit
    "gasRefunded": n,                        (numeric) gas refunded
    "depositSize": n,                        (numeric) deposit size
    "gasForDeposit": n                       (numeric) gas for deposit
  },
  "transactionReceipt": {                    (object)  transaction receipt
    "stateRoot": "hash",                     (string)  state root hash
    "gasUsed": n,                            (numeric) gas used
    "bloom": "bloom",                        (string)  bloom
    "log": [                                 (array)  logs from the receipt
      {
        "address": "address",                (string)  contract address
        "topics":                            (array)  topics
        [
          "topic",                           (string)  topic
        ],
        "data": "data"                       (string)  logged data
      }
    ]
  }
}
```

### getaccountinfo

Safety: Read-only

Get contract details including balance, storage data and code.

Signature:

```text
getaccountinfo "address"
```

Example call:

```bash
zerohour-cli getaccountinfo 'eb23c0b3e6042821da281a2e2364feb22dd543e3'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getaccountinfo","params":["eb23c0b3e6042821da281a2e2364feb22dd543e3"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "address": "contract address",    (string)  address of the contract
  "balance": n,                     (numeric) balance of the contract
  "storage": {...},                 (object)  storage data of the contract
  "code": "bytecode"                (string)  bytecode of the contract
}
```

### getbestblockhash

Safety: Read-only

Returns the hash of the best (tip) block in the longest blockchain.

Signature:

```text
getbestblockhash
```

Example call:

```bash
zerohour-cli getbestblockhash
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getbestblockhash","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```text
83cbd288d04acdeece5d01d5c080273967625d5f71c70f82ae355384d8563246
```

### getblock

Safety: Read-only

If verbosity is 0, returns a string that is serialized, hex-encoded data for block 'hash'.
If verbosity is 1, returns an Object with information about block <hash>.
If verbosity is 2, returns an Object with information about block <hash> and information about each transaction.

Signature:

```text
getblock "blockhash" ( verbosity )
```

Example call:

```bash
zerohour-cli getblock '0000751feb032e4d1993d7852130092a6f95d529c44e425b06add2c79aa4a6c7' 1
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getblock","params":["0000751feb032e4d1993d7852130092a6f95d529c44e425b06add2c79aa4a6c7",1]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```json
{
  "hash": "0000751feb032e4d1993d7852130092a6f95d529c44e425b06add2c79aa4a6c7",
  "confirmations": 1671205,
  "strippedsize": 317,
  "size": 317,
  "weight": 1268,
  "height": 1,
  "version": 536870912,
  "versionHex": "20000000",
  "merkleroot": "9e6184762f52376a181dec17956aa2dfcac20f7185fab475f152731fc451c28b",
  "hashStateRoot": "9514771014c9ae803d8cea2731b2063e83de44802b40dce2d06acd02d0ff65e9",
  "hashUTXORoot": "21b463e3b52f6201c0ad6c991be0485b6ef8c092e64583ffa655cc1b171fe856",
  "tx": [
    "9e6184762f52376a181dec17956aa2dfcac20f7185fab475f152731fc451c28b"
  ],
  "time": 1575408604,
  "mediantime": 1575408604,
  "nonce": 60497,
  "bits": "1f00ffff",
  "difficulty": 1.52587890625e-05,
  "chainwork": "0000000000000000000000000000000000000000000000000000000000020002",
  "nTx": 1,
  "previousblockhash": "00007af309bdd818599502f8fc8af0943c4ce302df2298b14e59abd0c38e07b0",
  "nextblockhash": "000018346fd7d3d754a051680009360f4f3838c98a18673b4d70f4480a7623b9",
  "flags": "proof-of-work",
  "proofhash": "0000000000000000000000000000000000000000000000000000000000000000",
  "modifier": "7452517cc9a5dc168c65d526ff0a0ac26692d7b4bd808a0b41a6b35d468878a2"
}
```

### getblockchaininfo

Safety: Read-only

Returns an object containing various state info regarding blockchain processing.

Signature:

```text
getblockchaininfo
```

Example call:

```bash
zerohour-cli getblockchaininfo
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getblockchaininfo","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```json
{
  "chain": "main",
  "blocks": 1671205,
  "headers": 1671205,
  "bestblockhash": "83cbd288d04acdeece5d01d5c080273967625d5f71c70f82ae355384d8563246",
  "difficulty": 127458902.8985136,
  "moneysupply": 9316964000,
  "mediantime": 1786224672,
  "verificationprogress": 0.9999997263748919,
  "initialblockdownload": false,
  "chainwork": "00000000000000000000000000000000000000000001dec5902aec45a37d475e",
  "size_on_disk": 5838024387,
  "pruned": false,
  "softforks": [
    {
      "id": "bip34",
      "version": 2,
      "reject": {
        "status": true
      }
    },
    {
      "id": "bip66",
      "version": 3,
      "reject": {
        "status": true
      }
    },
    {
      "id": "bip65",
      "version": 4,
      "reject": {
        "status": true
      }
    }
  ],
  "bip9_softforks": {
    "csv": {
      "status": "active",
      "startTime": 0,
      "timeout": 999999999999,
      "since": 6048
    },
    "segwit": {
      "status": "active",
      "startTime": 0,
      "timeout": 999999999999,
      "since": 6048
    }
  },
  "warnings": ""
}
```

### getblockcount

Safety: Read-only

Returns the number of blocks in the longest blockchain.

Signature:

```text
getblockcount
```

Example call:

```bash
zerohour-cli getblockcount
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getblockcount","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```json
1671205
```

### getblockhash

Safety: Read-only

Returns hash of block in best-block-chain at height provided.

Signature:

```text
getblockhash height
```

Example call:

```bash
zerohour-cli getblockhash 1
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getblockhash","params":[1]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```text
0000751feb032e4d1993d7852130092a6f95d529c44e425b06add2c79aa4a6c7
```

### getblockheader

Safety: Read-only

If verbose is false, returns a string that is serialized, hex-encoded data for blockheader 'hash'.
If verbose is true, returns an Object with information about blockheader <hash>.

Signature:

```text
getblockheader "blockhash" ( verbose )
```

Example call:

```bash
zerohour-cli getblockheader '0000751feb032e4d1993d7852130092a6f95d529c44e425b06add2c79aa4a6c7' true
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getblockheader","params":["0000751feb032e4d1993d7852130092a6f95d529c44e425b06add2c79aa4a6c7",true]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result (for verbose = true):
{
  "hash" : "hash",     (string) the block hash (same as provided)
  "confirmations" : n,   (numeric) The number of confirmations, or -1 if the block is not on the main chain
  "height" : n,          (numeric) The block height or index
  "version" : n,         (numeric) The block version
  "versionHex" : "00000000", (string) The block version formatted in hexadecimal
  "merkleroot" : "xxxx", (string) The merkle root
  "time" : ttt,          (numeric) The block time in seconds since epoch (Jan 1 1970 GMT)
  "mediantime" : ttt,    (numeric) The median block time in seconds since epoch (Jan 1 1970 GMT)
  "nonce" : n,           (numeric) The nonce
  "bits" : "1d00ffff", (string) The bits
  "difficulty" : x.xxx,  (numeric) The difficulty
  "chainwork" : "0000...1f3"     (string) Expected number of hashes required to produce the current chain (in hex)
  "nTx" : n,             (numeric) The number of transactions in the block.
  "previousblockhash" : "hash",  (string) The hash of the previous block
  "nextblockhash" : "hash",      (string) The hash of the next block
}

Result (for verbose=false):
"data"             (string) A string that is serialized, hex-encoded data for block 'hash'.
```

### getblockstats

Safety: Read-only

Compute per block statistics for a given window. All amounts are in satoshis.
It won't work for some heights with pruning.
It won't work without -txindex for utxo_size_inc, *fee or *feerate stats.

Signature:

```text
getblockstats hash_or_height ( stats )
```

Example call:

```bash
zerohour-cli getblockstats 1
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getblockstats","params":[1]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{                           (json object)
  "avgfee": xxxxx,          (numeric) Average fee in the block
  "avgfeerate": xxxxx,      (numeric) Average feerate (in satoshis per virtual byte)
  "avgtxsize": xxxxx,       (numeric) Average transaction size
  "blockhash": xxxxx,       (string) The block hash (to check for potential reorgs)
  "feerate_percentiles": [  (array of numeric) Feerates at the 10th, 25th, 50th, 75th, and 90th percentile weight unit (in satoshis per virtual byte)
      "10th_percentile_feerate",      (numeric) The 10th percentile feerate
      "25th_percentile_feerate",      (numeric) The 25th percentile feerate
      "50th_percentile_feerate",      (numeric) The 50th percentile feerate
      "75th_percentile_feerate",      (numeric) The 75th percentile feerate
      "90th_percentile_feerate",      (numeric) The 90th percentile feerate
  ],
  "height": xxxxx,          (numeric) The height of the block
  "ins": xxxxx,             (numeric) The number of inputs (excluding coinbase)
  "maxfee": xxxxx,          (numeric) Maximum fee in the block
  "maxfeerate": xxxxx,      (numeric) Maximum feerate (in satoshis per virtual byte)
  "maxtxsize": xxxxx,       (numeric) Maximum transaction size
  "medianfee": xxxxx,       (numeric) Truncated median fee in the block
  "mediantime": xxxxx,      (numeric) The block median time past
  "mediantxsize": xxxxx,    (numeric) Truncated median transaction size
  "minfee": xxxxx,          (numeric) Minimum fee in the block
  "minfeerate": xxxxx,      (numeric) Minimum feerate (in satoshis per virtual byte)
  "mintxsize": xxxxx,       (numeric) Minimum transaction size
  "outs": xxxxx,            (numeric) The number of outputs
  "subsidy": xxxxx,         (numeric) The block subsidy
  "swtotal_size": xxxxx,    (numeric) Total size of all segwit transactions
  "swtotal_weight": xxxxx,  (numeric) Total weight of all segwit transactions divided by segwit scale factor (4)
  "swtxs": xxxxx,           (numeric) The number of segwit transactions
  "time": xxxxx,            (numeric) The block time
  "total_out": xxxxx,       (numeric) Total amount in all outputs (excluding coinbase and thus reward [ie subsidy + totalfee])
  "total_size": xxxxx,      (numeric) Total size of all non-coinbase transactions
  "total_weight": xxxxx,    (numeric) Total weight of all non-coinbase transactions divided by segwit scale factor (4)
  "totalfee": xxxxx,        (numeric) The fee total
  "txs": xxxxx,             (numeric) The number of transactions (excluding coinbase)
  "utxo_increase": xxxxx,   (numeric) The increase/decrease in the number of unspent outputs
  "utxo_size_inc": xxxxx,   (numeric) The increase/decrease in size for the utxo index (not discounting op_return and similar)
}
```

### getchaintips

Safety: Read-only

Return information about all known tips in the block tree, including the main chain as well as orphaned branches.

Signature:

```text
getchaintips
```

Example call:

```bash
zerohour-cli getchaintips
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getchaintips","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
[
  {
    "height": xxxx,         (numeric) height of the chain tip
    "hash": "xxxx",         (string) block hash of the tip
    "branchlen": 0          (numeric) zero for main chain
    "status": "active"      (string) "active" for the main chain
  },
  {
    "height": xxxx,
    "hash": "xxxx",
    "branchlen": 1          (numeric) length of branch connecting the tip to the main chain
    "status": "xxxx"        (string) status of the chain (active, valid-fork, valid-headers, headers-only, invalid)
  }
]
Possible values for status:
1.  "invalid"               This branch contains at least one invalid block
2.  "headers-only"          Not all blocks for this branch are available, but the headers are valid
3.  "valid-headers"         All blocks are available for this branch, but they were never fully validated
4.  "valid-fork"            This branch is not part of the active chain, but is fully validated
5.  "active"                This is the tip of the active main chain, which is certainly valid
```

### getchaintxstats

Safety: Read-only

Compute statistics about the total number and rate of transactions in the chain.

Signature:

```text
getchaintxstats ( nblocks "blockhash" )
```

Example call:

```bash
zerohour-cli getchaintxstats
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getchaintxstats","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "time": xxxxx,                         (numeric) The timestamp for the final block in the window in UNIX format.
  "txcount": xxxxx,                      (numeric) The total number of transactions in the chain up to that point.
  "window_final_block_hash": "...",      (string) The hash of the final block in the window.
  "window_block_count": xxxxx,           (numeric) Size of the window in number of blocks.
  "window_tx_count": xxxxx,              (numeric) The number of transactions in the window. Only returned if "window_block_count" is > 0.
  "window_interval": xxxxx,              (numeric) The elapsed time in the window in seconds. Only returned if "window_block_count" is > 0.
  "txrate": x.xx,                        (numeric) The average rate of transactions per second in the window. Only returned if "window_interval" is > 0.
}
```

### getcontractcode

Safety: Read-only

Argument:
1. "address"          (string, required) The contract address

Signature:

```text
getcontractcode "address"
```

Example call:

```bash
zerohour-cli getcontractcode
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getcontractcode","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

### getdifficulty

Safety: Read-only

Returns the proof-of-work difficulty as a multiple of the minimum difficulty.

Returns the proof-of-stake difficulty as a multiple of the minimum difficulty.

Signature:

```text
getdifficulty
```

Example call:

```bash
zerohour-cli getdifficulty
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getdifficulty","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```json
{
  "proof-of-work": 1.52587890625e-05,
  "proof-of-stake": 127458902.8985136
}
```

### getmempoolancestors

Safety: Read-only

If txid is in the mempool, returns all in-mempool ancestors.

Signature:

```text
getmempoolancestors "txid" ( verbose )
```

Example call:

```bash
zerohour-cli getmempoolancestors '0000000000000000000000000000000000000000000000000000000000000000' false
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getmempoolancestors","params":["0000000000000000000000000000000000000000000000000000000000000000",false]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result (for verbose = false):
[                       (json array of strings)
  "transactionid"           (string) The transaction id of an in-mempool ancestor transaction
  ,...
]

Result (for verbose = true):
{                           (json object)
  "transactionid" : {       (json object)
    "size" : n,             (numeric) virtual transaction size as defined in BIP 141. This is different from actual serialized size for witness transactions as witness data is discounted.
    "fee" : n,              (numeric) transaction fee in ZHC (DEPRECATED)
    "modifiedfee" : n,      (numeric) transaction fee with fee deltas used for mining priority (DEPRECATED)
    "time" : n,             (numeric) local time transaction entered pool in seconds since 1 Jan 1970 GMT
    "height" : n,           (numeric) block height when transaction entered pool
    "descendantcount" : n,  (numeric) number of in-mempool descendant transactions (including this one)
    "descendantsize" : n,   (numeric) virtual transaction size of in-mempool descendants (including this one)
    "descendantfees" : n,   (numeric) modified fees (see above) of in-mempool descendants (including this one) (DEPRECATED)
    "ancestorcount" : n,    (numeric) number of in-mempool ancestor transactions (including this one)
    "ancestorsize" : n,     (numeric) virtual transaction size of in-mempool ancestors (including this one)
    "ancestorfees" : n,     (numeric) modified fees (see above) of in-mempool ancestors (including this one) (DEPRECATED)
    "wtxid" : hash,         (string) hash of serialized transaction, including witness data
    "fees" : {
        "base" : n,         (numeric) transaction fee in ZHC
        "modified" : n,     (numeric) transaction fee with fee deltas used for mining priority in ZHC
        "ancestor" : n,     (numeric) modified fees (see above) of in-mempool ancestors (including this one) in ZHC
        "descendant" : n,   (numeric) modified fees (see above) of in-mempool descendants (including this one) in ZHC
    }
    "depends" : [           (array) unconfirmed transactions used as inputs for this transaction
        "transactionid",    (string) parent transaction id
       ... ]
    "spentby" : [           (array) unconfirmed transactions spending outputs from this transaction
        "transactionid",    (string) child transaction id
       ... ]
    "bip125-replaceable" : true|false,  (boolean) Whether this transaction could be replaced due to BIP125 (replace-by-fee)
  }, ...
}
```

### getmempooldescendants

Safety: Read-only

If txid is in the mempool, returns all in-mempool descendants.

Signature:

```text
getmempooldescendants "txid" ( verbose )
```

Example call:

```bash
zerohour-cli getmempooldescendants '0000000000000000000000000000000000000000000000000000000000000000' false
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getmempooldescendants","params":["0000000000000000000000000000000000000000000000000000000000000000",false]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result (for verbose = false):
[                       (json array of strings)
  "transactionid"           (string) The transaction id of an in-mempool descendant transaction
  ,...
]

Result (for verbose = true):
{                           (json object)
  "transactionid" : {       (json object)
    "size" : n,             (numeric) virtual transaction size as defined in BIP 141. This is different from actual serialized size for witness transactions as witness data is discounted.
    "fee" : n,              (numeric) transaction fee in ZHC (DEPRECATED)
    "modifiedfee" : n,      (numeric) transaction fee with fee deltas used for mining priority (DEPRECATED)
    "time" : n,             (numeric) local time transaction entered pool in seconds since 1 Jan 1970 GMT
    "height" : n,           (numeric) block height when transaction entered pool
    "descendantcount" : n,  (numeric) number of in-mempool descendant transactions (including this one)
    "descendantsize" : n,   (numeric) virtual transaction size of in-mempool descendants (including this one)
    "descendantfees" : n,   (numeric) modified fees (see above) of in-mempool descendants (including this one) (DEPRECATED)
    "ancestorcount" : n,    (numeric) number of in-mempool ancestor transactions (including this one)
    "ancestorsize" : n,     (numeric) virtual transaction size of in-mempool ancestors (including this one)
    "ancestorfees" : n,     (numeric) modified fees (see above) of in-mempool ancestors (including this one) (DEPRECATED)
    "wtxid" : hash,         (string) hash of serialized transaction, including witness data
    "fees" : {
        "base" : n,         (numeric) transaction fee in ZHC
        "modified" : n,     (numeric) transaction fee with fee deltas used for mining priority in ZHC
        "ancestor" : n,     (numeric) modified fees (see above) of in-mempool ancestors (including this one) in ZHC
        "descendant" : n,   (numeric) modified fees (see above) of in-mempool descendants (including this one) in ZHC
    }
    "depends" : [           (array) unconfirmed transactions used as inputs for this transaction
        "transactionid",    (string) parent transaction id
       ... ]
    "spentby" : [           (array) unconfirmed transactions spending outputs from this transaction
        "transactionid",    (string) child transaction id
       ... ]
    "bip125-replaceable" : true|false,  (boolean) Whether this transaction could be replaced due to BIP125 (replace-by-fee)
  }, ...
}
```

### getmempoolentry

Safety: Read-only

Returns mempool data for given transaction

Signature:

```text
getmempoolentry "txid"
```

Example call:

```bash
zerohour-cli getmempoolentry '0000000000000000000000000000000000000000000000000000000000000000'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getmempoolentry","params":["0000000000000000000000000000000000000000000000000000000000000000"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{                           (json object)
    "size" : n,             (numeric) virtual transaction size as defined in BIP 141. This is different from actual serialized size for witness transactions as witness data is discounted.
    "fee" : n,              (numeric) transaction fee in ZHC (DEPRECATED)
    "modifiedfee" : n,      (numeric) transaction fee with fee deltas used for mining priority (DEPRECATED)
    "time" : n,             (numeric) local time transaction entered pool in seconds since 1 Jan 1970 GMT
    "height" : n,           (numeric) block height when transaction entered pool
    "descendantcount" : n,  (numeric) number of in-mempool descendant transactions (including this one)
    "descendantsize" : n,   (numeric) virtual transaction size of in-mempool descendants (including this one)
    "descendantfees" : n,   (numeric) modified fees (see above) of in-mempool descendants (including this one) (DEPRECATED)
    "ancestorcount" : n,    (numeric) number of in-mempool ancestor transactions (including this one)
    "ancestorsize" : n,     (numeric) virtual transaction size of in-mempool ancestors (including this one)
    "ancestorfees" : n,     (numeric) modified fees (see above) of in-mempool ancestors (including this one) (DEPRECATED)
    "wtxid" : hash,         (string) hash of serialized transaction, including witness data
    "fees" : {
        "base" : n,         (numeric) transaction fee in ZHC
        "modified" : n,     (numeric) transaction fee with fee deltas used for mining priority in ZHC
        "ancestor" : n,     (numeric) modified fees (see above) of in-mempool ancestors (including this one) in ZHC
        "descendant" : n,   (numeric) modified fees (see above) of in-mempool descendants (including this one) in ZHC
    }
    "depends" : [           (array) unconfirmed transactions used as inputs for this transaction
        "transactionid",    (string) parent transaction id
       ... ]
    "spentby" : [           (array) unconfirmed transactions spending outputs from this transaction
        "transactionid",    (string) child transaction id
       ... ]
    "bip125-replaceable" : true|false,  (boolean) Whether this transaction could be replaced due to BIP125 (replace-by-fee)
}
```

### getmempoolinfo

Safety: Read-only

Returns details on the active state of the TX memory pool.

Signature:

```text
getmempoolinfo
```

Example call:

```bash
zerohour-cli getmempoolinfo
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getmempoolinfo","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```json
{
  "size": 0,
  "bytes": 0,
  "usage": 96,
  "maxmempool": 300000000,
  "mempoolminfee": 0.00400000,
  "minrelaytxfee": 0.00400000
}
```

### getrawmempool

Safety: Read-only

Returns all transaction ids in memory pool as a json array of string transaction ids.

Hint: use getmempoolentry to fetch a specific transaction from the mempool.

Signature:

```text
getrawmempool ( verbose )
```

Example call:

```bash
zerohour-cli getrawmempool
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getrawmempool","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```json
[
]
```

### getstorage

Safety: Read-only

Get contract storage data.

Signature:

```text
getstorage "address" ( blockNum index )
```

Example call:

```bash
zerohour-cli getstorage 'eb23c0b3e6042821da281a2e2364feb22dd543e3'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getstorage","params":["eb23c0b3e6042821da281a2e2364feb22dd543e3"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
(object)  storage data of the contract
```

### gettransactionreceipt

Safety: Read-only

Get the transaction receipt.

Signature:

```text
gettransactionreceipt "hash"
```

Example call:

```bash
zerohour-cli gettransactionreceipt '0000000000000000000000000000000000000000000000000000000000000000'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"gettransactionreceipt","params":["0000000000000000000000000000000000000000000000000000000000000000"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
[
  {
    "blockHash": "hash",             (string)  block hash
    "blockNumber": n,                (numeric)  block number
    "transactionHash": "hash",       (string)  transaction hash
    "transactionIndex": n,           (numeric)  transaction index
    "from": "address",               (string)  from address
    "to": "address",                 (string)  to address
    "cumulativeGasUsed": n,          (numeric)  cumulative gas used
    "gasUsed": n,                    (numeric)  gas used
    "contractAddress": "address",    (string)  contract address
    "excepted": "exception",         (string)  thrown exception
    "log": [                         (array)  logs from the receipt
      {
        "address": "address",        (string)  contract address
        "topics":                    (array)  topics
        [
          "topic",                   (string)  topic
        ],
        "data": "data"               (string)  logged data
      }
    ]
  }
]
```

### gettxout

Safety: Read-only

Returns details about an unspent transaction output.

Signature:

```text
gettxout "txid" n ( include_mempool )
```

Example call:

```bash
zerohour-cli gettxout '0000000000000000000000000000000000000000000000000000000000000000' 0 true
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"gettxout","params":["0000000000000000000000000000000000000000000000000000000000000000",0,true]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "bestblock":  "hash",    (string) The hash of the block at the tip of the chain
  "confirmations" : n,       (numeric) The number of confirmations
  "value" : x.xxx,           (numeric) The transaction value in ZHC
  "scriptPubKey" : {         (json object)
     "asm" : "code",       (string) 
     "hex" : "hex",        (string) 
     "reqSigs" : n,          (numeric) Number of required signatures
     "type" : "pubkeyhash", (string) The type, eg pubkeyhash
     "addresses" : [          (array of string) array of zerohour addresses
        "address"     (string) zerohour address
        ,...
     ]
  },
  "coinbase" : true|false   (boolean) Coinbase or not
}
```

### gettxoutproof

Safety: Read-only

Returns a hex-encoded proof that "txid" was included in a block.

NOTE: By default this function only works sometimes. This is when there is an
unspent output in the utxo for this transaction. To make it always work,
you need to maintain a transaction index, using the -txindex command line option or
specify the block in which the transaction is included manually (by blockhash).

Signature:

```text
gettxoutproof ["txid",...] ( "blockhash" )
```

Example call:

```bash
zerohour-cli gettxoutproof '["0000000000000000000000000000000000000000000000000000000000000000"]'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"gettxoutproof","params":[["0000000000000000000000000000000000000000000000000000000000000000"]]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
"data"           (string) A string that is a serialized, hex-encoded data for the proof.
```

### gettxoutsetinfo

Safety: Read-only

Returns statistics about the unspent transaction output set.
Note this call may take some time.

Signature:

```text
gettxoutsetinfo
```

Example call:

```bash
zerohour-cli gettxoutsetinfo
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"gettxoutsetinfo","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```json
{
  "height": 1671205,
  "bestblock": "83cbd288d04acdeece5d01d5c080273967625d5f71c70f82ae355384d8563246",
  "transactions": 4604204,
  "txouts": 5678280,
  "bogosize": 602679401,
  "hash_serialized_2": "cbecb963bf6dcbbd4cbd855c686d51566a314f1f1053c490ccdce7927ecc8b42",
  "disk_size": 578539122,
  "total_amount": 9316963999.50000000
}
```

### listcontracts

Safety: Read-only

Get the contracts list.

Signature:

```text
listcontracts ( start maxDisplay )
```

Example call:

```bash
zerohour-cli listcontracts 1 10
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"listcontracts","params":[1,10]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "account": n,                            (numeric) balance for the account
  ...
}
```

### preciousblock

Safety: Advanced

Treats a block as if it were received before others with the same work.

A later preciousblock call can override the effect of an earlier one.

The effects of preciousblock are not retained across restarts.

Signature:

```text
preciousblock "blockhash"
```

Example call:

```bash
zerohour-cli preciousblock '0000751feb032e4d1993d7852130092a6f95d529c44e425b06add2c79aa4a6c7'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"preciousblock","params":["0000751feb032e4d1993d7852130092a6f95d529c44e425b06add2c79aa4a6c7"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

### pruneblockchain

Safety: Node-control

Signature:

```text
pruneblockchain height
```

Example call:

```bash
zerohour-cli pruneblockchain 1000
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"pruneblockchain","params":[1000]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
n    (numeric) Height of the last block pruned.
```

### savemempool

Safety: Node-control

Dumps the mempool to disk. It will fail until the previous dump is fully loaded.

Signature:

```text
savemempool
```

Example call:

```bash
zerohour-cli savemempool
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"savemempool","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

### scantxoutset

Safety: Advanced

EXPERIMENTAL warning: this call may be removed or changed in future releases.

Scans the unspent transaction output set for entries that match certain output descriptors.
Examples of output descriptors are:
    addr(<address>)                      Outputs whose scriptPubKey corresponds to the specified address (does not include P2PK)
    raw(<hex script>)                    Outputs whose scriptPubKey equals the specified hex scripts
    combo(<pubkey>)                      P2PK, P2PKH, P2WPKH, and P2SH-P2WPKH outputs for the given pubkey
    pkh(<pubkey>)                        P2PKH outputs for the given pubkey
    sh(multi(<n>,<pubkey>,<pubkey>,...)) P2SH-multisig outputs for the given threshold and pubkeys

In the above, <pubkey> either refers to a fixed public key in hexadecimal notation, or to an xpub/xprv optionally followed by one
or more path elements separated by "/", and optionally ending in "/*" (unhardened), or "/*'" or "/*h" (hardened) to specify all
unhardened or hardened child keys.
In the latter case, a range needs to be specified by below if different from 1000.
For more information on output descriptors, see the documentation in the doc/descriptors.md file.

Signature:

```text
scantxoutset "action" [scanobjects,...]
```

Example call:

```bash
zerohour-cli scantxoutset 'status'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"scantxoutset","params":["status"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "unspents": [
    {
    "txid" : "transactionid",     (string) The transaction id
    "vout": n,                    (numeric) the vout value
    "scriptPubKey" : "script",    (string) the script key
    "desc" : "descriptor",        (string) A specialized descriptor for the matched scriptPubKey
    "amount" : x.xxx,             (numeric) The total amount in ZHC of the unspent output
    "height" : n,                 (numeric) Height of the unspent transaction output
   }
   ,...], 
 "total_amount" : x.xxx,          (numeric) The total amount of all found unspent outputs in ZHC
]
```

### searchlogs

Safety: Read-only

Search logs, requires -logevents to be enabled.

Signature:

```text
searchlogs fromBlock toBlock ( "address" "topics" minconf )
```

Example call:

```bash
zerohour-cli searchlogs 1700000 1700100
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"searchlogs","params":[1700000,1700100]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
[
  {
    "blockHash": "hash",             (string)  block hash
    "blockNumber": n,                (numeric)  block number
    "transactionHash": "hash",       (string)  transaction hash
    "transactionIndex": n,           (numeric)  transaction index
    "from": "address",               (string)  from address
    "to": "address",                 (string)  to address
    "cumulativeGasUsed": n,          (numeric)  cumulative gas used
    "gasUsed": n,                    (numeric)  gas used
    "contractAddress": "address",    (string)  contract address
    "excepted": "exception",         (string)  thrown exception
    "log": [                         (array)  logs from the receipt
      {
        "address": "address",        (string)  contract address
        "topics":                    (array)  topics
        [
          "topics",                  (string)  topic
        ],
        "data": "data"               (string)  logged data
      }
    ]
  }
]
```

### verifychain

Safety: Read-only

Verifies blockchain database.

Signature:

```text
verifychain ( checklevel nblocks )
```

Example call:

```bash
zerohour-cli verifychain
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"verifychain","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
true|false       (boolean) Verified or not
```

### verifytxoutproof

Safety: Read-only

Verifies that a proof points to a transaction in a block, returning the transaction it commits to
and throwing an RPC error if the block is not in our best chain

Signature:

```text
verifytxoutproof "proof"
```

Example call:

```bash
zerohour-cli verifytxoutproof 'proof_hex'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"verifytxoutproof","params":["proof_hex"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
["txid"]      (array, strings) The txid(s) which the proof commits to, or empty array if the proof can not be validated.
```

### waitforlogs

Safety: Advanced

requires -logevents to be enabled

Waits for a new logs and return matching log entries. When the call returns, it also specifies the next block number to start waiting for new logs.
By calling waitforlogs repeatedly using the returned `nextBlock` number, a client can receive a stream of up-to-date log entires.

This call is different from the similarly named `waitforlogs`. This call returns individual matching log entries, `searchlogs` returns a transaction receipt if one of the log entries of that transaction matches the filter conditions.

Signature:

```text
waitforlogs ( fromBlock toBlock "filter" minconf )
```

Example call:

```bash
zerohour-cli waitforlogs 1700000 1700100
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"waitforlogs","params":[1700000,1700100]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
An object with the following properties:
1. logs (LogEntry[]) Array of matchiing log entries. This may be empty if `filter` removed all entries.2. count (int) How many log entries are returned.3. nextBlock (int) To wait for new log entries haven't seen before, use this number as `fromBlock`
Usage:
`waitforlogs` waits for new logs, starting from the tip of the chain.
`waitforlogs 600` waits for new logs, but starting from block 600. If there are logs available, this call will return immediately.
`waitforlogs 600 700` waits for new logs, but only up to 700th block
`waitforlogs null null` this is equivalent to `waitforlogs`, using default parameter values
`waitforlogs null null` { "addresses": [ "ff0011..." ], "topics": [ "c0fefe"] }` waits for logs in the future matching the specified conditions

Sample Output:
{
  "entries": [
    {
      "blockHash": "56d5f1f5ec239ef9c822d9ed600fe9aa63727071770ac7c0eabfc903bf7316d4",
      "blockNumber": 3286,
      "transactionHash": "00aa0f041ce333bc3a855b2cba03c41427cda04f0334d7f6cb0acad62f338ddc",
      "transactionIndex": 2,
      "from": "3f6866e2b59121ada1ddfc8edc84a92d9655675f",
      "to": "8e1ee0b38b719abe8fa984c986eabb5bb5071b6b",
      "cumulativeGasUsed": 23709,
      "gasUsed": 23709,
      "contractAddress": "8e1ee0b38b719abe8fa984c986eabb5bb5071b6b",
      "topics": [
        "f0e1159fa6dc12bb31e0098b7a1270c2bd50e760522991c6f0119160028d9916",
        "0000000000000000000000000000000000000000000000000000000000000002"
      ],
      "data": "00000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000003"
    }
  ],

  "count": 7,
  "nextblock": 801
}
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
