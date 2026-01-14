<p align="center">
  <img src="https://img.shields.io/badge/🛡️-x402_Payment_Firewall-00D4FF?style=for-the-badge&labelColor=0a0f12" alt="x402 Payment Firewall" />
</p>

<h1 align="center">x402 Payment Firewall</h1>

<p align="center">
  <strong>On-Chain Security Layer for Autonomous AI Agent Payments on Cronos</strong>
</p>

<p align="center">
  <a href="https://cronos.org/explorer/testnet3/address/0xC3C4E069B294C8ED3841c87d527c942F873CFAA9#code">
    <img src="https://img.shields.io/badge/🔴_LIVE-Cronos_Testnet-00D4FF?style=for-the-badge" alt="Live on Cronos" />
  </a>
  <a href="https://cronos.org/explorer/testnet3/address/0xC3C4E069B294C8ED3841c87d527c942F873CFAA9#code">
    <img src="https://img.shields.io/badge/✅_VERIFIED-Smart_Contracts-00FF88?style=for-the-badge" alt="Verified" />
  </a>
  <img src="https://img.shields.io/badge/Solidity-0.8.19-363636?style=for-the-badge&logo=solidity" alt="Solidity" />
</p>

---

## 📋 Project Overview

**x402 Payment Firewall** is a smart contract security layer that protects autonomous AI agent payments on the Cronos blockchain. It enforces spending policies directly on-chain, ensuring that even if an AI agent's private key is compromised, attackers cannot drain funds beyond configured limits.

### What It Does

- **Enforces spending limits** - Max per transaction, daily caps
- **Blocks unauthorized recipients** - Whitelist/blacklist support  
- **Prevents rapid draining** - Rate limiting between payments
- **Provides emergency controls** - Instant pause capability
- **Creates audit trail** - All attempts logged on-chain

### Key Innovation

Unlike off-chain security (which can be bypassed), our firewall is **enforced at the smart contract level**. The blockchain itself prevents unauthorized transfers - no trust assumptions, no external dependencies.

```
Traditional Security:  Agent → Wallet → Blockchain (no protection)
With Firewall:         Agent → Firewall → Policy Check → Blockchain (protected)
```

---

## 🌐 Why This Matters for Cronos

### The x402 Opportunity

The x402 protocol enables AI agents to make autonomous payments. Cronos is positioning itself as a leader in this space. But **autonomous payments without security = liability**.

### What We Bring to Cronos

| Benefit | Impact |
|---------|--------|
| **Enables Enterprise Adoption** | Companies won't deploy AI agents with unlimited spending power. Our firewall makes it safe. |
| **Reduces Risk** | Limits damage from compromised agents, prompt injection attacks, and bugs |
| **Increases Trust** | Users can authorize AI payments knowing there are guardrails |
| **Native Integration** | Built specifically for Cronos EVM, optimized for CRO payments |

### Market Need

- AI agents managing treasury funds need spending limits
- DAOs automating payments need recipient controls
- Subscription services need payment caps
- **All of these need on-chain enforcement that can't be bypassed**

---

## 🚀 Deployment Information

### Live Contracts on Cronos Testnet

| Contract | Address | Verified |
|----------|---------|----------|
| **X402PaymentFirewall** | `0xC3C4E069B294C8ED3841c87d527c942F873CFAA9` | [✅ View Code](https://cronos.org/explorer/testnet3/address/0xC3C4E069B294C8ED3841c87d527c942F873CFAA9#code) |
| **X402PolicyEngine** | `0xD0CE6F16969d81997750afE018A34921DeDd04A0` | [✅ View Code](https://cronos.org/explorer/testnet3/address/0xD0CE6F16969d81997750afE018A34921DeDd04A0#code) |

### Network Details

```
Network:     Cronos Testnet
Chain ID:    338
RPC URL:     https://evm-t3.cronos.org
Explorer:    https://cronos.org/explorer/testnet3
Currency:    tCRO (test CRO)
```

### Deploy Your Own

```bash
# 1. Clone the repository
git clone https://github.com/Venkat5599/x402-Intent-Firewall.git
cd x402-Intent-Firewall

# 2. Install dependencies
npm install

# 3. Configure environment
cp .env.example .env
# Edit .env with your private key

# 4. Deploy to Cronos Testnet
npx hardhat run scripts/deploy-firewall.ts --network cronosTestnet

# 5. Verify contracts (optional)
npx hardhat run scripts/verify-contracts.ts --network cronosTestnet
```

---

## 📖 How to Use the Contracts

### Option 1: Direct Contract Interaction

#### Execute a Protected Payment

```solidity
// Solidity - Call from your contract
interface IX402Firewall {
    function executePayment(address recipient) external payable;
}

IX402Firewall firewall = IX402Firewall(0xC3C4E069B294C8ED3841c87d527c942F873CFAA9);
firewall.executePayment{value: 100 ether}(recipientAddress);
// If policy violated → transaction REVERTS
// If policy passes → payment executes
```

#### Check If Payment Would Succeed

```solidity
// Simulate before executing
(bool allowed, string memory reason) = firewall.simulatePayment(
    senderAddress,
    recipientAddress,
    amount
);
// allowed = true/false
// reason = "Payment allowed" or "Exceeds max payment limit"
```

### Option 2: JavaScript/TypeScript Integration

```typescript
import { ethers } from 'ethers';

// Connect to firewall
const FIREWALL_ADDRESS = '0xC3C4E069B294C8ED3841c87d527c942F873CFAA9';
const FIREWALL_ABI = [
  'function executePayment(address recipient) payable',
  'function simulatePayment(address sender, address recipient, uint256 amount) view returns (bool allowed, string reason)',
];

const provider = new ethers.BrowserProvider(window.ethereum);
const signer = await provider.getSigner();
const firewall = new ethers.Contract(FIREWALL_ADDRESS, FIREWALL_ABI, signer);

// Simulate first
const [allowed, reason] = await firewall.simulatePayment(
  await signer.getAddress(),
  '0xRecipientAddress',
  ethers.parseEther('100')
);
console.log(allowed ? 'Will succeed' : `Will fail: ${reason}`);

// Execute payment (100 CRO)
const tx = await firewall.executePayment(
  '0xRecipientAddress',
  { value: ethers.parseEther('100') }
);
await tx.wait();
console.log('Payment executed:', tx.hash);
```

### Option 3: Intent-Based Flow (Advanced)

For higher security, use the intent registration flow:

```typescript
// 1. Register intent (announces payment before execution)
const tx1 = await firewall.registerIntent(
  recipientAddress,
  ethers.parseEther('100'),
  3600 // Valid for 1 hour
);
const receipt = await tx1.wait();
const intentHash = receipt.logs[0].args[0]; // Get from IntentRegistered event

// 2. Approve intent (done by authorized agent/backend)
await firewall.approveIntent(intentHash, 15, 'Low risk payment');

// 3. Execute approved intent
await firewall.executeIntent(intentHash, { value: ethers.parseEther('100') });
```

### Contract Functions Reference

| Function | Description | Access |
|----------|-------------|--------|
| `executePayment(recipient)` | Execute payment with policy check | Anyone |
| `simulatePayment(sender, recipient, amount)` | Check if payment would succeed | View |
| `registerIntent(recipient, amount, validFor)` | Register payment intent | Anyone |
| `approveIntent(hash, riskScore, reason)` | Approve registered intent | Agent only |
| `rejectIntent(hash, riskScore, reason)` | Reject registered intent | Agent only |
| `executeIntent(hash)` | Execute approved intent | Intent sender |
| `pause()` / `unpause()` | Emergency controls | Owner only |

---

## 🛡️ Security Policies

All policies are enforced on-chain. Violations cause transaction revert.

| Policy | Default Value | Configurable |
|--------|---------------|--------------|
| Max per transaction | 10,000 CRO | ✅ Yes |
| Daily spending limit | 50,000 CRO | ✅ Yes |
| Sender blacklist | Empty | ✅ Yes |
| Recipient blacklist | Empty | ✅ Yes |
| Rate limit (seconds between tx) | 0 (disabled) | ✅ Yes |
| Emergency pause | Off | ✅ Yes |

### Policy Violation Examples

```
Attempt: Send 15,000 CRO (limit is 10,000)
Result:  REVERT("X402Firewall: Exceeds max payment limit")

Attempt: Send to blacklisted address
Result:  REVERT("X402Firewall: Recipient blacklisted")

Attempt: Exceed daily limit
Result:  REVERT("X402Firewall: Daily limit exceeded")
```

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                              USER/AGENT                                  │
│                    (AI Agent, DApp, or Direct Call)                     │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                        X402PaymentFirewall                               │
│                   0xC3C4E069B294C8ED3841c87d527c942F873CFAA9            │
│                                                                          │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐         │
│  │ Intent Registry │  │ Direct Payments │  │ Emergency Pause │         │
│  │ register/approve│  │ executePayment()│  │ pause/unpause() │         │
│  └────────┬────────┘  └────────┬────────┘  └─────────────────┘         │
│           │                    │                                         │
│           └────────────────────┴──────────────────┐                     │
│                                                   ▼                     │
│                                    ┌─────────────────────────┐          │
│                                    │    Policy Check         │          │
│                                    │    (MUST PASS)          │          │
│                                    └───────────┬─────────────┘          │
└────────────────────────────────────────────────┼────────────────────────┘
                                                 │
                                                 ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                         X402PolicyEngine                                 │
│                   0xD0CE6F16969d81997750afE018A34921DeDd04A0            │
│                                                                          │
│  evaluate(sender, recipient, amount) → (bool allowed, string reason)    │
│                                                                          │
│  Checks:                                                                 │
│  ├── Amount ≤ maxPaymentLimit?                                          │
│  ├── Daily spent + amount ≤ dailyLimit?                                 │
│  ├── Sender not blocked?                                                │
│  ├── Recipient not blacklisted?                                         │
│  └── (If whitelist enabled) Recipient whitelisted?                      │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                    ┌───────────────┴───────────────┐
                    │                               │
                    ▼                               ▼
            ┌─────────────┐                 ┌─────────────┐
            │  ✅ ALLOWED  │                 │  ❌ BLOCKED  │
            │   Transfer   │                 │   REVERT    │
            │   Executes   │                 │   No funds  │
            └─────────────┘                 │   move      │
                                            └─────────────┘
```

---

## 📁 Project Structure

```
x402-Intent-Firewall/
│
├── contracts/                      # Solidity smart contracts
│   ├── X402PaymentFirewall.sol     # Main firewall (1 contract does it all)
│   ├── X402PolicyEngine.sol        # Policy evaluation logic
│   ├── X402IntentRegistry.sol      # Intent registration (standalone)
│   └── X402ExecutionRouter.sol     # Execution router (standalone)
│
├── frontend/                       # React dashboard
│   ├── src/
│   │   ├── App.tsx                 # Main UI component
│   │   ├── hooks/useContracts.ts   # Contract interaction hooks
│   │   └── hooks/useWallet.ts      # MetaMask integration
│   └── package.json
│
├── sdk/                            # TypeScript SDK for easy integration
│   └── index.ts                    # X402Firewall class
│
├── scripts/                        # Deployment and testing
│   ├── deploy-firewall.ts          # Deploy to Cronos
│   ├── verify-contracts.ts         # Verify on explorer
│   └── demo-full-flow.ts           # Full demo script
│
├── docs/                           # Documentation
│   ├── ARCHITECTURE.md             # Technical deep-dive
│   └── X402_INTEGRATION.md         # Integration guide
│
├── hardhat.config.ts               # Hardhat configuration
└── package.json
```

---

## 🖥️ Run the Frontend Demo

```bash
# 1. Clone and install
git clone https://github.com/Venkat5599/x402-Intent-Firewall.git
cd x402-Intent-Firewall/frontend
npm install

# 2. Start development server
npm run dev

# 3. Open http://localhost:5173

# 4. Connect MetaMask to Cronos Testnet
#    - Network: Cronos Testnet
#    - RPC: https://evm-t3.cronos.org
#    - Chain ID: 338

# 5. Get test CRO from https://cronos.org/faucet

# 6. Try sending 15,000 CRO → Watch it get BLOCKED!
```

---

## 🧪 Testing

```bash
# Run the full demo flow
npx hardhat run scripts/demo-full-flow.ts --network cronosTestnet

# Expected output:
# ✅ Direct payment (0.001 CRO) - Executed
# ✅ Intent registration - Success
# ✅ Intent approval - Success  
# ✅ Intent execution - Success
# ❌ Unapproved intent - Reverted (expected)
# ❌ Rejected intent - Reverted (expected)
```

---

## 🔗 Links

| Resource | URL |
|----------|-----|
| **Firewall Contract** | [View on Explorer](https://cronos.org/explorer/testnet3/address/0xC3C4E069B294C8ED3841c87d527c942F873CFAA9#code) |
| **PolicyEngine Contract** | [View on Explorer](https://cronos.org/explorer/testnet3/address/0xD0CE6F16969d81997750afE018A34921DeDd04A0#code) |
| **Example Transaction** | [View TX](https://cronos.org/explorer/testnet3/tx/0x26f363226771f9e359b6ed74c67eef0d2314bd21e458dcbfde3583e7b460fbae) |
| **Cronos Faucet** | [Get Test CRO](https://cronos.org/faucet) |
| **Architecture Docs** | [ARCHITECTURE.md](./docs/ARCHITECTURE.md) |

---

## 🛠️ Tech Stack

- **Smart Contracts:** Solidity 0.8.19
- **Development:** Hardhat, TypeScript
- **Frontend:** React, Vite, TailwindCSS
- **Blockchain:** Cronos EVM (Testnet)
- **Wallet:** MetaMask, ethers.js v6

---

## 📈 Roadmap

- [x] Core contracts deployed & verified
- [x] Policy enforcement working
- [x] Frontend dashboard
- [x] TypeScript SDK
- [x] Documentation
- [ ] Security audit
- [ ] Mainnet deployment
- [ ] npm package publish
- [ ] Multi-token support (ERC20)

---

<div align="center">

## Built for Cronos x402 Hackathon 2025

**Protecting AI Agent Payments on Cronos**

*Because autonomous doesn't mean unprotected.*

</div>
