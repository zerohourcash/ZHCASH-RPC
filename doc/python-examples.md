# Python JSON-RPC Examples

This page shows how to call ZHCASH JSON-RPC from Python.

There are two common modes:

1. Call your own local ZHCASH node.
2. Call the public read-only RPC endpoint at `https://rpc.zeroscan.st/`.

For production applications, prefer your own node when possible. A public RPC
endpoint is useful for quick tests, prototypes, and read-only queries, but it
should not be treated as a private or guaranteed backend.

## Install Requirements

The examples use `requests`.

```bash
python3 -m pip install requests
```

## Minimal JSON-RPC Helper

```python
import requests


def rpc_call(url, method, params=None, auth=None, timeout=30):
    payload = {
        "jsonrpc": "1.0",
        "id": "python-example",
        "method": method,
        "params": params or [],
    }

    response = requests.post(url, json=payload, auth=auth, timeout=timeout)
    response.raise_for_status()

    data = response.json()
    if data.get("error") is not None:
        raise RuntimeError(data["error"])

    return data["result"]
```

## Calling Your Own Local Node

Default local mainnet RPC endpoint:

```text
http://127.0.0.1:3889/
```

Recommended local `zerohour.conf` for RPC access from the same machine:

```ini
server=1
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
rpcuser=your_rpc_user
rpcpassword=your_strong_rpc_password
```

Python example:

```python
from requests.auth import HTTPBasicAuth


LOCAL_RPC_URL = "http://127.0.0.1:3889/"
LOCAL_RPC_AUTH = HTTPBasicAuth("your_rpc_user", "your_strong_rpc_password")


block_count = rpc_call(LOCAL_RPC_URL, "getblockcount", auth=LOCAL_RPC_AUTH)
print("Current block height:", block_count)

blockchain_info = rpc_call(LOCAL_RPC_URL, "getblockchaininfo", auth=LOCAL_RPC_AUTH)
print("Chain:", blockchain_info["chain"])
print("Blocks:", blockchain_info["blocks"])
print("Headers:", blockchain_info["headers"])

subsidy = rpc_call(LOCAL_RPC_URL, "getsubsidy", [1700000], auth=LOCAL_RPC_AUTH)
print("Subsidy at block 1700000:", subsidy)
```

## Calling Your Own Local Node With Cookie Authentication

If `rpcuser` and `rpcpassword` are not set, ZHCASH Core creates a local cookie
file. `zerohour-cli` reads it automatically. Python can read it too.

Linux cookie path:

```text
$HOME/.zerohour/.cookie
```

Windows cookie path:

```text
%APPDATA%\ZHCASH\.cookie
```

Python example for Linux/macOS:

```python
from pathlib import Path
from requests.auth import HTTPBasicAuth


LOCAL_RPC_URL = "http://127.0.0.1:3889/"
COOKIE_PATH = Path.home() / ".zerohour" / ".cookie"

cookie_user, cookie_password = COOKIE_PATH.read_text().strip().split(":", 1)
auth = HTTPBasicAuth(cookie_user, cookie_password)

print(rpc_call(LOCAL_RPC_URL, "getblockcount", auth=auth))
```

Python example for Windows:

```python
import os
from pathlib import Path
from requests.auth import HTTPBasicAuth


LOCAL_RPC_URL = "http://127.0.0.1:3889/"
COOKIE_PATH = Path(os.environ["APPDATA"]) / "ZHCASH" / ".cookie"

cookie_user, cookie_password = COOKIE_PATH.read_text().strip().split(":", 1)
auth = HTTPBasicAuth(cookie_user, cookie_password)

print(rpc_call(LOCAL_RPC_URL, "getblockcount", auth=auth))
```

## Calling the Public Zeroscan RPC Node

Public endpoint:

```text
https://rpc.zeroscan.st/
```

Read-only Python example:

```python
PUBLIC_RPC_URL = "https://rpc.zeroscan.st/"


block_count = rpc_call(PUBLIC_RPC_URL, "getblockcount")
print("Current block height:", block_count)

best_hash = rpc_call(PUBLIC_RPC_URL, "getbestblockhash")
print("Best block hash:", best_hash)

blockchain_info = rpc_call(PUBLIC_RPC_URL, "getblockchaininfo")
print("Chain:", blockchain_info["chain"])
print("Blocks:", blockchain_info["blocks"])
print("Headers:", blockchain_info["headers"])

staking_info = rpc_call(PUBLIC_RPC_URL, "getstakinginfo")
print("Staking enabled:", staking_info.get("enabled"))
print("Staking active:", staking_info.get("staking"))
```

## Fetch a Block by Height

```python
PUBLIC_RPC_URL = "https://rpc.zeroscan.st/"


height = 1
block_hash = rpc_call(PUBLIC_RPC_URL, "getblockhash", [height])
block = rpc_call(PUBLIC_RPC_URL, "getblock", [block_hash, 1])

print("Height:", block["height"])
print("Hash:", block["hash"])
print("Time:", block["time"])
print("Transactions:", len(block["tx"]))
```

## Validate a ZHCASH Address

```python
PUBLIC_RPC_URL = "https://rpc.zeroscan.st/"


address = "ZFVAfTbiVQukZGZakhLXv3Tm5qdXtLkMTr"
result = rpc_call(PUBLIC_RPC_URL, "validateaddress", [address])

print("Is valid:", result["isvalid"])
print("Address:", result.get("address"))
```

## Error Handling Example

```python
try:
    result = rpc_call("https://rpc.zeroscan.st/", "getblockhash", [-1])
    print(result)
except requests.HTTPError as exc:
    print("HTTP error:", exc)
except RuntimeError as exc:
    print("RPC error:", exc)
except requests.RequestException as exc:
    print("Network error:", exc)
```

## Security Notes

Do not expose your local RPC port to the public internet.

Safe local binding:

```ini
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
```

If an application on another server needs RPC access, put the node behind a
private network, VPN, firewall allowlist, or dedicated proxy. Never publish
wallet RPC with private-key or transaction-signing methods to the open internet.

For public RPC endpoints, use read-only methods only. Do not send private keys,
wallet passphrases, unsigned sensitive transactions, or operational secrets to a
public RPC service.
