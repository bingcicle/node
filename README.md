![Base](logo.webp)

# Base Node

Base is a secure, low‑cost, developer‑friendly Ethereum L2 built on Optimism’s [OP Stack](https://stack.optimism.io/). This repository contains Docker builds to run your own node on the Base network.

[![Website](https://img.shields.io/website?url=https%3A%2F%2Fbase.org)](https://base.org)
[![Docs](https://img.shields.io/badge/docs-base.org-green)](https://docs.base.org/)
[![Discord](https://img.shields.io/discord/1067165013397213286?label=discord)](https://base.org/discord)
[![Twitter Base](https://img.shields.io/twitter/follow/Base?style=social)](https://x.com/Base)
[![Farcaster Base](https://img.shields.io/badge/Farcaster_Base-3d8fcc)](https://farcaster.xyz/base)

## Quickstart

1. Ensure you have **Ethereum L1 endpoints** available (both execution RPC *and* beacon).
2. Choose your network env file:
   - Mainnet: `.env.mainnet`
   - Testnet (Sepolia): `.env.sepolia`
3. Configure your L1 endpoints in the chosen `.env`:
   ```bash
   OP_NODE_L1_ETH_RPC=<your L1 execution RPC>
   OP_NODE_L1_BEACON=<your L1 beacon RPC>
   OP_NODE_L1_BEACON_ARCHIVER=<your L1 beacon archiver RPC>
   ```
4. Start the node:
   ```bash
   # Mainnet (recommended explicit env file)
   docker compose --env-file .env.mainnet up --build

   # Testnet (Sepolia)
   docker compose --env-file .env.sepolia up --build

   # Use a specific client (optional), e.g. reth:
   CLIENT=reth docker compose --env-file .env.mainnet up --build

   # Testnet with a specific client
   CLIENT=reth docker compose --env-file .env.sepolia up --build
   ```

### Supported clients

- `geth` (default)
- `reth`
- `nethermind`

## Requirements

### Minimum

- Modern multi‑core CPU
- **32 GB RAM** (64 GB recommended)
- **NVMe SSD**
- **Storage**: `2 × (current chain size) + snapshot size + 20%` buffer  
  See [Base stats](https://base.org/stats) and [snapshot sizes](https://basechaindata.vercel.app) for estimates.
- Docker and Docker Compose v2

### Production hardware (reference)

#### Geth full node
- **Instance**: AWS i4i.12xlarge
- **Storage**: RAID0 across local NVMe (`/dev/nvme*`)
- **Filesystem**: ext4

#### Reth archive node
- **Instance**: AWS i7ie.6xlarge
- **Storage**: RAID0 across local NVMe (`/dev/nvme*`)
- **Filesystem**: ext4

> [!NOTE]
> To select a client at runtime:
> ```bash
> CLIENT=<geth|reth|nethermind> docker compose --env-file .env.mainnet up --build
> ```
> - `reth` supports Flashbots/Flashblocks options; see [reth/README.md](./reth/README.md).

## Configuration

### Required settings

- **L1 configuration**
  - `OP_NODE_L1_ETH_RPC` — Ethereum L1 execution RPC
  - `OP_NODE_L1_BEACON` — L1 beacon endpoint
  - `OP_NODE_L1_BEACON_ARCHIVER` — L1 beacon *archiver* endpoint
  - `OP_NODE_L1_RPC_KIND` — RPC provider type (default: `debug_geth`). Supported values:
    - `alchemy`, `quicknode`, `infura`, `parity`, `nethermind`, `debug_geth`, `erigon`, `basic`, `any`, `standard`

### Network settings

- **Mainnet**
  - `RETH_CHAIN=base`
  - `OP_NODE_NETWORK=base-mainnet`
  - Sequencer: `https://mainnet-sequencer.base.org`

### Performance (Geth)

```bash
GETH_CACHE=20480          # MB (~20 GB total cache)
GETH_CACHE_DATABASE=20    # % of --cache (~4 GB here)
GETH_CACHE_GC=12
GETH_CACHE_SNAPSHOT=24
GETH_CACHE_TRIE=44
```

### Optional features

- EthStats monitoring (uncomment/enable in compose/env)
- Trusted RPC mode (optional)
- Snap sync (experimental)

For the full list of options, see `.env.mainnet` / `.env.sepolia`.

## Snapshots

Snapshots can significantly speed up sync. See the docs for links and restore steps:  
<https://docs.base.org/chain/run-a-base-node#snapshots>

## Supported networks

| Network | Status |
|--------:|:------|
| Mainnet | ✅    |
| Testnet | ✅    |

## Troubleshooting

For support, join our [Discord](https://discord.gg/buildonbase) and post in **🛠｜node-operators**.  
Alternatively, open a new GitHub issue with logs, your client choice, and hardware specs.

## Disclaimer

THE NODE SOFTWARE IS PROVIDED “AS IS” WITHOUT WARRANTY OF ANY KIND. We make no guarantees about asset protection or security. Use at your own risk and follow all applicable laws and regulations.

For more information, visit the [official docs](https://docs.base.org/).
