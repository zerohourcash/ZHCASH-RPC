# Mining / Staking RPC

Mining RPC methods cover mining templates, staking status, subsidy calculation, transaction priority, and block submission.

## Menu

* [getblocktemplate](#getblocktemplate)
* [getmininginfo](#getmininginfo)
* [getnetworkhashps](#getnetworkhashps)
* [getstakinginfo](#getstakinginfo)
* [getsubsidy](#getsubsidy)
* [prioritisetransaction](#prioritisetransaction)
* [submitblock](#submitblock)
* [submitheader](#submitheader)
* [Full Method Index](#full-method-index)

## Methods

### getblocktemplate

Safety: Advanced

If the request parameters include a 'mode' key, that is used to explicitly select between the default 'template' request or a 'proposal'.
It returns data needed to construct a block to work on.
For full specification, see BIPs 22, 23, 9, and 145:
    https://github.com/bitcoin/bips/blob/master/bip-0022.mediawiki
    https://github.com/bitcoin/bips/blob/master/bip-0023.mediawiki
    https://github.com/bitcoin/bips/blob/master/bip-0009.mediawiki#getblocktemplate_changes
    https://github.com/bitcoin/bips/blob/master/bip-0145.mediawiki

Signature:

```text
getblocktemplate "template_request"
```

Example call:

```bash
zerohour-cli getblocktemplate '{}'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getblocktemplate","params":[{}]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "version" : n,                    (numeric) The preferred block version
  "rules" : [ "rulename", ... ],    (array of strings) specific block rules that are to be enforced
  "vbavailable" : {                 (json object) set of pending, supported versionbit (BIP 9) softfork deployments
      "rulename" : bitnumber          (numeric) identifies the bit number as indicating acceptance and readiness for the named softfork rule
      ,...
  },
  "vbrequired" : n,                 (numeric) bit mask of versionbits the server requires set in submissions
  "previousblockhash" : "xxxx",     (string) The hash of current highest block
  "transactions" : [                (array) contents of non-coinbase transactions that should be included in the next block
      {
         "data" : "xxxx",             (string) transaction data encoded in hexadecimal (byte-for-byte)
         "txid" : "xxxx",             (string) transaction id encoded in little-endian hexadecimal
         "hash" : "xxxx",             (string) hash encoded in little-endian hexadecimal (including witness data)
         "depends" : [                (array) array of numbers 
             n                          (numeric) transactions before this one (by 1-based index in 'transactions' list) that must be present in the final block if this one is
             ,...
         ],
         "fee": n,                    (numeric) difference in value between transaction inputs and outputs (in satoshis); for coinbase transactions, this is a negative Number of the total collected block fees (ie, not including the block subsidy); if key is not present, fee is unknown and clients MUST NOT assume there isn't one
         "sigops" : n,                (numeric) total SigOps cost, as counted for purposes of block limits; if key is not present, sigop cost is unknown and clients MUST NOT assume it is zero
         "weight" : n,                (numeric) total transaction weight, as counted for purposes of block limits
      }
      ,...
  ],
  "coinbaseaux" : {                 (json object) data that should be included in the coinbase's scriptSig content
      "flags" : "xx"                  (string) key name is to be ignored, and value included in scriptSig
  },
  "coinbasevalue" : n,              (numeric) maximum allowable input to coinbase transaction, including the generation award and transaction fees (in satoshis)
  "coinbasetxn" : { ... },          (json object) information for coinbase transaction
  "target" : "xxxx",                (string) The hash target
  "mintime" : xxx,                  (numeric) The minimum timestamp appropriate for next block time in seconds since epoch (Jan 1 1970 GMT)
  "mutable" : [                     (array of string) list of ways the block template may be changed 
     "value"                          (string) A way the block template may be changed, e.g. 'time', 'transactions', 'prevblock'
     ,...
  ],
  "noncerange" : "00000000ffffffff",(string) A range of valid nonces
  "sigoplimit" : n,                 (numeric) limit of sigops in blocks
  "sizelimit" : n,                  (numeric) limit of block size
  "weightlimit" : n,                (numeric) limit of block weight
  "curtime" : ttt,                  (numeric) current timestamp in seconds since epoch (Jan 1 1970 GMT)
  "bits" : "xxxxxxxx",              (string) compressed target of next block
  "height" : n                      (numeric) The height of the next block
}
```

### getmininginfo

Safety: Read-only

Returns a json object containing mining-related information.

Signature:

```text
getmininginfo
```

Example call:

```bash
zerohour-cli getmininginfo
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getmininginfo","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```json
{
  "blocks": 1671205,
  "difficulty": {
    "proof-of-work": 1.52587890625e-05,
    "proof-of-stake": 127458902.8985136,
    "search-interval": 0
  },
  "blockvalue": 80000000000,
  "netmhashps": 0,
  "netstakeweight": 7.418155927754824e+16,
  "errors": "",
  "networkhashps": 4449870909894546,
  "pooledtx": 0,
  "stakeweight": {
    "minimum": 0,
    "maximum": 0,
    "combined": 0
  },
  "chain": "main",
  "warnings": ""
}
```

### getnetworkhashps

Safety: Read-only

Returns the estimated network hashes per second based on the last n blocks (for PoW only).
Pass in [blocks] to override # of blocks, -1 specifies since last difficulty change.
Pass in [height] to estimate the network speed at the time when a certain block was found.

Signature:

```text
getnetworkhashps ( nblocks height )
```

Example call:

```bash
zerohour-cli getnetworkhashps
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getnetworkhashps","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
x             (numeric) Hashes per second estimated
```

### getstakinginfo

Safety: Read-only

Returns an object containing staking-related information.

Signature:

```text
getstakinginfo
```

Example call:

```bash
zerohour-cli getstakinginfo
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getstakinginfo","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```json
{
  "enabled": true,
  "staking": false,
  "errors": "",
  "pooledtx": 0,
  "difficulty": 127458902.8985136,
  "search-interval": 0,
  "weight": 0,
  "netstakeweight": 74181559277548240,
  "expectedtime": 0
}
```

### getsubsidy

Safety: Read-only

Returns subsidy value for the specified block height.
If endheight is provided, returns a fast dry-run summary for the inclusive height range.
The range mode does not mine or require the blocks to exist on the local chain.

Signature:

```text
getsubsidy ( height endheight )
```

Example call:

```bash
zerohour-cli getsubsidy 1700000
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getsubsidy","params":[1700000]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```json
40000000000
```

### prioritisetransaction

Safety: Advanced

Accepts the transaction into mined blocks at a higher (or lower) priority

Signature:

```text
prioritisetransaction "txid" ( dummy ) fee_delta
```

Example call:

```bash
zerohour-cli prioritisetransaction '0000000000000000000000000000000000000000000000000000000000000000' 0 1000
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"prioritisetransaction","params":["0000000000000000000000000000000000000000000000000000000000000000",0,1000]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
true              (boolean) Returns true
```

### submitblock

Safety: Advanced

Attempts to submit new block to network.
See https://en.bitcoin.it/wiki/BIP_0022 for full specification.

Signature:

```text
submitblock "hexdata" ( "dummy" )
```

Example call:

```bash
zerohour-cli submitblock 'block_hex'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"submitblock","params":["block_hex"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

### submitheader

Safety: Advanced

Decode the given hexdata as a header and submit it as a candidate chain tip if valid.
Throws when the header is invalid.

Signature:

```text
submitheader "hexdata"
```

Example call:

```bash
zerohour-cli submitheader 'header_hex'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"submitheader","params":["header_hex"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
None
```

## Full Method Index

| Method | Safety | Signature |
| --- | --- | --- |
| `getblocktemplate` | Advanced | `getblocktemplate "template_request"` |
| `getmininginfo` | Read-only | `getmininginfo` |
| `getnetworkhashps` | Read-only | `getnetworkhashps ( nblocks height )` |
| `getstakinginfo` | Read-only | `getstakinginfo` |
| `getsubsidy` | Read-only | `getsubsidy ( height endheight )` |
| `prioritisetransaction` | Advanced | `prioritisetransaction "txid" ( dummy ) fee_delta` |
| `submitblock` | Advanced | `submitblock "hexdata" ( "dummy" )` |
| `submitheader` | Advanced | `submitheader "hexdata"` |
