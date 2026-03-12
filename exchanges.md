# Supported Exchanges

## Currently Live

### Bybit

**Status:** Live — Full Support

AutoPerp currently supports Bybit USDT perpetual futures with full feature coverage:

- All 20+ supported trading pairs
- Limit order entry with exchange-enforced stop losses
- Real-time position monitoring
- Moonbag system with partial TP and trailing SL
- Full API integration (read + trade permissions)

**Getting started with Bybit:**
1. Create a [Bybit account](https://www.bybit.com) if you don't have one
2. Generate API keys (Account & Security → API Management)
3. Set permissions to **Read + Trade only** (no withdrawal)
4. Send the keys to [@autoperpbot](https://t.me/autoperpbot) via `/setup`

**Recommended settings:**
- Enable USDT perpetual trading on your account
- Fund your account with USDT (minimum recommended: $100)
- Consider using a sub-account dedicated to bot trading

## Coming Soon

### Binance
**Expected:** Q2 2026

Binance USDT-M Futures integration. Will support the same feature set as Bybit — limit entries, exchange-enforced SLs, moonbag system, and all risk management features.

### Hyperliquid
**Expected:** Q2 2026

On-chain perpetual futures on Hyperliquid L1. This will be AutoPerp's first on-chain exchange integration, enabling fully decentralized perp trading with the same V6 strategy engine.

### BloFin
**Expected:** Q2 2026

BloFin USDT perpetual futures support.

### MEXC
**Expected:** Q2 2026

MEXC USDT-M Futures integration.

### BingX
**Expected:** Q2 2026

BingX USDT-M perpetual futures.

## Exchange Rollout Philosophy

We add exchanges one at a time and thoroughly test each integration before release. Each new exchange goes through:

1. **API integration** — Order placement, position management, balance queries
2. **SL verification** — Ensuring exchange-enforced stop losses work correctly
3. **Edge case testing** — Partial fills, API errors, rate limits, reconnection
4. **Live testing** — Real trades on real accounts before public release

We don't rush exchange integrations. A poorly integrated exchange is worse than no integration at all — your money is at stake.

## Requesting an Exchange

Want to see a specific exchange supported? Let us know in the [community group](https://t.me/autoperpentry). We prioritize based on demand and API quality.
