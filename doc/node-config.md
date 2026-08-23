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

## Recommended Network-Supporting User Node Config

Use this as the target configuration for a normal user node after the snapshot
is installed and the node is close to being synchronized. This profile helps the
ZHCASH network by accepting inbound peers, relaying transactions, and serving
blocks to other nodes.

```ini
# ZHCASH network-supporting user node config

# Local RPC for wallet tools, local apps, or local proxy
server=1
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
rpcthreads=4
rpcworkqueue=64
rpcservertimeout=120

# P2P: accept inbound peers and help the network
listen=1
upnp=1
dnsseed=1
maxconnections=32
maxuploadtarget=0

# Normal network behavior
blocksonly=0
staking=1

# Desktop-friendly performance settings
dbcache=1024
par=0
txindex=0

# Lightweight logs
debug=0
logtimemicros=0
```

This is the recommended long-term profile for users who want to support the
network. `upnp=1` lets home routers automatically open the ZHCASH P2P port when
possible. `maxuploadtarget=0` means unlimited daily outbound block serving; if a
user has a limited internet plan, set a cap such as `maxuploadtarget=2000`.

## Staking Profile Selection

Choose one of these profiles for staking nodes:

| Profile | Hardware | Goal |
| --- | --- | --- |
| Weak staker | Low-power laptop, old desktop, 4 GB RAM, slow disk | Stake without freezing the machine. |
| Powerful staker | Modern desktop/server, 8-16 GB RAM, SSD/NVMe | Stake and support the network actively. |
| Aggressive staking | Always-on strong machine, stable internet, SSD/NVMe | Maximum network support and fast staking responsiveness. |

For staking-only nodes, keep `txindex=0`. The transaction index is useful for
explorers and API backends, but it is not required for staking and adds disk I/O.

## Weak Staker Config

Use this for older Windows machines, low-power mini PCs, laptops, or systems
where GUI responsiveness matters more than maximum peer count.

```ini
# ZHCASH weak staker config

server=1
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
rpcthreads=2
rpcworkqueue=32
rpcservertimeout=120

# Still help the network, but keep peer/load limits modest
listen=1
upnp=1
dnsseed=1
maxconnections=16
maxuploadtarget=1000

# Staking
blocksonly=0
staking=1
stakecache=0
reservebalance=0

# Low-memory / low-I/O settings
dbcache=512
par=1
txindex=0

debug=0
logtimemicros=0
```

`stakecache=0` reduces memory usage. It can make staking less efficient than on
a stronger machine, but it is safer for weak systems.

## Powerful Staker Config

Use this for a modern user node with SSD/NVMe and enough RAM. This is the best
default profile for users who want to stake and strengthen the network.

```ini
# ZHCASH powerful staker config

server=1
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
rpcthreads=4
rpcworkqueue=64
rpcservertimeout=120

# Strong network participation
listen=1
upnp=1
dnsseed=1
maxconnections=48
maxuploadtarget=0

# Staking
blocksonly=0
staking=1
stakecache=1
reservebalance=0

# Performance
dbcache=2048
par=0
txindex=0

debug=0
logtimemicros=0
```

`stakecache=1` is recommended here because it improves staking performance. It
can use more memory, so avoid it on weak machines.

## Aggressive Staking Config

Use this only for an always-on strong machine with stable internet, SSD/NVMe,
and enough RAM. This profile prioritizes staking responsiveness and network
support over resource conservation.

```ini
# ZHCASH aggressive staking config

server=1
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
rpcthreads=8
rpcworkqueue=128
rpcservertimeout=180

# Maximum network support for a user-operated staker
listen=1
upnp=1
dnsseed=1
maxconnections=96
maxuploadtarget=0

# Aggressive staking
blocksonly=0
staking=1
stakecache=1
reservebalance=0

# High-performance local database/cache settings
dbcache=4096
par=0
txindex=0

debug=0
logtimemicros=0
```

This profile is not recommended for weak Windows desktops. If the GUI becomes
unresponsive, lower `maxconnections`, lower `dbcache`, or use the weak staker
profile.

## Temporary Windows Snapshot Sync Config

Use this profile for a normal Windows node after installing a snapshot. It
reduces peer load, disables staking during catch-up, and keeps RPC local. It is
not the best long-term network-supporting profile; switch to the profile above
after the first sync.

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

After the node is fully synchronized, switch to the network-supporting profile
above. At minimum, change:

```ini
listen=1
upnp=1
blocksonly=0
staking=1
maxuploadtarget=0
```

## Alternative Synced Home Node Config With Upload Cap

Use this profile if the user wants to help the network but also wants a daily
upload cap.

```ini
server=1
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
rpcthreads=4
rpcworkqueue=64
rpcservertimeout=120

# P2P: accept inbound peers and ask the router to map the port automatically
listen=1
upnp=1
dnsseed=1
maxconnections=32
maxuploadtarget=2000

# Normal synced-node behavior
blocksonly=0
staking=1

# Desktop-friendly database and index settings
dbcache=1024
par=0
txindex=0

debug=0
logtimemicros=0
```

`upnp=1` is useful for users behind a home router with a public WAN IP. It asks
the router to open the ZHCASH P2P port automatically, so other nodes can connect
inbound. This strengthens the network because more home nodes can serve blocks
and relay transactions.

`upnp=1` does not help when the user is behind CGNAT, double NAT, a mobile
carrier network, or an ISP router that does not expose a real public IP. In
those cases the node can still connect outbound and sync normally, but it will
not become a reachable public peer without a real public IP, port forwarding,
VPN, or another routing solution.

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
| `listen=0` | Temporary first-sync or local-only RPC mode | The node does not accept inbound P2P connections. It still connects out, downloads blocks, syncs, and broadcasts transactions. |
| `listen=1` | Recommended for synced user nodes and public/server nodes | Accepts inbound P2P peer connections and helps serve the network. |
| `upnp=0` | First sync, local-only RPC, servers with manual firewall rules | Prevents automatic router port mapping. Safer and more predictable during initial sync or on managed servers. |
| `upnp=1` | Synced home nodes behind a router | Asks the router to open the P2P port automatically. Useful for users with a public WAN IP who want to help the network. |
| `dnsseed=1` | General use | Finds peers through DNS seeds. |
| `dnsseed=0` | Managed/trusted-peer sync | Disables automatic DNS peer discovery. Use with `addnode`. |
| `addnode=<host>` | Managed sync | Adds a trusted node and tries to keep a connection to it. |
| `maxconnections` | Stability tuning | Limits peer count. Lower values reduce CPU, memory, socket, and upload pressure. |
| `maxuploadtarget` | Network-supporting nodes | Limits outbound traffic per 24 hours in MiB. `0` means unlimited and is best for strengthening the network. |
| `blocksonly=1` | First sync / snapshot catch-up | Rejects network mempool transactions while still accepting blocks and wallet/RPC transactions. Reduces noise during sync. |
| `blocksonly=0` | Fully synced wallet | Enables normal transaction relay and mempool behavior. |
| `staking=0` | First sync | Disables staking while the node catches up. |
| `staking=1` | Synced staking wallet | Enables staking when the wallet is ready and synchronized. |
| `stakecache=0` | Weak stakers | Disables staking cache to reduce memory use. |
| `stakecache=1` | Powerful and aggressive stakers | Enables staking cache for better staking performance, with higher memory use. |
| `reservebalance=0` | Stakers | Allows the full available wallet balance to participate in staking. Increase it to keep part of the balance out of staking. |
| `dbcache` | Performance tuning | Increases database cache in MiB. Higher improves chainstate performance but uses more RAM. |
| `par=0` | Default recommended | Uses automatic script-verification thread selection. |
| `par=1` | Weak machines | Limits script verification parallelism to reduce CPU pressure. |
| `txindex=0` | Normal wallet | Avoids maintaining the full transaction index. Faster and lighter. |
| `txindex=1` | Explorer/API/indexer | Maintains a full transaction index, required for many backend and explorer workflows. |
| `staker-min-tx-gas-price` | Advanced block creation | Minimum gas price for contract transactions included by the staker. Leave unset unless operating a tuned block-production node. |
| `staker-max-tx-gas-limit` | Advanced block creation | Maximum gas limit per contract transaction included by the staker. Leave unset unless you know the intended policy. |
| `staker-soft-block-gas-limit` | Advanced block creation | Soft gas limit for contract execution included in a staker block. Leave unset for normal stakers. |
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
2. `listen=0`, `blocksonly=1`, and `staking=0` may be used only during the
   first catch-up if the node is unstable.
3. The long-term recommended profile is `listen=1`, `upnp=1`, `blocksonly=0`,
   `staking=1`, and `maxuploadtarget=0`.
4. `upnp=1` helps when the router has a public WAN IP. It does not bypass
   CGNAT or double NAT.
5. Keep `txindex=0` unless the machine is used as an indexer or API backend.
6. Prefer a small set of trusted `addnode` peers if random peers are unstable.

This does not remove all possible causes of UI freezes. Slow disks, antivirus
scanning, corrupted snapshots, and low RAM can still make the GUI appear
unresponsive during initial catch-up.
