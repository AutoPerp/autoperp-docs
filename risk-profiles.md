# Risk Profiles

AutoPerp offers four risk profiles that control how aggressively the bot trades. Each profile adjusts timeframes, confidence thresholds, trade frequency, and volatility handling.

## Profile Comparison

| Setting | Conservative | Standard | High Risk | Degen |
|---------|-------------|----------|-----------|-------|
| **Timeframes** | 1D / 4H / 1H | 4H / 2H / 1H | 1H / 30M / 15M | 30M / 15M / 5M |
| **Trades/Week** | 2-6 | 6-20 | 20-50 | 60-120 |
| **Confidence Threshold** | 8.0 | 6.0 | 5.0 | 4.0 |
| **ATR Multiplier** | 1.5x | 1.75x | 1.75x | 2.0x |
| **Volatile Trading** | Disabled | Disabled | Enabled | Enabled |
| **Best For** | Capital preservation | Balanced growth | Active traders | High-frequency aggressive |

## Conservative

**Timeframes:** 1D / 4H / 1H
**Confidence Threshold:** 8.0 (only the cleanest setups)
**ATR Multiplier:** 1.5x (tighter stop losses relative to volatility)
**Volatile Trading:** Disabled — skips highly volatile tokens/regimes

Conservative mode is built for traders who prioritize capital preservation over trade frequency. It only takes setups with overwhelming confluence across all three timeframes, strong VPVR confirmation, and textbook price action.

**Expect:** 2-6 trades per week. Mostly swing-style entries on higher timeframes. Lower drawdown, steadier equity curve, but also lower absolute returns in trending markets.

**Best for:** Larger accounts, risk-averse traders, or anyone who wants the bot running with minimal monitoring.

## Standard

**Timeframes:** 4H / 2H / 1H
**Confidence Threshold:** 6.0 (good quality with solid confluence)
**ATR Multiplier:** 1.75x (moderate stop distance)
**Volatile Trading:** Disabled

Standard is the default mode and the recommended starting point for most traders. It balances trade quality with frequency, taking setups that have clear MTF alignment and VPVR support without requiring perfection.

**Expect:** 6-20 trades per week. Mix of intraday and short swing trades. Moderate drawdowns with solid recovery potential.

**Best for:** Most traders. Good balance of activity and quality.

## High Risk

**Timeframes:** 1H / 30M / 15M
**Confidence Threshold:** 5.0 (moderate confidence accepted)
**ATR Multiplier:** 1.75x
**Volatile Trading:** Enabled — will trade during volatile conditions

High Risk mode increases frequency significantly by dropping to lower timeframes and accepting setups with moderate confidence. It will also trade during volatile market conditions that Conservative and Standard skip.

**Expect:** 20-50 trades per week. Mostly intraday entries. Higher drawdown potential but also faster compounding when the strategy is in sync with the market.

**Best for:** Experienced traders comfortable with drawdowns. Requires adequate capital to handle the increased position count.

## Degen

**Timeframes:** 30M / 15M / 5M
**Confidence Threshold:** 4.0 (lower bar, maximum frequency)
**ATR Multiplier:** 2.0x (wider stops for faster timeframes)
**Volatile Trading:** Enabled

Degen mode is the most aggressive setting. It operates on the lowest timeframes, accepts lower-confidence entries, and trades through volatile conditions with wider ATR-adjusted stops. This is scalp-heavy trading with high frequency.

**Expect:** 60-120 trades per week. Rapid entries and exits. Significant drawdown potential. The risk management system (circuit breakers, daily limits) becomes especially important at this level.

**Best for:** Traders who want maximum exposure and understand the risks. Not recommended for small accounts or risk-averse traders.

## Per-Direction Profiles

One of AutoPerp's unique features is independent risk profiles for Long and Short trades.

### Why This Matters

Markets behave differently in each direction. Rallies tend to be slower and more structured. Drops tend to be faster and more volatile. You might want:

- **Conservative Longs + Degen Shorts** — Careful on the way up, aggressive on the way down
- **Standard Longs + Standard Shorts** — Balanced in both directions
- **Degen Longs + Conservative Shorts** — Aggressive upside, cautious downside (bullish bias)

### How to Configure

```
/mode long conservative
/mode short highrisk
```

Each direction gets its own:
- Timeframe ladder
- Confidence threshold
- ATR multiplier
- Volatile trading setting

### Example Setup

A trader who believes the current market favors short-term bearish moves but long-term bullish structure might configure:

- **Long Mode: Conservative (1D/4H/1H)** — Only take the highest-quality long setups on swing timeframes
- **Short Mode: High Risk (1H/30M/15M)** — Actively short intraday on faster timeframes

This gives directional flexibility without compromising the risk management on either side.

## Changing Profiles

Switching profiles takes effect on the next scan cycle (within 60 seconds). Existing open positions are not affected — they continue with the risk parameters they were opened under.

```
/mode standard           — Sets both Long and Short to Standard
/mode long conservative  — Sets only Long to Conservative
/mode short degen        — Sets only Short to Degen
```

## Which Profile Should I Start With?

**If you're new to AutoPerp:** Start with Standard. It gives you enough trade activity to see the bot in action while keeping risk controlled.

**If you have a smaller account (<$200):** Consider Conservative. Fewer trades means less exposure to fees and slippage relative to your balance.

**If you're experienced and well-capitalized:** Try Standard for a week, then move to High Risk if you're comfortable with the drawdown characteristics.

**Degen is not a "better" mode.** It's a different tool for a different purpose. More trades ≠ more profit. The best profile is the one that matches your risk tolerance and capital.
