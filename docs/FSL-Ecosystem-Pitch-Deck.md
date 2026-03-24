# Future Systems Lab — Ecosystem Pitch Deck
### Sovereign Wellness Infrastructure · March 2026

---

## Slide 1 — What Is FSL?

**Future Systems Lab (FSL)** is the world's first sovereign wellness infrastructure stack.

We are not a healthcare provider. We are not a SaaS company. We are infrastructure.

FSL builds the rails, protocols, and tools that allow practitioners, operators, and individuals to deliver and receive frequency-based wellness sessions — with every interaction anchored on two blockchains, every token earned rather than purchased, and no protected health information ever stored on any centralized server.

**Tagline:** *Sovereign wellness infrastructure for the frequency age.*

**What we are not:**
- We are not diagnosing, treating, or curing any condition
- We are not a healthcare provider under HIPAA definitions
- We are not offering securities — HNT is a digital tool (Walsh Protocol wellness framing, SEC March 17 2026 token taxonomy)
- We are not Headspace, BetterHelp, or Calm

**What we are:**
- A dual-chain anchored session infrastructure protocol
- A practitioner credentialing and marketplace layer
- A token-incentivized wellness engagement model
- A turn-key operator licensing system for clinics, retreats, and practitioners

---

## Slide 2 — The Five Platforms

| Platform | Function | Status | Live URL |
|---|---|---|---|
| **HypnoNeuro** | 27 frequency wellness games, L1/L2/L3, HNT mint on completion | ✅ Live | hypnoneuro-frontend.vercel.app |
| **EncryptHealth** | Backend API, session logging, ML recommendations, provider registry | ✅ Live | frontend-omega-six-b9szh0blg5.vercel.app |
| **ClaimChain** | Insurance eligibility verification, dual-chain claim anchoring, DAO governance | ✅ Live | claimchain-frontend.vercel.app |
| **NeuroBalance Watch** | Biometric integration layer, wearable data ingest, HRV/EEG session scoring | ✅ Live | neurobalance-deploy.vercel.app |
| **AlchemistForge** | HNT cross-chain bridge, XRPL ↔ Sepolia token flow, operator liquidity | ✅ Live | Sepolia: 0xE092336F… |

**Command Center:** fsl-command-center.vercel.app — live operational dashboard

All five platforms share:
- A unified wallet identity layer (MetaMask EVM + GemWallet/Crossmark XRPL)
- A common session schema anchored on Sepolia + XRPL simultaneously
- The HNT utility token as the engagement and reward medium
- The EncryptHealth API as the shared data spine (VPS: 74.208.202.239, PM2-managed, PostgreSQL)

---

## Slide 3 — Technical Architecture

### Dual-Chain Session Anchoring

Every completed wellness session produces two on-chain records:

**Sepolia (EVM):**
- `POST /api/game/complete` → `processSessionReward()` → `mintHNT(wallet, 1)` via deployer key
- Transaction hash returned to frontend, displayed on Blockscout
- NFT milestone minted at sessions 3, 6, 9 (L1/L2/L3 MindMasteryNFT)

**XRPL Testnet:**
- SHA-256 of session payload anchored via `AccountSet` memo transaction
- Wallet: `rwR3etLJVxtQXYs2d9jgBQxzHBivtCMdPd` (100 XRP funded)
- Explorer: testnet.xrpl.org — every session publicly verifiable
- No PHI in memo field — only a hash

### Why Two Chains?

| Property | Sepolia (EVM) | XRPL |
|---|---|---|
| Token minting | HNT ERC-20, NFT ERC-721 | — |
| Audit trail | Blockscout explorer | testnet.xrpl.org |
| Cost | Deployer pays gas (free to user) | ~0.000012 XRP per anchor |
| Compliance relevance | SEC digital tool taxonomy | XRPL HIPAA-grade infrastructure (XRPH) |
| Finality | ~12s Sepolia | ~3-4s XRPL |

### Stack

```
Frontend:    React 18 + Vite, React Router v6, Zustand, Ethers.js v6
Backend:     Express.js, PostgreSQL, PM2, Nginx reverse proxy
Blockchain:  viem (Sepolia), xrpl.js 4.6.0 (XRPL)
Auth:        MetaMask EIP-712 personal_sign gate
AI:          ML recommendation engine (/api/recommend/:wallet) — NT time-weight scoring
Infra:       Vercel (5 frontends), VPS 74.208.202.239 (EncryptHealth API)
```

---

## Slide 4 — Competitive Differentiators

### FSL vs. Headspace / Calm

| Dimension | FSL | Headspace/Calm |
|---|---|---|
| Architecture | Sovereign, wallet-gated, on-chain | Centralized SaaS, subscription |
| Data model | No PHI stored, hash-only anchoring | User data on centralized servers |
| Practitioner layer | 4-tier credentialed directory | None |
| Token model | Earned via session completion | None |
| Insurance path | ClaimChain DAO-governed eligibility | Out-of-pocket only |
| Frequency modality | L1 Hypnosis, L2 Orthomolecular, L3 Inner-Child | Meditation only |
| Operator model | Turn-key white-label for clinics/retreats | No |

### FSL vs. Biocanic

| Dimension | FSL | Biocanic |
|---|---|---|
| Session types | 27 cross-modality games + avatar rooms | Biofeedback only |
| Blockchain anchoring | Dual-chain (EVM + XRPL) | None |
| Token economy | HNT utility token + NFT progression | None |
| Insurance integration | ClaimChain live portal | Pending |
| Provider credentialing | 4-tier automated | Manual |
| API openness | FHIR export, biosensing ingest, public schema | Closed |

### FSL vs. XRPH

XRPH and FSL are **complementary, not competing.** XRPH provides HIPAA-grade healthcare infrastructure on XRPL. FSL uses XRPL as an audit rail and is positioned as the wellness (non-healthcare) consumer layer that feeds the XRPH compliance stack upward.

**Proposed integration:** FSL sessions anchored on XRPL → XRPH compliance verification layer → insurance-eligible claims via ClaimChain.

---

## Slide 5 — Legal Compliance Framework

### SEC Token Taxonomy (March 17, 2026)

The SEC's March 2026 staff bulletin clarified the digital tool taxonomy. HNT qualifies as a **digital tool token** under this framework:

- HNT is earned through session completion — not purchased
- HNT has no secondary market listing or exchange liquidity
- HNT functions as a session credit and platform access tool
- HNT cannot be exchanged for fiat on FSL infrastructure
- No profit expectation is marketed to users

**Conclusion:** HNT is not a security under the Howey test as clarified by March 2026 guidance.

### HIPAA Alignment (No-PHI Architecture)

FSL is architected to avoid HIPAA scope entirely:

- **No PHI stored:** All session records use wallet addresses, not names, emails, or medical identifiers
- **Hash-only anchoring:** On-chain data is SHA-256 of session metadata — not readable health data
- **Wallet-gated identity:** Users are their wallet address. No PII collected at registration.
- **Disclaimer on-chain:** FTC liability disclaimer anchored to Sepolia + XRPL at deployment
- **No provider-patient relationship:** FSL facilitates frequency wellness sessions — practitioners are listed as wellness professionals, not licensed medical providers

### CLARITY Act Readiness

The CLARITY Act (pending Congressional passage, expected 2026) would establish a formal digital commodity vs. security distinction. FSL's architecture is designed to meet the digital commodity standard:
- Decentralized session logging
- No centralized control of token supply post-deployment
- Open-source smart contracts on Sepolia (Blockscout-verified)

### Walsh Protocol Wellness Framing

FSL uses the Walsh Research Institute's orthomolecular wellness framework (not medical treatment):
- L2 Orthomolecular game layer maps to NT-targeted supplement protocols
- All supplement recommendations are framed as wellness support, not treatment
- Dr. Meg's Fullscript protocols are practitioner-grade, not prescription
- ISOM (International Society for Orthomolecular Medicine) practitioner alignment

### FTC Liability Disclaimer

A three-placement disclaimer is rendered on the FSL frontend:
1. At wallet connect (consent gate)
2. At game selection (before any session)
3. Anchored on-chain at each session completion via XRPL memo

Disclaimer text: *FSL is a sovereign wellness infrastructure platform. Nothing on this platform constitutes medical advice, diagnosis, or treatment. Sessions are frequency-based wellness tools. Consult a qualified healthcare provider for medical concerns.*

---

## Slide 6 — Token Economy

### HNT (HypnoNeuro Token)

| Property | Value |
|---|---|
| Contract | `0x1ae1e10929f008d1f9883ce574a318abd86084e2` (Sepolia) |
| Standard | ERC-20 |
| Mint mechanism | Server-side, deployer pays gas, minted to user wallet |
| Earn rate | 1 HNT per completed session |
| Total supply | Uncapped (utility token, not investment) |
| Use cases | Platform access credits, provider booking, operator licensing |

### MindMastery NFT Progression

| Milestone | Sessions | NFT | Level Unlocked |
|---|---|---|---|
| L1 Completion | 3 | MindMastery L1 NFT | Orthomolecular (L2) |
| L2 Completion | 6 | MindMastery L2 NFT | Inner-Child Healing (L3) |
| L3 Completion | 9 | MindMastery L3 NFT | Full practitioner network access |

**Contract:** `0xCb9EcB00574DB29976c7C54045d443666D5C7771` (MindMasteryNFT, Sepolia)

### Platform Fee Model

| Revenue Stream | Model | Status |
|---|---|---|
| Crypto access (MetaMask) | One-time session unlock via wallet signature | Live |
| Fiat access (Stripe) | Recurring subscription / single session | Pending (EIN processing) |
| Insurance (ClaimChain) | Eligibility → claim → DAO-governed reimbursement | Live portal |
| XRPL payment channel | Native XRP micropayments per session | Testnet live |
| Operator licensing | Turn-key white-label fee to clinics/retreats | Active |
| Fullscript affiliate | Dr. Meg practitioner margin on supplement orders | Live |

### Wellness Score

Every wallet accumulates a **Wellness Score** (0–100) based on:
- Session completion frequency
- Neurotransmitter coverage diversity
- ML-recommended next session adherence
- Provider engagement (booked sessions)

Score is displayed in real-time on the GameSelection dashboard and auto-refreshes via `hn_game_complete` custom event.

---

## Slide 7 — Provider Credentialing System

### Four-Tier Architecture

| Tier | Name | Requirements | Privileges |
|---|---|---|---|
| 1 | Platinum | ISOM board certification + 10yr experience + peer review | Featured placement, group session facilitation, clinical protocol submission |
| 2 | Gold | Licensed practitioner + 5yr orthomolecular experience | Booking available, L3 session facilitation |
| 3 | Silver | Certified wellness practitioner + 2yr experience | Booking available, L1/L2 facilitation |
| 4 | Verified | Basic identity verification + FSL orientation | Directory listing, L1 facilitation only |

### Current Directory

- **Dr. Meg (Platform Director):** `pin_order=999`, `bookable=false` — displayed first with `◈ PLATFORM DIRECTOR` badge, not bookable
- Providers sorted: `pin_order DESC, credential_tier ASC`
- `display_note` field shown below credentials for custom messaging

### ISOM Alignment

FSL provider credentialing follows ISOM practitioner standards for orthomolecular wellness:
- Magnesium/mineral protocol certification
- Amino acid therapy competency
- NT-targeted supplement protocol knowledge
- Inner-child and trauma-informed facilitation (L3)

### Provider Portal

Live at: `provider-portal-wheat.vercel.app`

Features: 14-tab dashboard — patients, sessions, client summary, intake forms, supplement protocols, lab results, booking link, ClaimChain claims, payment, admin, earnings, provider directory, session reminders, e-sign.

---

## Slide 8 — Biosensing Hardware Partner Pathway

### Why Biosensing?

FSL's AI recommendation engine currently uses session history and time-of-day neurotransmitter weighting. Adding real-time biometric data (HRV, EEG, GSR, respiratory rate) transforms it from reactive to **predictive sovereign wellness.**

### Five Identified Partners

| Company | Modality | Integration Path | Priority |
|---|---|---|---|
| **Caltech Biophysics** | EEG + MEG research | FHIR API ingest → EncryptHealth `/api/biometrics` | 1 (outreach Q2 2026) |
| **Muse (InteraXon)** | Consumer EEG (Muse headband) | WebBluetooth → NeuroBalance Watch ingest | 2 |
| **Empatica** | Clinical-grade HRV + EDA | FHIR R4 export → EncryptHealth | 3 |
| **Polar** | HRV + sleep tracking | REST API → NeuroBalance Watch | 4 |
| **Oura Ring** | HRV, temperature, sleep | Webhook → EncryptHealth `/api/biometrics/:wallet` | 5 |

### Integration Architecture

```
Biosensor Device
      ↓
NeuroBalance Watch (ingest layer)
      ↓
POST /api/biometrics/{wallet} → EncryptHealth
      ↓
ML Recommender (/api/recommend/:wallet)
      ↓
Personalized game/session selection
      ↓
XRPL anchor includes biometric hash (no raw data)
```

### FHIR Compliance

EncryptHealth exposes a FHIR R4-compatible export endpoint (`GET /api/export/:wallet`) for practitioner and insurance interoperability. No raw PHI is stored — all biometric anchoring uses SHA-256 hashing.

---

## Slide 9 — XRPL Dual-Chain Audit Proof

### Architecture

Every FSL session produces an immutable audit record:

```
Session Complete
      ↓
POST /api/game/complete
      ↓
┌─────────────────────┐     ┌──────────────────────┐
│   Sepolia (EVM)     │     │   XRPL Testnet        │
│   mintHNT(wallet,1) │     │   AccountSet + memo   │
│   HNT tx hash       │     │   SHA-256 of payload  │
│   Blockscout URL    │     │   testnet.xrpl.org    │
└─────────────────────┘     └──────────────────────┘
      ↓                               ↓
   Returned to frontend — displayed on win screen
```

### Live Proof

Recent session anchor:
- **XRPL:** `75C3673A67624B3D1733190B0778281F9DB139EB4EB83F808AE55F9EEBE93888`
- **Explorer:** https://testnet.xrpl.org/transactions/75C3673A67624B3D1733190B0778281F9DB139EB4EB83F808AE55F9EEBE93888
- **Sepolia HNT Mint:** `0x36a592e35c23d9a55140b212c316783eda1bb992c2c7e883c40e27962769f3c5`
- **Blockscout:** https://eth-sepolia.blockscout.com/tx/0x36a592e3...

### Why This Matters for Compliance

1. **Session immutability:** No centralized actor can alter or delete session records
2. **Privacy-preserving:** Hash-only, no PHI on-chain
3. **Insurance auditability:** ClaimChain can verify session occurrence without accessing health data
4. **Legal defensibility:** On-chain timestamp is irrefutable proof of session delivery
5. **XRPH pathway:** XRPL anchors are compatible with XRPH's HIPAA-grade infrastructure layer

---

## Slide 10 — Partnership Opportunities

### 1. XRPH (XRP Healthcare)

**Opportunity:** XRPH is building HIPAA-grade healthcare infrastructure on XRPL. FSL is the wellness (non-clinical) consumer layer. Proposed integration:

- FSL session XRPL anchors feed into XRPH compliance verification
- XRPH provides HIPAA-grade data rails for future FSL clinical tier
- ClaimChain insurance claims processed via XRPH-verified provider identities
- Joint grant application to XRPL Foundation ($25K FSL application in progress)

**Ask:** Technical partnership MOU + XRPL Foundation co-application.

### 2. Biosensing Hardware Partners

**Opportunity:** FSL has built the ingest API and biometric anchoring architecture. We need hardware distribution and data partnership agreements.

- NeuroBalance Watch as the software integration layer for physical wearables
- FSL provides the sovereign data model (user owns their biometric hash, not the hardware company)
- Revenue share on HNT earned from biosensor-enhanced sessions

**Ask:** API integration pilot with one biosensing partner (Caltech or Muse recommended).

### 3. ISOM Practitioner Network

**Opportunity:** 400+ ISOM-certified practitioners globally. FSL provides them a credentialed platform to deliver orthomolecular wellness sessions at scale, with tokenized session proofs and insurance-eligible documentation.

**Ask:** ISOM endorsement + practitioner directory onboarding partnership.

### 4. Insurance / Benefits Platforms

**Opportunity:** Employers and insurance companies are seeking evidence-based wellness programs. FSL provides on-chain proof of session delivery, ML-scored wellness outcomes, and ClaimChain insurance integration.

**Ask:** Pilot program agreement with 1 benefits platform or self-insured employer.

### 5. Retreat / Clinic Operators

**Opportunity:** Turn-key FSL operator licensing. A retreat or clinic gets the full FSL stack (branded, white-labeled) with provider directory, session logging, HNT rewards, and ClaimChain insurance — for a flat operator licensing fee.

**Ask:** First operator licensing deal — psilocybin-adjacent retreat or biohacking clinic preferred.

---

## Slide 11 — Turn-Key Operator Model

### What an Operator Gets

```
FSL White-Label License
├── Branded HypnoNeuro frontend (custom domain + colors)
├── EncryptHealth API (dedicated namespace in shared DB)
├── Provider directory (operator's practitioners, tiered)
├── HNT minting (operator earns % of session HNT)
├── ClaimChain insurance path (operator-specific payer network)
├── NeuroBalance Watch integration (if hardware provided)
├── AlchemistForge bridge access (HNT ↔ XRP for operator treasury)
└── FSL Command Center dashboard (real-time operator analytics)
```

### Operator Pricing Model (Proposed)

| Tier | Monthly Fee | Sessions Included | HNT Split |
|---|---|---|---|
| Starter | $497/mo | 100 sessions | 70% operator / 30% FSL |
| Growth | $1,497/mo | 500 sessions | 75% operator / 25% FSL |
| Enterprise | Custom | Unlimited | 80% operator / 20% FSL |

### Target Operator Profiles

- Psilocybin-adjacent integration retreat centers
- Biohacking clinics (functional medicine, IV therapy, neurofeedback)
- Hypnotherapy group practices
- Corporate wellness program operators
- Online wellness coaches with existing audience

---

## Slide 12 — Roadmap

### Q2 2026 (Current)
- [x] All 5 platforms live
- [x] Dual-chain anchoring operational (Sepolia + XRPL)
- [x] HNT mint + MindMastery NFT confirmed end-to-end
- [x] Provider directory with 4-tier credentialing
- [x] ClaimChain insurance portal live
- [x] Avatar session rooms (Daily.co WebRTC)
- [ ] Stripe activation (EIN confirmed, processing)
- [ ] XRPL grants application submitted
- [ ] First biosensing partner API pilot (Caltech outreach)

### Q3 2026
- [ ] XRPH partnership MOU signed
- [ ] First operator license deployed
- [ ] Mainnet migration (Polygon or Base — gas cost optimization)
- [ ] ISOM practitioner network onboarding (target: 20 Tier 3+ providers)
- [ ] NeuroBalance Watch v2 with first hardware integration
- [ ] Stripe live — fiat payment path active

### Q4 2026
- [ ] CLARITY Act compliance review (pending Congressional passage)
- [ ] 100 active operators
- [ ] 10,000 sessions anchored on-chain
- [ ] AlchemistForge mainnet bridge live
- [ ] Insurance reimbursement first claim processed via ClaimChain

---

## Slide 13 — Team & Infrastructure

**Dr. Meg (Platform Director):**
Orthomolecular wellness practitioner, ISOM-aligned, Walsh Protocol-trained. Fullscript practitioner account holder. Product vision and clinical protocol design.

**Future Systems Lab (FSL):**
Autonomous software engineering infrastructure. Full-stack monorepo. Operates on VPS + Vercel deployment architecture. PM2-managed API, PostgreSQL database, dual-chain smart contract deployment.

**Infrastructure Footprint:**
- 5 Vercel frontends (hypnoneuro, encrypthealth, claimchain, neurobalance, command center)
- 1 VPS (74.208.202.239) — Express API, PostgreSQL, PM2, Nginx
- 6 Sepolia smart contracts (FSLToken, HypnoNeuroNFT, MindMasteryNFT, EncryptHealthVault, ClaimChainDAO, AlchemistForge)
- XRPL testnet wallet funded and anchoring live
- fail2ban: 188+ SSH brute-force attempts blocked, 119 IPs banned

---

## Slide 14 — The Ask

**FSL is seeking:**

1. **Strategic partners** — XRPH, Biocanic (complementary), ISOM, biosensing hardware companies
2. **First operator licenses** — retreat centers, biohacking clinics, group practices
3. **XRPL Foundation grant** ($25K) — dual-chain anchor infrastructure, XRPL wallet connect, HNT token bridge
4. **Introductions** — to insurance/benefits procurement, SEC-adjacent legal review, CLARITY Act working groups

**What we bring:**
- A fully operational, dual-chain anchored wellness infrastructure stack
- 27 content experiences across 3 therapeutic modalities
- A legal framework designed from the ground up to survive 2026 regulatory clarity
- An architecture that treats user sovereignty, not platform lock-in, as the product

**Contact:** Via FSL Command Center — fsl-command-center.vercel.app

---

*Future Systems Lab · Frequency Sovereignty Lab · All rights reserved · Unlicensed*
*Nothing in this document constitutes financial, legal, or medical advice.*
*HNT is a digital tool token. Sessions are frequency-based wellness experiences.*
