# Pre-Market Bias & Liquidity — a Claude Code Skill

A Claude Code skill that runs a disciplined pre-session markup on index futures — reading live TradingView charts, screenshots, or levels you dictate, and walking a fixed four-step model instead of improvising an opinion.

The point is not to generate more trade ideas. It is to make the assistant refuse to produce one until the conditions are actually met.

## Why this exists

Ask a general-purpose assistant for "the bias on NQ today" and you get fluent, confident prose that is not anchored to anything. It will happily give you an entry, a stop and a target for a setup that has not confirmed, because nothing in its instructions makes that a failure.

This skill fixes that with three constraints:

- **It never guesses the data.** Every level comes from a source you actually chose — a live chart, a screenshot, or numbers you dictated — never from the model's memory of what the market was doing at training time.
- **The steps are ordered and gated.** Bias, then key level, then confirmation, then execution. A validation gate sits in front of the output, and if confirmation has not printed, the answer is what still has to happen — not an entry.
- **It knows what time it is.** Outside 09:30–11:00 ET it switches to preparation mode and stops issuing entries, instead of analyzing a closed window as though it were live.

## Requirements

- **Claude Code**, or another client that loads Claude skills

Everything else is optional and chosen per session — see Data sources below. The strongest mode additionally needs:

- A **TradingView MCP server**, providing the `mcp__tradingview__*` tools
- **TradingView Desktop**, launched with remote debugging enabled

## Install

Clone straight into your skills directory:

```bash
git clone https://github.com/resccrew/premarket-bias-liquidity.git \
  ~/.claude/skills/premarket-bias-liquidity
```

## Data sources

The skill asks which source to use before it starts analyzing — it never assumes.

| Mode | What you provide | Notes |
|---|---|---|
| **Live (TradingView MCP)** | Nothing — the skill reads the chart itself | Strongest mode: exact, current prices. Requires the MCP server and TradingView Desktop with remote debugging on; the skill's preflight checks the connection and offers to relaunch it if needed. |
| **Screenshots** | Chart images, one per timeframe you're asked for | Prices are read off the image and are approximate. The skill will ask for a specific timeframe rather than work around a gap. |
| **Dictated levels** | You state the structure and prices directly | No chart reading at all — the skill checks your inputs for consistency and runs the model on them, attributing the numbers to you in the output. |

Modes can mix — live data for the higher timeframes, a screenshot for the entry timeframe, for instance. Whichever you use, the skill states the source in the briefing so you know how much weight the prices carry, and it will not fill a gap from memory in any mode.

## Use

Invoke it directly:

```
/premarket-bias-liquidity
```

Or just ask, and it triggers on intent:

- "Give me the bias on NQ for today"
- "Mark up the market before the open"
- "Where's the liquidity sitting?"
- "Check my setup — is this confirmed?"
- "Premarket brief on MNQ"

It reads whatever instrument is on the chart, or asks which one you want.

## The model

**1 — HTF bias and Draw on Liquidity.** Daily, 4H and 1H, with 15m as a final check near the open. Two questions only: which fair value gaps does price respect and which does it close straight through, and which swing high or low is it drawing toward. Cross-checked against a correlated instrument for SMT divergence where possible.

**2 — A valid key level.** Two or three, never twenty. An unmitigated FVG, a change in state of delivery, or a rejection block — each with the reason it is valid at this moment rather than in general.

**3 — iFVG confirmation.** The step that does the actual work. Inside the manipulation leg that reached the level, on the highest gap timeframe that exists in that leg, wait for a candle *body* to close through. A wick is not confirmation, and a lower timeframe gap is not a shortcut worth taking for a better fill.

**4 — Execution and risk.** Entry at the confirming close or on a limit back into the gap, stop beyond the swing that has to break for the read to be wrong, target the nearest sensible high or low at 1:1 to 1:3. One to two trades a day, hard stop after two losses.

Concept definitions live in [`references/glossary.md`](references/glossary.md). Two full worked runs — one that correctly refuses to trade, one that reaches a plan — are in [`references/worked-example.md`](references/worked-example.md).

## What the output looks like

Bias with the level and timeframe it is marked on, two or three key levels with validity reasons, the confirmation status of the leg being tracked, and — only if the gate passes — a plan where the entry, stop and target each carry the reason they sit where they do.

When the gate does not pass, you get the specific missing piece instead: *"waiting on a body close through the 4m gap at 29310."* That is a complete answer, and the skill is built to treat it as one.

## Limitations

Read these before relying on it.

- **Not autonomous.** It reads and reasons; it does not place orders, and it should not be wired to anything that does.
- **Index futures.** Built and tested against NQ/MNQ and ES/MES. The concepts travel to other instruments, but the session-timing logic is specific to US index futures.
- **One chart, one process, in live mode.** The MCP drives a single TradingView chart. Another process moving the symbol or timeframe mid-analysis will corrupt the read — the skill re-checks chart state, but do not run two live analyses concurrently.
- **Screenshots and dictated levels are only as good as what you provide.** The skill checks internal consistency but cannot verify a screenshot is current or that dictated numbers are accurate — that responsibility stays with you in those modes.
- **News is a blind spot it declares.** It prompts for an economic calendar check and will tell you when it could not do one, but it does not have a calendar built in.
- **The model can still be wrong.** A confirmed setup is a structured setup, not a correct prediction.

## Disclaimer

This is not financial advice, and it is not a signal service.

The skill is an analysis structure built on publicly documented ICT/SMC concepts. It makes **no performance claims** — no win rate, no expectancy, no backtest. Trading futures involves substantial risk of loss and is not suitable for every investor. Every entry, stop and position size decision is yours alone, and you are responsible for the outcome.

Nothing here is affiliated with or endorsed by TradingView, the CME, or any trading educator.

## Contributing

Issues and pull requests are welcome — particularly around edge cases in the confirmation logic, session handling on holidays and shortened sessions, and quirks in the MCP tooling.

Changes that add performance claims, win-rate numbers, or automated order placement will not be merged.

## License

[MIT](LICENSE)
