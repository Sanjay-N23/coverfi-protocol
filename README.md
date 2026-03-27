# CoverFi Protocol

> On-chain credit default swap equivalent for ERC-3643 RWA tokens on BNB Chain

---

## What is CoverFi?

CoverFi is a permissionless on-chain risk-sharing protocol that protects Real World Asset (RWA) token holders against issuer default. It mirrors the structure of traditional Credit Default Swaps (CDS) but replaces manual processes with deterministic smart contract execution on BNB Chain.

The tokenized RWA market holds $12 billion in ERC-3643 security tokens with zero automated issuer default protection. CoverFi fills this critical infrastructure gap, enabling institutional capital to flow into on-chain RWA markets with the same hedging guarantees expected in traditional finance.

## Key Features

- **Issuer Reputation Score (IRS)** -- Continuous behavioral credit score (0-1000) across 5 dimensions driving exponential premium pricing. Includes an Early Warning System that fires alerts before formal default proceedings.
- **Dual-Tranche Insurance Pool** -- 70% senior / 30% junior tranche split with a three-layer loss waterfall: issuer bond (first-loss), junior tranche, senior tranche. Modeled after Compound's cToken architecture.
- **2-of-3 Attestation** -- Default confirmation requires independent attestations from 2 of 3 bonded professionals (custodian, legal rep, auditor) via BNB Attestation Service (BAS). Four precisely defined default event types with specific triggers and grace periods.
- **Soulbound Protection Certificates** -- ERC-5192 non-transferable coverage positions with estimated recovery ratios. Burns on payout execution.
- **ERC-3643 Compliance-Native Payouts** -- All payout transfers verify investor identity status (`isVerified()`) and frozen status (`isFrozen()`) before execution, correctly handling regulated security tokens.
- **Mandatory Issuer Bond** -- 5% of token market cap in USDT posted as first-loss capital before any external coverage activates.

## Architecture

CoverFi consists of 12 smart contracts deployed across 8 architectural layers:

| Layer | Contract | Purpose |
|-------|----------|---------|
| 0 | IssuerRegistry | Central issuer lifecycle FSM (Observation > Active > Monitoring > Defaulted > Wind-Down > Closed) |
| 1 | IssuerBond | Mandatory 5% USDT first-loss bond storage |
| 2 | IRSOracle | Behavioral credit scoring engine + TWAS cache + Early Warning System |
| 3 | TIR | Trusted Issuer Registry -- manages bonded attestors (custodian, legal rep, auditor) |
| 4 | DefaultOracle | Aggregates 2-of-3 TIR attestations across 4 default event types |
| 5 | InsurancePool | Dual-tranche underwriter pool with 70/30 senior/junior split |
| 6 | PayoutEngine | ERC-3643-compliant USDT payout execution |
| 7 | SubrogationNFT | ERC-721 legal recovery rights minted to CoverFi Foundation post-default |
| -- | srCVR / jrCVR | Senior and junior tranche receipt tokens (cToken model) |
| -- | ProtectionCert | ERC-5192 soulbound coverage position NFT |

## Quick Start

```bash
# Install dependencies
npm install

# Compile contracts
npx hardhat compile

# Run tests
npx hardhat test

# Deploy to BSC Testnet
npx hardhat run scripts/deploy.ts --network bscTestnet
```

### Environment Setup

Create a `.env` file:

```
PRIVATE_KEY=your_deployer_private_key
BSC_TESTNET_RPC=https://data-seed-prebsc-1-s1.binance.org:8545
BSCSCAN_API_KEY=your_bscscan_api_key
```

## Deployed Contracts (BSC Testnet)

| Contract | Address |
|----------|---------|
| MockUSDT | `0x38907cC4E615D3C7BDCBC9910C050260bBC836E5` |
| MockIdentityRegistry | `0x7dD7C1adC65D9e6e7Bd5532b678f856C8Ea627fC` |
| MockERC3643Token | `0x65A3Ae0e4787856CfcDdE505015c5CC3d5560212` |
| MockBAS | `0x20619c533854C5a0c20284f7Dc7F5Dc3DFdD06B3` |
| MockChainlink | `0xa7C664459C66325Cd9dB15245DD901f1623c9655` |
| TIR | `0xB10b1c9D88126965E57cCa2a7ED5a1348dbf7552` |
| IssuerBond | `0xF1E25246D7Dcc8E63EAe39BE03DEae0C2Ed93E71` |
| IRSOracle | `0xa4ECEB47F80a32D7176C23e4993cDa4d2337Fc3A` |
| DefaultOracle | `0x1Ca7B678BDf1deCe9964c5178C01AB9312F2664D` |
| IssuerRegistry | `0x8D4C37f45883aAEEd20d2CC1020e6Ab193D3A50C` |
| InsurancePool | `0xBCF0012388045eA1183c96EEbe24754842a549eA` |
| srCVR | `0xc07859b25FC869F0a81fae86b9B5bEa868D08A9f` |
| jrCVR | `0xa5d64A7770136B1EEade6B980404140D8D5F7C06` |
| ProtectionCert | `0x2Aad26de595752d7D6FCc2f4C79F1Bf15B60E1CD` |
| PayoutEngine | `0xD01e871c97746FC6a3f4B406aA60BE1Fb7FAcf6B` |
| SubrogationNFT | `0x91062e509E75AAe31f1d6425b78D8815Ad941e73` |

Network: BSC Testnet (Chain ID 97) | Deployer: `0xce220d9eD9527f9997c8045844210637F3A42fb3`

## Tech Stack

| Component | Technology |
|-----------|------------|
| Smart Contracts | Solidity 0.8.19 |
| Framework | Hardhat + TypeScript |
| Blockchain | BNB Chain (BSC Testnet) |
| Token Standard | ERC-3643 (T-REX compliant security tokens) |
| Protection Certificates | ERC-5192 (soulbound / non-transferable) |
| Math Library | ABDKMath64x64 (fixed-point exponential calculations) |
| Access Control | OpenZeppelin (Ownable, ReentrancyGuard, Pausable) |
| Oracle Integration | Chainlink Proof of Reserve |
| Attestation Layer | BNB Attestation Service (BAS) |
| Contract Verification | BNBScan (sourcify) |

## Hackathon

**BNB Chain RWA Demo Day -- DoraHacks / HK Web3 Festival 2026**

- Submission Deadline: March 31, 2026
- Demo Day: April 8, 2026
- Winners Announced: April 21, 2026 (Hong Kong Web3 Festival)

## License

MIT
