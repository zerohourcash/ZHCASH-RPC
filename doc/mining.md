# Mining / Staking RPC

These RPC methods cover mining templates, staking status, subsidy calculation,
and block submission.

## Menu

* [Mining](#mining)
* [Staking](#staking)
* [Subsidy](#subsidy)
* [Full Method Index](#full-method-index)

## Mining

| Method | Safety | Signature |
| --- | --- | --- |
| `getblocktemplate` | Advanced | `getblocktemplate "template_request"` |
| `getmininginfo` | Read-only | `getmininginfo` |
| `getnetworkhashps` | Read-only | `getnetworkhashps ( nblocks height )` |
| `prioritisetransaction` | Advanced | `prioritisetransaction "txid" ( dummy ) fee_delta` |
| `submitblock` | Advanced | `submitblock "hexdata" ( "dummy" )` |
| `submitheader` | Advanced | `submitheader "hexdata"` |

Examples:

```bash
zerohour-cli getmininginfo
zerohour-cli getnetworkhashps
```

## Staking

`getstakinginfo` shows whether the wallet is staking and what network staking
weight the node sees.

| Method | Safety | Signature |
| --- | --- | --- |
| `getstakinginfo` | Read-only | `getstakinginfo` |

Example:

```bash
zerohour-cli getstakinginfo
```

Typical fields:

| Field | Meaning |
| --- | --- |
| `enabled` | Whether staking support is enabled. |
| `staking` | Whether this node is currently staking. |
| `weight` | Local wallet staking weight. |
| `netstakeweight` | Estimated network staking weight. |
| `expectedtime` | Estimated time to stake, when local weight is non-zero. |

## Subsidy

`getsubsidy` returns the block reward in satoshis. It can also calculate a
range without mining or requiring those blocks to exist locally.

| Method | Safety | Signature |
| --- | --- | --- |
| `getsubsidy` | Read-only | `getsubsidy ( height endheight )` |

Examples:

```bash
zerohour-cli getsubsidy
zerohour-cli getsubsidy 1700000
zerohour-cli getsubsidy 1700000 1700005
```

Checked values:

```text
zerohour-cli getsubsidy
80000000000

zerohour-cli getsubsidy 1700000
40000000000
```

`80000000000` satoshis = `800 ZHC`.
`40000000000` satoshis = `400 ZHC`.

Range output shape:

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
