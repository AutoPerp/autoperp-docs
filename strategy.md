# V6 Strategy Engine

The V6 Strategy Engine is AutoPerp's core trading intelligence. It combines multi-timeframe price action analysis, volume profile validation, regime detection, and confidence-gated entries to identify high-probability setups.

## How It Works (Overview)

```
Regime Detection → MTF Analysis → VPVR Validation → Confidence Score → Entry Gate → Execution
```

Every 60 seconds, the engine scans each token on your watchlist through this pipeline. Only setups that pass every stage and meet your profile's confidence threshold get executed.

## Regime Detection

Before analyzing any setup, the engine classifies the current market regime:

| Regime | Description | Trading Approach |
|--------|-------------|-----------------|
| **Trending** | Clear directional bias with higher highs/lows (or lower) | Trend-following entries, wider targets |
| **Ranging** | Price oscillating between defined support and resistance | Mean-reversion entries, tighter targets |
| **Breakout** | Price breaking out of consolidation with momentum | Momentum entries, breakout confirmation required |

The regime determines which entry patterns the engine looks for. A ranging setup in a trending market gets rejected — context matters more than the pattern itself.

## Multi-Timeframe Analysis (MTF)

The engine uses a 3-timeframe ladder for each risk profile:

| Profile | Higher TF | Mid TF | Entry TF |
|---------|----------|--------|----------|
| **Conservative** | 1D | 4H | 1H |
| **Standard** | 4H | 2H | 1H |
| **High Risk** | 1H | 30M | 15M |
| **Degen** | 30M | 15M | 5M |

**How MTF works:**

1. **Higher timeframe** — Establishes directional bias and key levels
2. **Mid timeframe** — Confirms structure alignment with higher TF
3. **Entry timeframe** — Identifies precise entry zones with trigger candles

All three timeframes must agree for a setup to score well. A bullish entry signal on the 15M that contradicts the 1H structure gets penalized in the confidence score.

## VPVR Validation (Volume Profile Visible Range)

Every potential entry is cross-referenced against the Volume Profile:

| Zone Type | Effect | Reasoning |
|-----------|--------|-----------|
| **HVN** (High Volume Node) | +1.0 confidence boost | Strong historical interest = likely support/resistance |
| **POC** (Point of Control) | +2.0 confidence boost | Highest traded volume = strongest S/R zone |
| **LVN** (Low Volume Node) | **Hard reject** | Thin volume = price likely to slice through, not hold |

This means the engine won't enter at a technically valid level if the volume profile says there's nothing there to hold price. LVN rejection is absolute — no override.

## Confidence Scoring

Every setup receives a score from 0 to 10 based on:

- **Regime alignment** — Does the setup fit the current market environment?
- **MTF confluence** — Do all three timeframes agree?
- **VPVR validation** — Is the entry zone backed by volume?
- **Candle structure** — Are there confirmation patterns (engulfing, pin bar, etc.)?
- **Key level proximity** — How close is the entry to a significant S/R level?

### Confidence Thresholds by Profile

| Profile | Minimum Score | What This Means |
|---------|--------------|----------------|
| **Conservative** | 8.0 | Only the cleanest, most confirmed setups |
| **Standard** | 6.0 | Good-quality setups with solid confluence |
| **High Risk** | 5.0 | Moderate confidence, more frequent entries |
| **Degen** | 4.0 | Lower bar, higher frequency, more risk |

A setup scoring 5.8 will execute on High Risk and Degen profiles but get rejected on Standard and Conservative.

## Entry Engine V3

The Entry Engine V3 runs three independent native deciders:

### Long Decider V3
Evaluates long setups: bullish structure, demand zones, trend continuation patterns. Confirms with VPVR support and MTF alignment.

### Short Decider V3
Evaluates short setups: bearish structure, supply zones, reversal patterns. Mirrors the long decider logic for downside opportunities.

### Range Decider V3
Specialized for ranging regimes: identifies mean-reversion opportunities at range boundaries. Automatically activates when regime detection classifies the market as ranging.

All three deciders run in parallel. The one matching the current regime and setup type produces the final entry signal.

## Anti-Fakeout Veto Layer

On top of the confidence score, every entry passes through the anti-fakeout veto layer:

- Checks for signs of fakeout patterns (false breakouts, stop hunts, wicks into levels without follow-through)
- Analyzes recent price action for rejection patterns that suggest the move is fake
- Can veto an otherwise qualifying entry if fakeout probability is high

This is a safety net that catches setups where the numbers look good but the price action smells wrong.

## Independent Long/Short Modes

You can run completely independent configurations for Long and Short trades:

- **Different risk profiles** — Conservative Longs + Degen Shorts (or any combination)
- **Different confidence thresholds** — Each direction has its own gate
- **Different timeframe ladders** — Based on the selected profile per direction

This lets you express directional bias at the strategy level. If you think the market is better for shorts right now, you can run aggressive Short mode while keeping Longs conservative.

Configure via Telegram:
```
/mode long conservative
/mode short degen
```

## Entry Placement

AutoPerp uses **limit orders only** — never market orders. Entries are placed at the validated support/resistance zone price, which typically gets a better fill than market execution.

- **Stale order timeout:** Limit orders auto-cancel after 60 minutes (swing) if not filled
- **Chase prevention:** The entry zone must be at least 0.3% away from current price — no chasing moves that already happened
- **Fill verification:** Once filled, stop loss is placed on the exchange immediately and verified

## What the Engine Does NOT Do

- **No indicators.** No RSI, no MACD, no moving average crossovers. Pure price action + volume.
- **No prediction.** The engine doesn't predict where price will go. It identifies high-probability zones and waits for confirmation.
- **No override.** The engine cannot bypass its own rules. If the score is below threshold, it doesn't trade. Period.
