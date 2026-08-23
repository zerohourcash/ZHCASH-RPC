# ZMQ RPC

ZMQ RPC methods show configured ZeroMQ notification endpoints.

## Menu

* [getzmqnotifications](#getzmqnotifications)
* [Full Method Index](#full-method-index)

## Methods

### getzmqnotifications

Safety: Read-only

Returns information about the active ZeroMQ notifications.

Signature:

```text
getzmqnotifications
```

Example call:

```bash
zerohour-cli getzmqnotifications
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"getzmqnotifications","params":[]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Checked example response:

```json
[
]
```

## Full Method Index

| Method | Safety | Signature |
| --- | --- | --- |
| `getzmqnotifications` | Read-only | `getzmqnotifications` |
