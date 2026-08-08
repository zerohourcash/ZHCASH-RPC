# Control RPC

Control RPC methods expose node diagnostics, logging controls, uptime, and
dynamic gas parameters.

## Menu

* [Node Control](#node-control)
* [Diagnostics](#diagnostics)
* [Dynamic Gas Parameters](#dynamic-gas-parameters)
* [Full Method Index](#full-method-index)

## Node Control

| Method | Safety | Signature |
| --- | --- | --- |
| `help` | Read-only | `help ( "command" )` |
| `logging` | Node-control | `logging ( ["include_category",...] ["exclude_category",...] )` |
| `stop` | Node-control | `stop` |

Examples:

```bash
zerohour-cli help getblockchaininfo
zerohour-cli logging
```

`stop` shuts down the node. Do not call it from application health checks.

## Diagnostics

| Method | Safety | Signature |
| --- | --- | --- |
| `getmemoryinfo` | Read-only | `getmemoryinfo ( "mode" )` |
| `getrpcinfo` | Read-only | `getrpcinfo` |
| `uptime` | Read-only | `uptime` |

Examples:

```bash
zerohour-cli getrpcinfo
zerohour-cli uptime
```

## Dynamic Gas Parameters

`getdgpinfo` returns current smart-contract gas and block-size policy values.

| Method | Safety | Signature |
| --- | --- | --- |
| `getdgpinfo` | Read-only | `getdgpinfo` |

Example:

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

## Full Method Index

| Method | Safety | Signature |
| --- | --- | --- |
| `getdgpinfo` | Read-only | `getdgpinfo` |
| `getmemoryinfo` | Read-only | `getmemoryinfo ( "mode" )` |
| `getrpcinfo` | Read-only | `getrpcinfo` |
| `help` | Read-only | `help ( "command" )` |
| `logging` | Node-control | `logging ( ["include_category",...] ["exclude_category",...] )` |
| `stop` | Node-control | `stop` |
| `uptime` | Read-only | `uptime` |
