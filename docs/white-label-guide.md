# FSL White-Label Operator Guide

Deploy HypnoNeuro under your institution's brand — colors, logo, domain, and session economics — without touching the therapeutic game logic.

---

## Overview

The FSL white-label system is file-based. One config file (`src/config/operator.js`) controls all branding and commercial parameters. Operators fork the HypnoNeuro repo, edit this file, and deploy to Vercel (or any static host).

---

## Quick Start

```bash
# 1. Fork or clone the HypnoNeuro frontend
git clone https://github.com/Future-Systems-Lab/hypnoneuro-games
cd hypnoneuro-games/build/frontend

# 2. Edit the operator config
nano src/config/operator.js

# 3. Install and build
npm install
npm run build

# 4. Deploy to Vercel under your domain
vercel --prod
```

---

## Operator Config Reference

File: `src/config/operator.js`

| Field | Type | Description | FSL Default |
|---|---|---|---|
| `operatorName` | string | Displayed in UI headers and footers | `'Future Systems Lab'` |
| `domain` | string | Your deployment domain (informational) | `'hypnoneuro-frontend.vercel.app'` |
| `primaryColor` | CSS color | Page background | `'#050a0f'` |
| `accentColor` | CSS color | Primary accent (buttons, borders, headings) | `'#00FFD4'` |
| `accentSecondary` | CSS color | Secondary highlight color | `'#7FFFD4'` |
| `goldColor` | CSS color | Reward / token color | `'#FFD700'` |
| `fontHeading` | CSS font-family | Heading font | `'Orbitron', monospace` |
| `fontBody` | CSS font-family | Body font | `'Exo 2', sans-serif` |
| `fontSerif` | CSS font-family | Display serif font | `'Cinzel', serif` |
| `logoUrl` | string \| null | URL to your logo image; `null` uses text wordmark | `null` |
| `sessionFee` | number | USD per session (0 = token/NFT gated only) | `0` |
| `platformFeePercent` | number | FSL platform cut percentage | `20` |
| `hntSplitOperator` | number | % of HNT minted per session going to operator | `80` |
| `hntSplitPlatform` | number | % of HNT going to FSL platform wallet | `20` |
| `showProviderDirectory` | boolean | Show/hide Provider Directory card | `true` |
| `showSupportCircles` | boolean | Show/hide Support Circles card | `true` |
| `showAlchemistForge` | boolean | Show/hide AlchemistForge card | `true` |
| `showCOALookup` | boolean | Show/hide COA Lookup card | `true` |
| `showAvatarSession` | boolean | Show/hide Avatar Session card | `true` |
| `showHelpCenter` | boolean | Show/hide Help Center link | `true` |

---

## Example: University Wellness Center

```js
const operator = {
  operatorName: 'UNC Wellness Hub',
  domain: 'wellness.unc.edu',
  primaryColor: '#001A57',         // UNC navy
  accentColor: '#4B9CD3',          // Carolina blue
  accentSecondary: '#7EB8DA',
  goldColor: '#FFD700',
  fontHeading: "'Orbitron', monospace",
  fontBody: "'Exo 2', sans-serif",
  fontSerif: "'Cinzel', serif",
  logoUrl: 'https://your-cdn.com/unc-logo.png',
  sessionFee: 0,
  platformFeePercent: 30,
  hntSplitOperator: 70,
  hntSplitPlatform: 30,
  showAlchemistForge: false,       // hide shadow work module for campus deployment
}
```

---

## Environment Variables (VPS / API)

Operators running their own EncryptHealth API instance must set:

```bash
# /opt/encrypthealth/backend/.env
DATABASE_URL=postgresql://...
DAILY_API_KEY=your-daily-co-key
STRIPE_SECRET_KEY=sk_live_...
INFURA_KEY=your-infura-key
PORT=4001
NODE_ENV=production
```

---

## Operator Tier Summary

| Tier | Price | Sessions | HNT Split |
|---|---|---|---|
| Starter | $497/mo | 100/mo | 70 / 30 |
| Growth | $1,497/mo | 500/mo | 75 / 25 |
| Enterprise | Custom | Unlimited | 80 / 20 |

See `/operator` in-app or contact `contact@futuresystemslab.com`.

---

## Custom Domain on Vercel

```bash
# After deploying, add your domain in the Vercel dashboard
# Or via CLI:
vercel domains add wellness.unc.edu
vercel alias your-deployment-url.vercel.app wellness.unc.edu
```

Set DNS CNAME: `cname.vercel-dns.com`

---

## What Operators Cannot Change Without a License Agreement

- The underlying therapeutic game logic and content
- The EncryptHealth blockchain anchoring protocol
- The ClaimChain contract ABI (operators get a separate contract deployment)
- FSL smart contract addresses (operators get their own HNT/NFT contracts on mainnet)

---

## Support

- Email: `contact@futuresystemslab.com`
- Apply at: `https://hypnoneuro-frontend.vercel.app/operator`
- Command Center: `https://fsl-command-center.vercel.app`
