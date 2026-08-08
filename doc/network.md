# Network RPC

Network RPC methods show connected peers, network totals, local addresses, and
let operators manage peers and bans.

## Menu

* [Peer Connections](#peer-connections)
* [Ban List](#ban-list)
* [Network State](#network-state)
* [Full Method Index](#full-method-index)

## Peer Connections

| Method | Safety | Signature |
| --- | --- | --- |
| `addnode` | Node-control | `addnode "node" "command"` |
| `disconnectnode` | Node-control | `disconnectnode ( "address" nodeid )` |
| `getaddednodeinfo` | Read-only | `getaddednodeinfo ( "node" )` |
| `getconnectioncount` | Read-only | `getconnectioncount` |
| `getnodeaddresses` | Read-only | `getnodeaddresses ( count )` |
| `getpeerinfo` | Read-only | `getpeerinfo` |
| `ping` | Node-control | `ping` |

Examples:

```bash
zerohour-cli getconnectioncount
zerohour-cli getpeerinfo
zerohour-cli getnodeaddresses 10
```

## Ban List

| Method | Safety | Signature |
| --- | --- | --- |
| `clearbanned` | Node-control | `clearbanned` |
| `listbanned` | Read-only | `listbanned` |
| `setban` | Node-control | `setban "subnet" "command" ( bantime absolute )` |

Examples:

```bash
zerohour-cli listbanned
zerohour-cli setban "192.0.2.1" "add" 3600
zerohour-cli clearbanned
```

## Network State

| Method | Safety | Signature |
| --- | --- | --- |
| `getnettotals` | Read-only | `getnettotals` |
| `getnetworkinfo` | Read-only | `getnetworkinfo` |
| `setnetworkactive` | Node-control | `setnetworkactive state` |

Examples:

```bash
zerohour-cli getnetworkinfo
zerohour-cli getnettotals
```

Do not call `setnetworkactive false` unless you intentionally want the node to
disconnect from the network.

## Full Method Index

| Method | Safety | Signature |
| --- | --- | --- |
| `addnode` | Node-control | `addnode "node" "command"` |
| `clearbanned` | Node-control | `clearbanned` |
| `disconnectnode` | Node-control | `disconnectnode ( "address" nodeid )` |
| `getaddednodeinfo` | Read-only | `getaddednodeinfo ( "node" )` |
| `getconnectioncount` | Read-only | `getconnectioncount` |
| `getnettotals` | Read-only | `getnettotals` |
| `getnetworkinfo` | Read-only | `getnetworkinfo` |
| `getnodeaddresses` | Read-only | `getnodeaddresses ( count )` |
| `getpeerinfo` | Read-only | `getpeerinfo` |
| `listbanned` | Read-only | `listbanned` |
| `ping` | Node-control | `ping` |
| `setban` | Node-control | `setban "subnet" "command" ( bantime absolute )` |
| `setnetworkactive` | Node-control | `setnetworkactive state` |
