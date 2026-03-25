# FSL Mainnet Migration Plan

**Current:** Ethereum Sepolia testnet
**Target:** Polygon (MATIC) or Base (Coinbase L2) — decision pending cost analysis
**Estimated Timeline:** 4–6 weeks post-revenue confirmation

---

## Target Chain Decision

| Factor | Polygon | Base |
|---|---|---|
| Gas cost per tx | ~$0.001–0.01 | ~$0.0005–0.005 |
| Finality | ~2 sec | ~2 sec |
| EVM compatible | Yes | Yes |
| Ecosystem | Large — DeFi, gaming | Growing — Coinbase rails |
| USDC native | Yes | Yes (native USDC) |
| HNT liquidity path | Uniswap v3 Polygon | Uniswap v3 Base |
| Recommendation | ✓ Primary | ✓ Fallback / dual-deploy |

**Decision trigger:** First paying operator contract signed → deploy to Polygon mainnet.

---

## Phase 1 — Pre-Migration (Week 1–2)

### Contract Audit
- [ ] Internal audit of all 5 FSL contracts for mainnet readiness
- [ ] Remove any Sepolia-specific test logic (`require(block.chainid == 11155111, ...)`)
- [ ] Review HNT mint limits — set appropriate mainnet caps
- [ ] Review FSLToken `mint()` access control — confirm owner-only
- [ ] Confirm EncryptHealthVault SHA-256 anchoring logic is chain-agnostic

### Environment Preparation
- [ ] Acquire MATIC (Polygon) or ETH (Base) for deployer wallet gas
- [ ] Estimate deployment cost (see Cost Estimates section)
- [ ] Set up mainnet Infura/Alchemy endpoint
- [ ] Add mainnet RPC to `.env` on VPS
- [ ] Generate a dedicated mainnet deployer wallet (separate from Sepolia deployer)

### Hardhat Config Update
```js
// hardhat.config.js — add polygon mainnet
polygon: {
  url: process.env.POLYGON_RPC_URL,
  accounts: [process.env.DEPLOYER_PRIVATE_KEY],
  chainId: 137,
}

// or Base mainnet
base: {
  url: 'https://mainnet.base.org',
  accounts: [process.env.DEPLOYER_PRIVATE_KEY],
  chainId: 8453,
}
```

---

## Phase 2 — Contract Redeployment (Week 2–3)

### Deployment Order

Deploy in this exact order — later contracts may depend on earlier addresses:

1. **FSLToken (HNT)**
   ```bash
   npx hardhat run scripts/deploy-fsl-token.js --network polygon
   ```
   - Note new address → update `REACT_APP_HNT_ADDRESS`
   - Verify on Polygonscan: `npx hardhat verify --network polygon <address>`

2. **HypnoNeuroNFT**
   ```bash
   npx hardhat run scripts/deploy-hypnoneuro-nft.js --network polygon
   ```
   - Note new address → update `REACT_APP_NFT_ADDRESS`

3. **MindMasteryNFT**
   ```bash
   npx hardhat run scripts/deploy-mindmastery-nft.js --network polygon
   ```
   - Note new address → update `useNFTGate.js`

4. **EncryptHealthVault**
   ```bash
   npx hardhat run scripts/deploy-vault.js --network polygon
   ```
   - Note new address → update backend `blockchain.js`

5. **ClaimChainDAO**
   ```bash
   npx hardhat run scripts/deploy-claimchain.js --network polygon
   ```
   - Note new address → update ClaimChain frontend

6. **AlchemistForge**
   ```bash
   npx hardhat run scripts/deploy-alchemist.js --network polygon
   ```
   - Note new address → update GameSelection AlchemistForge card href

### Post-Deploy Verification Checklist
- [ ] All 6 contracts verified on Polygonscan / Basescan
- [ ] HNT `mint()` called once with test wallet — confirm success
- [ ] NFT `safeMint()` called — confirm token appears in MetaMask
- [ ] EncryptHealthVault `anchor()` called — confirm event emitted
- [ ] ClaimChainDAO governance functions callable

---

## Phase 3 — Frontend Contract Address Updates (Week 3)

### Files to Update

**`src/hooks/useNFTGate.js`**
```js
// Change:
const CONTRACT_ADDRESS = '0xCb9EcB00574DB29976c7C54045d443666D5C7771'  // Sepolia
// To:
const CONTRACT_ADDRESS = process.env.VITE_MINDMASTERY_NFT_ADDRESS      // mainnet
```

**`src/config/operator.js`** — no contract addresses here, just branding

**`src/pages/GameSelection.jsx`** — AlchemistForge href
```js
// Update AlchemistForge card href to new Polygonscan/Basescan URL
href="https://polygonscan.com/address/NEW_ADDRESS"
```

**`src/pages/HelpCenter.jsx`** — contract addresses in FAQ text
- Update all 5 contract addresses in FAQ copy

### Environment Variables (Vercel)
Set in Vercel project settings → Environment Variables:
```
VITE_HNT_ADDRESS=0xNEW_POLYGON_HNT
VITE_NFT_ADDRESS=0xNEW_POLYGON_NFT
VITE_MINDMASTERY_NFT=0xNEW_POLYGON_MINDMASTERY
VITE_VAULT_ADDRESS=0xNEW_POLYGON_VAULT
VITE_CLAIMCHAIN_ADDRESS=0xNEW_POLYGON_CLAIMCHAIN
VITE_CHAIN_ID=137
```

### VPS `.env` Updates (`/opt/encrypthealth/backend/.env`)
```bash
CHAIN_ID=137
RPC_URL=https://polygon-mainnet.infura.io/v3/YOUR_KEY
HNT_CONTRACT=0xNEW_POLYGON_HNT
VAULT_CONTRACT=0xNEW_POLYGON_VAULT
```

---

## Phase 4 — Infrastructure Updates (Week 3–4)

### MetaMask Network Switch
Update WalletConnect prompts to request Polygon (chainId 137) instead of Sepolia (11155111):

```js
// src/store/walletStore.js — update chainId check
const REQUIRED_CHAIN_ID = 137  // Polygon mainnet (was 11155111 Sepolia)
const REQUIRED_CHAIN_NAME = 'Polygon'
const REQUIRED_RPC = 'https://polygon-rpc.com'
```

### EncryptHealth API
- Update `blockchain.js` RPC to Polygon mainnet endpoint
- Update contract addresses in `token-engine.js`
- Run `node --check index.js` → `pm2 restart encrypthealth-api`

### XRPL Dual-Chain Anchor
- XRPL anchor is chain-agnostic — no changes needed
- Update Sepolia explorer URLs to Polygonscan in anchor response JSON

### ClaimChain Frontend
- Update contract address in `Web3Context.jsx`
- Rebuild and redeploy to Vercel

---

## Phase 5 — Testing & Cutover (Week 4–5)

### Mainnet Pre-Launch Test (with real funds, small amounts)
- [ ] Connect MetaMask to Polygon mainnet
- [ ] Complete one full session end-to-end: wallet connect → game → HNT mint
- [ ] Verify HNT appears in MetaMask on Polygon
- [ ] Verify EncryptHealthVault anchor tx on Polygonscan
- [ ] Verify XRPL anchor (unchanged)
- [ ] Test ClaimChain insurance path with mainnet contract
- [ ] Test Provider Directory — booking flow creates session on mainnet

### DNS / Domain
- If using custom domain: update Vercel project domain
- No changes needed for `hypnoneuro-frontend.vercel.app`

### Cutover
- [ ] Announce to all current testers (email / Telegram)
- [ ] Update Command Center status.json — change all "Sepolia" references to "Polygon"
- [ ] Archive Sepolia contract addresses in a `docs/sepolia-archive.md`
- [ ] Tag the Git release: `git tag v1.0.0-mainnet`

---

## Cost Estimates

### Polygon Mainnet Gas (estimated at MATIC $0.80)

| Contract | Estimated Gas | Estimated Cost |
|---|---|---|
| FSLToken (HNT) | ~2,500,000 gas | ~$0.50 |
| HypnoNeuroNFT | ~1,800,000 gas | ~$0.35 |
| MindMasteryNFT | ~1,800,000 gas | ~$0.35 |
| EncryptHealthVault | ~1,200,000 gas | ~$0.25 |
| ClaimChainDAO | ~2,000,000 gas | ~$0.40 |
| AlchemistForge | ~1,500,000 gas | ~$0.30 |
| **Total deployment** | ~10,800,000 gas | **~$2.15** |

**Per session anchor (mainnet):** ~50,000 gas → ~$0.01 per session

### Base Mainnet Gas (estimated at ETH $3,500)

| Item | Estimated Cost |
|---|---|
| Total deployment | ~$8–15 |
| Per session anchor | ~$0.005–0.02 |

**Polygon is strongly preferred for high-volume session anchoring.**

---

## Rollback Plan

If mainnet deployment fails or a critical bug is found post-launch:

1. Revert Vercel env vars to Sepolia addresses
2. Redeploy frontend from last Sepolia build (`vercel rollback`)
3. Restart EncryptHealth API with Sepolia `.env`
4. Notify users via Telegram / email
5. Keep mainnet contracts deployed — do NOT selfdestruct

---

## Post-Migration

- [ ] Update `docs/white-label-guide.md` with mainnet contract addresses
- [ ] Update `status.json` — all projects mainnet
- [ ] File for Polygonscan/Basescan "verified project" badge
- [ ] Add Polygon to HNT token listing on CoinGecko (informational)
- [ ] Begin Uniswap v3 liquidity pool setup for HNT/USDC (optional)
