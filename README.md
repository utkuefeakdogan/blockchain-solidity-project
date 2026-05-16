# 🎰 Decentralized Lottery System on Ethereum

![Solidity](https://img.shields.io/badge/Solidity-0.8.9-363636?style=flat-square&logo=solidity)
![Truffle](https://img.shields.io/badge/Truffle-Framework-5E464D?style=flat-square)
![IPFS](https://img.shields.io/badge/IPFS-Integrated-65C2CB?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

A fully on-chain, trustless lottery protocol built with Solidity and deployed via Truffle. Participants buy tiered tickets, contribute to a shared prize pool, and winners are determined through a **commit-reveal hash scheme** using `keccak256`. The contract autonomously manages weekly lottery rounds, multi-tier prize distribution, and ETH refunds — all without a centralized intermediary.

> Developed as part of CMPE483 – Blockchain Technologies @ Boğaziçi University.

---

## ✨ Features

- **3-Tier Ticket System** — Full (8 ETH), Half (4 ETH), and Quarter (2 ETH) tickets with proportional prize payouts
- **Commit-Reveal Randomness** — Ticket hashes using `keccak256(block.timestamp, block.difficulty)` to prevent front-running
- **Autonomous Lottery Rounds** — Weekly rounds driven by Unix timestamp scheduling; owner calls `LotteryFunction()` to finalize each draw
- **Prize Pool Management** — Accumulated per-lottery prize pool tracked independently; winners receive proportional payouts based on ticket type
- **Refund Mechanism** — Active tickets can be refunded before the lottery concludes
- **On-Chain Balance Ledger** — Each participant's ETH balance is managed within the contract via `depositEther` / `withdrawEther`
- **Access Control** — `onlyOwner` modifier gates administrative functions (draw execution, ticket audits)
- **IPFS Integration** — Off-chain metadata and assets stored via IPFS for decentralized data availability
- **Full Test Suite** — JavaScript-based tests with Truffle covering core contract interactions

---

## 🏗️ Architecture

blockchain-solidity-project/
├── contracts/
│   ├── MyLottery.sol        # Core lottery contract
│   └── HelloBlockchain.sol  # Entry-point demo contract
├── migrations/              # Truffle deployment scripts
├── test/                    # JavaScript test suite
├── scripts/                 # Utility & interaction scripts
├── ipfs/                    # IPFS-linked metadata
└── truffle-config.js        # Network & compiler configuration

### Smart Contract: `MyLottery.sol`

| Component | Description |
|-----------|-------------|
| `Ticket` struct | Stores owner address, hash, ticket type, status, lottery round, and win amount |
| `Lottery` struct | Stores winning hashes, prize pool, and winner ID per round |
| `buyTicket()` | Deducts balance, records ticket with commit hash, adds to prize pool |
| `LotteryFunction()` | Finalizes a round — generates winning hashes, selects winner by ID |
| `checkIfTicketWon()` | Compares ticket ID against winning ID; updates win amount |
| `collectTicketPrize()` | Transfers prize to winner based on ticket tier |
| `collectTicketRefund()` | Returns ticket cost if lottery hasn't concluded |
| `revealRndNumber()` | Allows ticket owner to reveal their committed hash |
| `getLotteryNos()` | Maps a Unix timestamp to its corresponding lottery round number |

---

## 🔐 Randomness & Fairness

Winner selection uses `keccak256(block.timestamp, block.difficulty, i)` to generate three candidate winning hashes per round. The final winner ID is computed as:

```solidity
uint256 randomNumber = uint256(
    keccak256(abi.encodePacked(block.timestamp, block.difficulty))
) % (upperBound - lowerBound + 1);
```

> **Note:** Block-based randomness is suitable for a course-level implementation. Production systems should integrate a VRF oracle (e.g., Chainlink VRF) for cryptographically secure randomness.

---

## 🚀 Getting Started

### Prerequisites

- Node.js >= 14
- Truffle: `npm install -g truffle`
- Ganache (local testnet)
- MetaMask (for testnet interaction)

### Installation

```bash
git clone https://github.com/utkuefeakdogan/blockchain-solidity-project.git
cd blockchain-solidity-project
npm install
```

### Compile & Deploy

```bash
# Compile contracts
truffle compile

# Deploy to local Ganache
truffle migrate --network development

# Deploy to a public testnet (configure truffle-config.js first)
truffle migrate --network sepolia
```

### Run Tests

```bash
truffle test
```

---

## 🎟️ How It Works

1. **Deposit ETH** — Call `depositEther(amount)` to load your contract balance
2. **Buy a Ticket** — Choose your tier and submit a `keccak256` hash of your secret random number
3. **Lottery Draw** — Owner calls `LotteryFunction()` at the end of the weekly round
4. **Check & Claim** — Call `checkIfTicketWon()` then `collectTicketPrize()` if you won
5. **Refund** — Call `collectTicketRefund()` anytime before the draw if you change your mind

---

## 📊 Prize Distribution

| Ticket Type | Cost | Prize (if winner) |
|-------------|------|-------------------|
| Full        | 8 ETH | 100% of round pool |
| Half        | 4 ETH | 50% of round pool  |
| Quarter     | 2 ETH | 25% of round pool  |

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Smart Contract | Solidity ^0.8.9 |
| Development Framework | Truffle Suite |
| Local Blockchain | Ganache |
| Off-chain Storage | IPFS |
| Testing | JavaScript (Truffle test runner) |
| Wallet | MetaMask |

---

## 📝 License

This project is licensed under the MIT License. See [LICENSE](./LICENSE) for details.
