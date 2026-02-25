# Hyperspace Node

![GitHub stars](https://img.shields.io/github/stars/hyperspaceai/hyperspace-node?style=flat-square)
![License](https://img.shields.io/github/license/hyperspaceai/hyperspace-node?style=flat-square)
![Downloads](https://img.shields.io/github/downloads/hyperspaceai/hyperspace-node/total?style=flat-square&label=desktop%20downloads)
![Latest Release](https://img.shields.io/github/v/release/hyperspaceai/hyperspace-node?style=flat-square)

**The world's largest decentralized P2P AI inference network -- 2,000,000+ nodes and counting.**

Hyperspace is a fully decentralized peer-to-peer network for AI inference, built on [libp2p](https://libp2p.io/) (the same protocol stack that powers IPFS). There are no central servers. Every node connects directly to other nodes, discovers peers via DHT, and communicates through GossipSub. You contribute compute, storage, or bandwidth -- and earn points for it.

- **Website:** [https://hyper.space](https://hyper.space)
- **Dashboard:** [https://p2p.hyper.space](https://p2p.hyper.space)
- **Network Stats:** [https://network.hyper.space](https://network.hyper.space)
- **X / Twitter:** [@HyperspaceAI](https://x.com/HyperspaceAI)

---

## Table of Contents

- [Download Stats](#download-stats)
- [Quick Start](#quick-start)
- [Installation](#installation)
- [Client Comparison](#client-comparison)
- [Network Capabilities](#network-capabilities)
- [Architecture](#architecture)
- [Points System](#points-system)
- [Pulse Verification](#pulse-verification)
- [Supported Platforms](#supported-platforms)
- [Migrating from v1](#migrating-from-v1)
- [Troubleshooting](#troubleshooting)
- [License](#license)

---

## Download Stats

**3,653,316+ total downloads** across all clients and releases.

### Desktop App (hyperspace-node)

| Release | Downloads |
|---------|-----------|
| v0.2.3  | 269,794   |
| v0.2.1  | 333,134   |
| v0.1.6  | 24,169    |
| v0.1.5  | 47,376    |
| v0.1.4  | 56,604    |
| v0.1.3  | 48,588    |
| **Desktop Total** | **911,552** |

### CLI (aios-cli)

| Client | Downloads |
|--------|-----------|
| CLI (all releases) | 2,741,764 |

### Combined

| | Count |
|---|---|
| **Grand Total** | **3,653,316+** |

---

## Quick Start

Install and start a node in two commands:

```bash
curl -fsSL https://download.hyper.space/api/install | bash
hyperspace start
```

Common commands after installation:

```bash
hyperspace start                    # Start with auto-detected profile
hyperspace start --profile full     # All capabilities enabled
hyperspace models pull --auto       # Auto-download best models for your GPU
hyperspace models downloaded        # List downloaded models
hyperspace status                   # Check node status
```

---

## Installation

There are four ways to run a Hyperspace node. Choose the one that fits your setup.

### 1. One-Line Install (Recommended)

```bash
curl -fsSL https://download.hyper.space/api/install | bash
```

This auto-detects your platform and environment:

- **Desktop machines** (macOS, Linux with GUI, Windows): Installs both the CLI and the Tray App.
- **Headless servers** (no display): Installs the CLI only.
- Use `--no-tray` to skip tray app installation on desktop machines.

### 2. Tray App (Desktop Users)

The tray app is a lightweight system tray controller that manages the CLI as a background process. It survives reboots, shows real-time stats, and auto-updates.

The install script above handles tray app installation automatically. It downloads the appropriate package for your platform:

- `.dmg` on macOS
- `.deb` on Linux
- `.exe` / `.msi` on Windows

You can also download the tray app manually from the [latest release](https://github.com/hyperspaceai/hyperspace-node/releases/latest).

### 3. CLI Only (Headless Servers)

For servers or machines where you want CLI-only operation:

```bash
curl -fsSL https://download.hyper.space/api/install | bash -s -- --no-tray
```

The CLI runs as a background daemon and is the recommended option for cloud VMs, dedicated GPU servers, and headless machines.

### 4. Browser

Visit [https://p2p.hyper.space](https://p2p.hyper.space) -- no installation required. Runs entirely in your browser using WebGPU and WebLLM. Ideal for trying out the network without installing anything.

---

## Client Comparison

| Feature | Browser | CLI | Tray App |
|---------|---------|-----|----------|
| GPU Access | WebGPU (limited) | Full native GPU | Full native GPU (via CLI sidecar) |
| Models | WebLLM only | GGUF up to 32B params | GGUF up to 32B params |
| Uptime | Tab must stay open | Background daemon | System tray -- survives reboot |
| Inference Speed | ~10-20 tok/s | ~40-80 tok/s (CUDA) | ~40-80 tok/s (CUDA) |
| Ollama Support | No | Yes | Yes |
| Setup | Zero-install | One command | One command (auto-installed) |
| Auto-Update | Always latest | Manual | Built-in updater |

---

## Network Capabilities

Every node can contribute one or more capabilities to the network. The profile system auto-detects what your hardware supports.

| Capability | What It Does | Requires |
|------------|-------------|----------|
| **Inference** | Run LLM inference -- answer prompts, generate text | GPU (4+ GB VRAM) |
| **Embedding** | Generate vector embeddings for semantic search | CPU only |
| **Storage** | Store and serve content blocks via DHT | Disk space |
| **Memory** | Distributed vector database with replication | CPU + storage |
| **Relay** | NAT traversal -- help browser nodes connect to the network | Public IP |
| **Validation** | Verify pulse proofs from other nodes | CPU |
| **Orchestration** | Decompose complex tasks into sub-tasks and route them | GPU |
| **Caching** | Cache inference results for repeated queries | Memory |
| **Proxy** | Residential IP proxy for AI agents | Bandwidth |

### Node Profiles

Profiles are a convenient way to enable a set of capabilities at once:

```bash
hyperspace start --profile full        # All 9 capabilities
hyperspace start --profile inference   # GPU inference + validation + caching
hyperspace start --profile embedding   # CPU-only embedding node
hyperspace start --profile storage     # Storage + memory
hyperspace start --profile relay       # Relay-only (help others connect)
```

---

## Architecture

```
+-------------------------------------------------------------+
|                      APPLICATIONS                           |
|  Web Dashboard  .  CLI Agent  .  Tray App  .  Extension     |
+-------------------------------------------------------------+
|                      SERVICES                               |
|  InferenceRouter . PulseCoordinator . ProxyEngine           |
|  TaskRouter . EmbeddingRouter . ModelDownloader              |
+-------------------------------------------------------------+
|                      NETWORK                                |
|  libp2p . GossipSub . DHT . Circuit Relay . Yamux           |
|  CapabilityRegistry . AgentDirectory . PeerCache            |
+-------------------------------------------------------------+
|                      STORAGE                                |
|  IndexedDB (browser) . SQLite (CLI) . Supabase (sync)       |
+-------------------------------------------------------------+
|                      COMPUTE                                |
|  WebLLM (browser) . node-llama-cpp (native) . Ollama        |
|  WASM Pulse (proof-of-work) . ONNX (embeddings)            |
+-------------------------------------------------------------+
```

**Network layer:** Built on libp2p v3 with GossipSub for pub/sub messaging, Kademlia DHT for peer discovery and content routing, Circuit Relay v2 for NAT traversal (browser nodes), and Yamux for stream multiplexing. All connections are encrypted with Noise.

**Inference layer:** Three-tier model routing -- first checks local capability registry, then queries DHT, then falls back to gossip broadcast. Supports GGUF models via node-llama-cpp (native) and WebLLM (browser). Ollama integration available for CLI and tray app.

**Storage layer:** Content-addressed block storage via DHT. Distributed vector store with configurable replication factor for memory capability.

---

## Points System

Hyperspace uses a two-stream points model that rewards both presence and useful work.

### Presence Points (Passive)

Earned every pulse round (~90 seconds) just for being online and responsive. Factors:

- **Base reward** -- earned each round you participate in.
- **Uptime bonus** -- grows logarithmically with continuous uptime. Caps out gently over ~30 days.
- **Liveness multiplier** -- ramps up over 1-2 weeks of consistent participation. Discourages node churning.
- **Capability bonus** -- up to +49% for running more capabilities. Inference and proxy are weighted highest.

### Work Points (Active)

Earned by serving real requests -- inference queries, proxy traffic, storage operations. Each completed job generates a signed receipt that is recorded on-chain.

### Key Design Principles

- Time-normalized so daily totals remain stable regardless of round frequency.
- Uptime bonus rewards reliability but is not punishing for short outages.
- Capability weights incentivize the capabilities the network needs most.

---

## Pulse Verification

Pulse is the heartbeat protocol that proves nodes are real and have the resources they claim.

1. **Commit phase** -- every ~90 seconds, a deterministic leader is elected. All participants compute a matrix derived from the round seed, build a Merkle tree over the result, and commit the root hash.
2. **Reveal phase** -- the leader selects random challenge indices. Nodes respond with Merkle proofs for those indices.
3. **Verification** -- peers and validators verify the proofs against the committed root. Valid proofs earn presence points; invalid or missing proofs are flagged.

This commit-reveal protocol makes it infeasible to fake participation without actually performing the computation.

---

## Supported Platforms

| Platform | Architecture | CLI | Tray App |
|----------|-------------|-----|----------|
| macOS    | Apple Silicon (arm64) | Yes | Yes (.dmg) |
| macOS    | Intel (x86_64) | Yes | Yes (.dmg) |
| Linux    | x86_64 | Yes | Yes (.deb) |
| Windows  | x86_64 | Yes | Yes (.exe / .msi) |

The browser client works on any platform with a modern browser that supports WebGPU (Chrome 113+, Edge 113+).

---

## Migrating from v1

The original Hyperspace Desktop (v0.x releases in this repo) was a centralized Tauri v1 application that connected to managed servers. In v2, the entire network was rebuilt from scratch as a fully decentralized P2P system using libp2p.

**What you need to know:**

- **v1 points are frozen and preserved.** They are not lost.
- **All new earning happens on v2.** The v2 points system is live.
- **Migration is automatic.** Running `hyperspace start` with an existing v1 identity detects and migrates it to v2.
- **No action required** beyond installing the v2 CLI or tray app using the install command above.

---

## Troubleshooting

### Node not connecting to peers

Check that your firewall allows outbound WebSocket connections on port 4002. The node needs to reach at least one bootstrap node to join the network.

```bash
hyperspace status
```

### Models not downloading

Ensure you have sufficient disk space and an internet connection. Model files range from 1 GB to 20 GB depending on parameter count.

```bash
hyperspace models pull --auto    # Auto-selects models for your VRAM
hyperspace models downloaded     # Check what is already downloaded
hyperspace models delete <name>  # Remove a model to free space
```

### Low inference speed

- Ensure your GPU drivers are up to date (CUDA 12+ for NVIDIA).
- Check that the correct model size is selected for your VRAM. The auto-selector picks the largest model that fits.
- Close other GPU-intensive applications.

### Tray app not starting

On Linux, ensure you have a system tray compatible desktop environment (GNOME with AppIndicator extension, KDE, XFCE). On macOS, check System Settings > Login Items to ensure the app is allowed to launch at startup.

---

## Contributing

Contributions are welcome. The main development happens in the core monorepo. This repository hosts releases and the install script.

For bug reports and feature requests, please [open an issue](https://github.com/hyperspaceai/hyperspace-node/issues).

---

## Links

- **Website:** [https://hyper.space](https://hyper.space)
- **Web Dashboard:** [https://p2p.hyper.space](https://p2p.hyper.space)
- **Network Dashboard:** [https://network.hyper.space](https://network.hyper.space)
- **X / Twitter:** [@HyperspaceAI](https://x.com/HyperspaceAI)

---

## License

MIT License. See [LICENSE](LICENSE) for details.

---

Maintained by [Varun](https://github.com/twobitapps) and the [Hyperspace](https://x.com/HyperspaceAI) team.
