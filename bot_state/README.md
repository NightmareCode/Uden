# bot_state/ — persistent memory for the Uden hourly routine

This folder is the **persistent memory** for the hourly Uden trading routine
(Signum bot **26438** → Hyperliquid). The routine **reads and writes** these
files on the `claude/uden-bot-state` branch on every run — that single branch is
the bot's source of truth across runs.

## Files

- **`state.json`** — the live state the routine carries forward: the equity
  **high-water mark** (`equity_high_water_usd`), the currently `open_trade` (if
  any), and per-setup statistics (`setup_stats`: `breakout_long`, `hedge_short`,
  `bear_short` — trades / wins / pnl_usd).
- **`journal.jsonl`** — the closed-trade log: **one closed trade per line**
  (JSON Lines). Appended to as trades close; never rewritten.

## ⚠️ Do not hand-edit while the routine is live

The routine **owns and overwrites** `state.json` and appends to `journal.jsonl`
each run. Editing these by hand while it is active will be clobbered and can
corrupt the high-water mark and stats. Stop the routine before touching anything
here.
