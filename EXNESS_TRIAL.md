# SuperTrend trial

Magic number: **26111306**. The only approved live entry point is `exness_trial.py`, which uses the shared package in the sibling `Monn` checkout.

## Strategy profile

Original `DataProcessing.Indicator`: 18-period, 5x ATR SuperTrend levels, original entry conditions, average-line target and original reward/1.2 stop formula. The forming bar contributes only its open; its high, low and close are excluded. The adapter replaces the interactive login, balance-based lot sizing and retrying order sender with the shared execution guard. Numeric NaN handling and pandas positional slicing are fixed.

## Shared execution policy

Every live proposal goes through the same shared `Monn/exness_arena/guard.py`. Current fixed risk cap is 3.50 USC per position, 7.00 USC combined open stop-risk, 5.2444 USC cumulative booked loss budget per repo, and a 1% account-equity drawdown entry halt. A 10% execution buffer is reserved; volume never rounds up and remains capped at 0.01 cent lots. Missing state, stale tracker/quotes and ambiguous results fail closed. A first buy and first sell may coexist. Further same-direction entries require every existing position in that direction to be positive after swap and known opening fees. There is no numeric position-count or lifetime-entry cap. New opposite signals do not liquidate existing positions; server-side SL/TP remain active. Existing manual positions are excluded from management and per-repo results. Gaps can exceed estimated stop risk. Adding a repo never raises the shared budget or resets the baseline, history or loss counters.

## Validation and operations

`exness_trial.py --preflight` loads the original strategy and evaluates current broker data through a no-order gateway. Aurum artifact hashes are verified. The shared tests exercise ownership, sizing, hedging, profitable additions, failure handling and each new adapter. Preflight proves plumbing, not profitability. All rule-based trial profiles incur zero AI API costs.

Account pins, runtime settings, artifacts and the SQLite deal journal stay local in `Monn/.exness-arena/`, excluded from Git. The authoritative enabled repo list is `settings.json.enabled_bots`. `Monn/restart_exness_trial.ps1` checks and restarts this list while preserving open positions and history. Dashboard: http://127.0.0.1:8765 . Stop new entries with `Monn/stop_exness_trial.ps1`.
