# CoverFi Protocol — DoraHacks Hackathon Submission

**RWA Demo Day — DoraHacks / BNB Chain / HK Web3 Festival 2026**
Submission Deadline: March 31, 2026 | Demo Day: April 8, 2026

---

## 1. Project Name

**CoverFi Protocol**

---

## 2. One-liner

The first on-chain credit default swap equivalent for ERC-3643 RWA tokens — bringing the $8 trillion CDS market's protection mechanics to tokenized real-world assets on BNB Chain.

---

## 3. Problem Statement

The tokenized Real World Asset market holds $26.6 billion in ERC-3643 security tokens on public blockchains as of March 2026. These tokens represent enforceable claims on private credit loans, invoice receivables, real estate, agricultural commodities, and fund units. Every holder of these tokens bears 100% unhedged issuer default risk. If the entity managing the underlying real-world asset fails to honor repayment obligations, abandons the protocol, misappropriates collateral, or becomes insolvent, the token holder has zero automated protection and must rely on slow, expensive, multi-jurisdictional legal proceedings that may take years and yield partial recovery at best.

Existing DeFi insurance protocols — Nexus Mutual, Risk Harbor, Neptune Mutual — cover smart contract bugs, protocol exploits, oracle manipulation, and stablecoin depegs. None of them cover RWA issuer default risk. This is not an oversight; it is an architectural limitation. RWA issuers are regulated entities requiring compliance-native payout mechanics (ERC-3643 identity verification, transfer restrictions) that existing insurance protocols were never designed to handle.

The result is a critical infrastructure gap: institutions, DAO treasuries, and retail investors cannot deploy capital into RWA tokens with the same hedging guarantees they expect in traditional finance. The $8 trillion CDS market exists in TradFi precisely because capital does not flow without the ability to hedge counterparty default risk. On-chain, that mechanism is entirely absent — until CoverFi.

---

## 4. Solution

CoverFi is a permissionless on-chain risk-sharing protocol that protects RWA token holders against issuer default. It mirrors the structure of traditional Credit Default Swaps but replaces manual processes with deterministic smart contract execution. Protection buyers (RWA investors) pay premiums priced by a continuous behavioral credit score. Protection sellers (underwriters) deposit USDT into dual-tranche insurance pools and earn yield. When a default is confirmed through a 2-of-3 multisig attestation by bonded professionals, payout executes automatically in a single transaction — no claims process, no waiting, no legal intermediaries.

Every RWA issuer must post a mandatory 5% bond in USDT as first-loss capital before any external coverage activates. This bond is the first capital liquidated on confirmed default, ensuring issuers have direct financial skin in the game. The protocol continuously scores issuer behavior through the Issuer Reputation Score (IRS), a 5-dimension on-chain credit score (0-1000) that drives premium pricing via an exponential formula and fires early warning signals before formal default confirmation. All payouts are ERC-3643 compliance-native, checking isVerified() and isFrozen() before every transfer to correctly handle regulated security tokens.

The entire lifecycle — from issuer registration and bond deposit, through underwriter deposits and coverage purchase, to default confirmation and automatic payout — executes on BNB Chain with full verifiability on BNBScan. CoverFi is infrastructure, not an application: it provides the missing credit protection primitive that enables institutional capital to flow into on-chain RWA markets with confidence.

---

## 5. Key Innovations

### Innovation 1: Issuer Reputation Score (IRS)

A continuously updating behavioral credit score (0-1000) derived from five on-chain signal dimensions: NAV punctuality (0-250), attestation accuracy (0-250), repayment history (0-300), collateral health via Chainlink PoR (0-150), and protocol activity (0-50). The IRS drives premium pricing through an exponential formula — IRS 1000 pays 4% APR, IRS 0 pays 16% APR. The score includes an Early Warning System that fires alerts 24-48 hours before formal default proceedings, giving investors time to react. No existing DeFi protocol has a behavioral credit scoring system for RWA issuers.

### Innovation 2: Dual-Tranche Insurance Pool

Underwriter capital is split into a 70% senior tranche (srCVR tokens) and 30% junior tranche (jrCVR tokens) using a cToken model inspired by Compound's proven architecture. Junior capital absorbs losses first, protecting senior depositors and enabling risk-adjusted yield tiers. Combined with the mandatory issuer bond (which absorbs losses before any underwriter capital is touched), CoverFi creates a three-layer loss waterfall: issuer bond, then junior tranche, then senior tranche. This architecture directly mirrors the TIN/DROP first-loss model used by Centrifuge but applied to insurance rather than lending.

### Innovation 3: 2-of-3 Multisig Attestation via Trusted Issuer Registry (TIR)

Default confirmation requires independent attestations from 2 of 3 bonded professionals (custodian, legal representative, auditor) registered in the Trusted Issuer Registry. Each attestor must post their own bond (calculated to make collusion economically unprofitable), and attestations use BNB Attestation Service (BAS) for on-chain verifiability. Four precisely defined default event types — payment delay, ghost issuer, collateral shortfall, and asset misappropriation — each have specific triggers, grace periods, and evidence requirements. This eliminates subjective governance votes and provides deterministic, auditable default confirmation.

---

## 6. Technical Architecture

CoverFi consists of 12 smart contracts deployed in a strict dependency order across 8 architectural layers on BNB Chain (BSC Testnet). The protocol is built on Solidity 0.8.19 with Hardhat and TypeScript, using ABDKMath64x64 for fixed-point exponential math in premium calculations.

The architecture is ERC-3643 compliance-native: all payout transfers verify investor identity status (isVerified()) and frozen status (isFrozen()) before execution. The protocol integrates with Chainlink Proof of Reserve for real-time collateral health monitoring and BNB Attestation Service (BAS) for on-chain attestation verification. Protection Certificates are ERC-5192 soulbound tokens representing non-transferable coverage positions.

Contract interactions follow a layered dependency graph: the Trusted Issuer Registry (TIR) sits at the foundation, followed by IssuerBond and IRSOracle, then DefaultOracle and IssuerRegistry, the InsurancePool with its tranche tokens, and finally PayoutEngine and SubrogationNFT at the top. Permission wiring connects these contracts into a secure, automated pipeline where default confirmation triggers payout execution without human intervention.

---

## 7. Smart Contracts

| # | Contract | Description |
|---|----------|-------------|
| 1 | **TIR.sol** | Trusted Issuer Registry — manages bonded attestors (custodians, legal reps, auditors) who confirm default events |
| 2 | **IssuerBond.sol** | Holds mandatory 5% USDT first-loss bonds posted by RWA issuers; first capital liquidated on default |
| 3 | **IRSOracle.sol** | Issuer Reputation Score engine — computes 5-dimension behavioral credit score (0-1000) with early warning system |
| 4 | **DefaultOracle.sol** | Aggregates 2-of-3 TIR attestations to confirm default events across four event types with grace periods |
| 5 | **IssuerRegistry.sol** | Central issuer lifecycle manager — registration, two-tier onboarding, activation, monitoring, wind-down FSM |
| 6 | **InsurancePool.sol** | Dual-tranche underwriter pool — accepts USDT deposits, manages 70/30 senior/junior split, handles loss waterfall |
| 7 | **srCVR.sol** | Senior tranche receipt token (cToken model) — represents 70% pool share with last-loss protection |
| 8 | **jrCVR.sol** | Junior tranche receipt token (cToken model) — represents 30% pool share with first-loss (higher yield) |
| 9 | **ProtectionCert.sol** | ERC-5192 soulbound Protection Certificate — non-transferable coverage position with estimated recovery ratio |
| 10 | **PayoutEngine.sol** | Executes ERC-3643 compliant USDT payouts on confirmed default; checks isVerified() and isFrozen() |
| 11 | **SubrogationNFT.sol** | Mints NFT representing CoverFi Foundation's legal recovery rights against defaulted issuers |
| 12 | **Mock Contracts** | MockERC3643, MockBAS, MockChainlinkPoR — simulate external dependencies for testnet demo |

---

## 8. Demo Transactions

Three end-to-end transactions demonstrate the complete CoverFi lifecycle on BSC Testnet, each verifiable on BNBScan.

### TX1: Issuer Registration + Bond Deposit + Coverage Activation

A Matrixdock-style RWA issuer registers their tokenized Treasury token. They deposit USDT as their mandatory first-loss bond — their own capital at risk before any underwriter exposure. After the observation period with clean attestations, the system activates their coverage. Their IRS starts at 600 (Good tier), translating to a premium rate of 696 basis points (6.96% APR) via the exponential pricing formula. Events emitted: `IssuerRegistered`, `CoverageActivated`.

### TX2: Underwriter Deposits + Coverage Purchase

An underwriter deposits USDT into the insurance pool split across senior (70%) and junior (30%) tranches — $7 senior, $3 junior for the demo. They receive srCVR and jrCVR tokens representing their pool shares with yield accrual. An investor holding the issuer's RWA token then purchases a Protection Certificate covering 100 tokens. The soulbound ERC-5192 certificate shows their estimated payout ratio based on current pool TVL. Events emitted: `SeniorDeposited`, `CoveragePurchased`, `CoverageRatioUpdated`.

### TX3: Default Confirmation + Payout + SubrogationNFT

Three bonded professionals (custodian, legal representative, auditor) submit independent attestations confirming the issuer missed payment by 30+ days. The 2-of-3 threshold is met. DefaultOracle confirms the credit event. PayoutEngine executes automatically in the same transaction: the investor receives $15 USDT after ERC-3643 compliance checks. No human intervention, no claims process, no legal proceedings required. A SubrogationNFT is minted to the CoverFi Foundation, representing its legal right to pursue recovery against the defaulted issuer. Events emitted: `DefaultConfirmed`, `PayoutExecuted`, `Transfer`, `SubrogationClaimed`.

---

## 9. Tech Stack

| Component | Technology |
|-----------|------------|
| Smart Contracts | Solidity 0.8.19 |
| Development Framework | Hardhat + TypeScript |
| Blockchain | BNB Chain (BSC Testnet) |
| Token Standard | ERC-3643 (T-REX compliant security tokens) |
| Protection Certificates | ERC-5192 (soulbound / non-transferable) |
| Math Library | ABDKMath64x64 (fixed-point exponential calculations) |
| Access Control | OpenZeppelin (Ownable, ReentrancyGuard, Pausable) |
| Oracle Integration | Chainlink Proof of Reserve |
| Attestation Layer | BNB Attestation Service (BAS) |
| Contract Verification | BNBScan (sourcify) |

---

## 10. Team

Solo developer project built for the BNB Chain RWA Demo Day Hackathon 2026. Full protocol design, smart contract development, testing, and deployment executed by a single builder with deep knowledge of traditional credit derivatives, DeFi insurance mechanics, and ERC-3643 compliance requirements.

---

## 11. Links

| Resource | URL |
|----------|-----|
| Frontend | localhost (development) |
| **Core Contracts** | |
| PayoutEngine | [0xD01e871c97746FC6a3f4B406aA60BE1Fb7FAcf6B](https://testnet.bscscan.com/address/0xD01e871c97746FC6a3f4B406aA60BE1Fb7FAcf6B) |
| InsurancePool | [0xBCF0012388045eA1183c96EEbe24754842a549eA](https://testnet.bscscan.com/address/0xBCF0012388045eA1183c96EEbe24754842a549eA) |
| IRSOracle | [0xa4ECEB47F80a32D7176C23e4993cDa4d2337Fc3A](https://testnet.bscscan.com/address/0xa4ECEB47F80a32D7176C23e4993cDa4d2337Fc3A) |
| IssuerRegistry | [0x8D4C37f45883aAEEd20d2CC1020e6Ab193D3A50C](https://testnet.bscscan.com/address/0x8D4C37f45883aAEEd20d2CC1020e6Ab193D3A50C) |
| DefaultOracle | [0x1Ca7B678BDf1deCe9964c5178C01AB9312F2664D](https://testnet.bscscan.com/address/0x1Ca7B678BDf1deCe9964c5178C01AB9312F2664D) |
| TIR | [0xB10b1c9D88126965E57cCa2a7ED5a1348dbf7552](https://testnet.bscscan.com/address/0xB10b1c9D88126965E57cCa2a7ED5a1348dbf7552) |
| IssuerBond | [0xF1E25246D7Dcc8E63EAe39BE03DEae0C2Ed93E71](https://testnet.bscscan.com/address/0xF1E25246D7Dcc8E63EAe39BE03DEae0C2Ed93E71) |
| SubrogationNFT | [0x91062e509E75AAe31f1d6425b78D8815Ad941e73](https://testnet.bscscan.com/address/0x91062e509E75AAe31f1d6425b78D8815Ad941e73) |
| **Token Contracts** | |
| srCVR (Senior Tranche) | [0xc07859b25FC869F0a81fae86b9B5bEa868D08A9f](https://testnet.bscscan.com/address/0xc07859b25FC869F0a81fae86b9B5bEa868D08A9f) |
| jrCVR (Junior Tranche) | [0xa5d64A7770136B1EEade6B980404140D8D5F7C06](https://testnet.bscscan.com/address/0xa5d64A7770136B1EEade6B980404140D8D5F7C06) |
| ProtectionCert (SBT) | [0x2Aad26de595752d7D6FCc2f4C79F1Bf15B60E1CD](https://testnet.bscscan.com/address/0x2Aad26de595752d7D6FCc2f4C79F1Bf15B60E1CD) |
| **Mock Contracts** | |
| MockUSDT | [0x38907cC4E615D3C7BDCBC9910C050260bBC836E5](https://testnet.bscscan.com/address/0x38907cC4E615D3C7BDCBC9910C050260bBC836E5) |
| MockERC3643Token | [0x65A3Ae0e4787856CfcDdE505015c5CC3d5560212](https://testnet.bscscan.com/address/0x65A3Ae0e4787856CfcDdE505015c5CC3d5560212) |
| MockBAS | [0x20619c533854C5a0c20284f7Dc7F5Dc3DFdD06B3](https://testnet.bscscan.com/address/0x20619c533854C5a0c20284f7Dc7F5Dc3DFdD06B3) |
| MockChainlink | [0xa7C664459C66325Cd9dB15245DD901f1623c9655](https://testnet.bscscan.com/address/0xa7C664459C66325Cd9dB15245DD901f1623c9655) |
| MockIdentityRegistry | [0x7dD7C1adC65D9e6e7Bd5532b678f856C8Ea627fC](https://testnet.bscscan.com/address/0x7dD7C1adC65D9e6e7Bd5532b678f856C8Ea627fC) |
| **Demo Transactions** | |
| TX1: BAS Attestation | [0x04ef952...](https://testnet.bscscan.com/tx/0x04ef95232232b6c2143ed53bfa60adb16eaa98c684e7a02dab59b450d0085c95) |
| TX1: Issuer Registration | [0x76654f6...](https://testnet.bscscan.com/tx/0x76654f6954651e6139ec6ffdb51edd5d67000a7e4be8ebc5ec1683a21bba8001) |
| TX1: Bond Deposit | [0x703a37c...](https://testnet.bscscan.com/tx/0x703a37cc62f434af56c996c3142dde5dcae29f2d1f6e4261ce7a35f1ecc5d379) |
| TX1: Coverage Activation | [0xac6fd98...](https://testnet.bscscan.com/tx/0xac6fd98eeb40a66509f760ca139ee74cbf6d2398af1b1f3f3cf1c80e20adde51) |
| TX2: Junior Deposit | [0x9aaafc0...](https://testnet.bscscan.com/tx/0x9aaafc0b7c6927d0ae20578b20a2d57c9a045ddf495429e0cdd66619dcdc5c9b) |
| TX2: Senior Deposit | [0x983541c...](https://testnet.bscscan.com/tx/0x983541ce383611f3d1bca92519bf2fb686cee4aa386dd839578adc252530b7f9) |
| TX2: Coverage Purchase | [0xca3ac57...](https://testnet.bscscan.com/tx/0xca3ac579eeffd138e02849203f76f95ec958552cfd117aad65d1ac48a9a1727e) |
| TX3: Default Confirmation | [0xc366dc7...](https://testnet.bscscan.com/tx/0xc366dc7e84be2a52ecf4f110c6773b04beba54c40ca9c3503a5ee89872d1fda1) |
| TX3: Payout Execution | [0x5381147...](https://testnet.bscscan.com/tx/0x5381147c824b4006cd95af66434f57795578c050000b24674b06a16078d74c65) |
| **Other** | |
| Full Deployment JSON | See `deployments/bscTestnet.json` for all 16 contract addresses |
| DoraHacks | [dorahacks.io/hackathon/rwademoday](https://dorahacks.io/hackathon/rwademoday) |

---

## 12. What's Next

**Phase 1 — Testnet Beta (April-July 2026):** Onboard the first real RWA issuer on BNB Testnet. Replace mock contracts with real BAS attestations from actual custodians and real Chainlink PoR feeds. Complete external security audit with a BNB-aligned auditor (Hacken or PeckShield). Launch the IRS Dashboard frontend for live score monitoring. File 5 provisional patent applications covering behavioral credit scoring, contagion modeling, and on-chain actuarial stress testing.

**Phase 2 — Mainnet Launch (August-December 2026):** Deploy to BSC Mainnet post-audit. Target first 3 active issuers (Matrixdock, OpenEden, and one MSME platform). Reach $5M TVL through institutional LP partnerships. Launch the IRS Oracle API as a subscription product for DeFi protocols seeking live RWA credit signals. Explore Venus Protocol integration for srCVR as accepted collateral.

**Phase 3 — Ecosystem Expansion (2027):** Scale to 10+ active issuers and $50M insured. Expand to Ethereum L2s and Avalanche. License the IRS Oracle to major DeFi lending protocols (Aave Horizon, Venus). Incorporate the Subrogation Foundation in Singapore for cross-jurisdictional legal recovery. Productize the Actuarial Solvency Reserve Framework as "CoverFi Risk Engine" — a standalone stress-testing product for the broader RWA ecosystem.
