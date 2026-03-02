<p align="center">
  <img src="assets/images/icon.png" width="120" alt="ProofSnap Logo" />
</p>

<h1 align="center">ProofSnap</h1>

<p align="center">
  <strong>Capture. Hash. Sign. Verify. Trust.</strong>
</p>

<p align="center">
  A mobile app that combats deepfakes and media manipulation by generating cryptographic proofs at the moment of capture, anchoring them on the <b>DataHaven blockchain</b>, and running AI-powered authenticity detection — giving every photo an unforgeable <b>Trust Score</b>.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Expo-54-000020?logo=expo&logoColor=white" />
  <img src="https://img.shields.io/badge/React%20Native-0.81-61DAFB?logo=react&logoColor=white" />
  <img src="https://img.shields.io/badge/TypeScript-5.9-3178C6?logo=typescript&logoColor=white" />
  <img src="https://img.shields.io/badge/Solidity-0.8.20-363636?logo=solidity&logoColor=white" />
  <img src="https://img.shields.io/badge/DataHaven-Testnet-6C3CE1" />
  <img src="https://img.shields.io/badge/License-MIT-green" />
</p>

---

## Table of Contents

- [The Problem](#-the-problem)
- [How ProofSnap Solves It](#-how-proofsnap-solves-it)
- [Features](#-features)
- [Architecture](#-architecture)
- [Tech Stack](#-tech-stack)
- [Screenshots & Demo](#-screenshots--demo)
- [Getting Started](#-getting-started)
- [Project Structure](#-project-structure)
- [Verification Pipeline](#-verification-pipeline)
- [Trust Score Algorithm](#-trust-score-algorithm)
- [Smart Contract](#-smart-contract)
- [External Services Setup](#-external-services-setup)
- [API Reference](#-api-reference)
- [Security Architecture](#-security-architecture)
- [Real-World Use Cases](#-real-world-use-cases)
- [Cost](#-cost)
- [Contributing](#-contributing)
- [License](#-license)

---
APP Link : https://drive.google.com/file/d/1CUZ1MKIMmI70ugoxJvDNGFyOzJV6auPd/view?usp=drive_link
## 🔴 The Problem

Anyone with a laptop can generate a hyper-realistic photo that never happened. Deepfakes are being used to spread misinformation, fabricate evidence, and erode trust in digital media. When someone shows you a photo, **how do you prove it's real?**

## 💡 How ProofSnap Solves It

ProofSnap creates an **unforgeable proof of capture** for every photo. The moment you snap a picture, ProofSnap performs these steps in under 10 seconds:

1. **Generates a SHA-256 cryptographic hash** — a unique digital fingerprint of every pixel
2. **Signs it with your device's Ed25519 key** — proving it came from YOUR phone
3. **Anchors the proof on a real blockchain** — the DataHaven Testnet — making it permanent and tamper-proof
4. **Runs AI deepfake analysis** — detecting manipulation, face-swaps, and AI generation
5. **Computes a Trust Score out of 100** — a single number that tells you: is this media authentic?
6. **Watermarks & syncs to the cloud** — provenance watermark + Supabase backup

**If even a single pixel is changed — a screenshot, a crop, a filter — the hash won't match. Tampering is mathematically impossible to hide.**

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 📸 **Proof-of-Capture** | SHA-256 hash computed from raw file bytes at capture time |
| ✍️ **Digital Signatures** | Ed25519 elliptic-curve key pair signing per device |
| ⛓️ **Blockchain Anchoring** | Immutable proof stored on DataHaven Testnet (EVM, Chain 55931) via a custom Solidity smart contract |
| ☁️ **Cloud Sync** | Proof records + thumbnails synced to Supabase (PostgreSQL + Object Storage) |
| 🤖 **AI Deepfake Detection** | SightEngine API for deepfake, face-swap, and AI-generated content detection |
| 🔍 **Plagiarism Check** | Reverse-image similarity analysis |
| 📊 **Trust Score** | 0–100 weighted score with S / A / B / C / F grading |
| 💧 **Watermarking** | Visible provenance overlay + invisible `PS-XXXX-XXXX` watermark ID |
| 🔎 **3-Mode Verification** | Verify any media by blockchain TX hash, file hash, or image re-hash |
| 📱 **Gallery Scanner** | Batch-scan every image on your phone (WhatsApp, Snapchat, Camera, etc.) and detect tampering |
| 🌙 **Dark / Light Theme** | Automatic system theme detection with custom palettes |
| 📶 **Offline-First** | Local SQLite cache — full functionality without internet; syncs when online |

---

## 🏗 Architecture

```
┌──────────────────────────────────────────────────────────────────┐
│                     React Native (Expo) App                       │
├──────────────┬────────────┬───────────┬──────────┬───────────────┤
│   Capture    │  Gallery   │  Scanner  │ Profile  │ Verify Proof  │
│   Screen     │  Screen    │  Screen   │ Screen   │    Screen     │
├──────────────┴────────────┴───────────┴──────────┴───────────────┤
│                    Zustand State Management                       │
├──────────────────────────────────────────────────────────────────┤
│                   7-Step Verification Pipeline                    │
│  Hash → Sign → Blockchain → AI Detection → Trust Score →         │
│  Watermark → Cloud Sync                                          │
├──────────┬───────────┬──────────┬──────────┬────────┬────────────┤
│  Crypto  │ Blockchain│    AI    │  Trust   │Watermark│  Supabase │
│ Ed25519  │ ethers.js │SightEngine│ 0–100  │Overlay  │   Cloud   │
│ SHA-256  │ DataHaven │ Deepfake │ Scoring │  + ID   │  Storage  │
├──────────┴───────────┴──────────┴──────────┴────────┴────────────┤
│    SQLite (local records)     │   Gallery Scanner DB              │
├───────────────────────────────┴──────────────────────────────────┤
│           Express.js Backend (Vercel) + Admin Dashboard           │
├──────────────────────────────┬───────────────────────────────────┤
│  DataHaven Testnet (EVM)     │  Supabase (PostgreSQL + Storage)  │
│  MediaProof.sol Contract     │  Blockscout Explorer API          │
│  Chain ID: 55931             │                                   │
└──────────────────────────────┴───────────────────────────────────┘
```

---

## 🛠 Tech Stack

| Layer | Technology |
|-------|------------|
| **Mobile Framework** | Expo SDK 54 + React Native 0.81 |
| **Language** | TypeScript 5.9 |
| **Navigation** | Expo Router v6 (file-based routing) |
| **State Management** | Zustand v5 |
| **Cryptography** | SHA-256 (`expo-crypto`) + Ed25519 (`@noble/ed25519`) |
| **Blockchain** | `ethers.js` v6 → DataHaven Testnet (EVM, Chain 55931) |
| **Smart Contract** | Solidity 0.8.20 (`MediaProof.sol`) |
| **Cloud Database** | Supabase (PostgreSQL + Object Storage) |
| **Local Database** | `expo-sqlite` v16 |
| **AI Detection** | SightEngine API (deepfake + AI-generated) |
| **Backend** | Express.js + multer (deployed on Vercel) |
| **Animations** | React Native Reanimated v4 |
| **Styling** | NativeWind / Tailwind CSS v3 |
| **Camera** | `expo-camera` v17 |
| **Key Storage** | `expo-secure-store` (hardware-backed keychain) |

---

## 📸 Screenshots & Demo

> _Take screenshots of the app and place them in the `assets/images/screenshots/` directory._

<!-- Add screenshots here:
| Home | Capture | Verification | Gallery | Scanner | Profile |
|------|---------|-------------|---------|---------|---------|
| ![](assets/images/screenshots/home.png) | ![](assets/images/screenshots/capture.png) | ... | ... | ... | ... |
-->

For a full walkthrough, see the [Demo Script](DEMO_SCRIPT.md).

---

## 🚀 Getting Started

### Prerequisites

- **Node.js** >= 18
- **Expo CLI** — `npm install -g expo-cli`
- **Android device** or emulator (with [Expo Go](https://expo.dev/go))

### Installation

```bash
# Clone the repository
git clone https://github.com/your-username/ProofSnap.git
cd ProofSnap

# Install dependencies
npm install

# Start the Expo dev server
npx expo start
```

### Running on Device

1. Install **Expo Go** from the Play Store / App Store
2. Scan the QR code from the terminal
3. The app loads on your device

### Running on Emulator

```bash
# Android
npx expo start --android

# iOS
npx expo start --ios
```

### Backend Server (Optional)

The app works **fully offline** — without the backend, AI detection returns realistic simulated results.

```bash
cd server
npm install
npm run dev
# Server runs at http://localhost:3001
```

> See the [Full Setup Guide](SETUP.md) for detailed instructions on configuring DataHaven, Supabase, SightEngine, and Vercel deployment.

---

## 📁 Project Structure

```
ProofSnap/
├── app/                        # Expo Router screens
│   ├── _layout.tsx             # Root layout with onboarding guard
│   ├── onboarding.tsx          # 3-page swipeable onboarding
│   ├── verify-proof.tsx        # Multi-mode proof verification (TX / hash / image)
│   ├── (tabs)/                 # Main tab navigation
│   │   ├── _layout.tsx         # Tab bar configuration
│   │   ├── index.tsx           # Home dashboard (stats + recent activity)
│   │   ├── capture.tsx         # Camera capture + live verification modal
│   │   ├── gallery.tsx         # Media gallery grid with filters
│   │   ├── scanner.tsx         # Gallery integrity scanner
│   │   └── profile.tsx         # Profile, wallet, device key, settings
│   └── verify/
│       └── [id].tsx            # Verification detail view
│
├── lib/                        # Core business logic
│   ├── pipeline.ts             # 7-step verification orchestrator
│   ├── crypto.ts               # Ed25519 key management + SHA-256 hashing
│   ├── blockchain.ts           # DataHaven EVM integration (ethers.js)
│   ├── ai-detection.ts         # SightEngine API proxy + simulation fallback
│   ├── trust-score.ts          # Weighted trust scoring algorithm
│   ├── watermark.ts            # Visible/invisible watermarking
│   ├── supabase.ts             # Supabase cloud integration
│   ├── db.ts                   # Local SQLite database layer
│   ├── gallery-scanner.ts      # Device gallery scanner + tamper detection
│   ├── gallery-db.ts           # Gallery scanner persistence
│   └── types.ts                # TypeScript interfaces
│
├── components/                 # Reusable UI components
│   ├── TrustScoreCircle.tsx    # Animated circular score indicator
│   ├── TrustBadge.tsx          # Grade badge (S/A/B/C/F)
│   ├── VerificationSteps.tsx   # Pipeline step timeline
│   └── MediaCard.tsx           # Gallery grid/list card
│
├── stores/
│   └── media-store.ts          # Zustand global state
│
├── constants/
│   ├── config.ts               # API URLs, blockchain config, keys
│   ├── Colors.ts               # Theme palettes
│   ├── abi.ts                  # Smart contract ABI
│   └── theme.ts                # Theme constants
│
├── contracts/
│   └── MediaProof.sol          # Solidity smart contract (0.8.20)
│
├── server/                     # Express.js backend API
│   ├── index.js                # API endpoints + admin dashboard
│   └── package.json
│
├── hooks/                      # Custom React hooks
│   └── useThemeColors.ts       # Dark/light theme hook
│
├── supabase-setup.sql          # Database schema & RLS policies
├── app.json                    # Expo configuration
├── package.json
├── tailwind.config.js
└── tsconfig.json
```

---

## 🔄 Verification Pipeline

When you capture or import a photo, ProofSnap runs a **7-step verification pipeline** (orchestrated by `lib/pipeline.ts`):

```
Step 1: Hash       → SHA-256 digest of raw file bytes
Step 2: Sign       → Ed25519 signature using device private key
Step 3: Blockchain → Anchor proof on DataHaven (smart contract or data tx)
Step 4: AI Detect  → Deepfake + AI-generation analysis via SightEngine
Step 5: Trust      → Compute weighted 0–100 trust score
Step 6: Watermark  → Apply visible overlay + generate invisible ID
Step 7: Cloud Sync → Upload proof record + thumbnail to Supabase
```

Each step reports its status in real-time to the UI via progress callbacks. Steps are fault-tolerant — blockchain or cloud failures are recorded as errors but don't block the rest of the pipeline.

---

## 📊 Trust Score Algorithm

The trust score starts at **100** and applies weighted deductions:

| Factor | Condition | Penalty |
|--------|-----------|---------|
| **Hash integrity** | Hash not verified | **-50** |
| **Signature** | Signature invalid | **-30** |
| **Blockchain** | Not anchored on-chain | **-10** |
| **Deepfake score** | > 0.7 / > 0.4 / > 0.2 | **-40 / -20 / -5** |
| **AI-generated score** | > 0.7 / > 0.4 / > 0.2 | **-30 / -15 / -3** |
| **Plagiarism** | > 50% / > 30% | **-20 / -10** |
| **Metadata** | Has metadata | **+2 bonus** |

### Grade Scale

| Grade | Score Range | Meaning |
|-------|-------------|---------|
| **S** | 95 – 100 | Pristine — cryptographically perfect |
| **A** | 80 – 94 | Verified authentic |
| **B** | 60 – 79 | Minor concerns |
| **C** | 40 – 59 | Significant issues |
| **F** | 0 – 39 | Failed verification |

---

## ⛓ Smart Contract

**`contracts/MediaProof.sol`** — A Solidity 0.8.20 contract deployed on **DataHaven Testnet** (Chain ID `55931`).

### Contract Interface

```solidity
// Store an immutable proof
function anchorProof(bytes32 _fileHash, string _signature, string _publicKey) external

// Retrieve a proof by file hash
function getProof(bytes32 _fileHash) external view returns (Proof memory)

// Check if a proof exists
function proofExists(bytes32 _fileHash) external view returns (bool)

// Get total number of proofs
function getProofCount() external view returns (uint256)
```

### Proof Struct

```solidity
struct Proof {
    bytes32 fileHash;
    string  signature;
    string  publicKey;
    address submitter;
    uint256 timestamp;
    uint256 blockNumber;
}
```

### Deployment Info

| Field | Value |
|-------|-------|
| **Network** | DataHaven Testnet |
| **RPC URL** | `https://services.datahaven-testnet.network/testnet` |
| **Chain ID** | `55931` |
| **Currency** | `MOCK` |
| **Explorer** | `https://datahaven-testnet.explorer.caldera.xyz` |
| **Faucet** | `https://apps.datahaven.xyz/faucet` |
| **Compiler** | Solidity 0.8.20 |

> See [SETUP.md](SETUP.md) for step-by-step deployment instructions via Remix IDE + MetaMask.

---

## 🔧 External Services Setup

ProofSnap is designed to work **fully offline** with simulated results. Enable real features by configuring these services one-by-one:

| Service | Purpose | Required? | Cost |
|---------|---------|-----------|------|
| **DataHaven Testnet** | Blockchain proof anchoring | No (simulated by default) | Free (testnet) |
| **Supabase** | Cloud database + file storage | No (local SQLite used) | Free tier |
| **SightEngine** | Real AI deepfake detection | No (simulated results) | Free (500 ops/month) |
| **Render / Vercel** | Backend API hosting | No (simulated locally) | Free tier |

All configuration goes in **`constants/config.ts`** (mobile) and **`server/.env`** (backend).

> Full setup guide with screenshots: [SETUP.md](SETUP.md)

---

## 📡 API Reference

The Express.js backend (deployed on Vercel) exposes these endpoints:

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/health` | Health check |
| `POST` | `/api/detect` | AI deepfake detection (SightEngine proxy / simulation) |
| `POST` | `/api/plagiarism` | Plagiarism check |
| `POST` | `/api/verify` | Verify proof against Supabase |
| `GET` | `/api/verify/:hash` | Lookup proof by file hash |
| `GET` | `/api/proofs` | Recent proofs feed |
| `POST` | `/api/upload-image` | Upload image to Supabase Storage |
| `GET` | `/admin` | Admin monitoring dashboard |

The backend includes a built-in **admin dashboard** with login, real-time stats, filterable request logs, and auto-refresh.

---

## 🔐 Security Architecture

### Cryptographic Chain of Trust

```
Raw Photo Bytes → SHA-256 Hash → Ed25519 Signature → Blockchain Anchor
```

1. **SHA-256 Hash** — Computed from raw file bytes at capture time using `expo-crypto`
2. **Ed25519 Signature** — Device private key signs the hash; key stored in `expo-secure-store` (hardware-backed keychain)
3. **Blockchain Anchor** — Hash + signature stored immutably on DataHaven via smart contract
4. **Cloud Backup** — Proof record synced to Supabase PostgreSQL with Row Level Security

### Key Storage

| Key | Storage | Access |
|-----|---------|--------|
| Ed25519 Private Key | `expo-secure-store` (hardware keychain) | Device-only |
| Ed25519 Public Key | SQLite + on-chain | Public |
| Wallet Mnemonic | `expo-secure-store` | Device-only |

### Tamper Detection

Any modification to a file changes its SHA-256 hash → **broken chain of trust**. The original hash is immutably stored on the blockchain, so:

- **Screenshots** → Different hash
- **Cropping** → Different hash
- **Filters / edits** → Different hash
- **Re-compression** → Different hash
- **Metadata stripping** → Different hash

---

## 🌍 Real-World Use Cases

- **Journalists** in conflict zones — proving a photo is real, not staged
- **Legal professionals** — submitting digital evidence that courts can trust
- **Insurance companies** — verifying damage photos weren't sourced from the internet
- **Social media platforms** — providing a verified "proof of capture" badge
- **Whistleblowers** — creating an immutable record that can't be denied or erased
- **Content creators** — proving ownership and originality of their work

---

## 💰 Cost

**$0** — Everything uses free tiers:

| Service | Tier | Limit |
|---------|------|-------|
| Expo | Free | Unlimited |
| DataHaven Testnet | Free | MOCK tokens from faucet |
| Supabase | Free | 500 MB database, 1 GB storage |
| SightEngine | Free | 500 operations/month |
| Vercel / Render | Free | Serverless hosting |

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📄 License

MIT — Built for **HackSRM Hackathon**.

---

<p align="center">
  <i>"In the age of AI, truth needs a receipt."</i>
</p>
