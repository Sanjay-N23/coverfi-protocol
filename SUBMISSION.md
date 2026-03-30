# CoverFi Protocol — DoraHacks Submission
## RWA Demo Day 2026 — BNB Chain

### One-Line Description
The first on-chain RWA issuer-default protection protocol on BNB Chain —
combining mandatory issuer bonds, behavioral credit scoring (IRS), and
ERC-3643 compliance-native automated payout.

### Project Description

CoverFi solves the most critical missing piece in the $26.6 billion
tokenized RWA market: zero automated protection against issuer default.
Today, every RWA token holder who loses money when an issuer fails must
pursue expensive, slow, multi-year legal proceedings. CoverFi makes
protection automatic, on-chain, and instant.

Three innovations make CoverFi unique: (1) Mandatory Issuer Bond —
issuers post 5% of their token market cap in USDT as first-loss capital
before any coverage activates. (2) Issuer Reputation Score — a continuous
0-1000 behavioral credit score updated by 5 on-chain signals, driving
premiums via the formula: 1600 x e^(-0.001386 x IRS). This creates the
first on-chain equivalent of a credit rating for RWA issuers. (3)
ERC-3643 compliance-native payout — the only insurance protocol that
checks KYC verification status and regulatory freeze status before
distributing USDT to investors.

Built on BNB Chain with 12 smart contracts, 376 passing tests, 16 verified
contracts on BSC Testnet, and a complete default + payout lifecycle
demonstrated in 25+ on-chain transactions.

### Live Demo
- Frontend: https://coverfi-protocol.vercel.app
- GitHub: https://github.com/Sanjay-N23/coverfi-protocol

### Smart Contract Addresses (BSC Testnet)
- IssuerRegistry: 0x8D4C37f45883aAEEd20d2CC1020e6Ab193D3A50C
- IRSOracle: 0xa4ECEB47F80a32D7176C23e4993cDa4d2337Fc3A
- InsurancePool: 0xBCF0012388045eA1183c96EEbe24754842a549eA
- PayoutEngine: 0xD01e871c97746FC6a3f4B406aA60BE1Fb7FAcf6B
- SubrogationNFT: 0x91062e509E75AAe31f1d6425b78D8815Ad941e73
- TIR: 0xB10b1c9D88126965E57cCa2a7ED5a1348dbf7552
- IssuerBond: 0xF1E25246D7Dcc8E63EAe39BE03DEae0C2Ed93E71
- DefaultOracle: 0x1Ca7B678BDf1deCe9964c5178C01AB9312F2664D
- srCVR: 0xc07859b25FC869F0a81fae86b9B5bEa868D08A9f
- jrCVR: 0xa5d64A7770136B1EEade6B980404140D8D5F7C06
- ProtectionCert: 0x2Aad26de595752d7D6FCc2f4C79F1Bf15B60E1CD
- MockUSDT: 0x38907cC4E615D3C7BDCBC9910C050260bBC836E5
- MockERC3643Token: 0x65A3Ae0e4787856CfcDdE505015c5CC3d5560212
- MockBAS: 0x20619c533854C5a0c20284f7Dc7F5Dc3DFdD06B3
- MockChainlink: 0xa7C664459C66325Cd9dB15245DD901f1623c9655
- MockIdentityRegistry: 0x7dD7C1adC65D9e6e7Bd5532b678f856C8Ea627fC

### Key Demo Transactions (BSC Testnet — all verified Success)
1. Issuer Registration + Bond: 0x76654f6954651e6139ec6ffdb51edd5d67000a7e4be8ebc5ec1683a21bba8001
2. Senior Pool Deposit: 0x983541ce383611f3d1bca92519bf2fb686cee4aa386dd839578adc252530b7f9
3. Junior Pool Deposit: 0x9aaafc0b7c6927d0ae20578b20a2d57c9a045ddf495429e0cdd66619dcdc5c9b
4. Coverage Certificate Purchase: 0xca3ac579eeffd138e02849203f76f95ec958552cfd117aad65d1ac48a9a1727e
5. 2-of-3 Default Confirmation: 0xc366dc7e84be2a52ecf4f110c6773b04beba54c40ca9c3503a5ee89872d1fda1
6. Automated Payout Execution: 0x5381147c824b4006cd95af66434f57795578c050000b24674b06a16078d74c65

### What Each Transaction Proves
1. Any issuer can register and post first-loss capital trustlessly
2. Underwriters can provide senior tranche liquidity (10% APY)
3. Underwriters can provide junior tranche liquidity (20%+ APY)
4. Investors can purchase ERC-5192 soulbound Protection Certificates
5. 2-of-3 bonded professional attestors can confirm default on-chain
6. $15,000 USDT paid to investor automatically — bond + junior + senior waterfall

### Technical Highlights
- IRS formula: premium_bps = 1600 x e^(-0.001386 x IRS)
  (4% APR at IRS 1000, 16% APR at IRS 0)
- Compound cToken exchange rate model for srCVR yield accrual
- ERC-3643 isVerified() + isFrozen() compliance checks before payout
- ERC-5192 soulbound Protection Certificate NFT
- ERC-721 SubrogationNFT with complete default evidence package
- 376 tests passing, 9 second test suite
- ABDKMath64x64 for gas-efficient fixed-point exponential calculation
