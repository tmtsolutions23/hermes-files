# MGC Evening Eval Report

## What was fixed before retesting
- Corrected MGC micro point value in the Python engine to **$10/point**, not $100/point.
- Replaced fragile `US/Eastern` timezone conversion with **America/New_York** so the engine runs reliably on this box.
- Added explicit killzone handling for `golden_wide` and a new `overnight` mode. Previously, unknown modes effectively bypassed session filtering and polluted results.

## Eval framing
- Account target: $3,000 on a $50K eval.
- Drawdown budget: keep practical strategy-level pain well under the $2,000 max loss.
- Conclusion up front: **night MGC is viable as a secondary/supplemental engine, not as the primary 3-week pass engine**.

## Tested configs
| Config | Full PnL | Full $/wk | Full MaxDD | Full PF | Full Sharpe | Full Trades | 2026 PnL | 2026 $/wk | 2026 MaxDD | 2026 PF | 2026 Sharpe | 2026 Trades |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| core_6_8pm_trail_only | 7528.18 | 44.81 | 1250.13 | 1.689 | 2.52 | 222 | 2612.58 | 225.78 | 663.86 | 1.565 | 2.32 | 57 |
| overnight_6pm_2am_both | 25762.19 | 153.35 | 2179.70 | 1.588 | 2.50 | 949 | 4324.10 | 373.69 | 2179.70 | 1.375 | 1.59 | 163 |
| full_session_bugfix_check | 35787.59 | 213.02 | 3383.23 | 1.388 | 1.87 | 1843 | 4687.16 | 405.06 | 2464.98 | 1.292 | 1.36 | 197 |

## Readout
- **Core 6PM-8PM only** is the cleanest night-only version, but it is too slow to rely on for a 3-week eval pass by itself.
- **Overnight 6PM-2AM** improves throughput materially and has the best practical balance for a night-trading indicator, but its historical drawdown is still too large to make it the lone eval strategy.
- The `golden_wide` result is included mainly as a bugfix sanity check after adding explicit mode handling.

## Recommended use
1. Use **MNQ ORB+HTF as the primary pass engine**.
2. Use **MGC Evening IFVG** only as a night-session supplement when no MNQ setup is available.
3. Start with **0.5% risk and max 4 MGC micros** in the indicator defaults.
4. Treat the best-quality window as **Core 6PM-8PM ET**. Expand to **6PM-2AM** only if you want more throughput and can tolerate higher variance.

## Indicator artifact
- New Pine file: `/root/ict-turbo-v2/pine/mgc_evening_ifvg_indicator.pine`
- Existing strategy file patched: `/root/ict-turbo-v2/ict_turbo_mgc_v2.pine`

## Residual risk
- Pine indicator has been engineered for compile-safe patterns, but TradingView compile confirmation still needs one paste/import test in the UI.
- Backtest engine still reflects the project's simplified IFVG model, not a tick-accurate execution simulator.