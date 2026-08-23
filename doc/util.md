# Util RPC

Util RPC methods validate addresses and messages, estimate fees, and work with descriptors or multisig scripts.

## Menu

* [createmultisig](#createmultisig)
* [deriveaddresses](#deriveaddresses)
* [estimatesmartfee](#estimatesmartfee)
* [getdescriptorinfo](#getdescriptorinfo)
* [signmessagewithprivkey](#signmessagewithprivkey)
* [validateaddress](#validateaddress)
* [verifymessage](#verifymessage)
* [Full Method Index](#full-method-index)

## Methods

### createmultisig

Safety: Read-only

Creates a multi-signature address with n signature of m keys required.
It returns a json object with the address and redeemScript.

Signature:

```text
createmultisig nrequired ["key",...] ( "address_type" )
```

Example call:

```bash
zerohour-cli createmultisig 1 '["public_key_hex"]'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"createmultisig","params":[1,["public_key_hex"]]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "address":"multisigaddress",  (string) The value of the new multisig address.
  "redeemScript":"script"       (string) The string value of the hex-encoded redemption script.
}
```

### deriveaddresses

Safety: Read-only

Derives one or more addresses corresponding to an output descriptor.
Examples of output descriptors are:
    pkh(<pubkey>)                        P2PKH outputs for the given pubkey
    wpkh(<pubkey>)                       Native segwit P2PKH outputs for the given pubkey
    sh(multi(<n>,<pubkey>,<pubkey>,...)) P2SH-multisig outputs for the given threshold and pubkeys
    raw(<hex script>)                    Outputs whose scriptPubKey equals the specified hex scripts

In the above, <pubkey> either refers to a fixed public key in hexadecimal notation, or to an xpub/xprv optionally followed by one
or more path elements separated by "/", where "h" represents a hardened child key.
For more information on output descriptors, see the documentation in the doc/descriptors.md file.

Signature:

```text
deriveaddresses "descriptor" ( range )
```

Example call:

```bash
zerohour-cli deriveaddresses 'addr(ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr)'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"deriveaddresses","params":["addr(ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr)"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
[ address ] (array) the derived addresses
```

### estimatesmartfee

Safety: Read-only

Estimates the approximate fee per kilobyte needed for a transaction to begin
confirmation within conf_target blocks if possible and return the number of blocks
for which the estimate is valid. Uses virtual transaction size as defined
in BIP 141 (witness data is discounted).

Signature:

```text
estimatesmartfee conf_target ( "estimate_mode" )
```

Example call:

```bash
zerohour-cli estimatesmartfee 6
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"estimatesmartfee","params":[6]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "feerate" : x.x,     (numeric, optional) estimate fee-per-kilobyte (in ZHC)
  "errors": [ str... ] (json array of strings, optional) Errors encountered during processing
  "blocks" : n         (numeric) block number where estimate was found
}

The request target will be clamped between 2 and the highest target
fee estimation is able to return based on how long it has been running.
An error is returned if not enough transactions and blocks
have been observed to make an estimate for any number of blocks.
```

### getdescriptorinfo

Safety: Read-only

Analyses a descriptor.

Signature:

```text
getdescriptorinfo "descriptor"
```

Example call:

```bash
zerohour-cli getdescriptorinfo 'addr(ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr)'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getdescriptorinfo","params":["addr(ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr)"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "descriptor" : "desc",         (string) The descriptor in canonical form, without private keys
  "isrange" : true|false,        (boolean) Whether the descriptor is ranged
  "issolvable" : true|false,     (boolean) Whether the descriptor is solvable
  "hasprivatekeys" : true|false, (boolean) Whether the input descriptor contained at least one private key
}
```

### signmessagewithprivkey

Safety: Sensitive

Sign a message with the private key of an address

Signature:

```text
signmessagewithprivkey "privkey" "message"
```

Example call:

```bash
zerohour-cli signmessagewithprivkey 'private_key' 'hello'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"signmessagewithprivkey","params":["private_key","hello"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
"signature"          (string) The signature of the message encoded in base 64
```

Sensitive method: do not expose private keys, passphrases, wallet dumps, or backup paths in logs or screenshots.

### validateaddress

Safety: Read-only

Return information about the given zerohour address.

Signature:

```text
validateaddress "address"
```

Example call:

```bash
zerohour-cli validateaddress 'ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"validateaddress","params":["ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```json
{
  "isvalid": true,
  "address": "ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr",
  "scriptPubKey": "76a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac",
  "isscript": false,
  "iswitness": false
}
```

### verifymessage

Safety: Read-only

Verify a signed message

Signature:

```text
verifymessage "address" "signature" "message"
```

Example call:

```bash
zerohour-cli verifymessage 'ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr' 'signature' 'hello'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"verifymessage","params":["ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr","signature","hello"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
true|false   (boolean) If the signature is verified or not.
```

## Full Method Index

| Method | Safety | Signature |
| --- | --- | --- |
| `createmultisig` | Read-only | `createmultisig nrequired ["key",...] ( "address_type" )` |
| `deriveaddresses` | Read-only | `deriveaddresses "descriptor" ( range )` |
| `estimatesmartfee` | Read-only | `estimatesmartfee conf_target ( "estimate_mode" )` |
| `getdescriptorinfo` | Read-only | `getdescriptorinfo "descriptor"` |
| `signmessagewithprivkey` | Sensitive | `signmessagewithprivkey "privkey" "message"` |
| `validateaddress` | Read-only | `validateaddress "address"` |
| `verifymessage` | Read-only | `verifymessage "address" "signature" "message"` |
