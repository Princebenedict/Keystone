# Keystone

> The intelligence layer for tokenized real-world assets, built on Arc Network. Launch onchain asset tokens, fund them with USDC, and manage investor positions through a fully contract-backed interface.

[![Built on Arc](https://img.shields.io/badge/Built%20on-Arc-00d4aa)](https://www.arc.io)
[![Foundry](https://img.shields.io/badge/Built%20With-Foundry-f0b90b)](#local-development)
[![Arc Testnet](https://img.shields.io/badge/Network-Arc%20Testnet-00d4aa)](#deployed-addresses)
[![License](https://img.shields.io/badge/License-MIT-green)](#license)

## Live Demo
https://keystone.vercel.app

## One-Line Pitch
Keystone turns asset issuance and investment into an onchain experience: founders launch tokenized offerings, investors discover real contract-created listings, buy with USDC, and claim dividends directly from the app. Every listing is a real deployed contract. No mock data.

## Built on Arc
Keystone is built natively on Arc Network, Circle's stablecoin-native L1:
- USDC is the native gas and settlement token, so every fee is dollar-denominated and predictable
- Sub-second deterministic finality makes launching and investing feel like a normal app
- Circle-native rails (CCTP v2, StableFX, USYC) are surfaced in the Stablecoin Hub for cross-chain funding and treasury yield

## The Problem
Access to real-world asset participation is still fragmented:
- founders rely on slow, manual fundraising flows
- investors face opaque access and weak transparency
- product experiences often show mock data instead of real contract state
- there is no discovery and intelligence layer where trust is verifiable rather than assumed

## The Solution
Keystone is built around real onchain primitives:
- a factory contract deploys a dedicated asset token per listing
- the frontend reads only actual factory-created assets
- risk tier and investor cost basis are stored onchain
- an AI risk engine scores every asset from live chain data
- an onchain builder reputation lets investors verify a founder's track record

## Core Features
- **Builder Studio** — a guided five-step flow to launch a tokenized asset (project, tokenomics, risk, AI review, deploy)
- **Discover** — a live marketplace of real listings with AI risk scores, verified badges, and funding progress
- **AI Risk Engine** — a 0 to 10 score computed from onchain risk tier, funding traction, contract age, and valuation
- **Vault** — portfolio value, onchain cost basis, profit and loss, and claimable dividends, with one-click Claim All
- **Builder Reputation** — a 0 to 100 trust score from deployments, capital raised, activity, and tenure
- **Founder Dashboard** — distribute dividends, withdraw raised capital, and manage listing visibility
- **Stablecoin Hub** — USDC settlement view and Circle-native rails
- **AI Copilot** — an in-browser assistant that answers from live chain state
- Transaction feedback with Arcscan links, network guard, wallet connect and disconnect, and a fully responsive mobile layout

## Product Flow
```text
Founder
  -> launches asset via the factory (Builder Studio)
  -> factory deploys a dedicated asset token
  -> asset becomes visible in Discover
Investor
  -> Discover reads the factory contract
  -> selects an onchain asset
  -> approves USDC
  -> purchases tokens
  -> Vault reads holdings, cost basis, and dividends from token contracts
  -> claims dividends
```

## Contract Design

### AssetLaunchpad (factory)
- charges a listing fee in USDC
- deploys new asset token contracts
- stores the asset registry
- exposes all launched assets to the frontend via getAllAssets()

### AssetToken (per asset)
- asset metadata, company info, asset type, risk tier, valuation, price, and max supply
- total raised, total invested per wallet, total purchased tokens per wallet
- dividend distribution and claiming

## Deployed Addresses
```text
Factory (AssetLaunchpad): 0x705C2b9D3B06eeF72831F463Ca6eBc5A9B543e3b
USDC (native gas token):  0x3600000000000000000000000000000000000000
Network:                  Arc Testnet
Chain ID:                 5042002
RPC:                      https://rpc.testnet.arc.network
Explorer:                 https://testnet.arcscan.app
```
Per-asset token contracts are deployed by the factory on each launch and are readable onchain via getAllAssets().

## Tech Stack
Solidity, Foundry, Ethers.js, HTML, CSS, JavaScript, Vercel.

## Repository Structure
```text
keystone/
├── contracts/
│   ├── AssetToken.sol          asset token per listing plus dividends
│   └── AssetLaunchpad.sol      factory that deploys asset tokens
├── script/
│   └── DeployLaunchpad.s.sol   Foundry deploy script
├── test/
│   └── AssetLaunchpad.t.sol    unit tests
├── .env                        secrets, never commit
├── foundry.toml
├── .gitignore
└── index.html                  full frontend wired to Arc
```


## Local Development
```bash
forge build          # build contracts
forge test           # run tests
forge fmt            # format
```

### Deploy Contracts
```bash
PRIVATE_KEY=0xYOUR_PRIVATE_KEY forge script script/DeployLaunchpad.s.sol:DeployLaunchpad \
  --rpc-url https://rpc.testnet.arc.network --broadcast
```
After deployment, update the factory address in index.html and in this README, then push.

### Redeploy Frontend
```bash
git add .
git commit -m "update frontend"
git push origin main
```
Vercel redeploys automatically on push to main.

## Demo Walkthrough
1. Open the app and connect your wallet (auto-switches to Arc Testnet)
2. Launch an asset from Builder Studio
3. Explore live contract-created listings in Discover
4. Select an asset, approve USDC, and invest onchain
5. View live holdings in the Vault
6. Claim dividends

## Roadmap
- Event indexing for faster data loading
- USYC integration to route idle raise capital into yield
- CCTP v2 for cross-chain investor funding
- Onchain pause flag for trustless listing moderation
- Deeper founder and portfolio analytics

## Author
Princebenedict

## License
MIT

## Trademark
Built on Arc Network. Arc™ is a trademark of Circle Internet Group, Inc. and/or its affiliates. Keystone is an independent product and is not endorsed by or formally affiliated with Circle.
