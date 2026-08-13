# Cartesi Github Org

[Cartesi](https://cartesi.io) is a Linux-capable execution environment for blockchain applications. Computation runs off-chain inside a deterministic RISC-V virtual machine (the **Cartesi Machine**).

This document maps the public repositories under [`github.com/cartesi`](https://github.com/cartesi). It is oriented around the **Rollups v2** engineering stack that is under active integration as of mid-2026.


## Current engineering stack

The Rollups stack is versioned as a dependency chain. Changes propagate in a fixed order: machine → contracts / fraud proofs → node → TypeScript / explorer / CLI → templates.

![Cartesi Rollups v2 engineering stack](./engineering-stack-white.png)

**How the pieces fit**

1. **Cartesi Machine** — deterministic RISC-V + Linux execution ([`machine-emulator`](https://github.com/cartesi/machine-emulator)), with an on-chain micro-architecture step verifier ([`machine-solidity-step`](https://github.com/cartesi/machine-solidity-step)) and guest-side tooling / rootfs ([`machine-guest-tools`](https://github.com/cartesi/machine-guest-tools), Linux image repos).
2. **Rollups contracts** — L1 data availability (`InputBox`), application settlement, portals, Authority / Quorum consensus, and emergency-withdrawal primitives ([`rollups-contracts`](https://github.com/cartesi/rollups-contracts)).
3. **Fraud proofs** — permissionless dispute system ([`dave`](https://github.com/cartesi/dave), PRT today; Dave algorithm next), coupled tightly to contracts and the machine step.
4. **Node** — middleware between L1 contracts, the machine, and clients ([`rollups-node`](https://github.com/cartesi/rollups-node)): advance/inspect, claims, GraphQL / JSON-RPC surfaces.
5. **Sequencer** — optional low-latency soft-confirmation path ([`sequencer`](https://github.com/cartesi/sequencer)); shares the emulator and is converging on node deployment conventions, not yet a required dependency of the SDK release train.
6. **Docs and tooling** — [`CLI`](https://github.com/cartesi/cli), templates, TypeScript clients, explorer, and documentation.

---

## Repository map

### Cartesi Machine

| Repository | Purpose | Status |
| --- | --- | --- |
| [machine-emulator](https://github.com/cartesi/machine-emulator) | Off-chain RISC-V Cartesi Machine (C++). Deterministic Linux VM, Merkle proofs, JSON-RPC / Lua / C APIs. Optional RISC Zero step-proving path. | **Production** / ongoing **active development** |
| [machine-solidity-step](https://github.com/cartesi/machine-solidity-step) | On-chain micro-architecture state-transition function; must match the emulator bit-for-bit for fraud proofs. | **Production** / **active development** |
| [machine-guest-tools](https://github.com/cartesi/machine-guest-tools) | Guest tools, RISC-V Debian packages, and root filesystem artifacts for machines. | **Production** / **active development** |
| [linux](https://github.com/cartesi/linux) | Cartesi-patched Linux kernel sources. | **Production** |
| [machine-linux-image](https://github.com/cartesi/machine-linux-image) | Build pipeline for the kernel image. | **Production** |
| [machine-rootfs-image](https://github.com/cartesi/machine-rootfs-image) | Build pipeline for the root filesystem image. | **Production** |

### Rollups Contracts and Fraud proof system

| Repository | Purpose | Status |
| --- | --- | --- |
| [rollups-contracts](https://github.com/cartesi/rollups-contracts) | Rollups smart contracts: InputBox, Application, portals, Authority/Quorum, claim staging, emergency withdrawal. Deployed as a public good on major EVM chains. | **Production** (v2 line) / **active development** (v3 alphas) |
| [dave](https://github.com/cartesi/dave) | Permissionless fraud-proof suite (PRT algorithm; Dave algorithm research/next). Contracts + Rust/Lua nodes. Versioned with rollups-contracts. | **Active development** (experimental / alpha) |

### Application

| Repository | Purpose | Status |
| --- | --- | --- |
| [honeypot](https://github.com/cartesi/honeypot) | Hardened ERC-20 vault DApp used as a security reference on Cartesi Rollups. | **Production** (reference application) |

### Node and sequencer

| Repository | Purpose | Status |
| --- | --- | --- |
| [rollups-node](https://github.com/cartesi/rollups-node) | Reference Rollups node: L1 reader, machine runner, claims, GraphQL / inspect / JSON-RPC. Integration pivot for the SDK. | **Active development** (v2 alphas toward stable) |
| [sequencer](https://github.com/cartesi/sequencer) | Deterministic sequencer with soft confirmations, batch submission, recovery, and watchdog. | **Active development** (prototype / alpha; testnet deployment in progress) |

### Docs and tooling

| Repository | Purpose | Status |
| --- | --- | --- |
| [cli](https://github.com/cartesi/cli) | Cartesi CLI + SDK image: create, build, run, deploy. Successor to Sunodo branding. | **Active development** (v2 alphas) |
| [application-templates](https://github.com/cartesi/application-templates) | Templates consumed by `cartesi create`. | **Active development** |
| [rollups-ts](https://github.com/cartesi/rollups-ts) | TypeScript libraries (`@cartesi/rpc`, client/react packages, etc.) for frontends and tooling. | **Active development** |
| [rollups-explorer](https://github.com/cartesi/rollups-explorer) | Rollups application explorer UI. | **Active development** |
| [rollups-explorer-api](https://github.com/cartesi/rollups-explorer-api) | Indexer / API behind the Rollups explorer. | **Active development** |
| [docs](https://github.com/cartesi/docs) | Official documentation (Docusaurus), including machine-readable / LLM-oriented delivery. | **Production** / **active development** |

### CTSI staking and chain explorer (PoS)

Distinct from application Rollups: Cartesi token staking / PoS node software and the public staking explorer.

| Repository | Purpose | Status |
| --- | --- | --- |
| [noether](https://github.com/cartesi/noether) | Noether node (PoS). | **Production** / maintenance |
| [pos-dlib](https://github.com/cartesi/pos-dlib) | Proof-of-Stake library. | **Production** / maintenance |
| [staking-pool](https://github.com/cartesi/staking-pool) | Staking pool UI / contracts surface. | **Production** / maintenance |
| [explorer](https://github.com/cartesi/explorer) | Cartesi blockchain / staking explorer. | **Production** / maintenance |
| [subgraph](https://github.com/cartesi/subgraph) | The Graph subgraph definitions. | **Production** / maintenance |

---

## Notes on versioning

As of mid-2026, the Rollups **v2** line is shipping coordinated **alphas** (node, contracts, Dave/PRT, CLI/SDK, explorer, TypeScript clients). Cartesi Machine packages (emulator, solidity step, guest tools) publish production releases that the alpha line pins. Treat “production” above as *in production use or release-quality*, and “active development” as *still converging on a stable SDK designation* — especially for contracts that custody funds and for the sequencer before live testnet soak.

For the dependency order used in integration releases, see the stack diagram above: **emulator → contracts / dave → node → rollups-ts / explorer / CLI → templates**.
