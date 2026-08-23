# Wallet RPC

Wallet RPC methods read balances, create addresses, send funds, manage labels, import/export keys, lock or unlock the wallet, and create wallet-funded contract transactions.

## Menu

* [abandontransaction](#abandontransaction)
* [abortrescan](#abortrescan)
* [addmultisigaddress](#addmultisigaddress)
* [backupwallet](#backupwallet)
* [bumpfee](#bumpfee)
* [createcontract](#createcontract)
* [createwallet](#createwallet)
* [dumpprivkey](#dumpprivkey)
* [dumpwallet](#dumpwallet)
* [encryptwallet](#encryptwallet)
* [getaddressesbylabel](#getaddressesbylabel)
* [getaddressinfo](#getaddressinfo)
* [getbalance](#getbalance)
* [getnewaddress](#getnewaddress)
* [getrawchangeaddress](#getrawchangeaddress)
* [getreceivedbyaddress](#getreceivedbyaddress)
* [getreceivedbylabel](#getreceivedbylabel)
* [gettransaction](#gettransaction)
* [getunconfirmedbalance](#getunconfirmedbalance)
* [getwalletinfo](#getwalletinfo)
* [importaddress](#importaddress)
* [importmulti](#importmulti)
* [importprivkey](#importprivkey)
* [importprunedfunds](#importprunedfunds)
* [importpubkey](#importpubkey)
* [importwallet](#importwallet)
* [keypoolrefill](#keypoolrefill)
* [listaddressgroupings](#listaddressgroupings)
* [listlabels](#listlabels)
* [listlockunspent](#listlockunspent)
* [listreceivedbyaddress](#listreceivedbyaddress)
* [listreceivedbylabel](#listreceivedbylabel)
* [listsinceblock](#listsinceblock)
* [listtransactions](#listtransactions)
* [listunspent](#listunspent)
* [listwalletdir](#listwalletdir)
* [listwallets](#listwallets)
* [loadwallet](#loadwallet)
* [lockunspent](#lockunspent)
* [removeprunedfunds](#removeprunedfunds)
* [rescanblockchain](#rescanblockchain)
* [reservebalance](#reservebalance)
* [sendmany](#sendmany)
* [sendmanywithdupes](#sendmanywithdupes)
* [sendtoaddress](#sendtoaddress)
* [sendtocontract](#sendtocontract)
* [sethdseed](#sethdseed)
* [setlabel](#setlabel)
* [settxfee](#settxfee)
* [signmessage](#signmessage)
* [signrawtransactionwithwallet](#signrawtransactionwithwallet)
* [unloadwallet](#unloadwallet)
* [walletcreatefundedpsbt](#walletcreatefundedpsbt)
* [walletlock](#walletlock)
* [walletpassphrase](#walletpassphrase)
* [walletpassphrasechange](#walletpassphrasechange)
* [walletprocesspsbt](#walletprocesspsbt)
* [Full Method Index](#full-method-index)

## Methods

### abandontransaction

Safety: Wallet

Mark in-wallet transaction <txid> as abandoned
This will mark this transaction and all its in-wallet descendants as abandoned which will allow
for their inputs to be respent.  It can be used to replace "stuck" or evicted transactions.
It only works on transactions which are not included in a block and are not currently in the mempool.
It has no effect on transactions which are already abandoned.

Signature:

```text
abandontransaction "txid"
```

Example call:

```bash
zerohour-cli abandontransaction
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"abandontransaction","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

### abortrescan

Safety: Wallet

Stops current wallet rescan triggered by an RPC call, e.g. by an importprivkey call.

Signature:

```text
abortrescan
```

Example call:

```bash
zerohour-cli abortrescan
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"abortrescan","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

### addmultisigaddress

Safety: Wallet

Add a nrequired-to-sign multisignature address to the wallet. Requires a new wallet backup.
Each key is a ZHCASH address or hex-encoded public key.
This functionality is only intended for use with non-watchonly addresses.
See `importaddress` for watchonly p2sh address support.
If 'label' is specified, assign address to that label.

Signature:

```text
addmultisigaddress nrequired ["key",...] ( "label" "address_type" )
```

Example call:

```bash
zerohour-cli addmultisigaddress 1 '["public_key_hex"]' 'label'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"addmultisigaddress","params":[1,["public_key_hex"],"label"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "address":"multisigaddress",    (string) The value of the new multisig address.
  "redeemScript":"script"         (string) The string value of the hex-encoded redemption script.
}
```

### backupwallet

Safety: Sensitive

Safely copies current wallet file to destination, which can be a directory or a path with filename.

Signature:

```text
backupwallet "destination"
```

Example call:

```bash
zerohour-cli backupwallet '/safe/backup/wallet.dat'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"backupwallet","params":["/safe/backup/wallet.dat"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

Sensitive method: do not expose private keys, passphrases, wallet dumps, or backup paths in logs or screenshots.

### bumpfee

Safety: Broadcast

Bumps the fee of an opt-in-RBF transaction T, replacing it with a new transaction B.
An opt-in RBF transaction with the given txid must be in the wallet.
The command will pay the additional fee by decreasing (or perhaps removing) its change output.
If the change output is not big enough to cover the increased fee, the command will currently fail
instead of adding new inputs to compensate. (A future implementation could improve this.)
The command will fail if the wallet or mempool contains a transaction that spends one of T's outputs.
By default, the new fee will be calculated automatically using estimatesmartfee.
The user can specify a confirmation target for estimatesmartfee.
Alternatively, the user can specify totalFee, or use RPC settxfee to set a higher fee rate.
At a minimum, the new fee rate must be high enough to pay an additional new relay fee (incrementalfee
returned by getnetworkinfo) to enter the node's mempool.

Signature:

```text
bumpfee "txid" ( options )
```

Example call:

```bash
zerohour-cli bumpfee '0000000000000000000000000000000000000000000000000000000000000000'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"bumpfee","params":["0000000000000000000000000000000000000000000000000000000000000000"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "txid":    "value",   (string)  The id of the new transaction
  "origfee":  n,         (numeric) Fee of the replaced transaction
  "fee":      n,         (numeric) Fee of the new transaction
  "errors":  [ str... ] (json array of strings) Errors encountered during processing (may be empty)
}
```

Broadcast method: do not run this on mainnet unless you intend to create or publish a real transaction.

### createcontract

Safety: Broadcast

Create a contract with bytcode.

Signature:

```text
createcontract "bytecode" ( gasLimit gasPrice "senderaddress" broadcast changeToSender )
```

Example call:

```bash
zerohour-cli createcontract 'bytecode_hex' 2500000 4e-7
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"createcontract","params":["bytecode_hex",2500000,4e-7]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
[
  {
    "txid" : (string) The transaction id.
    "sender" : (string) ZHC address of the sender.
    "hash160" : (string) ripemd-160 hash of the sender.
    "address" : (string) expected contract address.
  }
]
```

Broadcast method: do not run this on mainnet unless you intend to create or publish a real transaction.

### createwallet

Safety: Wallet

Creates and loads a new wallet.

Signature:

```text
createwallet "wallet_name" ( disable_private_keys blank )
```

Example call:

```bash
zerohour-cli createwallet 'dev-wallet'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"createwallet","params":["dev-wallet"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "name" :    <wallet_name>,        (string) The wallet name if created successfully. If the wallet was created using a full path, the wallet_name will be the full path.
  "warning" : <warning>,            (string) Warning message if wallet was not loaded cleanly.
}
```

### dumpprivkey

Safety: Sensitive

Reveals the private key corresponding to 'address'.
Then the importprivkey can be used with this output

Signature:

```text
dumpprivkey "address"
```

Example call:

```bash
zerohour-cli dumpprivkey 'ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"dumpprivkey","params":["ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
"key"                (string) The private key
```

Sensitive method: do not expose private keys, passphrases, wallet dumps, or backup paths in logs or screenshots.

### dumpwallet

Safety: Sensitive

Dumps all wallet keys in a human-readable format to a server-side file. This does not allow overwriting existing files.

Signature:

```text
dumpwallet "filename"
```

Example call:

```bash
zerohour-cli dumpwallet '/safe/backup/wallet.txt'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"dumpwallet","params":["/safe/backup/wallet.txt"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{                           (json object)
  "filename" : {        (string) The filename with full absolute path
}
```

Sensitive method: do not expose private keys, passphrases, wallet dumps, or backup paths in logs or screenshots.

### encryptwallet

Safety: Sensitive

Encrypts the wallet with 'passphrase'. This is for first time encryption.
After this, any calls that interact with private keys such as sending or signing 
will require the passphrase to be set prior the making these calls.
Use the walletpassphrase call for this, and then walletlock call.
If the wallet is already encrypted, use the walletpassphrasechange call.

Signature:

```text
encryptwallet "passphrase"
```

Example call:

```bash
zerohour-cli encryptwallet 'strong_passphrase'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"encryptwallet","params":["strong_passphrase"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

Sensitive method: do not expose private keys, passphrases, wallet dumps, or backup paths in logs or screenshots.

### getaddressesbylabel

Safety: Wallet

Returns the list of addresses assigned the specified label.

Signature:

```text
getaddressesbylabel "label"
```

Example call:

```bash
zerohour-cli getaddressesbylabel 'label'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getaddressesbylabel","params":["label"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{ (json object with addresses as keys)
  "address": { (json object with information about address)
    "purpose": "string" (string)  Purpose of address ("send" for sending address, "receive" for receiving address)
  },...
}
```

### getaddressinfo

Safety: Wallet

Return information about the given zerohour address. Some information requires the address
to be in the wallet.

Signature:

```text
getaddressinfo "address"
```

Example call:

```bash
zerohour-cli getaddressinfo 'ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getaddressinfo","params":["ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "address" : "address",        (string) The zerohour address validated
  "scriptPubKey" : "hex",       (string) The hex-encoded scriptPubKey generated by the address
  "ismine" : true|false,        (boolean) If the address is yours or not
  "iswatchonly" : true|false,   (boolean) If the address is watchonly
  "solvable" : true|false,      (boolean) Whether we know how to spend coins sent to this address, ignoring the possible lack of private keys
  "desc" : "desc",            (string, optional) A descriptor for spending coins sent to this address (only when solvable)
  "isscript" : true|false,      (boolean) If the key is a script
  "ischange" : true|false,      (boolean) If the address was used for change output
  "iswitness" : true|false,     (boolean) If the address is a witness address
  "witness_version" : version   (numeric, optional) The version number of the witness program
  "witness_program" : "hex"     (string, optional) The hex value of the witness program
  "script" : "type"             (string, optional) The output script type. Only if "isscript" is true and the redeemscript is known. Possible types: nonstandard, pubkey, pubkeyhash, scripthash, multisig, nulldata, witness_v0_keyhash, witness_v0_scripthash, witness_unknown
  "hex" : "hex",                (string, optional) The redeemscript for the p2sh address
  "pubkeys"                     (string, optional) Array of pubkeys associated with the known redeemscript (only if "script" is "multisig")
    [
      "pubkey"
      ,...
    ]
  "sigsrequired" : xxxxx        (numeric, optional) Number of signatures required to spend multisig output (only if "script" is "multisig")
  "pubkey" : "publickeyhex",    (string, optional) The hex value of the raw public key, for single-key addresses (possibly embedded in P2SH or P2WSH)
  "embedded" : {...},           (object, optional) Information about the address embedded in P2SH or P2WSH, if relevant and known. It includes all getaddressinfo output fields for the embedded address, excluding metadata ("timestamp", "hdkeypath", "hdseedid") and relation to the wallet ("ismine", "iswatchonly").
  "iscompressed" : true|false,  (boolean, optional) If the pubkey is compressed
  "label" :  "label"         (string) The label associated with the address, "" is the default label
  "timestamp" : timestamp,      (number, optional) The creation time of the key if available in seconds since epoch (Jan 1 1970 GMT)
  "hdkeypath" : "keypath"       (string, optional) The HD keypath if the key is HD and available
  "hdseedid" : "<hash160>"      (string, optional) The Hash160 of the HD seed
  "hdmasterfingerprint" : "<hash160>" (string, optional) The fingperint of the master key.
  "labels"                      (object) Array of labels associated with the address.
    [
      { (json object of label data)
        "name": "labelname" (string) The label
        "purpose": "string" (string) Purpose of address ("send" for sending address, "receive" for receiving address)
      },...
    ]
}
```

### getbalance

Safety: Wallet

Returns the total available balance.
The available balance is what the wallet considers currently spendable, and is
thus affected by options which limit spendability such as -spendzeroconfchange.

Signature:

```text
getbalance ( "dummy" minconf include_watchonly )
```

Example call:

```bash
zerohour-cli getbalance
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getbalance","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
amount              (numeric) The total amount in ZHC received for this wallet.
```

### getnewaddress

Safety: Wallet

Returns a new ZHCASH address for receiving payments.
If 'label' is specified, it is added to the address book 
so payments received with the address will be associated with 'label'.

Signature:

```text
getnewaddress ( "label" "address_type" )
```

Example call:

```bash
zerohour-cli getnewaddress 'label'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getnewaddress","params":["label"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
"address"    (string) The new zerohour address
```

### getrawchangeaddress

Safety: Wallet

Returns a new ZHCASH address, for receiving change.
This is for use with raw transactions, NOT normal use.

Signature:

```text
getrawchangeaddress ( "address_type" )
```

Example call:

```bash
zerohour-cli getrawchangeaddress
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getrawchangeaddress","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
"address"    (string) The address
```

### getreceivedbyaddress

Safety: Wallet

Returns the total amount received by the given address in transactions with at least minconf confirmations.

Signature:

```text
getreceivedbyaddress "address" ( minconf )
```

Example call:

```bash
zerohour-cli getreceivedbyaddress 'ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getreceivedbyaddress","params":["ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
amount   (numeric) The total amount in ZHC received at this address.
```

### getreceivedbylabel

Safety: Wallet

Returns the total amount received by addresses with <label> in transactions with at least [minconf] confirmations.

Signature:

```text
getreceivedbylabel "label" ( minconf )
```

Example call:

```bash
zerohour-cli getreceivedbylabel 'label'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getreceivedbylabel","params":["label"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
amount              (numeric) The total amount in ZHC received for this label.
```

### gettransaction

Safety: Wallet

Get detailed information about in-wallet transaction <txid>

Signature:

```text
gettransaction "txid" ( include_watchonly waitconf )
```

Example call:

```bash
zerohour-cli gettransaction '0000000000000000000000000000000000000000000000000000000000000000'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"gettransaction","params":["0000000000000000000000000000000000000000000000000000000000000000"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "amount" : x.xxx,        (numeric) The transaction amount in ZHC
  "fee": x.xxx,            (numeric) The amount of the fee in ZHC. This is negative and only available for the 
                              'send' category of transactions.
  "confirmations" : n,     (numeric) The number of confirmations
  "blockhash" : "hash",  (string) The block hash
  "blockindex" : xx,       (numeric) The index of the transaction in the block that includes it
  "blocktime" : ttt,       (numeric) The time in seconds since epoch (1 Jan 1970 GMT)
  "txid" : "transactionid",   (string) The transaction id.
  "time" : ttt,            (numeric) The transaction time in seconds since epoch (1 Jan 1970 GMT)
  "timereceived" : ttt,    (numeric) The time received in seconds since epoch (1 Jan 1970 GMT)
  "bip125-replaceable": "yes|no|unknown",  (string) Whether this transaction could be replaced due to BIP125 (replace-by-fee);
                                                   may be unknown for unconfirmed transactions not in the mempool
  "details" : [
    {
      "address" : "address",          (string) The zerohour address involved in the transaction
      "category" :                      (string) The transaction category.
                   "send"                  Transactions sent.
                   "receive"               Non-coinbase transactions received.
                   "generate"              Coinbase transactions received with more than 100 confirmations.
                   "immature"              Coinbase transactions received with 100 or fewer confirmations.
                   "orphan"                Orphaned coinbase transactions received.
      "amount" : x.xxx,                 (numeric) The amount in ZHC
      "label" : "label",              (string) A comment for the address/transaction, if any
      "vout" : n,                       (numeric) the vout value
      "fee": x.xxx,                     (numeric) The amount of the fee in ZHC. This is negative and only available for the 
                                           'send' category of transactions.
      "abandoned": xxx                  (bool) 'true' if the transaction has been abandoned (inputs are respendable). Only available for the 
                                           'send' category of transactions.
    }
    ,...
  ],
  "hex" : "data"         (string) Raw data for transaction
}
```

### getunconfirmedbalance

Safety: Wallet

Returns the server's total unconfirmed balance

Signature:

```text
getunconfirmedbalance
```

Example call:

```bash
zerohour-cli getunconfirmedbalance
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getunconfirmedbalance","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

### getwalletinfo

Safety: Wallet

Returns an object containing various wallet state info.

Signature:

```text
getwalletinfo
```

Example call:

```bash
zerohour-cli getwalletinfo
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getwalletinfo","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "walletname": xxxxx,               (string) the wallet name
  "walletversion": xxxxx,            (numeric) the wallet version
  "balance": xxxxxxx,                (numeric) the total confirmed balance of the wallet in ZHC
  "stake": xxxxxxx,                  (numeric) the total stake balance of the wallet in ZHC
  "unconfirmed_balance": xxx,        (numeric) the total unconfirmed balance of the wallet in ZHC
  "immature_balance": xxxxxx,        (numeric) the total immature balance of the wallet in ZHC
  "txcount": xxxxxxx,                (numeric) the total number of transactions in the wallet
  "keypoololdest": xxxxxx,           (numeric) the timestamp (seconds since Unix epoch) of the oldest pre-generated key in the key pool
  "keypoolsize": xxxx,               (numeric) how many new keys are pre-generated (only counts external keys)
  "keypoolsize_hd_internal": xxxx,   (numeric) how many new keys are pre-generated for internal use (used for change outputs, only appears if the wallet is using this feature, otherwise external keys are used)
  "unlocked_until": ttt,             (numeric) the timestamp in seconds since epoch (midnight Jan 1 1970 GMT) that the wallet is unlocked for transfers, or 0 if the wallet is locked
  "paytxfee": x.xxxx,                (numeric) the transaction fee configuration, set in ZHC/kB
  "hdseedid": "<hash160>"            (string, optional) the Hash160 of the HD seed (only present when HD is enabled)
  "private_keys_enabled": true|false (boolean) false if privatekeys are disabled for this wallet (enforced watch-only wallet)
}
```

### importaddress

Safety: Wallet

Adds an address or script (in hex) that can be watched as if it were in your wallet but cannot be used to spend. Requires a new wallet backup.

Note: This call can take over an hour to complete if rescan is true, during that time, other rpc calls
may report that the imported address exists but related transactions are still missing, leading to temporarily incorrect/bogus balances and unspent outputs until rescan completes.
If you have the full public key, you should call importpubkey instead of this.

Note: If you import a non-standard raw script in hex form, outputs sending to it will be treated
as change, and not show up in many RPCs.

Signature:

```text
importaddress "address" ( "label" rescan p2sh )
```

Example call:

```bash
zerohour-cli importaddress 'ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr' 'watch-only' false
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"importaddress","params":["ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr","watch-only",false]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

### importmulti

Safety: Wallet

Import addresses/scripts (with private or public keys, redeem script (P2SH)), optionally rescanning the blockchain from the earliest creation time of the imported scripts. Requires a new wallet backup.
If an address/script is imported without all of the private keys required to spend from that address, it will be watchonly. The 'watchonly' option must be set to true in this case or a warning will be returned.
Conversely, if all the private keys are provided and the address/script is spendable, the watchonly option must be set to false, or a warning will be returned.

Note: This call can take over an hour to complete if rescan is true, during that time, other rpc calls
may report that the imported keys, addresses or scripts exists but related transactions are still missing.

Signature:

```text
importmulti "requests" ( "options" )
```

Example call:

```bash
zerohour-cli importmulti '[{"scriptPubKey":{"address":"ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr"},"timestamp":"now"}]' '{"rescan":false}'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"importmulti","params":[[{"scriptPubKey":{"address":"ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr"},"timestamp":"now"}],{"rescan":false}]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:

Response is an array with the same size as the input that has the execution result :
  [{"success": true}, {"success": true, "warnings": ["Ignoring irrelevant private key"]}, {"success": false, "error": {"code": -1, "message": "Internal Server Error"}}, ...]
```

### importprivkey

Safety: Sensitive

Adds a private key (as returned by dumpprivkey) to your wallet. Requires a new wallet backup.
Hint: use importmulti to import more than one private key.

Note: This call can take over an hour to complete if rescan is true, during that time, other rpc calls
may report that the imported key exists but related transactions are still missing, leading to temporarily incorrect/bogus balances and unspent outputs until rescan completes.

Signature:

```text
importprivkey "zerohourprivkey" ( "label" rescan )
```

Example call:

```bash
zerohour-cli importprivkey 'private_key' 'label' false
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"importprivkey","params":["private_key","label",false]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

Sensitive method: do not expose private keys, passphrases, wallet dumps, or backup paths in logs or screenshots.

### importprunedfunds

Safety: Wallet

Imports funds without rescan. Corresponding address or script must previously be included in wallet. Aimed towards pruned wallets. The end-user is responsible to import additional transactions that subsequently spend the imported outputs or rescan after the point in the blockchain the transaction is included.

Signature:

```text
importprunedfunds "rawtransaction" "txoutproof"
```

Example call:

```bash
zerohour-cli importprunedfunds '020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000' 'txoutproof_hex'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"importprunedfunds","params":["020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000","txoutproof_hex"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

### importpubkey

Safety: Wallet

Adds a public key (in hex) that can be watched as if it were in your wallet but cannot be used to spend. Requires a new wallet backup.

Note: This call can take over an hour to complete if rescan is true, during that time, other rpc calls
may report that the imported pubkey exists but related transactions are still missing, leading to temporarily incorrect/bogus balances and unspent outputs until rescan completes.

Signature:

```text
importpubkey "pubkey" ( "label" rescan )
```

Example call:

```bash
zerohour-cli importpubkey 'public_key_hex' 'label' false
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"importpubkey","params":["public_key_hex","label",false]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

### importwallet

Safety: Sensitive

Imports keys from a wallet dump file (see dumpwallet). Requires a new wallet backup to include imported keys.

Signature:

```text
importwallet "filename"
```

Example call:

```bash
zerohour-cli importwallet '/safe/import/wallet.txt'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"importwallet","params":["/safe/import/wallet.txt"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

Sensitive method: do not expose private keys, passphrases, wallet dumps, or backup paths in logs or screenshots.

### keypoolrefill

Safety: Wallet

Fills the keypool.

Signature:

```text
keypoolrefill ( newsize )
```

Example call:

```bash
zerohour-cli keypoolrefill 100
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"keypoolrefill","params":[100]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

### listaddressgroupings

Safety: Wallet

Lists groups of addresses which have had their common ownership
made public by common use as inputs or as the resulting change
in past transactions

Signature:

```text
listaddressgroupings
```

Example call:

```bash
zerohour-cli listaddressgroupings
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"listaddressgroupings","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
[
  [
    [
      "address",            (string) The zerohour address
      amount,                 (numeric) The amount in ZHC
      "label"               (string, optional) The label
    ]
    ,...
  ]
  ,...
]
```

### listlabels

Safety: Wallet

Returns the list of all labels, or labels that are assigned to addresses with a specific purpose.

Signature:

```text
listlabels ( "purpose" )
```

Example call:

```bash
zerohour-cli listlabels
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"listlabels","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
[               (json array of string)
  "label",      (string) Label name
  ...
]
```

### listlockunspent

Safety: Wallet

Returns list of temporarily unspendable outputs.
See the lockunspent call to lock and unlock transactions for spending.

Signature:

```text
listlockunspent
```

Example call:

```bash
zerohour-cli listlockunspent
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"listlockunspent","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
[
  {
    "txid" : "transactionid",     (string) The transaction id locked
    "vout" : n                      (numeric) The vout value
  }
  ,...
]
```

### listreceivedbyaddress

Safety: Wallet

List balances by receiving address.

Signature:

```text
listreceivedbyaddress ( minconf include_empty include_watchonly "address_filter" )
```

Example call:

```bash
zerohour-cli listreceivedbyaddress 1 false false
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"listreceivedbyaddress","params":[1,false,false]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
[
  {
    "involvesWatchonly" : true,        (bool) Only returned if imported addresses were involved in transaction
    "address" : "receivingaddress",  (string) The receiving address
    "amount" : x.xxx,                  (numeric) The total amount in ZHC received by the address
    "confirmations" : n,               (numeric) The number of confirmations of the most recent transaction included
    "label" : "label",               (string) The label of the receiving address. The default label is "".
    "txids": [
       "txid",                         (string) The ids of transactions received with the address 
       ...
    ]
  }
  ,...
]
```

### listreceivedbylabel

Safety: Wallet

List received transactions by label.

Signature:

```text
listreceivedbylabel ( minconf include_empty include_watchonly )
```

Example call:

```bash
zerohour-cli listreceivedbylabel 1 false false
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"listreceivedbylabel","params":[1,false,false]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
[
  {
    "involvesWatchonly" : true,   (bool) Only returned if imported addresses were involved in transaction
    "amount" : x.xxx,             (numeric) The total amount received by addresses with this label
    "confirmations" : n,          (numeric) The number of confirmations of the most recent transaction included
    "label" : "label"           (string) The label of the receiving address. The default label is "".
  }
  ,...
]
```

### listsinceblock

Safety: Wallet

Get all transactions in blocks since block [blockhash], or all transactions if omitted.
If "blockhash" is no longer a part of the main chain, transactions from the fork point onward are included.
Additionally, if include_removed is set, transactions affecting the wallet which were removed are returned in the "removed" array.

Signature:

```text
listsinceblock ( "blockhash" target_confirmations include_watchonly include_removed )
```

Example call:

```bash
zerohour-cli listsinceblock '0000751feb032e4d1993d7852130092a6f95d529c44e425b06add2c79aa4a6c7'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"listsinceblock","params":["0000751feb032e4d1993d7852130092a6f95d529c44e425b06add2c79aa4a6c7"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "transactions": [
    "address":"address",    (string) The zerohour address of the transaction.
    "category":               (string) The transaction category.
                "send"                  Transactions sent.
                "receive"               Non-coinbase transactions received.
                "generate"              Coinbase transactions received with more than 100 confirmations.
                "immature"              Coinbase transactions received with 100 or fewer confirmations.
                "orphan"                Orphaned coinbase transactions received.
    "amount": x.xxx,          (numeric) The amount in ZHC. This is negative for the 'send' category, and is positive
                                         for all other categories
    "vout" : n,               (numeric) the vout value
    "fee": x.xxx,             (numeric) The amount of the fee in ZHC. This is negative and only available for the 'send' category of transactions.
    "confirmations": n,       (numeric) The number of confirmations for the transaction.
                                          When it's < 0, it means the transaction conflicted that many blocks ago.
    "blockhash": "hashvalue",     (string) The block hash containing the transaction.
    "blockindex": n,          (numeric) The index of the transaction in the block that includes it.
    "blocktime": xxx,         (numeric) The block time in seconds since epoch (1 Jan 1970 GMT).
    "txid": "transactionid",  (string) The transaction id.
    "time": xxx,              (numeric) The transaction time in seconds since epoch (Jan 1 1970 GMT).
    "timereceived": xxx,      (numeric) The time received in seconds since epoch (Jan 1 1970 GMT).
    "bip125-replaceable": "yes|no|unknown",  (string) Whether this transaction could be replaced due to BIP125 (replace-by-fee);
                                                   may be unknown for unconfirmed transactions not in the mempool
    "abandoned": xxx,         (bool) 'true' if the transaction has been abandoned (inputs are respendable). Only available for the 'send' category of transactions.
    "comment": "...",       (string) If a comment is associated with the transaction.
    "label" : "label"       (string) A comment for the address/transaction, if any
    "to": "...",            (string) If a comment to is associated with the transaction.
  ],
  "removed": [
    <structure is the same as "transactions" above, only present if include_removed=true>
    Note: transactions that were re-added in the active chain will appear as-is in this array, and may thus have a positive confirmation count.
  ],
  "lastblock": "lastblockhash"     (string) The hash of the block (target_confirmations-1) from the best block on the main chain. This is typically used to feed back into listsinceblock the next time you call it. So you would generally use a target_confirmations of say 6, so you will be continually re-notified of transactions until they've reached 6 confirmations plus any new ones
}
```

### listtransactions

Safety: Wallet

If a label name is provided, this will return only incoming transactions paying to addresses with the specified label.

Returns up to 'count' most recent transactions skipping the first 'from' transactions.

Signature:

```text
listtransactions ( "label" count skip include_watchonly )
```

Example call:

```bash
zerohour-cli listtransactions '*' 10 0
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"listtransactions","params":["*",10,0]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
[
  {
    "address":"address",    (string) The zerohour address of the transaction.
    "category":               (string) The transaction category.
                "send"                  Transactions sent.
                "receive"               Non-coinbase transactions received.
                "generate"              Coinbase transactions received with more than 100 confirmations.
                "immature"              Coinbase transactions received with 100 or fewer confirmations.
                "orphan"                Orphaned coinbase transactions received.
    "amount": x.xxx,          (numeric) The amount in ZHC. This is negative for the 'send' category, and is positive
                                        for all other categories
    "label": "label",       (string) A comment for the address/transaction, if any
    "vout": n,                (numeric) the vout value
    "fee": x.xxx,             (numeric) The amount of the fee in ZHC. This is negative and only available for the 
                                         'send' category of transactions.
    "confirmations": n,       (numeric) The number of confirmations for the transaction. Negative confirmations indicate the
                                         transaction conflicts with the block chain
    "trusted": xxx,           (bool) Whether we consider the outputs of this unconfirmed transaction safe to spend.
    "blockhash": "hashvalue", (string) The block hash containing the transaction.
    "blockindex": n,          (numeric) The index of the transaction in the block that includes it.
    "blocktime": xxx,         (numeric) The block time in seconds since epoch (1 Jan 1970 GMT).
    "txid": "transactionid", (string) The transaction id.
    "time": xxx,              (numeric) The transaction time in seconds since epoch (midnight Jan 1 1970 GMT).
    "timereceived": xxx,      (numeric) The time received in seconds since epoch (midnight Jan 1 1970 GMT).
    "comment": "...",       (string) If a comment is associated with the transaction.
    "bip125-replaceable": "yes|no|unknown",  (string) Whether this transaction could be replaced due to BIP125 (replace-by-fee);
                                                     may be unknown for unconfirmed transactions not in the mempool
    "abandoned": xxx          (bool) 'true' if the transaction has been abandoned (inputs are respendable). Only available for the 
                                         'send' category of transactions.
  }
]
```

### listunspent

Safety: Wallet

Returns array of unspent transaction outputs
with between minconf and maxconf (inclusive) confirmations.
Optionally filter to only include txouts paid to specified addresses.

Signature:

```text
listunspent ( minconf maxconf ["address",...] include_unsafe query_options )
```

Example call:

```bash
zerohour-cli listunspent 1 9999999
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"listunspent","params":[1,9999999]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
[                   (array of json object)
  {
    "txid" : "txid",          (string) the transaction id 
    "vout" : n,               (numeric) the vout value
    "address" : "address",    (string) the zerohour address
    "label" : "label",        (string) The associated label, or "" for the default label
    "scriptPubKey" : "key",   (string) the script key
    "amount" : x.xxx,         (numeric) the transaction output amount in ZHC
    "confirmations" : n,      (numeric) The number of confirmations
    "redeemScript" : "script" (string) The redeemScript if scriptPubKey is P2SH
    "witnessScript" : "script" (string) witnessScript if the scriptPubKey is P2WSH or P2SH-P2WSH
    "spendable" : xxx,        (bool) Whether we have the private keys to spend this output
    "solvable" : xxx,         (bool) Whether we know how to spend this output, ignoring the lack of keys
    "desc" : xxx,             (string, only when solvable) A descriptor for spending this output
    "safe" : xxx              (bool) Whether this output is considered safe to spend. Unconfirmed transactions
                              from outside keys and unconfirmed replacement transactions are considered unsafe
                              and are not eligible for spending by fundrawtransaction and sendtoaddress.
  }
  ,...
]
```

### listwalletdir

Safety: Wallet

Returns a list of wallets in the wallet directory.

Signature:

```text
listwalletdir
```

Example call:

```bash
zerohour-cli listwalletdir
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"listwalletdir","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "wallets" : [                (json array of objects)
    {
      "name" : "name"          (string) The wallet name
    }
    ,...
  ]
}
```

### listwallets

Safety: Wallet

Returns a list of currently loaded wallets.
For full information on the wallet, use "getwalletinfo"

Signature:

```text
listwallets
```

Example call:

```bash
zerohour-cli listwallets
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"listwallets","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
[                         (json array of strings)
  "walletname"            (string) the wallet name
   ...
]
```

### loadwallet

Safety: Wallet

Loads a wallet from a wallet file or directory.
Note that all wallet command-line options used when starting zerohourd will be
applied to the new wallet (eg -zapwallettxes, upgradewallet, rescan, etc).

Signature:

```text
loadwallet "filename"
```

Example call:

```bash
zerohour-cli loadwallet 'wallet.dat'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"loadwallet","params":["wallet.dat"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "name" :    <wallet_name>,        (string) The wallet name if loaded successfully.
  "warning" : <warning>,            (string) Warning message if wallet was not loaded cleanly.
}
```

### lockunspent

Safety: Wallet

Updates list of temporarily unspendable outputs.
Temporarily lock (unlock=false) or unlock (unlock=true) specified transaction outputs.
If no transaction outputs are specified when unlocking then all current locked transaction outputs are unlocked.
A locked transaction output will not be chosen by automatic coin selection, when spending zerohours.
Locks are stored in memory only. Nodes start with zero locked outputs, and the locked output list
is always cleared (by virtue of process exit) when a node stops or fails.
Also see the listunspent call

Signature:

```text
lockunspent unlock ( [{"txid":"hex","vout":n},...] )
```

Example call:

```bash
zerohour-cli lockunspent true
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"lockunspent","params":[true]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
true|false    (boolean) Whether the command was successful or not
```

### removeprunedfunds

Safety: Wallet

Deletes the specified transaction from the wallet. Meant for use with pruned wallets and as a companion to importprunedfunds. This will affect wallet balances.

Signature:

```text
removeprunedfunds "txid"
```

Example call:

```bash
zerohour-cli removeprunedfunds '0000000000000000000000000000000000000000000000000000000000000000'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"removeprunedfunds","params":["0000000000000000000000000000000000000000000000000000000000000000"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

### rescanblockchain

Safety: Wallet

Rescan the local blockchain for wallet related transactions.

Signature:

```text
rescanblockchain ( start_height stop_height )
```

Example call:

```bash
zerohour-cli rescanblockchain 1 1000
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"rescanblockchain","params":[1,1000]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "start_height"     (numeric) The block height where the rescan started (the requested height or 0)
  "stop_height"      (numeric) The height of the last rescanned block. May be null in rare cases if there was a reorg and the call didn't scan any blocks because they were already scanned in the background.
}
```

### reservebalance

Safety: Wallet

Set reserve amount not participating in network protection.
If no parameters provided current setting is printed.

Signature:

```text
reservebalance ( reserve amount )
```

Example call:

```bash
zerohour-cli reservebalance true 1000
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"reservebalance","params":[true,1000]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

### sendmany

Safety: Broadcast

Send multiple times. Amounts are double-precision floating point numbers.

Signature:

```text
sendmany "" {"address":amount} ( minconf "comment" ["address",...] replaceable conf_target "estimate_mode" )
```

Example call:

```bash
zerohour-cli sendmany '' '{"ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr":0.001}'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"sendmany","params":["",{"ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr":0.001}]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
"txid"                   (string) The transaction id for the send. Only 1 transaction is created regardless of 
                                    the number of addresses.
```

Broadcast method: do not run this on mainnet unless you intend to create or publish a real transaction.

### sendmanywithdupes

Safety: Broadcast

Send multiple times. Amounts are double-precision floating point numbers. Supports duplicate addresses

Signature:

```text
sendmanywithdupes "fromaccount" {"address":"hex"} ( minconf "comment" ["address",...] )
```

Example call:

```bash
zerohour-cli sendmanywithdupes '' '{"ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr":"0.001"}'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"sendmanywithdupes","params":["",{"ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr":"0.001"}]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
"txid"                   (string) The transaction id for the send. Only 1 transaction is created regardless of 
                                    the number of addresses.
```

Broadcast method: do not run this on mainnet unless you intend to create or publish a real transaction.

### sendtoaddress

Safety: Broadcast

Send an amount to a given address.

Signature:

```text
sendtoaddress "address" amount ( "comment" "comment_to" subtractfeefromamount replaceable conf_target "estimate_mode" "senderaddress" changeToSender )
```

Example call:

```bash
zerohour-cli sendtoaddress 'ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr' 0.001
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"sendtoaddress","params":["ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr",0.001]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
"txid"                  (string) The transaction id.
```

Broadcast method: do not run this on mainnet unless you intend to create or publish a real transaction.

### sendtocontract

Safety: Broadcast

Send funds and data to a contract.

Signature:

```text
sendtocontract "contractaddress" "datahex" ( amount gasLimit gasPrice "senderaddress" broadcast changeToSender )
```

Example call:

```bash
zerohour-cli sendtocontract 'c6ca2697719d00446d4ea51f6fac8fd1e9310214' '54f6127f' 0 250000 4e-7
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"sendtocontract","params":["c6ca2697719d00446d4ea51f6fac8fd1e9310214","54f6127f",0,250000,4e-7]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
[
  {
    "txid" : (string) The transaction id.
    "sender" : (string) ZHC address of the sender.
    "hash160" : (string) ripemd-160 hash of the sender.
  }
]
```

Broadcast method: do not run this on mainnet unless you intend to create or publish a real transaction.

### sethdseed

Safety: Sensitive

Set or generate a new HD wallet seed. Non-HD wallets will not be upgraded to being a HD wallet. Wallets that are already
HD will have a new HD seed set so that new keys added to the keypool will be derived from this new seed.

Note that you will need to MAKE A NEW BACKUP of your wallet after setting the HD wallet seed.

Signature:

```text
sethdseed ( newkeypool "seed" )
```

Example call:

```bash
zerohour-cli sethdseed true 'seed_wif'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"sethdseed","params":[true,"seed_wif"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

Sensitive method: do not expose private keys, passphrases, wallet dumps, or backup paths in logs or screenshots.

### setlabel

Safety: Wallet

Sets the label associated with the given address.

Signature:

```text
setlabel "address" "label"
```

Example call:

```bash
zerohour-cli setlabel 'ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr' 'label'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"setlabel","params":["ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr","label"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

### settxfee

Safety: Wallet

Set the transaction fee per kB for this wallet. Overrides the global -paytxfee command line parameter.

Signature:

```text
settxfee amount
```

Example call:

```bash
zerohour-cli settxfee 0.004
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"settxfee","params":[0.004]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
true|false        (boolean) Returns true if successful
```

### signmessage

Safety: Wallet

Sign a message with the private key of an address

Signature:

```text
signmessage "address" "message"
```

Example call:

```bash
zerohour-cli signmessage 'ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr' 'hello'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"signmessage","params":["ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr","hello"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
"signature"          (string) The signature of the message encoded in base 64
```

### signrawtransactionwithwallet

Safety: Wallet

Sign inputs for raw transaction (serialized, hex-encoded).
The second optional argument (may be null) is an array of previous transaction outputs that
this transaction depends on but may not yet be in the block chain.

Signature:

```text
signrawtransactionwithwallet "hexstring" ( [{"txid":"hex","vout":n,"scriptPubKey":"hex","redeemScript":"hex","witnessScript":"hex","amount":amount},...] "sighashtype" )
```

Example call:

```bash
zerohour-cli signrawtransactionwithwallet '020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"signrawtransactionwithwallet","params":["020000000001a0860100000000001976a91425ceedce21c41b53b8266d6f224ed7acfa046d9688ac00000000"]}' \
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

### unloadwallet

Safety: Wallet

Unloads the wallet referenced by the request endpoint otherwise unloads the wallet specified in the argument.
Specifying the wallet name on a wallet endpoint is invalid.

Signature:

```text
unloadwallet ( "wallet_name" )
```

Example call:

```bash
zerohour-cli unloadwallet 'wallet.dat'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"unloadwallet","params":["wallet.dat"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

### walletcreatefundedpsbt

Safety: Wallet

Creates and funds a transaction in the Partially Signed Transaction format. Inputs will be added if supplied inputs are not enough
Implements the Creator and Updater roles.

Signature:

```text
walletcreatefundedpsbt [{"txid":"hex","vout":n,"sequence":n},...] [{"address":amount},{"data":"hex"},...] ( locktime options bip32derivs )
```

Example call:

```bash
zerohour-cli walletcreatefundedpsbt '[]' '{"ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr":0.001}'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"walletcreatefundedpsbt","params":[[],{"ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr":0.001}]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "psbt": "value",        (string)  The resulting raw transaction (base64-encoded string)
  "fee":       n,         (numeric) Fee in ZHC the resulting transaction pays
  "changepos": n          (numeric) The position of the added change output, or -1
}
```

### walletlock

Safety: Wallet

Removes the wallet encryption key from memory, locking the wallet.
After calling this method, you will need to call walletpassphrase again
before being able to call any methods which require the wallet to be unlocked.

Signature:

```text
walletlock
```

Example call:

```bash
zerohour-cli walletlock
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"walletlock","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

### walletpassphrase

Safety: Sensitive

Stores the wallet decryption key in memory for 'timeout' seconds.
This is needed prior to performing transactions related to private keys such as sending ZHC and staking

Note:
Issuing the walletpassphrase command while the wallet is already unlocked will set a new unlock
time that overrides the old one.

Signature:

```text
walletpassphrase "passphrase" timeout ( staking only )
```

Example call:

```bash
zerohour-cli walletpassphrase 'strong_passphrase' 60 true
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"walletpassphrase","params":["strong_passphrase",60,true]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

Sensitive method: do not expose private keys, passphrases, wallet dumps, or backup paths in logs or screenshots.

### walletpassphrasechange

Safety: Sensitive

Changes the wallet passphrase from 'oldpassphrase' to 'newpassphrase'.

Signature:

```text
walletpassphrasechange "oldpassphrase" "newpassphrase"
```

Example call:

```bash
zerohour-cli walletpassphrasechange 'old_passphrase' 'new_passphrase'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"walletpassphrasechange","params":["old_passphrase","new_passphrase"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

Sensitive method: do not expose private keys, passphrases, wallet dumps, or backup paths in logs or screenshots.

### walletprocesspsbt

Safety: Wallet

Update a PSBT with input information from our wallet and then sign inputs
that we can sign for.

Signature:

```text
walletprocesspsbt "psbt" ( sign "sighashtype" bip32derivs )
```

Example call:

```bash
zerohour-cli walletprocesspsbt 'cHNidP8BAHECAAAAAAABAKCGAQAAAAAAGXapFCXO7c4hxBtTuCZtbyJO16z6BG2WiKwAAAAAAA=='
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"walletprocesspsbt","params":["cHNidP8BAHECAAAAAAABAKCGAQAAAAAAGXapFCXO7c4hxBtTuCZtbyJO16z6BG2WiKwAAAAAAA=="]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{
  "psbt" : "value",          (string) The base64-encoded partially signed transaction
  "complete" : true|false,   (boolean) If the transaction has a complete set of signatures
  ]
}
```

## Full Method Index

| Method | Safety | Signature |
| --- | --- | --- |
| `abandontransaction` | Wallet | `abandontransaction "txid"` |
| `abortrescan` | Wallet | `abortrescan` |
| `addmultisigaddress` | Wallet | `addmultisigaddress nrequired ["key",...] ( "label" "address_type" )` |
| `backupwallet` | Sensitive | `backupwallet "destination"` |
| `bumpfee` | Broadcast | `bumpfee "txid" ( options )` |
| `createcontract` | Broadcast | `createcontract "bytecode" ( gasLimit gasPrice "senderaddress" broadcast changeToSender )` |
| `createwallet` | Wallet | `createwallet "wallet_name" ( disable_private_keys blank )` |
| `dumpprivkey` | Sensitive | `dumpprivkey "address"` |
| `dumpwallet` | Sensitive | `dumpwallet "filename"` |
| `encryptwallet` | Sensitive | `encryptwallet "passphrase"` |
| `getaddressesbylabel` | Wallet | `getaddressesbylabel "label"` |
| `getaddressinfo` | Wallet | `getaddressinfo "address"` |
| `getbalance` | Wallet | `getbalance ( "dummy" minconf include_watchonly )` |
| `getnewaddress` | Wallet | `getnewaddress ( "label" "address_type" )` |
| `getrawchangeaddress` | Wallet | `getrawchangeaddress ( "address_type" )` |
| `getreceivedbyaddress` | Wallet | `getreceivedbyaddress "address" ( minconf )` |
| `getreceivedbylabel` | Wallet | `getreceivedbylabel "label" ( minconf )` |
| `gettransaction` | Wallet | `gettransaction "txid" ( include_watchonly waitconf )` |
| `getunconfirmedbalance` | Wallet | `getunconfirmedbalance` |
| `getwalletinfo` | Wallet | `getwalletinfo` |
| `importaddress` | Wallet | `importaddress "address" ( "label" rescan p2sh )` |
| `importmulti` | Wallet | `importmulti "requests" ( "options" )` |
| `importprivkey` | Sensitive | `importprivkey "zerohourprivkey" ( "label" rescan )` |
| `importprunedfunds` | Wallet | `importprunedfunds "rawtransaction" "txoutproof"` |
| `importpubkey` | Wallet | `importpubkey "pubkey" ( "label" rescan )` |
| `importwallet` | Sensitive | `importwallet "filename"` |
| `keypoolrefill` | Wallet | `keypoolrefill ( newsize )` |
| `listaddressgroupings` | Wallet | `listaddressgroupings` |
| `listlabels` | Wallet | `listlabels ( "purpose" )` |
| `listlockunspent` | Wallet | `listlockunspent` |
| `listreceivedbyaddress` | Wallet | `listreceivedbyaddress ( minconf include_empty include_watchonly "address_filter" )` |
| `listreceivedbylabel` | Wallet | `listreceivedbylabel ( minconf include_empty include_watchonly )` |
| `listsinceblock` | Wallet | `listsinceblock ( "blockhash" target_confirmations include_watchonly include_removed )` |
| `listtransactions` | Wallet | `listtransactions ( "label" count skip include_watchonly )` |
| `listunspent` | Wallet | `listunspent ( minconf maxconf ["address",...] include_unsafe query_options )` |
| `listwalletdir` | Wallet | `listwalletdir` |
| `listwallets` | Wallet | `listwallets` |
| `loadwallet` | Wallet | `loadwallet "filename"` |
| `lockunspent` | Wallet | `lockunspent unlock ( [{"txid":"hex","vout":n},...] )` |
| `removeprunedfunds` | Wallet | `removeprunedfunds "txid"` |
| `rescanblockchain` | Wallet | `rescanblockchain ( start_height stop_height )` |
| `reservebalance` | Wallet | `reservebalance ( reserve amount )` |
| `sendmany` | Broadcast | `sendmany "" {"address":amount} ( minconf "comment" ["address",...] replaceable conf_target "estimate_mode" )` |
| `sendmanywithdupes` | Broadcast | `sendmanywithdupes "fromaccount" {"address":"hex"} ( minconf "comment" ["address",...] )` |
| `sendtoaddress` | Broadcast | `sendtoaddress "address" amount ( "comment" "comment_to" subtractfeefromamount replaceable conf_target "estimate_mode" "senderaddress" changeToSender )` |
| `sendtocontract` | Broadcast | `sendtocontract "contractaddress" "datahex" ( amount gasLimit gasPrice "senderaddress" broadcast changeToSender )` |
| `sethdseed` | Sensitive | `sethdseed ( newkeypool "seed" )` |
| `setlabel` | Wallet | `setlabel "address" "label"` |
| `settxfee` | Wallet | `settxfee amount` |
| `signmessage` | Wallet | `signmessage "address" "message"` |
| `signrawtransactionwithwallet` | Wallet | `signrawtransactionwithwallet "hexstring" ( [{"txid":"hex","vout":n,"scriptPubKey":"hex","redeemScript":"hex","witnessScript":"hex","amount":amount},...] "sighashtype" )` |
| `unloadwallet` | Wallet | `unloadwallet ( "wallet_name" )` |
| `walletcreatefundedpsbt` | Wallet | `walletcreatefundedpsbt [{"txid":"hex","vout":n,"sequence":n},...] [{"address":amount},{"data":"hex"},...] ( locktime options bip32derivs )` |
| `walletlock` | Wallet | `walletlock` |
| `walletpassphrase` | Sensitive | `walletpassphrase "passphrase" timeout ( staking only )` |
| `walletpassphrasechange` | Sensitive | `walletpassphrasechange "oldpassphrase" "newpassphrase"` |
| `walletprocesspsbt` | Wallet | `walletprocesspsbt "psbt" ( sign "sighashtype" bip32derivs )` |
