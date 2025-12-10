# AidChain

A decentralised platform that brings transparency, traceability, and integrity to humanitarian aid distribution using the Cardano blockchain.

AidChain ensures that donated funds move through a verifiable lifecycle: creation, funding, auditing, proof submission, fund release, and beneficiary confirmation. Each stage is recorded, enforced, and validated using Plutus smart contracts and clear role-based permissions.

---

## Table of Contents

1. [Overview](#overview)  
2. [Core Objectives](#core-objectives)  
3. [Key Features](#key-features)  
4. [System Architecture](#system-architecture)  
5. [Tech Stack](#tech-stack)  
6. [Setup and Installation](#setup-and-installation)  
7. [Smart Contract Workflow](#smart-contract-workflow)  
8. [Testing](#testing)  
9. [Project Structure](#project-structure)  
10. [License](#license)

---

## Overview

Humanitarian aid often suffers from opaque financial flows, fragmented oversight, and poor verification of impact. AidChain uses blockchain guarantees to enforce transparency; funds are only released when an independent auditor verifies that the intended milestone has been met.

The system supports role-based interaction across four stakeholders:

- **NGOs** create campaigns and submit proof of work.  
- **Donors** fund campaigns directly.  
- **Auditors** verify submitted evidence and trigger conditional fund releases.  
- **Beneficiaries** confirm receipt of support using Digital IDs (DID).

---

## Core Objectives

- Provide verifiable evidence of how aid funds are used.  
- Replace institutional trust with cryptographic guarantees.  
- Create immutable audit trails from donation to delivery.  
- Enable independent verification through decentralised infrastructure.  
- Bring financial and operational transparency to NGOs.

---

## Key Features

### Conditional Fund Release
Funds remain locked within Plutus smart contracts; release is only possible after auditor approval.

### Real-Time Campaign Tracking
Each campaign exposes status transitions: created, funded, proven, verified, and completed.

### Role-Based Access and Authentication
Users connect via Cardano wallets. Basic session logic simulates DID-backed identity for Beneficiaries and NGOs.

### Impact Verification NFTs
Upon successful verification, NGOs receive a non-transferable NFT representing validated social impact, minted through a dedicated Plutus policy.

### Local or Mock Execution
The frontend can operate against a local backend API or run in mock mode for offline testing.

---

## System Architecture

AidChain follows a three-layer architecture:


```bash
┌──────────────────────────────────────────────────────────┐
│                        Frontend                           │
│ React + Tailwind | Wallet API | ChainService abstraction  │
└──────────────────────────────────────────────────────────┘
│ REST / Wallet API Calls
▼
┌──────────────────────────────────────────────────────────┐
│                        Backend                            │
│ Node.js + Express | CampaignService | AuthService         │
│ Orchestrates off-chain logic and supports E2E simulations │
└──────────────────────────────────────────────────────────┘
│ Plutus Script Interaction
▼
┌──────────────────────────────────────────────────────────┐
│                     Smart Contracts                       │
│ Haskell / Plutus | On-chain Validators | Minting Policies │
└──────────────────────────────────────────────────────────┘

````

---

## Tech Stack

**Frontend**  
React, Tailwind CSS, JavaScript/TypeScript, Lucide React, Recharts.

**Backend**  
Node.js, Express, Blockfrost API, in-memory storage (simulating a persistent DB).

**Smart Contracts**  
Haskell, Plutus V2.

---

## Setup and Installation

### Prerequisites

- Node.js v18 or later  
- Cabal and GHC (for Plutus contracts)  
- A Cardano wallet extension (Nami, Eternl)

---

### 1. Frontend Setup

```bash
cd frontend
npm install
npm start
````

The application runs at:
[http://localhost:3000](http://localhost:3000)

Enable **Local Backend** from the footer if testing against the API.

---

### 2. Backend Setup

```bash
cd backend
npm install
npm run dev
```

Backend runs at:
[http://localhost:3001](http://localhost:3001)

---

### 3. Smart Contracts

```bash
cd contracts
cabal build
```

---

## Smart Contract Workflow

The on-chain logic manages three sequential steps:

1. **Locking Funds:** Donations are deposited into a validator address linked to a specific campaign.
2. **Verification:** NGOs submit proof; auditors inspect and sign verification data referenced by IPFS/Arweave hashes.
3. **Release:** Validator logic permits withdrawal only when the auditor’s approval is included in the redeemer.

Off-chain components construct transactions and enforce additional constraints before submission.

---

## Testing

A full end-to-end scenario is provided to validate backend behaviour without relying on the UI:

```bash
node scripts/testScenarios.js
```

This exercises the lifecycle: create → donate → submit proof → verify → release.

---

## Project Structure

```
.
├── frontend/               # React DApp
│   ├── src/
│   └── public/
│
├── backend/                # Express API
│   ├── src/services/       # Business logic (Auth, Campaign, Wallet ops)
│   ├── src/controllers/    # HTTP controllers
│   └── src/routes/         # API routes
│
├── contracts/              # Plutus validator + NFT policy
│   └── src/
│
└── scripts/                # Testing utilities and scenarios
```

---

## License

MIT Licence
Developed for the Cardano Hackathon.

```
By Maruh Akporowho
```