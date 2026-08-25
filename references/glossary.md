# Glossary

Definitions as this skill uses them. These are widely-documented ICT/SMC concepts; the wording here is what the four-step model depends on, so prefer these definitions over looser ones when they conflict.

## FVG — Fair Value Gap

A three-candle imbalance where the middle candle moves fast enough that the first and third candles' wicks do not overlap. The unfilled space between them is the gap.

- **Bullish FVG** — the low of candle 3 sits above the high of candle 1. The gap is that space.
- **Bearish FVG** — the high of candle 3 sits below the low of candle 1.

**Mitigated vs unmitigated.** A gap price has already traded back into is mitigated and is spent as a key level. Only unmitigated gaps qualify under Step 2 Type A. The exception: once an intermediate high/low *inside* a mitigated gap gets swept, the zone is live again.

**Respect vs disrespect** drives the Step 1 bias read. A market that holds and bounces from bullish gaps while closing straight through bearish ones is bullish, and vice versa. Bodies matter here — a wick poking through a gap is not disrespect.

## Intermediate high / low

A minor swing point sitting *inside* a larger structure such as an FVG, rather than at the edges of the leg. These are what get swept to re-validate a spent gap, and what CISD and rejection blocks form against.

## CISD — Change in State of Delivery

The moment delivery flips direction, marked at the opening price of the candle where it happened.

- **Bullish CISD** — a candle body closes *above* the open of the down-closing candle, or series of down-closing candles, that delivered into an FVG or into an intermediate low inside one.
- **Bearish CISD** — a candle body closes *below* the open of the up-closing candle or series that delivered into an FVG or an intermediate high inside one.

Mark the level at the opening price of that candle or series — not at the close, not at the wick.

CISD works standalone but is materially stronger when it coincides with an FVG, and strongest when the same zone produces CISD on more than one timeframe at once (15m and 1H together, for instance).

## RJB — Rejection Block

A wick that pushed into an FVG, or into an intermediate high/low inside one, and got rejected. Mark the wick's **entire range** as a box — high to low of the wick, not the body.

- **Bullish RJB** — a downward wick into a bullish FVG or an intermediate low inside one.
- **Bearish RJB** — an upward wick into a bearish FVG or an intermediate high inside one.

The extremes of the wick range are where precise fills tend to sit; truncating the box to "roughly where the wick was" loses the level.

## iFVG — Inverted Fair Value Gap

An FVG that price has closed *through* with a candle body, inverting its role: a bullish gap that gets body-closed below becomes resistance, a bearish gap body-closed above becomes support.

In this skill iFVG is the **confirmation trigger** in Step 3, and it carries three constraints that are easy to get wrong:

1. The gap must sit **inside the manipulation leg**. Gaps elsewhere on the chart are irrelevant, however clean they look.
2. You use the **highest timeframe gap that exists inside that leg**, not the most convenient one. A 4m gap in the leg means you wait on the 4m, even if a 1m gap would have triggered earlier.
3. Confirmation is a **body close** through the gap, on that gap's own timeframe. A wick through is not confirmation, and neither is a body close on a different timeframe.

## Manipulation leg

The move from swing high to swing low — or the reverse — that reached the key level. It is the search boundary for Step 3: every gap you consider must live inside this leg.

## DOL — Draw on Liquidity

The level price is being drawn toward: a swing high (buy-side liquidity) for a bullish bias, a swing low (sell-side liquidity) for a bearish one.

Separate the **near DOL** — the realistic draw for the current session, and the thing you actually reference in a plan — from the **far structural DOL**, which describes multi-day direction and is not a target for a single trade.

## Liquidity pools

Clusters of stops that make attractive targets. Equal highs and equal lows are the cleanest form: the flatter and more obvious the level, the more orders rest behind it. A 15m chart showing clean equal highs can overturn a bias read from higher timeframes alone, which is why Step 1 checks it near the open.

## SMT — Smart Money Technique

A divergence between correlated instruments: one index takes out a liquidity level while the other does not. NQ making a lower low while ES holds above its own is bearish-divergent, and vice versa.

Used in Step 1 as confirmation, never as the sole basis for a bias. Reading it requires actually loading both instruments — see the `quote_get` quirk noted in SKILL.md.

## The trading window

**09:30–11:00 ET.** The primary window for index futures. Outside it, this skill switches to preparation mode and does not issue entries. Trading after 11:00 is a rare exception, reserved for days where the window produced no setup and meaningful liquidity is still untaken.

Resolve ET explicitly rather than assuming a fixed UTC offset — US daylight saving shifts it twice a year, and futures holidays shorten sessions.
