# Telegram Commands

Full reference for all AutoPerp bot commands. Use these in your chat with [@autoperpbot](https://t.me/autoperpbot).

## Getting Started

| Command | Description |
|---------|-------------|
| `/start` | Initialize the bot, show welcome message |
| `/subscribe` | Start the subscription flow — choose plan and pay |
| `/setup` | Configure your Bybit API key and secret |
| `/help` | Show all available commands |

### `/start`
Opens the bot and shows the welcome message with quick-start instructions. If you're already set up, it shows your current status.

### `/subscribe`
Displays available plans (Starter, Pro, Lifetime) and generates a crypto payment address. Send the exact amount in SOL or USDC to activate.

### `/setup`
Prompts you for your Bybit API key and secret. Keys are encrypted and stored securely. You can re-run this command to update your keys at any time.

## Trading Control

| Command | Description |
|---------|-------------|
| `/startbot` | Start the trading engine — begin scanning and executing |
| `/stopbot` | Stop scanning for new trades (existing positions stay managed) |
| `/restartbot` | Restart your trading worker |
| `/mode <profile>` | Switch risk profile |
| `/leverage <value>` | Set leverage for new trades |

### `/startbot`
Starts your personal trading worker. The bot begins scanning your watchlist every 60 seconds and executing trades that meet your profile's criteria.

### `/stopbot`
Stops the bot from opening new positions. **Important:** Existing positions continue to be managed — stop losses, take profits, and trailing stops remain active. Use `/closeall` if you want to exit everything.

### `/restartbot`
Restarts your trading worker. Useful if you've changed settings and want them to take effect immediately.

### `/mode`
Change your risk profile. Can be set globally or per-direction:

```
/mode standard           — Both Long and Short set to Standard
/mode long conservative  — Only Long mode changed
/mode short degen        — Only Short mode changed
```

Available profiles: `conservative`, `standard`, `highrisk`, `degen`

### `/leverage`
Set the leverage multiplier for new trades:

```
/leverage 5     — Set to 5x leverage
/leverage 10    — Set to 10x leverage
```

Leverage only affects new positions — existing positions keep their current leverage.

## Token Management

| Command | Description |
|---------|-------------|
| `/addlist <tokens>` | Add tokens to your watchlist |
| `/removelist <token>` | Remove a token from your watchlist |
| `/applylist` | Apply watchlist changes (required after add/remove) |
| `/mylist` | Show your current active token watchlist |

### `/addlist`
Add one or more tokens (comma-separated):

```
/addlist SOL,BTC,ETH,XRP,DOGE
```

Changes don't take effect until you run `/applylist`.

### `/removelist`
Remove a specific token:

```
/removelist FARTCOIN
```

Run `/applylist` after to apply the change.

### `/applylist`
Applies any pending watchlist changes. The bot will start/stop scanning the updated token list on the next cycle.

### `/mylist`
Displays your currently active watchlist with all tokens being scanned.

## Position Management

| Command | Description |
|---------|-------------|
| `/positions` | Show all currently open positions |
| `/close <symbol>` | Close a specific position |
| `/closeall` | Close all open positions immediately |

### `/positions`
Shows details for each open position:
- Symbol, direction (Long/Short)
- Entry price, current price
- Unrealized P&L
- Stop loss and take profit levels
- Position size

### `/close`
Close a specific position by symbol:

```
/close SOLUSDT
/close BTCUSDT
```

Executes a market order for immediate fill. The exchange-level SL is cancelled automatically.

### `/closeall`
Closes every open position immediately. Use with caution — this is irreversible. Useful when you want to go fully flat quickly.

## Information

| Command | Description |
|---------|-------------|
| `/status` | Bot status, subscription info, current settings |
| `/balance` | Your Bybit account balance |
| `/pnl` | Today's profit and loss summary |
| `/journal` | Full trade journal with history and stats |
| `/risk` | View current risk settings and limits |

### `/status`
Comprehensive overview:
- Subscription tier and expiry
- Bot running state
- Current risk profile (Long and Short)
- Active tokens count
- Open positions count
- Circuit breaker state

### `/balance`
Shows your current Bybit USDT balance (available + in positions).

### `/pnl`
Today's P&L breakdown:
- Total P&L ($ and %)
- Number of trades (wins/losses)
- Best and worst trade
- Current daily limit usage

### `/journal`
Full trade history with:
- Entry and exit prices
- P&L per trade
- Confidence scores
- Close reasons (TP, SL, trailing, manual)
- Running totals

### `/risk`
Displays all active risk parameters:
- Daily/weekly loss limits and current usage
- Circuit breaker status
- Position limits
- Cooldown timers
- Current sizing parameters

## Safety

| Command | Description |
|---------|-------------|
| `/circuit` | View circuit breaker status |
| `/circuit reset` | Reset the circuit breaker and resume trading |

### `/circuit`
Shows whether the circuit breaker is active and why it tripped (daily limit, weekly limit, consecutive losses, etc.).

### `/circuit reset`
Manually resets the circuit breaker and allows the bot to resume trading. This is intentionally manual — it forces you to acknowledge the situation before continuing.

**Note:** If the underlying limit (e.g., daily 5%) is still breached, the circuit breaker will trip again immediately. Wait for the reset period (next day/week) or reduce position sizes.
