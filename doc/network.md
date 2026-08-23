# Network RPC

Network RPC methods show connected peers, network totals, local addresses, and let operators manage peers and bans.

## Menu

* [addnode](#addnode)
* [clearbanned](#clearbanned)
* [disconnectnode](#disconnectnode)
* [getaddednodeinfo](#getaddednodeinfo)
* [getconnectioncount](#getconnectioncount)
* [getnettotals](#getnettotals)
* [getnetworkinfo](#getnetworkinfo)
* [getnodeaddresses](#getnodeaddresses)
* [getpeerinfo](#getpeerinfo)
* [listbanned](#listbanned)
* [ping](#ping)
* [setban](#setban)
* [setnetworkactive](#setnetworkactive)
* [Full Method Index](#full-method-index)

## Methods

### addnode

Safety: Node-control

Attempts to add or remove a node from the addnode list.
Or try a connection to a node once.
Nodes added using addnode (or -connect) are protected from DoS disconnection and are not required to be
full nodes/support SegWit as other outbound peers are (though such peers will not be synced from).

Signature:

```text
addnode "node" "command"
```

Example call:

```bash
zerohour-cli addnode '127.0.0.1:38100' 'onetry'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"addnode","params":["127.0.0.1:38100","onetry"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

### clearbanned

Safety: Node-control

Clear all banned IPs.

Signature:

```text
clearbanned
```

Example call:

```bash
zerohour-cli clearbanned
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"clearbanned","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

### disconnectnode

Safety: Node-control

Immediately disconnects from the specified peer node.

Strictly one out of 'address' and 'nodeid' can be provided to identify the node.

To disconnect by nodeid, either set 'address' to the empty string, or call using the named 'nodeid' argument only.

Signature:

```text
disconnectnode ( "address" nodeid )
```

Example call:

```bash
zerohour-cli disconnectnode '127.0.0.1:38100'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"disconnectnode","params":["127.0.0.1:38100"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

### getaddednodeinfo

Safety: Read-only

Returns information about the given added node, or all added nodes
(note that onetry addnodes are not listed here)

Signature:

```text
getaddednodeinfo ( "node" )
```

Example call:

```bash
zerohour-cli getaddednodeinfo
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getaddednodeinfo","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
[
  {
    "addednode" : "192.168.0.201",   (string) The node IP address or name (as provided to addnode)
    "connected" : true|false,          (boolean) If connected
    "addresses" : [                    (list of objects) Only when connected = true
       {
         "address" : "192.168.0.201:3888",  (string) The zerohour server IP and port we're connected to
         "connected" : "outbound"           (string) connection, inbound or outbound
       }
     ]
  }
  ,...
]
```

### getconnectioncount

Safety: Read-only

Returns the number of connections to other nodes.

Signature:

```text
getconnectioncount
```

Example call:

```bash
zerohour-cli getconnectioncount
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getconnectioncount","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```json
22
```

### getnettotals

Safety: Read-only

Returns information about network traffic, including bytes in, bytes out,
and current time.

Signature:

```text
getnettotals
```

Example call:

```bash
zerohour-cli getnettotals
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getnettotals","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```json
{
  "totalbytesrecv": 704083,
  "totalbytessent": 18096783,
  "timemillis": 1786225131650,
  "uploadtarget": {
    "timeframe": 86400,
    "target": 0,
    "target_reached": false,
    "serve_historical_blocks": true,
    "bytes_left_in_cycle": 0,
    "time_left_in_cycle": 0
  }
}
```

### getnetworkinfo

Safety: Read-only

Returns an object containing various state info regarding P2P networking.

Signature:

```text
getnetworkinfo
```

Example call:

```bash
zerohour-cli getnetworkinfo
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getnetworkinfo","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```json
{
  "version": 1000000,
  "subversion": "/Evolution:1.0.0/",
  "protocolversion": 70018,
  "localservices": "000000000000040d",
  "localrelay": true,
  "timeoffset": 0,
  "networkactive": true,
  "connections": 22,
  "networks": [
    {
      "name": "ipv4",
      "limited": false,
      "reachable": true,
      "proxy": "",
      "proxy_randomize_credentials": false
    },
    {
      "name": "ipv6",
      "limited": false,
      "reachable": true,
      "proxy": "",
      "proxy_randomize_credentials": false
    },
    {
      "name": "onion",
      "limited": true,
      "reachable": false,
      "proxy": "",
      "proxy_randomize_credentials": false
    }
  ],
  "relayfee": 0.00400000,
  "incrementalfee": 0.00010000,
  "localaddresses": [
    {
      "address": "95.133.236.37",
      "port": 38100,
      "score": 19
    },
    {
      "address": "2a00:1911:1:798c:589b:1702:1ca3:6668",
      "port": 38100,
      "score": 5
    }
  ],
  "warnings": ""
}
```

### getnodeaddresses

Safety: Read-only

Return known addresses which can potentially be used to find new nodes in the network

Signature:

```text
getnodeaddresses ( count )
```

Example call:

```bash
zerohour-cli getnodeaddresses 10
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getnodeaddresses","params":[10]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
[
  {
    "time": ttt,                (numeric) Timestamp in seconds since epoch (Jan 1 1970 GMT) keeping track of when the node was last seen
    "services": n,              (numeric) The services offered
    "address": "host",          (string) The address of the node
    "port": n                   (numeric) The port of the node
  }
  ,....
]
```

### getpeerinfo

Safety: Read-only

Returns data about each connected network node as a json array of objects.

Signature:

```text
getpeerinfo
```

Example call:

```bash
zerohour-cli getpeerinfo
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getpeerinfo","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
[
  {
    "id": n,                   (numeric) Peer index
    "addr":"host:port",      (string) The IP address and port of the peer
    "addrbind":"ip:port",    (string) Bind address of the connection to the peer
    "addrlocal":"ip:port",   (string) Local address as reported by the peer
    "services":"xxxxxxxxxxxxxxxx",   (string) The services offered
    "relaytxes":true|false,    (boolean) Whether peer has asked us to relay transactions to it
    "lastsend": ttt,           (numeric) The time in seconds since epoch (Jan 1 1970 GMT) of the last send
    "lastrecv": ttt,           (numeric) The time in seconds since epoch (Jan 1 1970 GMT) of the last receive
    "bytessent": n,            (numeric) The total bytes sent
    "bytesrecv": n,            (numeric) The total bytes received
    "conntime": ttt,           (numeric) The connection time in seconds since epoch (Jan 1 1970 GMT)
    "timeoffset": ttt,         (numeric) The time offset in seconds
    "pingtime": n,             (numeric) ping time (if available)
    "minping": n,              (numeric) minimum observed ping time (if any at all)
    "pingwait": n,             (numeric) ping wait (if non-zero)
    "version": v,              (numeric) The peer version, such as 70001
    "subver": "/Satoshi:0.8.5/",  (string) The string version
    "inbound": true|false,     (boolean) Inbound (true) or Outbound (false)
    "addnode": true|false,     (boolean) Whether connection was due to addnode/-connect or if it was an automatic/inbound connection
    "startingheight": n,       (numeric) The starting height (block) of the peer
    "banscore": n,             (numeric) The ban score
    "synced_headers": n,       (numeric) The last header we have in common with this peer
    "synced_blocks": n,        (numeric) The last block we have in common with this peer
    "inflight": [
       n,                        (numeric) The heights of blocks we're currently asking from this peer
       ...
    ],
    "whitelisted": true|false, (boolean) Whether the peer is whitelisted
    "minfeefilter": n,         (numeric) The minimum fee rate for transactions this peer accepts
    "bytessent_per_msg": {
       "msg": n,               (numeric) The total bytes sent aggregated by message type
                               When a message type is not listed in this json object, the bytes sent are 0.
                               Only known message types can appear as keys in the object.
       ...
    },
    "bytesrecv_per_msg": {
       "msg": n,               (numeric) The total bytes received aggregated by message type
                               When a message type is not listed in this json object, the bytes received are 0.
                               Only known message types can appear as keys in the object and all bytes received of unknown message types are listed under '*other*'.
       ...
    }
  }
  ,...
]
```

### listbanned

Safety: Read-only

List all banned IPs/Subnets.

Signature:

```text
listbanned
```

Example call:

```bash
zerohour-cli listbanned
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"listbanned","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```json
[
]
```

### ping

Safety: Node-control

Requests that a ping be sent to all other nodes, to measure ping time.

Signature:

```text
ping
```

Example call:

```bash
zerohour-cli ping
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"ping","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Results provided in getpeerinfo, pingtime and pingwait fields are decimal seconds.
Ping command is handled in queue with all other commands, so it measures processing backlog, not just network ping.
```

### setban

Safety: Node-control

Attempts to add or remove an IP/Subnet from the banned list.

Signature:

```text
setban "subnet" "command" ( bantime absolute )
```

Example call:

```bash
zerohour-cli setban '192.0.2.1' 'add' 3600
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"setban","params":["192.0.2.1","add",3600]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

### setnetworkactive

Safety: Node-control

Disable/enable all p2p network activity.

Signature:

```text
setnetworkactive state
```

Example call:

```bash
zerohour-cli setnetworkactive true
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"setnetworkactive","params":[true]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

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
