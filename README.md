# CoverFi Protocol

The first on-chain RWA issuer-default protection protocol on HashKey Chain.

## Live Demo
- Frontend: https://coverfi-protocol.vercel.app
- GitHub: https://github.com/Sanjay-N23/coverfi-protocol

## HashKey Chain Testnet Contract Addresses (Chain ID 133)
- MockUSDT: 0x65A3Ae0e4787856CfcDdE505015c5CC3d5560212
- MockERC3643Token: 0xa7C664459C66325Cd9dB15245DD901f1623c9655
- TIR: 0xa4ECEB47F80a32D7176C23e4993cDa4d2337Fc3A
- IssuerBond: 0x1Ca7B678BDf1deCe9964c5178C01AB9312F2664D
- IRSOracle: 0x8D4C37f45883aAEEd20d2CC1020e6Ab193D3A50C
- DefaultOracle: 0xBCF0012388045eA1183c96EEbe24754842a549eA
- IssuerRegistry: 0xc07859b25FC869F0a81fae86b9B5bEa868D08A9f
- InsurancePool: 0xa5d64A7770136B1EEade6B980404140D8D5F7C06
- srCVR: 0x2Aad26de595752d7D6FCc2f4C79F1Bf15B60E1CD
- jrCVR: 0xD01e871c97746FC6a3f4B406aA60BE1Fb7FAcf6B
- ProtectionCert: 0x91062e509E75AAe31f1d6425b78D8815Ad941e73
- PayoutEngine: 0x44944cB598A750Df4C4Bf9A7D3FdDDf7575F88F5
- SubrogationNFT: 0xbBe8A2840E151cC8BF2B156e5d61a532eFCe2AB9

Explorer: https://testnet-explorer.hsk.xyz

## Smart Contracts
- 12 core contracts + 5 mocks
- 416 tests passing (unit + integration + edge cases)
- Deployed on HashKey Chain Testnet (Chain ID 133)

## Technology
- Solidity 0.8.19
- Hardhat
- ethers.js v6
- Vanilla HTML/JS frontend
- HashKey Chain (EVM-compatible L2)

## Quick Start
```bash
npm install
npm run compile
npm run test
npm run deploy:hashkey    # Deploy to HashKey Testnet
npm run demo:hashkey      # Seed demo data
```
