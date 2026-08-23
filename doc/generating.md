# Generating RPC

Generating RPC methods are mainly useful for regtest or automated tests. They are not normal mainnet mining/staking commands.

## Menu

* [generate](#generate)
* [generatetoaddress](#generatetoaddress)
* [Full Method Index](#full-method-index)

## Methods

### generate

Safety: Advanced

Mine up to nblocks blocks immediately (before the RPC call returns) to an address in the wallet.

Signature:

```text
generate nblocks ( maxtries )
```

Example call:

```bash
zerohour-cli generate 1
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"generate","params":[1]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
[ blockhashes ]     (array) hashes of blocks generated
```

### generatetoaddress

Safety: Advanced

Mine blocks immediately to a specified address (before the RPC call returns)

Signature:

```text
generatetoaddress nblocks "address" ( maxtries )
```

Example call:

```bash
zerohour-cli generatetoaddress 1 'ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr'
```

Example JSON-RPC request:

```bash
curl --user "$COOKIE" \
  --data-binary '{"jsonrpc":"1.0","id":"curltest","method":"generatetoaddress","params":[1,"ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr"]}' \
  -H 'content-type:text/plain;' \
  http://127.0.0.1:3889/
```

Example response shape:

```text
Result:
[ blockhashes ]     (array) hashes of blocks generated
```

## Full Method Index

| Method | Safety | Signature |
| --- | --- | --- |
| `generate` | Advanced | `generate nblocks ( maxtries )` |
| `generatetoaddress` | Advanced | `generatetoaddress nblocks "address" ( maxtries )` |
