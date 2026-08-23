# Recommended ZHCASH Node Configuration

This page documents practical `zerohour.conf` profiles and the most important
configuration parameters available in ZHCASH Core v1.0.0.

The goal is to prepare the network for stronger P2P participation,
`ZHP2PProxy`, and PWA wallet support while still giving weak machines a safe
low-resource mode.

The options below were checked against the node source:

| Source | What it defines |
| --- | --- |
| `src/init.cpp` | Core, network, RPC, relay, ZMQ, diagnostics, block creation, ZHC/EVM state options |
| `src/wallet/init.cpp` | Wallet, staking, fees, wallet database options |
| `src/chainparams.cpp` | Mainnet P2P port and DNS seeds |
| `src/chainparamsbase.cpp` | Mainnet RPC port |
| `src/miner.cpp` | `aggressive-staking` behavior |

## Configuration File Location

Mainnet configuration file:

| OS | Path |
| --- | --- |
| Windows | `%APPDATA%\ZHCASH\zerohour.conf` |
| Linux | `$HOME/.zerohour/zerohour.conf` |
| macOS | `$HOME/Library/Application Support/ZHCASH/zerohour.conf` |

Mainnet defaults:

| Purpose | Default |
| --- | --- |
| P2P port | `38100` |
| RPC port | `3889` |

Create the file if it does not exist. Restart the node after changing it.

All configuration examples are comment-free and safe to copy directly into
`zerohour.conf`. Use `key=value` format.

## Profile Selection

| User type | Recommended profile | Main goal |
| --- | --- | --- |
| Standard user, home internet, no manually configured public IP | Standard User / Network-Supporting Node | Support the network, PWA/proxy ready, safe local RPC |
| Weak PC or low-resource laptop | Weak Machine / Weak Staker | Avoid freezes and high disk/RAM pressure |
| Powerful desktop staker | Powerful Staker | Stake, relay, serve blocks, support PWA/proxy |
| Always-on aggressive staker | Aggressive Staking | Maximum staking responsiveness and network support |
| Server node / PWA backend / proxy backend | Server / RPC / PWA Proxy Node | Strong backend for PWA wallet and ZHP2PProxy |
| Explorer / indexer | Infrastructure / Explorer Node | Full indexing and backend RPC capability |

## Standard User / Network-Supporting Node

Use this for normal users. It is designed to strengthen the network even when
the user does not manually configure a public IP. `upnp=1` asks the home router
to open the P2P port automatically. If the user is behind CGNAT or double NAT,
inbound connections may still not work, but the node will still connect outbound
and participate normally.

This profile is also PWA/proxy-ready because it keeps `txindex=1`.

```ini
server=1
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
rpcthreads=4
rpcworkqueue=64
rpcservertimeout=120
listen=1
upnp=1
dnsseed=1
discover=1
maxconnections=32
maxuploadtarget=0
blocksonly=0
staking=1
stakecache=1
reservebalance=0
txindex=1
dbcache=1024
par=0
debug=0
logtimemicros=0
```

Use this as the default long-term user profile after the snapshot is installed.

## Weak Machine / Weak Staker

Use this for older laptops, weak Windows PCs, 4 GB RAM systems, slow disks, or
users who report GUI freezes. This profile still tries to help the network, but
it is not a full PWA/proxy backend because `txindex=0`.

```ini
server=1
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
rpcthreads=2
rpcworkqueue=32
rpcservertimeout=120
listen=1
upnp=1
dnsseed=1
discover=1
maxconnections=12
maxuploadtarget=1000
blocksonly=0
staking=1
stakecache=0
reservebalance=0
txindex=0
dbcache=512
par=1
debug=0
logtimemicros=0
```

Weak nodes can stake and relay normally, but they should not be counted as
reliable PWA wallet / proxy backends.

## Temporary Snapshot Catch-Up Mode

Use this only when a Windows node freezes during the first catch-up after
installing a snapshot. It reduces network and staking pressure. Switch to a
network-supporting profile after sync.

```ini
server=1
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
rpcthreads=4
rpcworkqueue=64
rpcservertimeout=120
listen=0
upnp=0
dnsseed=1
maxconnections=8
maxuploadtarget=500
blocksonly=1
staking=0
stakecache=0
txindex=0
dbcache=512
par=1
debug=0
logtimemicros=0
```

If the machine will later support PWA wallet or proxy calls, enable `txindex=1`
from the beginning when possible. If the node was synced with `txindex=0`,
switching to `txindex=1` later can take additional time while the transaction
index is built.

## Powerful Staker

Use this for a modern desktop/server with SSD or NVMe and at least 8 GB RAM.
This is the recommended staking profile for users who should support PWA/proxy
traffic as well.

```ini
server=1
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
rpcthreads=4
rpcworkqueue=96
rpcservertimeout=120
listen=1
upnp=1
dnsseed=1
discover=1
maxconnections=48
maxuploadtarget=0
blocksonly=0
staking=1
stakecache=1
reservebalance=0
txindex=1
dbcache=2048
par=0
debug=0
logtimemicros=0
```

## Aggressive Staking

Use this only on strong always-on machines with stable internet and SSD/NVMe.
`aggressive-staking=1` checks more often so the node can publish a valid PoS
block as soon as it becomes valid. In code this reduces the wait loop from about
3 seconds to about 100 milliseconds. It may improve responsiveness, but it can
increase stale/orphan risk and early broadcast rejection risk.

```ini
server=1
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
rpcthreads=8
rpcworkqueue=128
rpcservertimeout=180
listen=1
upnp=1
dnsseed=1
discover=1
maxconnections=96
maxuploadtarget=0
blocksonly=0
staking=1
stakecache=1
aggressive-staking=1
reservebalance=0
txindex=1
dbcache=4096
par=0
debug=0
logtimemicros=0
```

If the GUI becomes unresponsive, lower `maxconnections`, lower `dbcache`, remove
`aggressive-staking=1`, or use the weak profile.

## Server / RPC / PWA Proxy Node

Use this for a backend node that supports an API service, PWA wallet, or
`ZHP2PProxy`. RPC is private by default. On managed servers, use firewall rules
and keep RPC reachable only from trusted local services or private networks.

```ini
server=1
daemon=1
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
rpcthreads=12
rpcworkqueue=256
rpcservertimeout=180
listen=1
upnp=0
dnsseed=1
discover=1
maxconnections=128
maxuploadtarget=0
blocksonly=0
staking=0
txindex=1
addrindex=1
dbcache=4096
par=0
maxmempool=600
debug=0
logtimemicros=0
```

If the server also stakes, change:

```ini
staking=1
stakecache=1
reservebalance=0
```

Do not expose RPC directly to the public internet. Put public services behind a
proxy/API layer.

## Infrastructure / Explorer Node

Use this for explorers, indexers, analytics, and backend services that need
complete query capability. This is heavier than a normal node.

```ini
server=1
daemon=1
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
rpcthreads=16
rpcworkqueue=512
rpcservertimeout=300
listen=1
upnp=0
dnsseed=1
discover=1
maxconnections=192
maxuploadtarget=0
blocksonly=0
staking=0
txindex=1
addrindex=1
dbcache=6144
par=0
maxmempool=1000
zhcstatecache=512
zhcstatewritebuffer=128
zhcstatelookupcache=512
debug=0
logtimemicros=0
```

Use only on machines with enough RAM and fast storage.

## ZHP2PProxy Preparation

The target direction is that the ZHCASH network can support `ZHP2PProxy`
gradually, with proxy functionality first tested as an external layer and later
integrated into the node code where appropriate.

Proxy-ready nodes should use:

```ini
listen=1
dnsseed=1
blocksonly=0
txindex=1
maxuploadtarget=0
```

For home nodes behind a router:

```ini
upnp=1
```

For managed servers:

```ini
upnp=0
```

Weak machines may use `txindex=0`, lower `maxconnections`, and lower `dbcache`,
but they should not be treated as PWA/proxy backend nodes.

## PWA Wallet Usage Model

The PWA wallet can use this system by connecting to a `ZHP2PProxy` or backend
API layer instead of depending on one overloaded public node.

```text
PWA wallet
  -> ZHP2PProxy / API gateway
  -> pool of proxy-ready ZHCASH nodes
  -> ZHCASH P2P network
```

For the PWA wallet, proxy-ready nodes should provide:

| PWA wallet need | Node requirement |
| --- | --- |
| Current chain status | `getblockchaininfo`, `getblockcount`, `getbestblockhash` |
| Transaction lookup | `txindex=1` and `getrawtransaction` |
| Address and history backend indexing | Stable RPC and full historical transaction access |
| Transaction broadcast | `blocksonly=0` and working P2P connections |
| Fast failover | Multiple proxy-ready nodes behind `ZHP2PProxy` |
| Network resilience | `listen=1`, enough peers, and sufficient upload capacity |

## ZHP2PProxy Readiness Checklist

Before a node is used by `ZHP2PProxy` or a PWA wallet backend, verify:

```bash
zerohour-cli getblockchaininfo
zerohour-cli getnetworkinfo
zerohour-cli getconnectioncount
zerohour-cli getpeerinfo
zerohour-cli getmempoolinfo
zerohour-cli getindexinfo
```

Expected state:

| Check | Expected result |
| --- | --- |
| Sync | `blocks` is close to `headers`. |
| Initial block download | `initialblockdownload` is `false` when present. |
| P2P | `getconnectioncount` is greater than zero. |
| Relay | `blocksonly=0` for proxy-ready nodes. |
| Transaction index | `txindex=1` and historical `getrawtransaction <txid> true` works. |
| RPC safety | RPC is private unless protected by VPN/firewall/proxy. |
| Upload | `maxuploadtarget=0` for nodes intended to strengthen the network. |
| Logs | No repeated database corruption, reindex loop, or constant socket timeouts. |

For staking nodes, also verify:

```bash
zerohour-cli getstakinginfo
```

## Full Practical Parameter Reference

This is not a recommendation to set every option. It is a practical reference
for options exposed by the node code.

### Core and Data Directory

| Option | Use |
| --- | --- |
| `conf=<file>` | Use a non-default config file. |
| `datadir=<dir>` | Use a non-default data directory. Avoid changing this for normal users. |
| `blocksdir=<dir>` | Store block files outside the main data directory. Advanced only. |
| `includeconf=<file>` | Include another config file from the data directory. |
| `pid=<file>` | PID file path for daemon/service setups. |
| `daemon=1` | Run in background on Linux/server nodes. |
| `server=1` | Enable RPC server. Required for CLI, API, proxy, and PWA backends. |
| `sysperms=1` | Use system default file permissions. Usually avoid. |
| `alertnotify=<cmd>` | Run a command for alerts or long forks. Operations/monitoring only. |
| `blocknotify=<cmd>` | Run a command when the best block changes. Useful for indexers. |
| `loadblock=<file>` | Import blocks from an external blk file. Advanced recovery/import. |
| `help-debug=1` | Show debug/test help in CLI help output. Not a normal config setting. |

### Indexing and Storage

| Option | Use |
| --- | --- |
| `txindex=1` | Full transaction index. Required for PWA/proxy, explorers, APIs, arbitrary `getrawtransaction`. |
| `txindex=0` | Weak nodes only. Reduces disk I/O but is not PWA/proxy-ready. |
| `addrindex=1` | Address index. Useful for explorers/backends if supported by your workflows. Heavier than normal wallet mode. |
| `prune=<MiB>` | Pruned mode. Do not use with `txindex=1`. Not recommended for PWA/proxy nodes. |
| `dbcache=<MiB>` | Main database cache. Higher helps performance but uses RAM. |
| `dbbatchsize=<bytes>` | Advanced database write batch tuning. Leave unset normally. |
| `reindex=1` | Rebuild block index from block files. Use only for recovery. Remove after use. |
| `reindex-chainstate=1` | Rebuild chainstate from indexed blocks. Use only for recovery. Remove after use. |
| `deleteblockchaindata=1` | Deletes local blockchain data. Dangerous; installer/recovery only. |

### P2P Network

| Option | Use |
| --- | --- |
| `listen=1` | Accept inbound P2P peers. Recommended for network-supporting nodes. |
| `listen=0` | Outbound-only mode. Temporary sync mode or local-only setups. |
| `upnp=1` | Ask home router to map the P2P port automatically. Good for users without manual port forwarding. |
| `upnp=0` | Recommended on managed servers with manual firewall rules. |
| `port=38100` | Mainnet P2P port. Usually leave default. |
| `bind=<addr>` | Bind P2P to a specific local interface. Server advanced. |
| `whitebind=<addr>` | Bind and whitelist peers on that interface. Advanced/trusted networks. |
| `externalip=<ip>` | Announce a known public IP. Use only when correct. |
| `discover=1` | Discover own IP addresses. Good for normal listening nodes. |
| `dnsseed=1` | Use DNS seeds to find peers. Recommended generally. |
| `forcednsseed=1` | Always query DNS seeds. Useful for peer discovery troubleshooting. |
| `dns=1` | Allow DNS lookups for `addnode`, `seednode`, and `connect`. |
| `addnode=<host>` | Keep a connection to a specific node. Can be repeated. |
| `seednode=<host>` | Connect to retrieve peer addresses, then disconnect. |
| `connect=<host>` | Connect only to specified node(s). Use carefully; disables normal peer discovery behavior. |
| `maxconnections=<n>` | Maximum peer count. Higher supports the network but uses more resources. |
| `maxuploadtarget=<MiB>` | Daily upload target. `0` means unlimited. |
| `maxreceivebuffer=<n>` | Per-connection receive buffer in thousands of bytes. Advanced. |
| `maxsendbuffer=<n>` | Per-connection send buffer in thousands of bytes. Advanced. |
| `timeout=<ms>` | Outbound connection timeout. Leave default normally. |
| `peertimeout=<sec>` | Peer inactivity timeout. Leave default unless diagnosing peer issues. |
| `banscore=<n>` | Misbehavior score threshold. Leave default. |
| `bantime=<sec>` | Misbehaving peer ban duration. Leave default. |
| `onlynet=ipv4/ipv6/onion` | Restrict outgoing network type. Advanced. |
| `proxy=<ip:port>` | SOCKS5 proxy for outbound peers. |
| `onion=<ip:port>` | Separate SOCKS5 proxy for onion peers. |
| `listenonion=1` | Create Tor hidden service if Tor control is configured. |
| `torcontrol=<ip:port>` | Tor control port. |
| `torpassword=<pass>` | Tor control password. |
| `proxyrandomize=1` | Randomize proxy credentials for Tor stream isolation. |
| `peerbloomfilters=1` | Support bloom filters for peers. Usually leave default. |
| `permitbaremultisig=1` | Relay non-P2SH multisig. Leave default. |
| `enablebip61=1` | Send reject messages. Legacy/debug use. |
| `maxtimeadjustment=<sec>` | Max peer time offset adjustment. Advanced. |

### Fork Compatibility

| Option | Use |
| --- | --- |
| `forkminpeerversion=<n>` | Minimum peer protocol version after configured height. |
| `forkminpeerheight=<n>` | Height from which the minimum peer protocol version is enforced. |

Use these only when coordinating a planned network upgrade.

### RPC and PWA Backend

| Option | Use |
| --- | --- |
| `rpcbind=127.0.0.1` | Bind RPC to localhost. Recommended default. |
| `rpcallowip=127.0.0.1` | Allow localhost RPC clients. Recommended default. |
| `rpcport=3889` | Mainnet RPC port. Usually leave default. |
| `rpcuser=<user>` | RPC username. Use with `rpcpassword`, or prefer `rpcauth`. |
| `rpcpassword=<password>` | RPC password. Avoid public repos and logs. |
| `rpcauth=<user:salt$hash>` | Safer static RPC auth format. Recommended for production. |
| `rpccookiefile=<path>` | Custom RPC cookie path. Usually leave default. |
| `rpcthreads=<n>` | RPC worker threads. Increase for API/proxy backends. |
| `rpcworkqueue=<n>` | RPC request queue depth. Increase for bursts. |
| `rpcservertimeout=<sec>` | RPC HTTP timeout. Increase for slow backend calls. |
| `rpcserialversion=<n>` | Raw transaction/block serialization version. Advanced compatibility. |
| `rpcmaxcallcontractgas=<n>` | Max gas for local `callcontract` simulation only. |
| `rpcmaxgasprice=<n>` | Max gas price allowed through RPC wallet operations. |
| `rest=1` | Enable REST interface. Usually avoid unless needed behind trusted proxy. |

Do not expose RPC directly to the internet. Use a firewall, VPN, or API gateway.

### Wallet and Staking

| Option | Use |
| --- | --- |
| `disablewallet=1` | Disable wallet and wallet RPC. Use for non-staking backend/index nodes. |
| `wallet=<path>` | Load a specific wallet. Can be repeated. |
| `walletdir=<dir>` | Directory for wallets. |
| `walletbroadcast=1` | Let wallet broadcast transactions. Usually keep enabled. |
| `walletnotify=<cmd>` | Run command when wallet transaction changes. |
| `staking=1` | Enable staking. Node must be synced and wallet ready. |
| `staking=0` | Disable staking. Good for temporary sync/backend nodes without wallet. |
| `stakecache=1` | Improve staking performance, uses more memory. |
| `stakecache=0` | Lower memory mode for weak machines. |
| `aggressive-staking=1` | More frequent checks to publish valid PoS block immediately. Strong machines only. |
| `reservebalance=<amount>` | Keep this balance out of staking. Use `0` to stake all available balance. |
| `rescan=1` | Rescan blockchain for wallet transactions. Use only when needed. Remove after use. |
| `salvagewallet=1` | Try to recover private keys from corrupt wallet. Recovery only. |
| `zapwallettxes=<mode>` | Delete wallet tx records and recover via rescan. Recovery only. |
| `upgradewallet=1` | Upgrade wallet format. Use only when intended. |
| `keypool=<n>` | Wallet key pool size. Usually leave default. |
| `addresstype=<type>` | Address type for new receiving addresses. Leave default unless tested. |
| `changetype=<type>` | Change output address type. Leave default unless tested. |
| `avoidpartialspends=1` | Group outputs by address for better privacy; may increase fees. |
| `spendzeroconfchange=1` | Allow spending unconfirmed change. Usually leave default. |
| `usechangeaddress=1` | Use change addresses. Usually leave default. |
| `walletrejectlongchains=1` | Prevent wallet from creating long mempool chains. Usually leave default. |

### Fees and Relay

| Option | Use |
| --- | --- |
| `paytxfee=<amt>` | Fixed wallet transaction fee rate. Usually avoid fixed fees. |
| `fallbackfee=<amt>` | Fee when estimation is unavailable. |
| `mintxfee=<amt>` | Minimum fee for wallet transaction creation. |
| `discardfee=<amt>` | Fee threshold for discarding change as fee. |
| `txconfirmtarget=<n>` | Confirmation target for wallet fee estimation. |
| `minrelaytxfee=<amt>` | Minimum relay fee. Advanced policy. |
| `incrementalrelayfee=<amt>` | Replacement/incremental relay fee. Advanced policy. |
| `dustrelayfee=<amt>` | Dust threshold fee. Advanced policy. |
| `mempoolreplacement=1` | Enable replacement policy. Leave default normally. |
| `walletrbf=1` | Wallet creates opt-in RBF transactions for RPC sends. Wallet policy option. |
| `maxtxfee=<amount>` | Maximum total fee allowed for a wallet transaction. Safety limit. |
| `bytespersigop=<n>` | Sigop relay/mining cost tuning. Advanced. |
| `datacarrier=1` | Relay/mine data carrier transactions. Leave default. |
| `datacarriersize=<n>` | Max data carrier size. Advanced. |
| `whitelistrelay=1` | Relay transactions from whitelisted peers. Advanced. |
| `whitelistforcerelay=1` | Force relay from whitelisted peers. Advanced. |

### Mempool and Performance

| Option | Use |
| --- | --- |
| `maxmempool=<MiB>` | Mempool RAM limit. Increase for servers; reduce for weak machines. |
| `mempoolexpiry=<hours>` | How long mempool transactions are kept. |
| `maxorphantx=<n>` | Max orphan transactions kept in memory. |
| `limitancestorcount=<n>` | Mempool ancestor count policy. Advanced; leave default. |
| `limitancestorsize=<n>` | Mempool ancestor size policy. Advanced; leave default. |
| `limitdescendantcount=<n>` | Mempool descendant count policy. Advanced; leave default. |
| `limitdescendantsize=<n>` | Mempool descendant size policy. Advanced; leave default. |
| `persistmempool=1` | Save mempool on shutdown and restore on startup. |
| `blocksonly=1` | Reject network mempool transactions. Temporary sync mode only for proxy-ready nodes. |
| `blocksonly=0` | Normal relay. Required for PWA/proxy-ready network nodes. |
| `par=0` | Auto script verification threads. |
| `par=1` | Reduce CPU pressure on weak machines. |

### Block Creation and Contract Staker Policy

| Option | Use |
| --- | --- |
| `blockmintxfee=<amt>` | Minimum fee rate for transactions included in created blocks. |
| `blockmaxweight=<n>` | Max block weight. Advanced. |
| `blockversion=<n>` | Override block version for tests. Do not use on mainnet. |
| `blockreconstructionextratxn=<n>` | Extra transactions kept in memory for compact block reconstruction. Advanced. |
| `staker-min-tx-gas-price=<amt>` | Minimum gas price for contract tx included by staker. |
| `staker-max-tx-gas-limit=<n>` | Max gas limit per contract tx included by staker. |
| `staker-soft-block-gas-limit=<n>` | Soft gas limit for contract execution in staker block. |
| `dgpstorage=1` | Receive DGP data via storage. Advanced. |
| `dgpevm=1` | Receive DGP data via contract call. Default path. |

Leave contract staker policy options unset unless there is a coordinated
network policy.

### ZHC / EVM State Performance

| Option | Use |
| --- | --- |
| `zhcstatecache=<MiB>` | EVM state LevelDB block cache. Useful on strong backend nodes. |
| `zhcstatewritebuffer=<MiB>` | EVM state LevelDB write buffer. |
| `zhcstatemaxopenfiles=<n>` | Max open files for EVM state LevelDB. |
| `zhcstatebloom=<n>` | Bloom filter bits per key for EVM state LevelDB. |
| `zhcstatelookupcache=<MiB>` | In-memory read-through cache for immutable EVM trie nodes. |
| `zhcstateforcecompact=1` | Compact EVM state DBs on startup. Use only for maintenance. |
| `record-log-opcodes=1` | Log all EVM LOG opcode operations to `vmExecLogs.json`. Debug/indexing only. |
| `minmempoolgaslimit=<n>` | Minimum gas limit accepted into mempool. Debug/policy tuning. |

### ZMQ

| Option | Use |
| --- | --- |
| `zmqpubhashblock=<address>` | Publish block hashes. |
| `zmqpubhashtx=<address>` | Publish tx hashes. |
| `zmqpubrawblock=<address>` | Publish raw blocks. |
| `zmqpubrawtx=<address>` | Publish raw transactions. |
| `zmqpubhashblockhwm=<n>` | High water mark for hashblock publisher. |
| `zmqpubhashtxhwm=<n>` | High water mark for hashtx publisher. |
| `zmqpubrawblockhwm=<n>` | High water mark for rawblock publisher. |
| `zmqpubrawtxhwm=<n>` | High water mark for rawtx publisher. |

ZMQ is useful for indexers and explorers. It is not needed for normal wallets.

### Diagnostics and Logging

| Option | Use |
| --- | --- |
| `debug=0` | Normal production/user setting. |
| `debug=<category>` | Enable debug category such as `net` or `bench`. Temporary diagnostics only. |
| `debugexclude=<category>` | Exclude a debug category when broad debug logging is enabled. |
| `debuglogfile=<file>` | Custom debug log file. |
| `feefilter=1` | Tell peers to filter transaction announcements below the node mempool minimum fee. Leave default. |
| `logtimestamps=1` | Include timestamps in logs. |
| `logtimemicros=1` | Microsecond timestamps. Useful for performance debugging. |
| `logips=1` | Include peer IPs in logs. Be careful with privacy. |
| `printtoconsole=1` | Print logs to console. Useful for manual debugging. |
| `shrinkdebugfile=1` | Shrink `debug.log` on startup when debug is not enabled. |
| `zhcslowblockms=<n>` | Log slow block validation above threshold. |
| `zhcslowevmms=<n>` | Log slow EVM execution above threshold. |
| `zhcslowcommitms=<n>` | Log slow EVM commit above threshold. |
| `printpriority=1` | Log transaction priority/fee data when mining. Debug only. |
| `uacomment=<text>` | Add comment to user agent string. |
| `maxsigcachesize=<MiB>` | Limit signature/script cache memory. Advanced performance/debug tuning. |

Temporary freeze diagnostics:

```ini
debug=net
debug=bench
logtimemicros=1
zhcslowblockms=500
zhcslowevmms=500
zhcslowcommitms=500
```

Disable these after collecting logs.

### Consistency Checks and Test Options

| Option | Use |
| --- | --- |
| `checkblocks=<n>` | Number of recent blocks checked on startup. |
| `checklevel=<n>` | Thoroughness of startup block checks. |
| `checkblockindex=1` | Expensive internal block index consistency checks. Debug only. |
| `checkmempool=<n>` | Mempool consistency checks. Debug only. |
| `checkpoints=1` | Use checkpoints to reduce expensive historical verification. Usually leave default. |
| `assumevalid=<hex>` | Assume scripts valid up to a known block. Advanced. |
| `minimumchainwork=<hex>` | Minimum chain work expected. Advanced. |
| `maxtipage=<sec>` | Tip age threshold for IBD. Advanced/debug. |
| `mocktime=<n>` | Fake time for testing. Never use on mainnet. |
| `stopatheight=<n>` | Stop at height. Testing/maintenance. |
| `stopafterblockimport=1` | Stop after block import. Import workflows. |
| `dropmessagestest=<n>` | Randomly drop network messages. Testing only. |
| `addrmantest=1` | Address relay testing on localhost. Testing only. |
| `deprecatedrpc=<method>` | Enable deprecated RPC methods. Compatibility only. |
| `acceptnonstdtxn=1` | Accept non-standard transactions. Test/regtest policy only; avoid on mainnet. |
| `opsenderheight=<n>` | Regtest-only fork height override. Do not use on mainnet. |
| `btcecrecoverheight=<n>` | Regtest-only fork height override. Do not use on mainnet. |
| `constantinopleheight=<n>` | Regtest-only fork height override. Do not use on mainnet. |
| `difficultychangeheight=<n>` | Regtest-only fork height override. Do not use on mainnet. |

### Wallet Database Debug Options

| Option | Use |
| --- | --- |
| `dblogsize=<n>` | Flush wallet database activity after this many MB. Debug/tuning only. |
| `flushwallet=1` | Periodically flush wallet database. Usually leave default. |
| `privdb=1` | Use DB_PRIVATE for wallet DB environment. Debug/advanced only. |

### Header Spam Filter

| Option | Use |
| --- | --- |
| `headerspamfilter=<n>` | Enable/disable header spam filter behavior. |
| `headerspamfiltermaxsize=<n>` | Max tracked header spam filter size. |
| `headerspamfiltermaxavg=<n>` | Max average occurrence size in filter. |
| `headerspamfilterignoreport=<n>` | Ignore peer port when checking header spam. |

Leave these defaults unless diagnosing header spam or peer behavior.

## Options to Avoid in Normal User Configs

Do not put these in normal user configs unless performing a specific recovery,
test, or maintenance action:

```ini
reindex=1
reindex-chainstate=1
rescan=1
salvagewallet=1
zapwallettxes=1
deleteblockchaindata=1
debug=net
debug=bench
zhcstateforcecompact=1
record-log-opcodes=1
mocktime=1
stopatheight=1
```

Remove one-time recovery/debug options after they have been used.

## Practical Recommendations

1. For the network to become stronger, normal users should run with
   `listen=1`, `upnp=1`, `blocksonly=0`, `txindex=1`, and `maxuploadtarget=0`.
2. Users behind CGNAT may not receive inbound peers even with `upnp=1`, but they
   still help through outbound connections, relay, staking, and PWA/proxy-ready
   RPC when local services use them.
3. Weak machines are the only normal case where `txindex=0` is recommended.
4. Server and proxy nodes should run `txindex=1`, `addrindex=1`,
   `blocksonly=0`, `maxuploadtarget=0`, and higher RPC queues.
5. RPC must stay private. Public access should go through a controlled API or
   `ZHP2PProxy` layer.
