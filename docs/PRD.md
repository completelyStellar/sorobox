# Sorobox PRD (Product Requirements Document)

## Version

v1.0

## Status

Planning

## Project Name

Sorobox

## Tagline

A Local-First Development Sandbox for Stellar & Soroban

---

# 1. Executive Summary

Sorobox is a desktop application that provides developers with a complete local Stellar and Soroban development environment.

The platform simplifies blockchain development by orchestrating local Stellar infrastructure, eliminating dependence on external test networks, faucets, and fragmented tooling. Through a unified desktop interface and integrated CLI, developers can create accounts, deploy contracts, inspect transactions, manage snapshots, and test application flows entirely on their local machine.

Sorobox aims to become the default local development environment for the Stellar ecosystem, serving a role similar to Ganache in Ethereum development.

---

# 2. Problem Statement

Developing on Stellar currently requires multiple disconnected tools and services.

Common challenges include:

* Complex environment setup
* Dependency on external test networks
* Reliance on Friendbot for funding
* Manual service orchestration
* Limited debugging capabilities
* Difficulty reproducing testing scenarios
* Lack of visual inspection tools

These challenges increase onboarding friction and slow down development.

Sorobox solves this by providing a unified local development experience.

---

# 3. Product Vision

Enable any developer to build, test, debug, and demonstrate Stellar applications locally within minutes.

A new developer should be able to:

Install Sorobox
→ Start Local Network
→ Create Accounts
→ Deploy Contract
→ Test Application
→ Reset Environment

without configuring infrastructure manually.

---

# 4. Goals

## Primary Goal

Provide valuable developer infrastructure for the Stellar ecosystem.

## Secondary Goals

* Improve Stellar developer onboarding
* Accelerate Soroban contract development
* Create reproducible testing environments
* Reduce development setup complexity
* Become a foundational open-source ecosystem tool

---

# 5. Target Users

## Stellar Developers

Need:

* Fast development cycles
* Reliable local testing
* Contract deployment tools

## Soroban Developers

Need:

* Contract debugging
* Event inspection
* Deployment management

## Students & Learners

Need:

* Visual understanding of Stellar concepts
* Easy onboarding experience

## Hackathon Participants

Need:

* Rapid environment setup
* Reproducible demos

## Open Source Contributors

Need:

* Consistent development environments
* Debugging capabilities

---

# 6. Product Scope

## [MVP]

### Local Network Management

Features:

* Start local Stellar network
* Stop network
* Restart network
* Health monitoring
* Service status dashboard

Implementation:

* Powered by Stellar Quickstart
* Managed through Sorobox

---

### Account Management

Features:

* Create accounts
* Delete accounts
* List accounts
* View balances
* Export keys

---

### Local Faucet

Features:

* Fund local accounts
* Adjustable funding amounts
* Unlimited test funds

---

### Transaction Explorer

Features:

* Transaction history
* Operation details
* Ledger viewer
* Account explorer
* Transaction search

---

### Snapshot System

Features:

* Create snapshots
* Restore snapshots
* Delete snapshots
* Snapshot metadata

Purpose:

Allows developers to instantly return to known blockchain states.

---

### Network Reset

Features:

* Complete chain reset
* Restore clean genesis state

---

### Ledger Advancement

Features:

* Advance ledgers manually
* Simulate time progression

Use Cases:

* Contract expiration testing
* Time-based workflow testing

---

### Soroban Contract Deployment

Features:

* Upload WASM
* Deploy contracts
* Deployment history
* Contract registry

---

### Contract Invocation

Features:

* Execute contract methods
* Parameter editor
* Response viewer

---

### Event Viewer

Features:

* Contract events
* Execution logs
* Transaction logs
* Failure inspection

---

### Desktop Dashboard

Pages:

* Overview
* Accounts
* Explorer
* Contracts
* Snapshots
* Settings

---

## [V1.1]

### Contract Storage Inspector

Features:

* View contract storage
* Key/value inspection
* Storage growth monitoring

---

### Human-Friendly Error Analysis

Converts raw Stellar errors into meaningful messages.

Example:

Instead of:

tx_insufficient_balance

Display:

Insufficient Balance
Required: 100 XLM
Available: 25 XLM

---

### Account Templates

Predefined environments:

* Alice
* Bob
* Merchant
* Treasury
* Issuer
* Distributor

---

### Scenario Export

Export local environments into portable files.

Formats:

* JSON
* YAML

---

## [V2]

### Visual Transaction Graph

Visualize fund movement between accounts.

---

### Multi-Account Flow Simulator

Design and execute payment workflows.

Example:

Customer
→ Merchant
→ Treasury

---

### Event Replay

Replay contract executions and transaction flows.

---

### Failure Injection

Simulate:

* Low balances
* Invalid signatures
* Missing trustlines
* Contract failures

---

### Execution Timeline

Step-by-step transaction analysis.

---

## [Future]

### Test Automation Suite

Execute integration tests through the UI.

---

### Plugin Ecosystem

Community-developed extensions.

---

### AI-Assisted Debugging

Analyze failed transactions and suggest fixes.

---

### Team Workspaces

Shared development environments.

---

# 7. Functional Requirements

## FR-001 Network Startup

Description:

Sorobox shall initialize all required local Stellar infrastructure.

Requirements:

* Start Stellar Quickstart container
* Verify service readiness
* Display startup status

Success Criteria:

Environment available within 60 seconds.

---

## FR-002 Account Creation

Description:

Users shall create local Stellar accounts.

Inputs:

* Account Name
* Initial Balance

Outputs:

* Public Key
* Secret Key
* Balance

---

## FR-003 Account Funding

Description:

Users shall fund local accounts from the built-in faucet.

---

## FR-004 Snapshot Creation

Description:

Users shall save the current blockchain state.

Metadata:

* Name
* Description
* Timestamp

---

## FR-005 Snapshot Restore

Description:

Users shall restore previously saved blockchain states.

---

## FR-006 Contract Deployment

Description:

Users shall deploy Soroban contracts from WASM files.

Outputs:

* Contract ID
* Deployment Status
* Timestamp

---

## FR-007 Contract Invocation

Description:

Users shall execute contract methods using a graphical interface.

---

## FR-008 Event Inspection

Description:

Users shall inspect contract and transaction events.

---

## FR-009 Network Reset

Description:

Users shall reset the local blockchain environment.

---

# 8. Non-Functional Requirements

## Performance

| Metric              | Target   |
| ------------------- | -------- |
| Environment Startup | < 60 sec |
| Account Creation    | < 1 sec  |
| Funding Operation   | < 1 sec  |
| Contract Deployment | < 10 sec |
| Snapshot Save       | < 30 sec |
| Snapshot Restore    | < 30 sec |

---

## Reliability

Target startup success rate:

99%

Supported environments:

* Windows
* Linux
* macOS

---

## Security

Requirements:

* Local-only operation
* No external telemetry
* Secrets remain on-device
* Explicit export required for key sharing

---

## Usability

Requirements:

* New user onboarding under 5 minutes
* No Stellar expertise required
* Visual-first experience

---

# 9. Technical Architecture

## Frontend

Technology:

* React
* TypeScript
* Tailwind CSS

Responsibilities:

* Dashboard
* Visualization
* User workflows

---

## Desktop Runtime

Technology:

* Tauri (preferred)

Responsibilities:

* Native packaging
* OS integration

---

## Backend

Technology:

* Go

Responsibilities:

* Service orchestration
* API layer
* Snapshot management
* Stellar integrations

---

## Infrastructure Layer

Technology:

* Docker
* Stellar Quickstart

Components:

* Stellar Core
* Horizon
* Soroban RPC

---

# 10. Core Services

## Infrastructure Manager [MVP]

Responsibilities:

* Container lifecycle
* Startup validation
* Health monitoring

---

## Account Service [MVP]

Responsibilities:

* Account creation
* Funding
* Balance retrieval

---

## Snapshot Service [MVP]

Responsibilities:

* Save snapshots
* Restore snapshots
* Snapshot catalog

---

## Explorer Service [MVP]

Responsibilities:

* Transactions
* Ledgers
* Accounts

---

## Contract Service [MVP]

Responsibilities:

* Deployment
* Invocation
* Event retrieval

---

# 11. User Journey

Developer installs Sorobox

↓

Launches application

↓

Clicks Start Network

↓

Creates Alice and Bob accounts

↓

Funds accounts

↓

Deploys Soroban contract

↓

Invokes contract methods

↓

Views transactions and events

↓

Creates snapshot

↓

Continues testing

↓

Restores snapshot when needed

---

# 12. Success Metrics

## Ecosystem Metrics

* Stellar Drips acceptance
* Community adoption
* Positive ecosystem feedback

## Product Metrics

* < 5 minute onboarding
* < 60 second startup
* < 1 minute first contract deployment

## Open Source Metrics

Within six months:

* 100+ GitHub stars
* 20+ active users
* Community contributions

---

# 13. Milestones

## Milestone 1 — Infrastructure Foundation [MVP]

Deliverables:

* Quickstart integration
* Docker management
* Health monitoring

---

## Milestone 2 — Account System [MVP]

Deliverables:

* Account management
* Local faucet
* Balance tracking

---

## Milestone 3 — Explorer [MVP]

Deliverables:

* Transactions
* Ledgers
* Dashboard

---

## Milestone 4 — Snapshot Engine [MVP]

Deliverables:

* Snapshot creation
* Snapshot restoration
* Environment reset

---

## Milestone 5 — Soroban Support [MVP]

Deliverables:

* Contract deployment
* Contract invocation
* Event viewer

---

## Milestone 6 — Desktop Release [MVP]

Deliverables:

* Tauri packaging
* Installers
* Documentation
* Public repository

---

# 14. Competitive Positioning

Ganache provides local Ethereum development environments.

Sorobox provides the equivalent experience for Stellar and Soroban while adding modern desktop tooling, visual inspection capabilities, and snapshot-driven testing workflows.

Rather than competing with wallets or payment applications, Sorobox strengthens the entire Stellar ecosystem by improving developer productivity and reducing onboarding friction.
