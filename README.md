<p align="center">
  <img src="https://img.shields.io/badge/🛡️-x402_Payment_Firewall-00D4FF?style=for-the-badge&labelColor=0a0f12" alt="x402 Payment Firewall" />
</p>

<h1 align="center">x402 Payment Firewall</h1>

<p align="center">
  <strong>🔒 The Security Layer That Makes AI Agent Payments Safe</strong>
</p>

<p align="center">
  <a href="https://cronos.org/explorer/testnet3/address/0xC3C4E069B294C8ED3841c87d527c942F873CFAA9#code">
    <img src="https://img.shields.io/badge/🔴_LIVE-Cronos_Testnet-00D4FF?style=for-the-badge" alt="Live on Cronos" />
  </a>
  <a href="https://cronos.org/explorer/testnet3/address/0xC3C4E069B294C8ED3841c87d527c942F873CFAA9#code">
    <img src="https://img.shields.io/badge/✅_VERIFIED-Smart_Contracts-00FF88?style=for-the-badge" alt="Verified" />
  </a>
  <a href="#">
    <img src="https://img.shields.io/badge/Solidity-0.8.19-363636?style=for-the-badge&logo=solidity" alt="Solidity" />
  </a>
</p>

<p align="center">
  <a href="#-the-problem">Problem</a> •
  <a href="#-our-solution">Solution</a> •
  <a href="#-live-demo">Live Demo</a> •
  <a href="#-quick-start">Quick Start</a> •
  <a href="#-architecture">Architecture</a>
</p>

---

## 🚨 The Problem

> **"AI agents are getting wallets. What could go wrong?"**

The x402 protocol enables autonomous AI payments. But **autonomy without security = disaster waiting to happen.**

<table>
<tr>
<td width="50%">

### Without Firewall ❌

```
Agent Key Compromised
         ↓
Attacker has full access
         ↓
💸 ENTIRE WALLET DRAINED
         ↓
No way to stop it
         ↓
Game Over
```

</td>
<td width="50%">

### With Firewall ✅

```
Agent Key Compromised
         ↓
Attacker tries to drain
         ↓
🛡️ FIREWALL BLOCKS
         ↓
Max 10,000 CRO/day limit
         ↓
Damage contained
```

</td>
</tr>
</table>

### Real Threats We Prevent

| Attack Vector | Without Us | With Us |
|--------------|------------|---------|
| 🔓 **Key Compromise** | Total loss | Limited to daily cap |
| 💉 **Prompt Injection** | Unlimited payments | Policy enforced |
| 🏃 **Rug Pull** | Drain everything | Whitelist-only recipients |
| 📈 **Overspending** | No limits | Per-TX + daily limits |

---

## 💡 Our Solution

<p align="center">
  <img src="https://img.shields.io/badge/NOT_WARNINGS-WALLS-FF4757?style=for-the-badge" alt="Not Warnings - Walls" />
</p>

```
                         ┌─────────────────────────────────────┐
                         │      x402 PAYMENT FIREWALL          │
                         │    "The Bouncer for Your Wallet"    │
                         └─────────────────────────────────────┘
                                          │
           ┌──────────────────────────────┼──────────────────────────────┐
           │                              │                              │
           ▼                              ▼                              ▼
      ┌─────────┐                   ┌─────────┐                   ┌─────────┐
      │ 0.01 CRO│                   │ 100 CRO │                   │15000 CRO│
      │ Payment │                   │ Payment │                   │ Payment │
      └────┬────┘                   └────┬────┘                   └────┬────┘
           │                              │                              │
           ▼                              ▼                              ▼
      ┌─────────┐                   ┌─────────┐                   ┌─────────┐
      │✅ ALLOW │                   │✅ ALLOW │                   │❌ BLOCK │
      │ Execute │                   │ Execute │                   │ REVERT! │
      └─────────┘                   └─────────┘                   └─────────┘
```

### How It Works

1. **All payments go through the firewall** - No bypass possible
2. **Policy engine evaluates every transaction** - On-chain, deterministic
3. **Violations = REVERT** - Transaction fails, funds stay safe
4. **Full audit trail** - Every attempt logged on-chain

**The key insight:** Even if an attacker has your private key, they can only operate within your policy limits. The smart contract physically prevents unauthorized transfers.

---

## 🔴 Live Demo

### Deployed & Verified on Cronos Testnet

| Contract | Address | Status |
|----------|---------|--------|
| **X402PaymentFirewall** | [`0xC3C4E069B294C8ED3841c87d527c942F873CFAA9`](https://cronos.org/explorer/testnet3/address/0xC3C4E069B294C8ED3841c87d527c942F873CFAA9#code) | ✅ Verified |
| **X402PolicyEngine** | [`0xD0CE6F16969d81997750afE018A34921DeDd04A0`](https://cronos.org/explorer/testnet3/address/0xD0CE6F16969d81997750afE018A34921DeDd04A0#code) | ✅ Verified |

### 🎬 Demo Video

> *Coming soon - Watch the firewall block a 15,000 CRO payment in real-time!*

### Proof It Works

| Test | Amount | Expected | Result | Evidence |
|------|--------|----------|--------|----------|
| Normal payment | 0.01 CRO | ✅ Allow | ✅ Executed | [View TX](https://cronos.org/explorer/testnet3/tx/0x26f363226771f9e359b6ed74c67eef0d2314bd21e458dcbfde3583e7b460fbae) |
| Over limit | 15,000 CRO | ❌ Block | ❌ Reverted | Policy enforced |
| Blacklisted recipient | Any | ❌ Block | ❌ Reverted | Policy enforced |

---

## 🚀 Quick Start

### Try the Live Frontend

```bash
# Clone the repo
git clone https://github.com/Venkat5599/x402-Intent-Firewall.git
cd x402-Intent-Firewall

# Install & run frontend
cd frontend
npm install
npm run dev

# Open http://localhost:5173
# Connect MetaMask → Cronos Testnet
# Try sending 15,000 CRO → Watch it get BLOCKED! 🛡️
```

### Get Test CRO
1. Visit [Cronos Faucet](https://cronos.org/faucet)
2. Enter your wallet address
3. Receive free tCRO

### Integrate in Your Project (2 Lines!)

```typescript
import { X402Firewall } from './sdk';

// That's it - all payments now go through the firewall
const firewall = new X402Firewall(signer);
await firewall.pay(recipient, '100'); // Policy enforced automatically
```

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                              FRONTEND                                    │
│         React + TypeScript + Vite + TailwindCSS + ethers.js             │
│                                                                          │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐    │
│  │   Wallet    │  │   Policy    │  │  Payment    │  │   Audit     │    │
│  │  Connect    │  │  Display    │  │   Form      │  │    Logs     │    │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘    │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                           SMART CONTRACTS                                │
│                         Cronos Testnet (338)                            │
│                                                                          │
│  ┌──────────────────────────┐      ┌──────────────────────────┐        │
│  │   X402PaymentFirewall    │─────►│    X402PolicyEngine      │        │
│  │                          │      │                          │        │
│  │  • executePayment()      │      │  • evaluate()            │        │
│  │  • registerIntent()      │      │  • Max per TX: 10K CRO   │        │
│  │  • approveIntent()       │      │  • Daily limit: 50K CRO  │        │
│  │  • Emergency pause       │      │  • Sender blacklist      │        │
│  │  • Rate limiting         │      │  • Recipient whitelist   │        │
│  └──────────────────────────┘      └──────────────────────────┘        │
│                                                                          │
│  Events: IntentRegistered, PaymentExecuted, PaymentBlocked              │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 🛡️ Security Policies

All policies are **enforced on-chain**. No off-chain components. No trust assumptions.

| Policy | Default | What Happens on Violation |
|--------|---------|---------------------------|
| **Max Per Transaction** | 10,000 CRO | `REVERT("Exceeds max payment")` |
| **Daily Spending Limit** | 50,000 CRO | `REVERT("Daily limit exceeded")` |
| **Sender Blacklist** | Configurable | `REVERT("Sender blocked")` |
| **Recipient Blacklist** | Configurable | `REVERT("Recipient blacklisted")` |
| **Rate Limiting** | Configurable | `REVERT("Rate limited")` |
| **Emergency Pause** | Owner only | `REVERT("Firewall paused")` |

---

## 📁 Project Structure

```
x402-firewall/
├── 📜 contracts/                    # Solidity smart contracts
│   ├── X402PaymentFirewall.sol      # Main firewall contract
│   ├── X402PolicyEngine.sol         # Policy evaluation logic
│   ├── X402IntentRegistry.sol       # Intent registration
│   └── X402ExecutionRouter.sol      # Execution gate
│
├── 🎨 frontend/                     # React dashboard
│   └── src/
│       ├── App.tsx                  # Main application
│       ├── hooks/useContracts.ts    # Contract interactions
│       └── hooks/useWallet.ts       # MetaMask integration
│
├── 📦 sdk/                          # TypeScript SDK
│   └── index.ts                     # Drop-in integration
│
├── 🔧 scripts/                      # Deployment & testing
│   ├── deploy-firewall.ts           # Deploy to Cronos
│   └── demo-full-flow.ts            # Full demo script
│
└── 📚 docs/                         # Documentation
    ├── ARCHITECTURE.md              # Technical deep-dive
    └── X402_INTEGRATION.md          # Integration guide
```

---

## 🎯 Use Cases

### 1. 🤖 AI Agent Treasury Protection
```
Scenario: AI agent manages 100,000 CRO treasury
Policy:   Max 1,000 CRO/tx, 10,000 CRO/day
Result:   Even if agent is compromised, max loss = 10,000 CRO/day
          (vs. 100,000 CRO without firewall)
```

### 2. 🏛️ DAO Automated Payments
```
Scenario: DAO pays contractors automatically
Policy:   Whitelist-only recipients
Result:   Funds can ONLY go to approved addresses
          Unauthorized addresses = REVERT
```

### 3. 💳 Subscription Services
```
Scenario: User authorizes recurring payments
Policy:   Max 100 CRO, specific recipient only
Result:   Service cannot overcharge or redirect funds
```

---

## 🏆 Why This Wins

<table>
<tr>
<td>

### Technical Excellence
- ✅ **Deployed on Cronos** - Live, working contracts
- ✅ **Verified source code** - Transparent, auditable
- ✅ **Gas optimized** - Efficient on-chain checks
- ✅ **No external dependencies** - Pure Solidity

</td>
<td>

### Real-World Impact
- ✅ **Solves real problem** - Agent security is unsolved
- ✅ **x402 native** - Built for the protocol
- ✅ **Production ready** - Emergency pause, rate limits
- ✅ **Developer friendly** - 2-line SDK integration

</td>
</tr>
</table>

---

## 📈 Roadmap

- [x] ✅ Core contracts deployed & verified
- [x] ✅ Policy enforcement working
- [x] ✅ Frontend dashboard
- [x] ✅ TypeScript SDK
- [x] ✅ Documentation
- [ ] 🎬 Demo video
- [ ] 🔐 Security audit
- [ ] 🌐 Mainnet deployment
- [ ] 📦 npm package publish
- [ ] ⛓️ Multi-chain support

---

## 🔗 Links & Resources

| Resource | Link |
|----------|------|
| 📜 **Firewall Contract** | [View Verified Code](https://cronos.org/explorer/testnet3/address/0xC3C4E069B294C8ED3841c87d527c942F873CFAA9#code) |
| 📜 **PolicyEngine Contract** | [View Verified Code](https://cronos.org/explorer/testnet3/address/0xD0CE6F16969d81997750afE018A34921DeDd04A0#code) |
| 📝 **Architecture Docs** | [ARCHITECTURE.md](./docs/ARCHITECTURE.md) |
| 📝 **Integration Guide** | [X402_INTEGRATION.md](./docs/X402_INTEGRATION.md) |
| 🧪 **Demo Transaction** | [View on Explorer](https://cronos.org/explorer/testnet3/tx/0x26f363226771f9e359b6ed74c67eef0d2314bd21e458dcbfde3583e7b460fbae) |

---

## 🛠️ Tech Stack

<p align="center">
  <img src="https://img.shields.io/badge/Solidity-363636?style=for-the-badge&logo=solidity&logoColor=white" />
  <img src="https://img.shields.io/badge/Hardhat-FFF100?style=for-the-badge&logo=hardhat&logoColor=black" />
  <img src="https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black" />
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white" />
  <img src="https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white" />
  <img src="https://img.shields.io/badge/TailwindCSS-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white" />
  <img src="https://img.shields.io/badge/ethers.js-2535A0?style=for-the-badge&logo=ethereum&logoColor=white" />
</p>

---

<div align="center">

## 🏆 Built for Cronos x402 Hackathon 2025

<br />

**Real Security. Real Enforcement. Real Protection.**

<br />

<img src="https://img.shields.io/badge/NOT_WARNINGS-WALLS-FF4757?style=for-the-badge" alt="Not Warnings - Walls" />

<br /><br />

*When AI agents control money, you need more than warnings.*  
*You need walls.*

<br />

---

<sub>Made with 💙 for the Cronos ecosystem</sub>

</div>
