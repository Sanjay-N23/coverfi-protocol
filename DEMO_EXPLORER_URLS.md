# CoverFi — Demo Day Explorer Cheat Sheet

> Run `npx hardhat run scripts/seed-default-demo.ts --network hashkeyTestnet`
> to generate real TX hashes. Replace `<TX1>` and `<TX2>` below with the printed output.

---

## 🟢 Live App (Happy Path — open these first)

| Page | URL | What to show |
|------|-----|-------------|
| Dashboard | https://coverfi-protocol.vercel.app/dashboard.html | $25,175 TVL, IRS radar, premium calculator |
| Pool | https://coverfi-protocol.vercel.app/pool.html | Active pool, 4.2% senior APY, 14.6% junior APY |
| Coverage | https://coverfi-protocol.vercel.app/coverage.html | Protection certificates (connect wallet) |
| Subrogation | https://coverfi-protocol.vercel.app/subrogation.html | **SubrogationNFT #1 from GhostIssuer default** |

---

## 🔴 Default/Payout On-Chain Evidence (Explorer)

### TX1 — Attestor Consensus (TIR.forceConfirmDefault)
```
https://testnet-explorer.hsk.xyz/tx/<TX1>
```
**What to point at:** The `DefaultConfirmed` or `TIRConfirmed` event in the Logs tab.
**Pitch line:** *"Two-of-three attestors reached consensus — custodian, legal rep, and auditor all signed. That confirmation is immutable."*

---

### TX2 — Waterfall Execution (PayoutEngine.executePayout)
```
https://testnet-explorer.hsk.xyz/tx/<TX2>
```
**What to point at:** The Logs tab — walk through 4 events in order:
1. `BondLiquidated` — issuer's 5% bond hit first
2. `PoolLiquidated` — junior tranche absorbs remaining losses
3. `SubrogationClaimed` — NFT minted as evidence container
4. `IssuerDefaulted` — issuer status locked to DEFAULTED on-chain

**Pitch line:** *"One transaction. Four contracts. The entire loss waterfall is atomic — it either all executes or nothing does."*

---

### SubrogationNFT — Read Contract View
```
https://testnet-explorer.hsk.xyz/address/0xbBe8A2840E151cC8BF2B156e5d61a532eFCe2AB9#readContract
```
**What to do:** Call `getClaimData(1)` — shows the full struct:
- `issuerToken` — GhostIssuer address
- `defaultType` — 0 = PAYMENT_DELAY
- `totalPayoutAmount` — USDT paid out
- `bondLiquidated` — bond amount absorbed
- `juniorLiquidated` — junior tranche absorbed
- `insuredHolderCount` — number of holders compensated
- `payoutBlock` — block number (immutable timestamp)

**Pitch line:** *"This NFT is the Foundation's legal weapon. Cryptographic proof of default, timestamped on-chain, portable for court."*

---

### GhostIssuer Token (defaulted state)
```
https://testnet-explorer.hsk.xyz/address/<GHOST_TOKEN_ADDRESS>
```
Replace `<GHOST_TOKEN_ADDRESS>` with the address printed by the script.

---

## 📋 Contract Addresses (for judges who ask)

| Contract | Address |
|----------|---------|
| InsurancePool | 0xa5d64A7770136B1EEade6B980404140D8D5F7C06 |
| PayoutEngine | 0x44944cB598A750Df4C4Bf9A7D3FdDDf7575F88F5 |
| SubrogationNFT | 0xbBe8A2840E151cC8BF2B156e5d61a532eFCe2AB9 |
| TIR (Attestors) | 0xa4ECEB47F80a32D7176C23e4993cDa4d2337Fc3A |
| IssuerRegistry | 0xc07859b25FC869F0a81fae86b9B5bEa868D08A9f |
| srCVR (Senior token) | 0x2Aad26de595752d7D6FCc2f4C79F1Bf15B60E1CD |
| jrCVR (Junior token) | 0xD01e871c97746FC6a3f4B406aA60BE1Fb7FAcf6B |
| Primary Issuer (ACTIVE) | 0xa7C664459C66325Cd9dB15245DD901f1623c9655 |

---

## 🎤 7-Minute Demo Flow (quick reference)

| Min | Action |
|-----|--------|
| 0:00 | Open dashboard.html — "This is live on HashKey Chain right now" |
| 0:45 | Show TVL ($25,175), IRS radar, premium calculator |
| 1:30 | Walk the pool page — senior/junior tranches, APY split |
| 2:30 | Coverage purchase flow (Get Coverage button → form) |
| 3:30 | Switch to TX2 explorer — "Let me show you what happens on default" |
| 4:30 | Walk 4 events in log — bond → junior → SubrogationNFT → DEFAULTED |
| 5:15 | Open subrogation.html — show real minted NFT with claim data |
| 6:00 | Closing: "$89M in real RWA defaults with zero protection. CoverFi fixes that." |
