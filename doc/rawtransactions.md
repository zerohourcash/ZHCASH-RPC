# Rawtransactions RPC

Rawtransactions RPC methods create, decode, sign, test, and broadcast transactions. They are used by wallets, exchanges, indexers, and advanced tooling.

## Menu

* [analyzepsbt](#analyzepsbt)
* [combinepsbt](#combinepsbt)
* [combinerawtransaction](#combinerawtransaction)
* [converttopsbt](#converttopsbt)
* [createpsbt](#createpsbt)
* [createrawtransaction](#createrawtransaction)
* [decodepsbt](#decodepsbt)
* [decoderawtransaction](#decoderawtransaction)
* [decodescript](#decodescript)
* [finalizepsbt](#finalizepsbt)
* [fromhexaddress](#fromhexaddress)
* [fundrawtransaction](#fundrawtransaction)
* [gethexaddress](#gethexaddress)
* [getrawtransaction](#getrawtransaction)
* [joinpsbts](#joinpsbts)
* [sendrawtransaction](#sendrawtransaction)
* [signrawtransactionwithkey](#signrawtransactionwithkey)
* [testmempoolaccept](#testmempoolaccept)
* [utxoupdatepsbt](#utxoupdatepsbt)
* [Full Method Index](#full-method-index)

## Methods

### analyzepsbt

Safety: Read-only

Analyzes and provides information about the current status of a PSBT and its inputs

Signature:

```text
analyzepsbt "psbt"
```

Example call:

```bash
zerohour-cli analyzepsbt 'cHNidP8BAHECAAAAAAABAKCGAQAAAAAAGXapFCXO7c4hxBtTuCZtbyJO16z6BG2WiKwAAAAAAA=='
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"analyzepsbt","params":["cHNidP8BAHECAAAAAAABAKCGAQAAAAAAGXapFCXO7c4hxBtTuCZtbyJO16z6BG2WiKwAAAAAAA=="]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "inputs" : [                      (array of json objects)
    {
      "has_utxo" : true|false     (boolean) Whether a UTXO is provided
      "is_final" : true|false     (boolean) Whether the input is finalized
      "missing" : {               (json object, optional) Things that are missing that are required to complete this input
        "pubkeys" : [             (array, optional)
          "keyid"                 (string) Public key ID, hash160 of the public key, of a public key whose BIP 32 derivation path is missing
        ]
        "signatures" : [          (array, optional)
          "keyid"                 (string) Public key ID, hash160 of the public key, of a public key whose signature is missing
        ]
        "redeemscript" : "hash"   (string, optional) Hash160 of the redeemScript that is missing
        "witnessscript" : "hash"  (string, optional) SHA256 of the witnessScript that is missing
      }
      "next" : "role"             (string, optional) Role of the next person that this input needs to go to
    }
    ,...
  ]
  "estimated_vsize" : vsize       (numeric, optional) Estimated vsize of the final signed transaction
  "estimated_feerate" : feerate   (numeric, optional) Estimated feerate of the final signed transaction in ZHC/kB. Shown only if all UTXO slots in the PSBT have been filled.
  "fee" : fee                     (numeric, optional) The transaction fee paid. Shown only if all UTXO slots in the PSBT have been filled.
  "next" : "role"                 (string) Role of the next person that this psbt needs to go to
}
```

### combinepsbt

Safety: Read-only

Combine multiple partially signed ZHCASH transactions into one transaction.
Implements the Combiner role.

Signature:

```text
combinepsbt ["psbt",...]
```

Example call:

```bash
zerohour-cli combinepsbt '["cHNidP8BAHECAAAAAAABAKCGAQAAAAAAGXapFCXO7c4hxBtTuCZtbyJO16z6BG2WiKwAAAAAAA=="]'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"combinepsbt","params":[["cHNidP8BAHECAAAAAAABAKCGAQAAAAAAGXapFCXO7c4hxBtTuCZtbyJO16z6BG2WiKwAAAAAAA=="]]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
  "psbt"          (string) The base64-encoded partially signed transaction
```

### combinerawtransaction

Safety: Read-only

Combine multiple partially signed transactions into one transaction.
The combined transaction may be another partially signed transaction or a 
fully signed transaction.

Signature:

```text
combinerawtransaction ["hexstring",...]
```

Example call:

```bash
zerohour-cli combinerawtransaction '["020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000"]'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"combinerawtransaction","params":[["020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000"]]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
"hex"            (string) The hex-encoded raw transaction with signature(s)
```

### converttopsbt

Safety: Read-only

Converts a network serialized transaction to a PSBT. This should be used only with createrawtransaction and fundrawtransaction
createpsbt and walletcreatefundedpsbt should be used for new applications.

Signature:

```text
converttopsbt "hexstring" ( permitsigdata iswitness )
```

Example call:

```bash
zerohour-cli converttopsbt '020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"converttopsbt","params":["020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
  "psbt"        (string)  The resulting raw transaction (base64-encoded string)
```

### createpsbt

Safety: Read-only

Creates a transaction in the Partially Signed Transaction format.
Implements the Creator role.

Signature:

```text
createpsbt [{"txid":"hex","vout":n,"sequence":n},...] [{"address":amount},{"data":"hex"},...] ( locktime replaceable )
```

Example call:

```bash
zerohour-cli createpsbt '[]' '{}'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"createpsbt","params":[[],{}]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
  "psbt"        (string)  The resulting raw transaction (base64-encoded string)
```

### createrawtransaction

Safety: Read-only

Create a transaction spending the given inputs and creating new outputs.
Outputs can be addresses or data.
Returns hex-encoded raw transaction.
Note that the transaction's inputs are not signed, and
it is not stored in the wallet or transmitted to the network.

Signature:

```text
createrawtransaction [{"txid":"hex","vout":n,"sequence":n},...] [{"address":amount},{"data":"hex"},{"contractAddress":"hex","data":"hex","amount":amount,"gasLimit":n,"gasPrice":n},...] ( locktime replaceable )
```

Example call:

```bash
zerohour-cli createrawtransaction '[]' '{"ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr":0.001}'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"createrawtransaction","params":[[],{"ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr":0.001}]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```text
020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000
```

### decodepsbt

Safety: Read-only

Return a JSON object representing the serialized, base64-encoded partially signed ZHCASH transaction.

Signature:

```text
decodepsbt "psbt"
```

Example call:

```bash
zerohour-cli decodepsbt 'cHNidP8BAHECAAAAAAABAKCGAQAAAAAAGXapFCXO7c4hxBtTuCZtbyJO16z6BG2WiKwAAAAAAA=='
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"decodepsbt","params":["cHNidP8BAHECAAAAAAABAKCGAQAAAAAAGXapFCXO7c4hxBtTuCZtbyJO16z6BG2WiKwAAAAAAA=="]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "tx" : {                   (json object) The decoded network-serialized unsigned transaction.
    ...                                      The layout is the same as the output of decoderawtransaction.
  },
  "unknown" : {                (json object) The unknown global fields
    "key" : "value"            (key-value pair) An unknown key-value pair
     ...
  },
  "inputs" : [                 (array of json objects)
    {
      "non_witness_utxo" : {   (json object, optional) Decoded network transaction for non-witness UTXOs
        ...
      },
      "witness_utxo" : {            (json object, optional) Transaction output for witness UTXOs
        "amount" : x.xxx,           (numeric) The value in ZHC
        "scriptPubKey" : {          (json object)
          "asm" : "asm",            (string) The asm
          "hex" : "hex",            (string) The hex
          "type" : "pubkeyhash",    (string) The type, eg 'pubkeyhash'
          "address" : "address"     (string) ZHCASH address if there is one
        }
      },
      "partial_signatures" : {             (json object, optional)
        "pubkey" : "signature",           (string) The public key and signature that corresponds to it.
        ,...
      }
      "sighash" : "type",                  (string, optional) The sighash type to be used
      "redeem_script" : {       (json object, optional)
          "asm" : "asm",            (string) The asm
          "hex" : "hex",            (string) The hex
          "type" : "pubkeyhash",    (string) The type, eg 'pubkeyhash'
        }
      "witness_script" : {       (json object, optional)
          "asm" : "asm",            (string) The asm
          "hex" : "hex",            (string) The hex
          "type" : "pubkeyhash",    (string) The type, eg 'pubkeyhash'
        }
      "bip32_derivs" : {          (json object, optional)
        "pubkey" : {                     (json object, optional) The public key with the derivation path as the value.
          "master_fingerprint" : "fingerprint"     (string) The fingerprint of the master key
          "path" : "path",                         (string) The path
        }
        ,...
      }
      "final_scriptsig" : {       (json object, optional)
          "asm" : "asm",            (string) The asm
          "hex" : "hex",            (string) The hex
        }
       "final_scriptwitness": ["hex", ...] (array of string) hex-encoded witness data (if any)
      "unknown" : {                (json object) The unknown global fields
        "key" : "value"            (key-value pair) An unknown key-value pair
         ...
      },
    }
    ,...
  ]
  "outputs" : [                 (array of json objects)
    {
      "redeem_script" : {       (json object, optional)
          "asm" : "asm",            (string) The asm
          "hex" : "hex",            (string) The hex
          "type" : "pubkeyhash",    (string) The type, eg 'pubkeyhash'
        }
      "witness_script" : {       (json object, optional)
          "asm" : "asm",            (string) The asm
          "hex" : "hex",            (string) The hex
          "type" : "pubkeyhash",    (string) The type, eg 'pubkeyhash'
      }
      "bip32_derivs" : [          (array of json objects, optional)
        {
          "pubkey" : "pubkey",                     (string) The public key this path corresponds to
          "master_fingerprint" : "fingerprint"     (string) The fingerprint of the master key
          "path" : "path",                         (string) The path
          }
        }
        ,...
      ],
      "unknown" : {                (json object) The unknown global fields
        "key" : "value"            (key-value pair) An unknown key-value pair
         ...
      },
    }
    ,...
  ]
  "fee" : fee                      (numeric, optional) The transaction fee paid if all UTXOs slots in the PSBT have been filled.
}
```

### decoderawtransaction

Safety: Read-only

Return a JSON object representing the serialized, hex-encoded transaction.

Signature:

```text
decoderawtransaction "hexstring" ( iswitness )
```

Example call:

```bash
zerohour-cli decoderawtransaction '020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"decoderawtransaction","params":["020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```json
{
  "txid": "64ac03cb9afb605f483ded4c2c0cb1d861ee74257143a4fc971ebb17661b5971",
  "hash": "64ac03cb9afb605f483ded4c2c0cb1d861ee74257143a4fc971ebb17661b5971",
  "version": 2,
  "size": 44,
  "vsize": 44,
  "weight": 176,
  "locktime": 0,
  "vin": [
  ],
  "vout": [
    {
      "value": 0.00100000,
      "n": 0,
      "scriptPubKey": {
        "asm": "OP_DUP OP_HASH160 25ceedce21c41b53b8266d6f224ed7acfa046d96 OP_EQUALVERIFY OP_CHECKSIG",
        "hex": "76a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac",
        "reqSigs": 1,
        "type": "pubkeyhash",
        "addresses": [
          "ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr"
        ]
      }
    }
  ]
}
```

### decodescript

Safety: Read-only

Decode a hex-encoded script.

Signature:

```text
decodescript "hexstring"
```

Example call:

```bash
zerohour-cli decodescript '76a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"decodescript","params":["76a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "asm":"asm",   (string) Script public key
  "hex":"hex",   (string) hex-encoded public key
  "type":"type", (string) The output type
  "reqSigs": n,    (numeric) The required signatures
  "addresses": [   (json array of string)
     "address"     (string) zerohour address
     ,...
  ],
  "p2sh","address" (string) address of P2SH script wrapping this redeem script (not returned if the script is already a P2SH).
}
```

### finalizepsbt

Safety: Read-only

Finalize the inputs of a PSBT. If the transaction is fully signed, it will produce a
network serialized transaction which can be broadcast with sendrawtransaction. Otherwise a PSBT will be
created which has the final_scriptSig and final_scriptWitness fields filled for inputs that are complete.
Implements the Finalizer and Extractor roles.

Signature:

```text
finalizepsbt "psbt" ( extract )
```

Example call:

```bash
zerohour-cli finalizepsbt 'cHNidP8BAHECAAAAAAABAKCGAQAAAAAAGXapFCXO7c4hxBtTuCZtbyJO16z6BG2WiKwAAAAAAA=='
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"finalizepsbt","params":["cHNidP8BAHECAAAAAAABAKCGAQAAAAAAGXapFCXO7c4hxBtTuCZtbyJO16z6BG2WiKwAAAAAAA=="]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "psbt" : "value",          (string) The base64-encoded partially signed transaction if not extracted
  "hex" : "value",           (string) The hex-encoded network transaction if extracted
  "complete" : true|false,   (boolean) If the transaction has a complete set of signatures
  ]
}
```

### fromhexaddress

Safety: Read-only

Converts a raw hex address to a base58 pubkeyhash address

Signature:

```text
fromhexaddress "hexaddress"
```

Example call:

```bash
zerohour-cli fromhexaddress '25ceedce21c41b53b8266d6f224ed7acfa046d96'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"fromhexaddress","params":["25ceedce21c41b53b8266d6f224ed7acfa046d96"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```text
ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr
```

### fundrawtransaction

Safety: Wallet

Add inputs to a transaction until it has enough in value to meet its out value.
This will not modify existing inputs, and will add at most one change output to the outputs.
No existing outputs will be modified unless "subtractFeeFromOutputs" is specified.
Note that inputs which were signed may need to be resigned after completion since in/outputs have been added.
The inputs added will not be signed, use signrawtransactionwithkey
 or signrawtransactionwithwallet for that.
Note that all existing inputs must have their previous output transaction be in the wallet.
Note that all inputs selected must be of standard form and P2SH scripts must be
in the wallet using importaddress or addmultisigaddress (to calculate fees).
You can see whether this is the case by checking the "solvable" field in the listunspent output.
Only pay-to-pubkey, multisig, and P2SH versions thereof are currently supported for watch-only

Signature:

```text
fundrawtransaction "hexstring" ( options iswitness )
```

Example call:

```bash
zerohour-cli fundrawtransaction '020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"fundrawtransaction","params":["020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "hex":       "value", (string)  The resulting raw transaction (hex-encoded string)
  "fee":       n,         (numeric) Fee in ZHC the resulting transaction pays
  "changepos": n          (numeric) The position of the added change output, or -1
}
```

### gethexaddress

Safety: Read-only

Converts a base58 pubkeyhash address to a hex address for use in smart contracts.

Signature:

```text
gethexaddress "address"
```

Example call:

```bash
zerohour-cli gethexaddress 'ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"gethexaddress","params":["ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```text
25ceedce21c41b53b8266d6f224ed7acfa046d96
```

### getrawtransaction

Safety: Read-only

Return the raw transaction data.

By default this function only works for mempool transactions. When called with a blockhash
argument, getrawtransaction will return the transaction if the specified block is available and
the transaction is found in that block. When called without a blockhash argument, getrawtransaction
will return the transaction if it is in the mempool, or if -txindex is enabled and the transaction
is in a block in the blockchain.

Hint: Use gettransaction for wallet transactions.

If verbose is 'true', returns an Object with information about 'txid'.
If verbose is 'false' or omitted, returns a string that is serialized, hex-encoded data for 'txid'.

Signature:

```text
getrawtransaction "txid" ( verbose "blockhash" )
```

Example call:

```bash
zerohour-cli getrawtransaction '0000000000000000000000000000000000000000000000000000000000000000' true
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getrawtransaction","params":["0000000000000000000000000000000000000000000000000000000000000000",true]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result (if verbose is not set or set to false):
"data"      (string) The serialized, hex-encoded data for 'txid'

Result (if verbose is set to true):
{
  "in_active_chain": b, (bool) Whether specified block is in the active chain or not (only present with explicit "blockhash" argument)
  "hex" : "data",       (string) The serialized, hex-encoded data for 'txid'
  "txid" : "id",        (string) The transaction id (same as provided)
  "hash" : "id",        (string) The transaction hash (differs from txid for witness transactions)
  "size" : n,             (numeric) The serialized transaction size
  "vsize" : n,            (numeric) The virtual transaction size (differs from size for witness transactions)
  "weight" : n,           (numeric) The transaction's weight (between vsize*4-3 and vsize*4)
  "version" : n,          (numeric) The version
  "locktime" : ttt,       (numeric) The lock time
  "vin" : [               (array of json objects)
     {
       "txid": "id",    (string) The transaction id
       "vout": n,         (numeric) 
       "scriptSig": {     (json object) The script
         "asm": "asm",  (string) asm
         "hex": "hex"   (string) hex
       },
       "sequence": n      (numeric) The script sequence number
       "txinwitness": ["hex", ...] (array of string) hex-encoded witness data (if any)
     }
     ,...
  ],
  "vout" : [              (array of json objects)
     {
       "value" : x.xxx,            (numeric) The value in ZHC
       "n" : n,                    (numeric) index
       "scriptPubKey" : {          (json object)
         "asm" : "asm",          (string) the asm
         "hex" : "hex",          (string) the hex
         "reqSigs" : n,            (numeric) The required sigs
         "type" : "pubkeyhash",  (string) The type, eg 'pubkeyhash'
         "addresses" : [           (json array of string)
           "address"        (string) zerohour address
           ,...
         ]
       }
     }
     ,...
  ],
  "blockhash" : "hash",   (string) the block hash
  "confirmations" : n,      (numeric) The confirmations
  "blocktime" : ttt         (numeric) The block time in seconds since epoch (Jan 1 1970 GMT)
  "time" : ttt,             (numeric) Same as "blocktime"
}
```

### joinpsbts

Safety: Read-only

Joins multiple distinct PSBTs with different inputs and outputs into one PSBT with inputs and outputs from all of the PSBTs
No input in any of the PSBTs can be in more than one of the PSBTs.

Signature:

```text
joinpsbts ["psbt",...]
```

Example call:

```bash
zerohour-cli joinpsbts '["cHNidP8BAHECAAAAAAABAKCGAQAAAAAAGXapFCXO7c4hxBtTuCZtbyJO16z6BG2WiKwAAAAAAA=="]'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"joinpsbts","params":[["cHNidP8BAHECAAAAAAABAKCGAQAAAAAAGXapFCXO7c4hxBtTuCZtbyJO16z6BG2WiKwAAAAAAA=="]]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
  "psbt"          (string) The base64-encoded partially signed transaction
```

### sendrawtransaction

Safety: Broadcast

Submits raw transaction (serialized, hex-encoded) to local node and network.

Also see createrawtransaction and signrawtransactionwithkey calls.

Signature:

```text
sendrawtransaction "hexstring" ( allowhighfees )
```

Example call:

```bash
zerohour-cli sendrawtransaction '020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"sendrawtransaction","params":["020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
"hex"             (string) The transaction hash in hex
```

Broadcast method: do not run this on mainnet unless you intend to create or publish a real transaction.

### signrawtransactionwithkey

Safety: Sensitive

Sign inputs for raw transaction (serialized, hex-encoded).
The second argument is an array of base58-encoded private
keys that will be the only keys used to sign the transaction.
The third optional argument (may be null) is an array of previous transaction outputs that
this transaction depends on but may not yet be in the block chain.

Signature:

```text
signrawtransactionwithkey "hexstring" ["privatekey",...] ( [{"txid":"hex","vout":n,"scriptPubKey":"hex","redeemScript":"hex","witnessScript":"hex","amount":amount},...] "sighashtype" )
```

Example call:

```bash
zerohour-cli signrawtransactionwithkey '020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000' '[]'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"signrawtransactionwithkey","params":["020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000",[]]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```json
{
  "hex": "020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000",
  "complete": true
}
```

Sensitive method: do not expose private keys, passphrases, wallet dumps, or backup paths in logs or screenshots.

### testmempoolaccept

Safety: Read-only

Returns result of mempool acceptance tests indicating if raw transaction (serialized, hex-encoded) would be accepted by mempool.

This checks if the transaction violates the consensus or policy rules.

See sendrawtransaction call.

Signature:

```text
testmempoolaccept ["rawtx",...] ( allowhighfees )
```

Example call:

```bash
zerohour-cli testmempoolaccept '["020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000"]'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"testmempoolaccept","params":[["020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000"]]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
[                   (array) The result of the mempool acceptance test for each raw transaction in the input array.
                            Length is exactly one for now.
 {
  "txid"           (string) The transaction hash in hex
  "allowed"        (boolean) If the mempool allows this tx to be inserted
  "reject-reason"  (string) Rejection string (only present when 'allowed' is false)
 }
]
```

### utxoupdatepsbt

Safety: Read-only

Updates a PSBT with witness UTXOs retrieved from the UTXO set or the mempool.

Signature:

```text
utxoupdatepsbt "psbt"
```

Example call:

```bash
zerohour-cli utxoupdatepsbt 'cHNidP8BAHECAAAAAAABAKCGAQAAAAAAGXapFCXO7c4hxBtTuCZtbyJO16z6BG2WiKwAAAAAAA=='
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"utxoupdatepsbt","params":["cHNidP8BAHECAAAAAAABAKCGAQAAAAAAGXapFCXO7c4hxBtTuCZtbyJO16z6BG2WiKwAAAAAAA=="]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
  "psbt"          (string) The base64-encoded partially signed transaction with inputs updated
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
| `testmempoolaccept` | Read-only | `testmempoolaccept ["rawtx",...] ( allowhighfees )` |
| `utxoupdatepsbt` | Read-only | `utxoupdatepsbt "psbt"` |
