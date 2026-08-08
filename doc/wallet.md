# Wallet RPC

Wallet RPC methods read balances, create addresses, send funds, manage labels,
import/export keys, lock or unlock the wallet, and interact with smart
contracts through wallet-funded transactions.

## Menu

* [Wallet Information](#wallet-information)
* [Addresses / Labels](#addresses--labels)
* [Balances / UTXO](#balances--utxo)
* [Sending Funds](#sending-funds)
* [Smart Contract Write RPC](#smart-contract-write-rpc)
* [Import / Export](#import--export)
* [Encryption / Locking / Staking](#encryption--locking--staking)
* [Wallet Files](#wallet-files)
* [Full Method Index](#full-method-index)

## Wallet Information

| Method | Safety | Signature |
| --- | --- | --- |
| `getwalletinfo` | Wallet | `getwalletinfo` |
| `listwalletdir` | Wallet | `listwalletdir` |
| `listwallets` | Wallet | `listwallets` |

Examples:

```bash
zerohour-cli getwalletinfo
zerohour-cli listwallets
```

## Addresses / Labels

| Method | Safety | Signature |
| --- | --- | --- |
| `addmultisigaddress` | Wallet | `addmultisigaddress nrequired ["key",...] ( "label" "address_type" )` |
| `getaddressesbylabel` | Wallet | `getaddressesbylabel "label"` |
| `getaddressinfo` | Wallet | `getaddressinfo "address"` |
| `getnewaddress` | Wallet | `getnewaddress ( "label" "address_type" )` |
| `getrawchangeaddress` | Wallet | `getrawchangeaddress ( "address_type" )` |
| `setlabel` | Wallet | `setlabel "address" "label"` |

Examples:

```bash
zerohour-cli getnewaddress "dev-test"
zerohour-cli getaddressinfo "address"
zerohour-cli setlabel "address" "customer-1"
```

## Balances / UTXO

| Method | Safety | Signature |
| --- | --- | --- |
| `getbalance` | Wallet | `getbalance ( "dummy" minconf include_watchonly )` |
| `getreceivedbyaddress` | Wallet | `getreceivedbyaddress "address" ( minconf )` |
| `getreceivedbylabel` | Wallet | `getreceivedbylabel "label" ( minconf )` |
| `gettransaction` | Wallet | `gettransaction "txid" ( include_watchonly waitconf )` |
| `getunconfirmedbalance` | Wallet | `getunconfirmedbalance` |
| `listaddressgroupings` | Wallet | `listaddressgroupings` |
| `listlockunspent` | Wallet | `listlockunspent` |
| `listreceivedbyaddress` | Wallet | `listreceivedbyaddress ( minconf include_empty include_watchonly "address_filter" )` |
| `listreceivedbylabel` | Wallet | `listreceivedbylabel ( minconf include_empty include_watchonly )` |
| `listsinceblock` | Wallet | `listsinceblock ( "blockhash" target_confirmations include_watchonly include_removed )` |
| `listtransactions` | Wallet | `listtransactions ( "label" count skip include_watchonly )` |
| `listunspent` | Wallet | `listunspent ( minconf maxconf ["address",...] include_unsafe query_options )` |
| `lockunspent` | Wallet | `lockunspent unlock ( [{"txid":"hex","vout":n},...] )` |

Examples:

```bash
zerohour-cli getbalance
zerohour-cli listunspent 1 9999999
zerohour-cli listtransactions "*" 10 0
```

## Sending Funds

These methods can create or broadcast spend transactions.

| Method | Safety | Signature |
| --- | --- | --- |
| `bumpfee` | Broadcast | `bumpfee "txid" ( options )` |
| `sendmany` | Broadcast | `sendmany "" {"address":amount} ( minconf "comment" ["address",...] replaceable conf_target "estimate_mode" )` |
| `sendmanywithdupes` | Broadcast | `sendmanywithdupes "fromaccount" {"address":"hex"} ( minconf "comment" ["address",...] )` |
| `sendtoaddress` | Broadcast | `sendtoaddress "address" amount ( "comment" "comment_to" subtractfeefromamount replaceable conf_target "estimate_mode" "senderaddress" changeToSender )` |
| `settxfee` | Wallet | `settxfee amount` |

Example:

```bash
zerohour-cli sendtoaddress "address" 1.25
```

Do not run broadcast examples on mainnet unless you intend to spend real ZHC.

## Smart Contract Write RPC

These methods create wallet-funded contract transactions.

| Method | Safety | Signature |
| --- | --- | --- |
| `createcontract` | Broadcast | `createcontract "bytecode" ( gasLimit gasPrice "senderaddress" broadcast changeToSender )` |
| `sendtocontract` | Broadcast | `sendtocontract "contractaddress" "datahex" ( amount gasLimit gasPrice "senderaddress" broadcast changeToSender )` |

Syntax examples:

```bash
zerohour-cli createcontract "bytecode_hex" 2500000 0.0000004

zerohour-cli sendtocontract \
  "c6ca2697719d00446d4ea51f6fac8fd1e9310214" \
  "54f6127f" \
  0 \
  250000 \
  0.0000004
```

For real use, provide valid bytecode/contract address and an unlocked funded
wallet.

## Import / Export

These methods expose or import sensitive wallet material.

| Method | Safety | Signature |
| --- | --- | --- |
| `backupwallet` | Sensitive | `backupwallet "destination"` |
| `dumpprivkey` | Sensitive | `dumpprivkey "address"` |
| `dumpwallet` | Sensitive | `dumpwallet "filename"` |
| `importaddress` | Wallet | `importaddress "address" ( "label" rescan p2sh )` |
| `importmulti` | Wallet | `importmulti "requests" ( "options" )` |
| `importprivkey` | Sensitive | `importprivkey "zerohourprivkey" ( "label" rescan )` |
| `importprunedfunds` | Wallet | `importprunedfunds "rawtransaction" "txoutproof"` |
| `importpubkey` | Wallet | `importpubkey "pubkey" ( "label" rescan )` |
| `importwallet` | Sensitive | `importwallet "filename"` |
| `removeprunedfunds` | Wallet | `removeprunedfunds "txid"` |

Do not publish output from `dumpprivkey` or `dumpwallet`.

## Encryption / Locking / Staking

| Method | Safety | Signature |
| --- | --- | --- |
| `encryptwallet` | Sensitive | `encryptwallet "passphrase"` |
| `keypoolrefill` | Wallet | `keypoolrefill ( newsize )` |
| `reservebalance` | Wallet | `reservebalance ( reserve amount )` |
| `sethdseed` | Sensitive | `sethdseed ( newkeypool "seed" )` |
| `signmessage` | Wallet | `signmessage "address" "message"` |
| `walletlock` | Wallet | `walletlock` |
| `walletpassphrase` | Sensitive | `walletpassphrase "passphrase" timeout ( staking only )` |
| `walletpassphrasechange` | Sensitive | `walletpassphrasechange "oldpassphrase" "newpassphrase"` |

For staking unlock:

```bash
zerohour-cli walletpassphrase "your_passphrase" 999999 true
```

## Wallet Files

| Method | Safety | Signature |
| --- | --- | --- |
| `abandontransaction` | Wallet | `abandontransaction "txid"` |
| `abortrescan` | Wallet | `abortrescan` |
| `createwallet` | Wallet | `createwallet "wallet_name" ( disable_private_keys blank )` |
| `loadwallet` | Wallet | `loadwallet "filename"` |
| `rescanblockchain` | Wallet | `rescanblockchain ( start_height stop_height )` |
| `unloadwallet` | Wallet | `unloadwallet ( "wallet_name" )` |
| `walletcreatefundedpsbt` | Wallet | `walletcreatefundedpsbt [{"txid":"hex","vout":n,"sequence":n},...] [{"address":amount},{"data":"hex"},...] ( locktime options bip32derivs )` |
| `walletprocesspsbt` | Wallet | `walletprocesspsbt "psbt" ( sign "sighashtype" bip32derivs )` |

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
