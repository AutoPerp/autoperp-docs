# Frequently Asked Questions

## General

### What is AutoPerp?
AutoPerp is an automated trading bot for Bybit USDT perpetual futures. It runs 24/7, controlled entirely through Telegram, using the V6 Strategy Engine for multi-timeframe analysis and confidence-gated entries.

### What exchange does it support?
Currently **Bybit** only. Binance, Hyperliquid, BloFin, MEXC, and BingX are planned for Q2 2026.

### Can it go short?
Yes. AutoPerp supports both Long and Short trades. You can even set independent risk profiles for each direction — for example, Conservative on Longs and Degen on Shorts.

### What strategy does it use?
The V6 Strategy Engine combines regime detection, multi-timeframe price action analysis, and VPVR (Volume Profile) validation. Every setup receives a 0-10 confidence score, and only entries above your profile's threshold are executed. See [Strategy](strategy.md) for full details.

### Is this a signal service?
No. AutoPerp executes trades automatically on your Bybit account. You don't receive signals to manually execute — the bot does everything from detection to exit management.

## Safety & Security

### Is my money safe?
Your funds stay on Bybit at all times. AutoPerp connects via API with **trade-only permissions** — it can place and manage orders but cannot withdraw funds. You can revoke API access from Bybit at any time.

### What happens if the bot crashes?
Your stop losses are placed directly on Bybit as exchange orders. If AutoPerp crashes, the exchange still enforces your stops. This is the key difference from bots that track stops locally.

### Are my API keys secure?
API keys are encrypted at rest. They are never logged in plaintext, never shared, and never transmitted to any third party. The keys are only used to communicate with Bybit's API.

### Can someone else access my account?
No. Each user has an isolated trading worker. Your API keys, settings, and positions are completely separate from other users.

## Trading

### What's the minimum balance recommended?
We recommend **$100 minimum** for a reasonable experience. The bot needs enough capital for proper position sizing — too small and individual trades become impractical due to Bybit's minimum order sizes.

### How many trades should I expect?
Depends on your risk profile:
- **Conservative:** 2-6 per week
- **Standard:** 6-20 per week
- **High Risk:** 20-50 per week
- **Degen:** 60-120 per week

### Will I definitely make money?
No. Trading perpetual futures involves substantial risk of loss. AutoPerp is a tool that enforces discipline and manages risk — it does not guarantee profits. Past performance is not indicative of future results.

### What pairs does it support?
20+ high-liquidity USDT perpetual pairs on Bybit, including BTC, ETH, SOL, XRP, DOGE, and more. Your specific pairs are configured via `/addlist`.

### Can I trade manually alongside the bot?
Yes, but be cautious. If you manually open a position on a symbol the bot is also trading, it could cause conflicts with position management. We recommend using a dedicated sub-account for bot trading.

### What's the moonbag system?
When price hits TP1, 50% of the position is closed for profit and the stop loss moves to breakeven. The remaining 50% (the "moonbag") rides with a trailing stop. This locks profit while letting winners run. See [Trade Execution](trade-execution.md).

## Settings & Configuration

### How do I change my risk profile?
```
/mode standard           — Both directions
/mode long conservative  — Long only
/mode short degen        — Short only
```

### How do I add or remove tokens?
```
/addlist SOL,BTC,ETH     — Add tokens
/removelist DOGE         — Remove a token
/applylist               — Apply changes
/mylist                  — View current list
```

### Can I change settings while the bot is running?
Yes. Most settings take effect on the next scan cycle (within 60 seconds). Existing positions are not affected by setting changes.

### What's the circuit breaker?
An automatic safety stop that halts trading when risk limits are breached (5% daily loss, 10% weekly loss, or 4 consecutive losses). Reset it manually with `/circuit reset`. See [Risk Management](risk-management.md).

## Subscription

### How do I pay?
Crypto only — SOL. Type `/subscribe` in the bot and follow the payment flow.

### Can I upgrade my plan?
Yes. Run `/subscribe` again and select the higher tier. Upgrades activate immediately.

### What if I want to cancel?
Simply don't renew. Your subscription expires at the end of the billing period. The bot stops trading when your subscription lapses.

### Is there a free trial?
Yes. New users get a **14-day free Standard trial** before choosing a paid plan.

### What’s the difference between Starter and Pro?
Starter gives you **2 concurrent positions** and **5 pairs**. Pro expands you to **5 concurrent positions**, **20 pairs**, plus scanner/filtering and deeper dashboard analytics.

## Support

### Where do I get help?
- **Community group:** [t.me/autoperpentry](https://t.me/autoperpentry) — Ask questions, share results, get help from other users
- **Bot support:** Message [@autoperpbot](https://t.me/autoperpbot) directly
- **Lifetime members:** Access to private strategy group and 1-on-1 support
- **X / Twitter:** [@autoperpbot](https://x.com/autoperpbot) — Updates and announcements

### I found a bug. How do I report it?
Message us in the [community group](https://t.me/autoperpentry) or contact the bot directly. Include as much detail as possible: what happened, what you expected, and any error messages.

### The bot isn't taking trades. What's wrong?
Check these in order:
1. `/status` — Is the bot running?
2. `/circuit` — Is the circuit breaker tripped?
3. `/mylist` — Do you have tokens on your watchlist?
4. `/balance` — Do you have sufficient USDT?
5. Risk profile threshold — A very high threshold (Conservative) means fewer qualifying setups

If everything looks correct and the bot still isn't trading, the market may simply not have setups that meet your criteria. This is the bot working as designed — it waits for quality over quantity.
