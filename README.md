# Sorobox

**A Local-First Development Sandbox for Stellar & Soroban**

Sorobox provides developers with a complete local Stellar and Soroban development environment, eliminating dependence on external test networks, faucets, and fragmented tooling. Through a unified desktop interface and integrated API, developers can create accounts, deploy contracts, inspect transactions, manage snapshots, and test application flows entirely on their local machine.

The role Sorobox aims to fill for Stellar is analogous to what Ganache provides for Ethereum.

---

## Why Sorobox?

Developing on Stellar today requires multiple disconnected tools and services:

- Complex environment setup
- Dependency on external test networks
- Reliance on Friendbot for funding accounts
- Manual service orchestration
- Limited debugging and inspection capabilities
- Difficulty reproducing testing scenarios
- Lack of visual tooling

Sorobox solves this by providing a single, unified local development experience.

---

## Features

### Local Network Management
Start, stop, and monitor a full local Stellar network (Core, Horizon, Soroban RPC) with real-time health visibility. Advance ledgers manually to simulate time progression for testing contract expiration and time-based workflows.

**Why it matters:** No Docker commands, no manual configuration. One click and you have a development network.

---

### Account Management
Create named Stellar accounts with initial balances, fund them from a built-in faucet, view balances, and export keys.

**Why it matters:** No Friendbot. No testnet dependency. Unlimited funds for development and testing.

---

### Transaction Explorer
Browse transaction history, inspect operation details (payments, account creation, trustlines), view ledgers with sequence numbers and close times, and search by transaction hash or account ID.

**Why it matters:** Visual inspection replaces manual Horizon curl commands. See exactly what happened in every operation.

---

### Snapshot System
Save the current blockchain state as a named snapshot and restore it later, enabling instant rollback to known states.

**Why it matters:** Reproducible testing. Set up a scenario once, snapshot it, and restore it instantly for every test run.

---

### Soroban Contract Support
Upload WASM files, deploy Soroban contracts, execute contract methods through a parameter editor, and inspect contract events, execution logs, and transaction logs.

**Why it matters:** Full local smart contract development lifecycle without touching a public network.

---

### Desktop Dashboard
A polished desktop application with a unified dashboard, navigation across all features, application settings, and native OS integration.

**Why it matters:** Everything in one place. No terminal juggling, no context switching.

---

## Architecture

| Layer | Technology | Responsibility |
|-------|-----------|---------------|
| Frontend | React, TypeScript, Tailwind CSS | Dashboard, visualization, user workflows |
| Desktop Runtime | Tauri | Native packaging, OS integration |
| Backend API | Go | Service orchestration, REST API, snapshot management, Stellar integrations |
| Infrastructure | Docker, Stellar Quickstart | Stellar Core, Horizon, Soroban RPC |

---

## Roadmap

**MVP** — Local network management, account system, transaction explorer, snapshot engine, Soroban support, desktop release

**V1.1** — Contract storage inspector, human-friendly error analysis, account templates, scenario export

**V2** — Visual transaction graph, multi-account flow simulator, event replay, failure injection, execution timeline

**Future** — Test automation suite, plugin ecosystem, AI-assisted debugging, team workspaces

---

## Getting Started

### MakeFile

Build the application with tests:
```bash
make all
```

Build the application:
```bash
make build
```

Run the application:
```bash
make run
```

Create DB container:
```bash
make docker-run
```

Shutdown DB Container:
```bash
make docker-down
```

DB Integrations Test:
```bash
make itest
```

Live reload the application:
```bash
make watch
```

Run the test suite:
```bash
make test
```

Clean up binary from the last build:
```bash
make clean
```
