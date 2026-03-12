# Trade Execution

This document explains how AutoPerp handles trade execution from entry to exit.

## Entry: Limit Orders Only

AutoPerp places **limit orders** at the validated support/resistance zone — never market orders. This provides:

- **Better fills** — You enter at your intended price, not at whatever the market gives you
- **Lower fees** — Maker fees on Bybit are lower than taker fees
- **Precision** — The entry matches the technical level the strategy identified

### Stale Order Timeout

If a limit order isn't filled within the timeout window, it's automatically cancelled:

- **Swing entries:** 60 minutes
- **Scalp entries:** Shorter, based on timeframe

This prevents stale orders from filling hours later when the setup context has changed.

### Chase Prevention

The engine enforces a minimum distance between current price and the entry zone:

- **Longs:** Entry zone must be at least 0.3% below current price
- **Shorts:** Entry zone must be at least 0.3% above current price

This prevents chasing moves that have already happened. If price has already moved through the zone, the opportunity is gone — the engine waits for the next one.

## Stop Loss Placement

Immediately on fill:

1. Stop loss is calculated from the **actual fill price** (not the signal price)
2. SL order is placed directly on Bybit as a reduce-only order
3. The bot verifies the SL order was accepted by the exchange
4. SL distance is based on ATR and the risk profile's multiplier

### Why Fill Price Matters

Limit orders can fill at slightly different prices than the original signal. If the SL was calculated from the signal price, it could be too tight or too loose relative to your actual entry. AutoPerp recalculates from the real fill to ensure correct risk-reward.

## The Moonbag System

AutoPerp uses a two-phase exit strategy designed to lock profits while letting winners run.

### Phase 1: Take Profit at TP1 (50% Close)

When price reaches the first take-profit target:
- **50% of the position is closed** — locking in profit
- The remaining 50% becomes the "moonbag"
- Stop loss moves to breakeven (entry price)

This guarantees you can't lose on the trade from this point. Even if price reverses and hits your stop, you've already banked half the profit.

### Phase 2: Trailing Stop on the Moonbag

The remaining 50% is managed by an adaptive trailing stop:

- The trailing SL tightens as profit increases
- It adapts to the asset's volatility — SOL gets different trailing behavior than BTC
- It gives enough room for natural pullbacks while protecting the majority of gains

The goal: Let the moonbag ride trends as far as they go, while progressively locking in more profit with every new high.

### Exit Scenarios

| Scenario | What Happens |
|----------|-------------|
| Price hits TP1 | 50% closed at profit, SL moves to breakeven |
| Price hits trailing SL after TP1 | Moonbag closed at profit (SL was above entry) |
| Price hits SL before TP1 | Full position closed at defined risk |
| Price reverses after TP1 to entry | Moonbag closed at breakeven (no additional loss) |
| Price keeps running after TP1 | Trailing SL follows, profits compound |

## Breakeven Protection

Once TP1 is hit and 50% is closed, the stop loss is moved to the entry price (breakeven). This is a hard rule — the moonbag can never turn into a loser.

Breakeven protection activates:
- **Immediately** after the TP1 partial close
- **On the exchange** — the SL order is updated on Bybit directly

## Adaptive Trailing Stop

The trailing stop isn't a fixed percentage. It adapts based on:

- **Asset volatility** — Higher-volatility assets (meme coins) get wider trailing stops to avoid getting stopped out on normal wicks. Lower-volatility assets (BTC, ETH) get tighter trailing to lock more profit.
- **Profit distance** — As the trade moves further into profit, the trailing stop tightens. Early in the move, it gives more room; deeper in profit, it protects more aggressively.
- **Regime** — Trending regimes get wider trailing (let it run); ranging regimes get tighter trailing (take what's available).

## Fill Verification

After every order (entry, SL, TP, trailing update):

1. The bot queries Bybit to confirm the order exists and has the correct parameters
2. If verification fails, the bot retries
3. If the order is rejected by Bybit, the bot logs the error and alerts you

This ensures you're never in a state where you think you have protection but don't.

## Manual Overrides

You always have full control:

```
/close SOLUSDT    — Close a specific position immediately
/closeall         — Close all open positions
/stopbot          — Stop scanning for new trades
```

Manual closes execute as market orders for immediate fill. Exchange-level SLs are cancelled automatically when you manually close.
