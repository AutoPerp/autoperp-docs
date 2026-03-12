# Risk Management

AutoPerp enforces risk management at every level. These limits are hard-coded — the bot cannot override its own rules, and neither can you. This is by design.

## Hard Limits

### Daily Loss Limit: 5%

If your account drops 5% from the day's starting balance, **all trading stops automatically**. No new positions will be opened until the next day or until you manually reset the circuit breaker.

### Weekly Loss Limit: 10%

If cumulative losses for the week reach 10%, trading halts for the remainder of the week.

### Peak Drawdown: 8%

If your account drops 8% from its highest recorded equity, trading pauses until recovery or manual reset.

## Circuit Breaker

The circuit breaker is AutoPerp's emergency stop system. It triggers on:

- **Any hard limit breach** (daily 5%, weekly 10%, peak drawdown 8%)
- **4 consecutive losing trades** — even if within risk limits

When the circuit breaker trips:
1. All scanning stops immediately
2. No new positions are opened
3. Existing positions continue to be managed (SL/TP remain active)
4. You receive a Telegram notification explaining why

### Resetting the Circuit Breaker

```
/circuit         — View current circuit breaker status
/circuit reset   — Manually reset and resume trading
```

The reset is intentionally manual. It forces you to acknowledge the losses and make a conscious decision to continue. This prevents the bot from revenge trading.

## Position Limits

| Rule | Limit |
|------|-------|
| **Maximum concurrent positions** | 3 |
| **Positions per symbol** | 1 (no duplicates) |
| **Cooldown between new positions** | 5 minutes |
| **Per-symbol cooldown after SL** | 2 hours |
| **Direction balance** | Max 70% of trades in one direction |

### Why These Limits Exist

- **3 concurrent positions** — Prevents over-exposure. Even if all three hit SL simultaneously, the damage is contained.
- **1 per symbol** — No doubling down on a losing trade. If SOL hits your SL, you wait 2 hours before re-entering SOL.
- **5-minute cooldown** — Prevents rapid-fire entries during volatile conditions.
- **Direction balance** — If 70% of your recent trades are longs, the bot starts favoring shorts to maintain balance. Prevents directional concentration risk.

## Position Sizing

AutoPerp uses smart position sizing that accounts for both volatility and confidence:

### Volatility-Adjusted (ATR-Based)

Position size is adjusted based on the asset's Average True Range (ATR). More volatile assets get smaller positions; calmer assets get larger positions. This ensures each trade has roughly equal risk exposure regardless of the asset's volatility.

### Confidence-Weighted

The confidence score from the strategy engine also affects sizing:

| Confidence Level | Size Multiplier |
|-----------------|----------------|
| **High** (8.0+) | 1.5x base size |
| **Normal** (6.0-7.9) | 1.0x base size |
| **Low** (4.0-5.9) | 0.7x base size |

Higher-confidence setups get more capital. Lower-confidence setups (only taken by High Risk and Degen profiles) get reduced sizing.

### Hard Cap

No single position can ever exceed **35% of your account balance**, regardless of confidence or volatility calculations. This is a non-negotiable ceiling.

### Loss Streak Reduction

After 3 consecutive losses, the next 3 trades are automatically placed at **50% of normal size**. This reduces exposure during cold streaks and preserves capital for recovery.

## Exchange-Enforced Stop Losses

This is one of AutoPerp's most critical features:

1. **Every trade gets a stop loss** — There are no exceptions.
2. **SL is placed on Bybit** — Not just tracked locally, but set as an actual exchange order.
3. **Verified after placement** — The bot confirms the SL order was accepted by Bybit.
4. **Calculated from fill price** — SL distance is computed from the actual fill price, not the signal price. This is critical for limit orders that fill at different prices.

### Why This Matters

If AutoPerp crashes, your internet goes down, or the server restarts, **your stop loss is still active on Bybit**. The exchange enforces it regardless of whether the bot is running. This is your safety net.

Many trading bots track stops locally — if the bot crashes, the stop disappears. AutoPerp never has this problem.

## Risk Summary

```
Max daily loss:           5%
Max weekly loss:          10%
Max drawdown from peak:   8%
Max concurrent positions: 3
Max per trade:            35% of balance
Consecutive loss halt:    4 trades
Loss streak sizing:       50% for 3 trades after 3 losses
Per-symbol cooldown:      2 hours after SL
Direction balance:        Max 70% one direction
Stop loss:                Exchange-enforced on every fill
```

These rules are not configurable. They exist to protect your capital. The bot is designed to make money over weeks and months — not to gamble on individual trades.
