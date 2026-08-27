# Cartesi Engineering Map

[Cartesi](https://cartesi.io) enables developers to build with the languages, libraries, and tools they know and love. Computation runs inside a deterministic RISC-V virtual machine with a full Linux runtime (the **Cartesi Machine**).

This page maps the repositories that make up Cartesi’s current engineering stack, with a focus on the **Rollups v2** architecture under active integration as of mid-2026.

> **Note:** This map is not exhaustive. Older, supporting, and superseded repositories remain public and open source on [`github.com/cartesi`](https://github.com/cartesi) for anyone to inspect, build on, or continue developing.

## Current engineering stack

The Rollups stack is versioned as a dependency chain. Changes propagate in a fixed order: machine → contracts / fraud proofs → node → TypeScript / explorer / CLI → templates.

![Cartesi Rollups v2 engineering stack](./engineering-stack-white.png)

#### How the pieces fit

1. **Cartesi Machine** — deterministic RISC-V + Linux execution ([`machine-emulator`](https://github.com/cartesi/machine-emulator)), with an on-chain micro-architecture step verifier ([`machine-solidity-step`](https://github.com/cartesi/machine-solidity-step)) and guest-side tooling / rootfs ([`machine-guest-tools`](https://github.com/cartesi/machine-guest-tools), Linux image repos).
2. **Rollups contracts** — L1 data availability (`InputBox`), application settlement, portals, Authority / Quorum consensus, and emergency-withdrawal primitives ([`rollups-contracts`](https://github.com/cartesi/rollups-contracts)).
3. **Fraud proofs** — permissionless dispute system ([`dave`](https://github.com/cartesi/dave), PRT today; Dave algorithm next), coupled tightly to contracts and the machine step.
4. **Node** — middleware between L1 contracts, the machine, and clients ([`rollups-node`](https://github.com/cartesi/rollups-node)): advance/inspect, claims, GraphQL / JSON-RPC surfaces.
5. **Sequencer** — optional low-latency soft-confirmation path ([`sequencer`](https://github.com/cartesi/sequencer)); shares the emulator and is converging on node deployment conventions, not yet a required dependency of the SDK release train.
6. **Docs and tooling** — [`CLI`](https://github.com/cartesi/cli), templates, TypeScript clients, explorer, and documentation.

## Start here

- **Understand the architecture** — [Rollups architecture](https://docs.cartesi.io/cartesi-rollups/2.0/getting-started/architecture/)
- **Explore the execution layer** — [Cartesi Machine docs](https://github.com/cartesi/machine-emulator/blob/main/doc/README.md)
- **Build an application** — [Quickstart guide](https://docs.cartesi.io/get-started/quickstart/)


## Repository map

| Repository | Purpose | Status |
| --- | --- | --- |
| [machine-emulator](https://github.com/cartesi/machine-emulator) | Off-chain RISC-V Cartesi Machine (C++). Deterministic Linux VM, Merkle proofs, JSON-RPC / Lua / C APIs. Optional RISC Zero step-proving path. Continuously developed alongside production releases. | Production |
| [machine-solidity-step](https://github.com/cartesi/machine-solidity-step) | On-chain micro-architecture state-transition function; must match the emulator bit-for-bit for fraud proofs. | Production |
| [machine-guest-tools](https://github.com/cartesi/machine-guest-tools) | Guest tools, RISC-V Debian packages, and root filesystem artifacts for machines. | Production |
| [linux](https://github.com/cartesi/linux) | Cartesi-patched Linux kernel sources. | Production |
| [machine-linux-image](https://github.com/cartesi/machine-linux-image) | Build pipeline for the kernel image. | Production |
| [machine-rootfs-image](https://github.com/cartesi/machine-rootfs-image) | Build pipeline for the root filesystem image. | Production |
| [rollups-contracts](https://github.com/cartesi/rollups-contracts) | Rollups smart contracts: InputBox, Application, portals, Authority/Quorum, claim staging, emergency withdrawal. Deployed as a public good on major EVM chains. v2 line in production; v3 alphas under active development. | Production |
| [dave](https://github.com/cartesi/dave) | Permissionless fraud-proof suite (PRT algorithm; Dave algorithm research/next). Contracts + Rust/Lua nodes. Versioned with rollups-contracts. Experimental / alpha. | Experimental |
| [honeypot](https://github.com/cartesi/honeypot) | Hardened ERC-20 vault DApp used as a security reference on Cartesi Rollups. | Production |
| [rollups-node](https://github.com/cartesi/rollups-node) | Reference Rollups node: L1 reader, machine runner, claims, GraphQL / inspect / JSON-RPC. Integration pivot for the SDK. v2 alphas toward stable. | Active development |
| [sequencer](https://github.com/cartesi/sequencer) | Deterministic sequencer with soft confirmations, batch submission, recovery, and watchdog. Prototype / alpha; testnet deployment in progress. | Experimental |
| [cli](https://github.com/cartesi/cli) | Cartesi CLI + SDK image: create, build, run, deploy. Successor to Sunodo branding. v2 alphas. | Active development |
| [application-templates](https://github.com/cartesi/application-templates) | Templates consumed by `cartesi create`. | Active development |
| [rollups-ts](https://github.com/cartesi/rollups-ts) | TypeScript libraries (`@cartesi/rpc`, client/react packages, etc.) for frontends and tooling. | Active development |
| [rollups-explorer](https://github.com/cartesi/rollups-explorer) | Rollups application explorer UI. | Active development |
| [rollups-explorer-api](https://github.com/cartesi/rollups-explorer-api) | Indexer / API behind the Rollups explorer. | Active development |
| [docs](https://github.com/cartesi/docs) | Official documentation (Docusaurus), including machine-readable / LLM-oriented delivery. Continuously updated alongside the stack. | Production |


## Release status

As of mid-2026, the Rollups **v2** stack is progressing through coordinated alpha releases across the node, contracts, fraud-proof system, CLI/SDK, explorer, and TypeScript clients. Cartesi Machine components, including the emulator, Solidity step, and guest tools, continue to publish production releases used by the v2 stack.

The **Status** column in the map above uses three labels:

- **Production**: released and in production use
- **Active development**: continues to evolve as part of the current engineering stack
- **Experimental**: still being tested or validated before broader use
