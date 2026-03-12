# Getting Started

This guide walks you through setting up AutoPerp from scratch — from opening the bot to your first automated trade.

## Step 1: Open the Bot

Go to [@autoperpbot](https://t.me/autoperpbot) on Telegram and press **Start** or type `/start`.

The bot will greet you and walk you through the initial setup.

## Step 2: Start Your Trial or Subscribe

New users get a **14-day free Standard trial** automatically on first `/start`, or you can use `/trial` to begin it manually if prompted.

When you're ready for a paid plan, type `/subscribe` to see available options:

| Plan | Price | What You Get |
|------|-------|-------------|
| **Starter** | $25/month | 2 concurrent positions, 5 pairs, Long + Short, V6 engine |
| **Pro** | $50/month | 5 concurrent positions, 20 pairs, scanner, advanced analytics |
| **Lifetime** | $500 once | Everything in Pro, private strategy group, 1-on-1 support |

Payment is handled via crypto (SOL or USDC). The bot generates a payment address — send the exact amount and confirmation is automatic.

## Step 3: Set Up Your API Keys

Type `/setup` to begin API key configuration.

You'll need a **Bybit API key** with the following permissions:
- **Read** — to check balances and positions
- **Trade** — to place and manage orders
- **No withdrawal permissions** — never grant withdrawal access

### How to Create a Bybit API Key

1. Log into [Bybit](https://www.bybit.com)
2. Go to **Account & Security** → **API Management**
3. Click **Create New Key**
4. Select **System-generated API Keys**
5. Set permissions: **Read** + **Trade** only
6. Optionally restrict to your server's IP for extra security
7. Copy the **API Key** and **API Secret**

Send both to the bot when prompted. They are encrypted and stored securely — never shared, never logged in plaintext.

### Security Notes

- Your funds stay on Bybit at all times. AutoPerp never has withdrawal access.
- API keys are encrypted at rest using AES-256.
- You can revoke your API key from Bybit at any time to instantly cut off bot access.
- We recommend creating a dedicated sub-account on Bybit for bot trading.

## Step 4: Choose a Risk Profile

Type `/mode` followed by your preferred profile:

```
/mode conservative    — Safest. 1D/4H/1H timeframes. 2-6 trades/week.
/mode standard        — Balanced. 4H/2H/1H. 6-20 trades/week.
/mode highrisk        — Aggressive. 1H/30M/15M. 20-50 trades/week.
/mode degen           — Maximum frequency. 30M/15M/5M. 60-120 trades/week.
```

You can also set **different profiles for Long and Short** independently. For example, run Conservative on Longs and Degen on Shorts:

```
/mode long conservative
/mode short degen
```

See [Risk Profiles](risk-profiles.md) for full details on each mode.

## Step 5: Build Your Token Watchlist

Add the tokens you want the bot to trade:

```
/addlist SOL,BTC,ETH,XRP,DOGE
```

Then apply the changes:

```
/applylist
```

You can view your current list with `/mylist` and remove tokens with `/removelist SOL`.

**Supported tokens:** The bot supports 20+ high-liquidity USDT perpetual pairs on Bybit. Stick to major and mid-cap tokens for best results.

## Step 6: Start Trading

```
/startbot
```

The bot will begin scanning your watchlist every 60 seconds, looking for setups that meet your risk profile's confidence threshold.

You'll receive Telegram notifications for:
- New position entries (with confidence score, entry price, SL, TP)
- Partial closes (TP1 hit, moonbag trailing)
- Full closes (SL hit, trailing stop, manual close)
- Circuit breaker events
- Daily P&L summaries

## Step 7: Monitor

- `/status` — Bot status, subscription info, active settings
- `/positions` — Currently open positions
- `/balance` — Your Bybit account balance
- `/pnl` — Today's P&L summary
- `/journal` — Full trade journal with history

## Stopping the Bot

```
/stopbot       — Stops scanning for new trades (existing positions stay open)
/closeall      — Closes all open positions immediately
/close SOLUSDT — Closes a specific position
```

## Tips for New Users

1. **Start with Conservative or Standard mode.** Get comfortable with how the bot operates before increasing risk.
2. **Don't over-leverage.** The bot respects your leverage setting — keep it reasonable (3-5x for starters).
3. **Fund appropriately.** Recommended minimum is $100. More capital = better position sizing flexibility.
4. **Check `/journal` regularly.** Understand why trades were taken and how they performed.
5. **Trust the risk management.** The circuit breaker exists for a reason. If it trips, take a break before resetting.
6. **Use per-direction profiles.** If you're bearish short-term but bullish long-term, set Short to aggressive and Long to conservative.

## Need Help?

- **Community:** [t.me/autoperpentry](https://t.me/autoperpentry) — ask questions, share results
- **Support:** Message [@autoperpbot](https://t.me/autoperpbot) with your issue
- **Docs:** You're here — browse the sidebar for specific topics
