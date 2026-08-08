# Generating RPC

Generating RPC methods are mainly useful for regtest or automated tests. They
are not normal mainnet mining/staking commands.

## Menu

* [Generating](#generating)
  * [generate](#generating)
  * [generatetoaddress](#generating)
* [Full Method Index](#full-method-index)

## Generating

| Method | Safety | Signature |
| --- | --- | --- |
| `generate` | Advanced | `generate nblocks ( maxtries )` |
| `generatetoaddress` | Advanced | `generatetoaddress nblocks "address" ( maxtries )` |

Example:

```bash
zerohour-cli generatetoaddress 1 "address"
```

Use these on regtest/test automation only unless you know exactly why you need
them.

## Full Method Index

| Method | Safety | Signature |
| --- | --- | --- |
| `generate` | Advanced | `generate nblocks ( maxtries )` |
| `generatetoaddress` | Advanced | `generatetoaddress nblocks "address" ( maxtries )` |
