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
