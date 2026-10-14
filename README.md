# Tracktos

A friendly, hobby-made window into Aptos. Tracktos started as a way to learn how the Aptos chain works by drawing it: who a wallet moves value with, where a token is flowing right now, and what mainnet looks and sounds like block by block.

**[Open it → tracktos.com](https://tracktos.com)** · free, no sign-up, works on phones and installs like an app

![Move Map](screenshots/movemap_desktop_dark.png)

## What you can do

### Explore a wallet: Move Map
Paste any Aptos address and its counterparties appear as living streams of value: bubbles sized by volume, flowing in and out of the wallet. Replay its history over time, spot the biggest moves as flares, expand two hops out, filter by token, period and kind (wallets, protocols, contracts), and share a card of the map. Known exchanges and protocols are labelled.

### Token Flow
Pick APT, USDC, USDt or any coin type / fungible asset and see its recent transfers as one picture: who is sending, who is receiving, and which exchanges and protocols sit in the middle, over the last hour, day or week.

![Token Flow](screenshots/tokenflow_desktop_dark.png)

### Live Aptos
- **Pulse**: mainnet's live transactions turned into light and sound. Several visual modes (Garden, Constellation, Orbit, Rain, Pulse Lines), each with its own in-key instrument palette that follows network activity, big APT and stablecoin moves called out as they happen, a Live Transactions feed linking to the explorer, ambient mode, and **Record 10s** to save a clip with sound.
- **Network**: a 3D globe of the validator set. Every pulse is a real block, drawn from the validator that proposed it, alongside live TPS, epoch progress, transaction mix, staking and supply.

![Pulse](screenshots/pulse_desktop_dark.png)
![Live Network](screenshots/network_desktop_light.png)

### Wallet explorer
Balances and staking, portfolio history, token holdings, NFT gallery, transaction analytics, DeFi activity and a playful on-chain identity, with export to image or PDF. Mainnet and testnet.

![Wallet](screenshots/wallet_desktop_dark.png)

### And
- **Nicknames** for wallets you follow, saved only in your browser
- **Light and dark** themes
- Designed for **phones** as carefully as for desktop

| | | |
|---|---|---|
| ![Home](screenshots/home_mobile_dark.png) | ![Move Map on a phone](screenshots/movemap_mobile_dark.png) | ![Pulse on a phone](screenshots/pulse_mobile_dark.png) |
| ![Network on a phone](screenshots/network_mobile_dark.png) | ![Wallet on a phone, light](screenshots/wallet_mobile_light.png) | ![Home, light](screenshots/home_desktop_light.png) |

## How it fits together

```
                    Browser (tracktos.com)
          React single-page app, canvas & WebGL views
              │                 │                 │
              │ static files    │ /api/live*      │ /api/flows*
              ▼                 │ /api/stats      ▼
      Cloudflare Pages          │ /api/portfolio  tracktos-flows worker
                                ▼                 · cron collector every 2 min
                     tracktos-edge worker         · rolls token transfers into D1
                     · one shared 2–30 s cache    │
                       for every visitor          ▼
                     · wallet stats endpoints   Cloudflare D1
                                │                 │
                                └────────┬────────┘
                                         ▼
                        Aptos APIs (fullnode REST + indexer GraphQL)
```

- **Cloudflare Pages** serves the app.
- **tracktos-edge** reads live ledger info, recent transactions, block proposers and the validator set once every few seconds and shares that answer with everyone watching, so the live pages stay fast and well inside Aptos rate limits. The browser falls back to reading Aptos directly if it is ever unavailable.
- **tracktos-flows** collects APT, USDC and USDt transfers on a schedule and keeps rolling windows in D1 for Token Flow.
- Prices come from public market data.

## About

Tracktos is a personal side project for learning about Aptos, made with care by [@angelataptos](https://x.com/angelataptos). It is not affiliated with Aptos Labs or the Aptos Foundation. For the authoritative view of the chain, use the [official Aptos Explorer](https://explorer.aptoslabs.com/).

The source code is private. See [LICENSE](LICENSE): all rights reserved.
