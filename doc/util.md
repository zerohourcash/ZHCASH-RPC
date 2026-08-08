# Util RPC

Utility RPC methods validate addresses and messages, estimate fees, and work
with descriptors or multisig scripts.

## Menu

* [Address / Message Validation](#address--message-validation)
* [Descriptors / Multisig](#descriptors--multisig)
* [Fee Estimation](#fee-estimation)
* [Full Method Index](#full-method-index)

## Address / Message Validation

| Method | Safety | Signature |
| --- | --- | --- |
| `signmessagewithprivkey` | Sensitive | `signmessagewithprivkey "privkey" "message"` |
| `validateaddress` | Read-only | `validateaddress "address"` |
| `verifymessage` | Read-only | `verifymessage "address" "signature" "message"` |

Example:

```bash
zerohour-cli validateaddress ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr
```

Do not expose private keys used with `signmessagewithprivkey`.

## Descriptors / Multisig

| Method | Safety | Signature |
| --- | --- | --- |
| `createmultisig` | Read-only | `createmultisig nrequired ["key",...] ( "address_type" )` |
| `deriveaddresses` | Read-only | `deriveaddresses "descriptor" ( range )` |
| `getdescriptorinfo` | Read-only | `getdescriptorinfo "descriptor"` |

Examples:

```bash
zerohour-cli getdescriptorinfo "addr(ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr)"
```

## Fee Estimation

| Method | Safety | Signature |
| --- | --- | --- |
| `estimatesmartfee` | Read-only | `estimatesmartfee conf_target ( "estimate_mode" )` |

Example:

```bash
zerohour-cli estimatesmartfee 6
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
