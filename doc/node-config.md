# Recommended ZHCASH Node Configuration

This page explains the most useful `zerohour.conf` options for stable ZHCASH
node operation, especially on Windows nodes that are started from a blockchain
snapshot and then used by a local wallet, local RPC client, or proxy system.

## Configuration File Location

Mainnet configuration file:

| OS | Path |
| --- | --- |
| Windows | `%APPDATA%\ZHCASH\zerohour.conf` |
| Linux | `$HOME/.zerohour/zerohour.conf` |
| macOS | `$HOME/Library/Application Support/ZHCASH/zerohour.conf` |

Create the file if it does not exist. Restart the node after changing it.

## Recommended Windows Snapshot Sync Config

Use this profile for a normal Windows node after installing a snapshot. It
reduces peer load, disables staking during catch-up, and keeps RPC local.

```ini
# ZHCASH Windows stable sync config

# Local RPC for wallet tools, local apps, or local proxy
server=1
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
rpcthreads=4
rpcworkqueue=64
rpcservertimeout=120

# P2P: do not accept inbound public peer connections
listen=0
upnp=0
dnsseed=1
maxconnections=12
maxuploadtarget=500

# Reduce unnecessary load while catching up
blocksonly=1
staking=0

# Database performance
dbcache=1024
par=0

# Avoid heavy indexes on a normal desktop node
txindex=0

# Keep logs lightweight
debug=0
logtimemicros=0
```

After the node is fully synchronized, a user who wants staking and normal
mempool behavior may change:

```ini
blocksonly=0
staking=1
```

## Recommended Config With Trusted ZHCASH Nodes

If you operate stable seed/full nodes, Windows users can connect only to those
nodes during the first sync. This avoids random slow or outdated peers.

```ini
server=1
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
rpcthreads=4
rpcworkqueue=64
rpcservertimeout=120

listen=0
upnp=0
dnsseed=0
maxconnections=8
maxuploadtarget=500

addnode=node1.zeroscan.io
addnode=node2.zeroscan.io
addnode=node3.zeroscan.io

blocksonly=1
staking=0
dbcache=1024
par=0
txindex=0

debug=0
logtimemicros=0
```

Replace `node1.zeroscan.io`, `node2.zeroscan.io`, and `node3.zeroscan.io` with
real ZHCASH full-node hostnames.

## Recommended Config for a Local RPC / Proxy Node

Use this when a local application or local proxy talks to the node through RPC
on the same machine.

```ini
server=1
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
rpcthreads=8
rpcworkqueue=128
rpcservertimeout=120

listen=0
upnp=0
dnsseed=0
maxconnections=8

addnode=node1.zeroscan.io
addnode=node2.zeroscan.io
addnode=node3.zeroscan.io

blocksonly=1
staking=0
dbcache=1024
par=0
txindex=0

debug=0
```

This keeps RPC private on `127.0.0.1`. Do not expose RPC to the public internet.

## Recommended Config for a Public Infrastructure Node

Use this profile for a server node, explorer backend, indexer, or other
infrastructure component. This node accepts P2P connections and keeps the full
transaction index.

```ini
server=1
daemon=1

listen=1
upnp=0
dnsseed=1
maxconnections=64
maxuploadtarget=0

rpcbind=127.0.0.1
rpcallowip=127.0.0.1
rpcthreads=8
rpcworkqueue=128
rpcservertimeout=120

txindex=1
staking=1

dbcache=2048
par=0

debug=0
logtimemicros=0
```

For a low-memory server, use a smaller profile:

```ini
dbcache=512
maxconnections=32
rpcthreads=4
rpcworkqueue=32
```

## Option Reference

| Option | Recommended Use | Effect |
| --- | --- | --- |
| `server=1` | RPC/API nodes | Enables JSON-RPC server support. Required for external apps using RPC. |
| `daemon=1` | Linux server nodes | Runs `zerohourd` in the background. Not used by Windows GUI. |
| `rpcbind=127.0.0.1` | Default safe setting | Binds RPC only to localhost. |
| `rpcallowip=127.0.0.1` | Default safe setting | Allows RPC only from the same machine. |
| `rpcthreads` | API/proxy nodes | Number of worker threads serving RPC calls. Higher helps when several apps call RPC at once. |
| `rpcworkqueue` | API/proxy nodes | Queue depth for pending RPC calls. Higher helps absorb bursts. |
| `rpcservertimeout` | API/proxy nodes | HTTP RPC timeout in seconds. Higher avoids failures on slow calls. |
| `listen=0` | Desktop / Windows sync | The node does not accept inbound P2P connections. It still connects out, downloads blocks, syncs, and broadcasts transactions. |
| `listen=1` | Public/server nodes | Accepts inbound P2P peer connections and helps serve the network. |
| `upnp=0` | Recommended | Prevents automatic router port mapping. Safer and more predictable. |
| `dnsseed=1` | General use | Finds peers through DNS seeds. |
| `dnsseed=0` | Managed/trusted-peer sync | Disables automatic DNS peer discovery. Use with `addnode`. |
| `addnode=<host>` | Managed sync | Adds a trusted node and tries to keep a connection to it. |
| `maxconnections` | Stability tuning | Limits peer count. Lower values reduce CPU, memory, socket, and upload pressure. |
| `maxuploadtarget` | Desktop nodes | Limits outbound traffic per 24 hours in MiB. `0` means unlimited. |
| `blocksonly=1` | First sync / snapshot catch-up | Rejects network mempool transactions while still accepting blocks and wallet/RPC transactions. Reduces noise during sync. |
| `blocksonly=0` | Fully synced wallet | Enables normal transaction relay and mempool behavior. |
| `staking=0` | First sync | Disables staking while the node catches up. |
| `staking=1` | Synced staking wallet | Enables staking when the wallet is ready and synchronized. |
| `dbcache` | Performance tuning | Increases database cache in MiB. Higher improves chainstate performance but uses more RAM. |
| `par=0` | Default recommended | Uses automatic script-verification thread selection. |
| `txindex=0` | Normal wallet | Avoids maintaining the full transaction index. Faster and lighter. |
| `txindex=1` | Explorer/API/indexer | Maintains a full transaction index, required for many backend and explorer workflows. |
| `debug=0` | Normal use | Keeps debug logging minimal. |
| `logtimemicros=0` | Normal use | Keeps timestamps shorter and logs lighter. |

## Options to Avoid During Normal Sync

Do not enable these unless you know why they are needed:

```ini
reindex=1
rescan=1
debug=net
debug=bench
txindex=1
```

Why:

| Option | Risk |
| --- | --- |
| `reindex=1` | Rebuilds block indexes and can make startup very slow. |
| `rescan=1` | Rescans wallet transactions and can take a long time. |
| `debug=net` | Creates large logs and adds disk I/O. |
| `debug=bench` | Useful for diagnosis, but noisy for normal users. |
| `txindex=1` | Useful for infrastructure, but unnecessary and heavier for normal wallets. |

## Temporary Diagnostic Config

If a node appears stuck, temporarily enable:

```ini
debug=net
debug=bench
logtimemicros=1
zhcslowblockms=500
```

Then reproduce the issue and inspect `debug.log`. Disable these options after
diagnosis to avoid excessive logs.

## Windows Stability Notes

For Windows desktop users:

1. Add the ZHCASH data directory to Microsoft Defender or antivirus exclusions.
2. Use `listen=0` during the first sync.
3. Use `blocksonly=1` and `staking=0` until the node is fully synchronized.
4. Keep `txindex=0` unless the machine is used as an indexer or API backend.
5. Prefer a small set of trusted `addnode` peers if random peers are unstable.

This does not remove all possible causes of UI freezes. Slow disks, antivirus
scanning, corrupted snapshots, and low RAM can still make the GUI appear
unresponsive during initial catch-up.
