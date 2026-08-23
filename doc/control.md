# Control RPC

Control RPC methods expose node diagnostics, logging controls, uptime, and dynamic gas parameters.

## Menu

* [getdgpinfo](#getdgpinfo)
* [getmemoryinfo](#getmemoryinfo)
* [getrpcinfo](#getrpcinfo)
* [help](#help)
* [logging](#logging)
* [stop](#stop)
* [uptime](#uptime)
* [Full Method Index](#full-method-index)

## Methods

### getdgpinfo

Safety: Read-only

Returns an object containing DGP state info.

Signature:

```text
getdgpinfo
```

Example call:

```bash
zerohour-cli getdgpinfo
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getdgpinfo","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```json
{
  "maxblocksize": 2000000,
  "mingasprice": 40,
  "blockgaslimit": 40000000
}
```

### getmemoryinfo

Safety: Read-only

Returns an object containing information about memory usage.

Signature:

```text
getmemoryinfo ( "mode" )
```

Example call:

```bash
zerohour-cli getmemoryinfo
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getmemoryinfo","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```json
{
  "locked": {
    "used": 64128,
    "free": 198016,
    "total": 262144,
    "locked": 262144,
    "chunks_used": 2002,
    "chunks_free": 2
  }
}
```

### getrpcinfo

Safety: Read-only

Returns details of the RPC server.

Signature:

```text
getrpcinfo
```

Example call:

```bash
zerohour-cli getrpcinfo
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getrpcinfo","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```json
{
  "active_commands": [
    {
      "method": "getrpcinfo",
      "duration": 2
    }
  ]
}
```

### help

Safety: Read-only

List all commands, or get help for a specified command.

Signature:

```text
help ( "command" )
```

Example call:

```bash
zerohour-cli help 'getblockchaininfo'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"help","params":["getblockchaininfo"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
"text"     (string) The help text
```

### logging

Safety: Node-control

Gets and sets the logging configuration.
When called without an argument, returns the list of categories with status that are currently being debug logged or not.
When called with arguments, adds or removes categories from debug logging and return the lists above.
The arguments are evaluated in order "include", "exclude".
If an item is both included and excluded, it will thus end up being excluded.
The valid logging categories are: net, tor, mempool, http, bench, zmq, db, rpc, estimatefee, addrman, selectcoins, reindex, cmpctblock, rand, prune, proxy, mempoolrej, libevent, coindb, qt, leveldb, coinstake, http-poll
In addition, the following are available as category names with special meanings:
  - "all",  "1" : represent all logging categories.
  - "none", "0" : even if other logging categories are specified, ignore all of them.

Signature:

```text
logging ( ["include_category",...] ["exclude_category",...] )
```

Example call:

```bash
zerohour-cli logging
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"logging","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
{                   (json object where keys are the logging categories, and values indicates its status
  "category": true|false,  (bool) if being debug logged or not. false:inactive, true:active
  ...
}
```

### stop

Safety: Node-control

Stop ZHCASH server.

Signature:

```text
stop
```

Example call:

```bash
zerohour-cli stop
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"stop","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```json
null
```

### uptime

Safety: Read-only

Returns the total uptime of the server.

Signature:

```text
uptime
```

Example call:

```bash
zerohour-cli uptime
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"uptime","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```json
9735
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
