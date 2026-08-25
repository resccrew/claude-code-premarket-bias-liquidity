# Changelog

All notable changes to this skill are documented here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] — 2026-08-25

First public release.

### Added

- Four-step pre-market model: HTF bias and DOL, key level selection, iFVG confirmation, execution and risk.
- Three data-source modes, chosen with the user before analysis starts: live TradingView MCP, chart screenshots, or user-dictated levels. Modes can be mixed per timeframe, and the source used is always stated in the output.
- TradingView MCP integration with a preflight sequence — health check, guided relaunch with CDP enabled, and chart-state verification after every symbol or timeframe change.
- Session-window logic anchored to 09:30–11:00 ET, with explicit preparation-mode behavior outside the window and on closed days. Eastern time is resolved from bar timestamps rather than assumed, so daylight saving and shortened sessions do not silently corrupt the read.
- Validation gate that blocks any entry, stop or target from being emitted while confirmation is missing, and names the specific missing piece instead.
- Economic calendar check as a required step before delivering a plan, with explicit disclosure when a calendar is unavailable.
- `references/glossary.md` — definitions for FVG, CISD, RJB, iFVG, manipulation leg, DOL, liquidity pools and SMT as the model uses them.
- `references/worked-example.md` — two full runs: one that correctly declines to trade, one that reaches a plan.
- `references/output-template.md` — the briefing structure, with the required disclaimer.
- Documented MCP tool quirks, including `quote_get` returning the chart's current symbol despite an explicit `symbol` argument.

### Notes

This release makes no performance claims. The skill is a structure for analysis, not a signal service.
