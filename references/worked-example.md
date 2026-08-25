# Worked examples

Two runs, for calibration. The first is the path most sessions actually take — the window is closed, nothing is confirmed, and the correct output is preparation rather than a trade. The second shows a complete flow through to a plan.

Read the first one carefully. Producing a disciplined "no entry" is the harder half of this skill.

---

## Example A — outside the window, preparation mode

**Real data**, MNQ1! (Micro E-mini Nasdaq-100), read at 16:00 ET on a Monday. Prices are genuine; they are here to show shape and reasoning depth, not as a market call.

### Preflight

`tv_health_check` failed with a CDP connection error. `tv_launch` with `kill_existing: true` (after asking), then health-check again: connected, chart on `CME_MINI:MNQ1!` at 1m.

Newest bar timestamp converted: `TZ=America/New_York date -r 1787601600` → 16:00 ET, Monday. **The 09:30–11:00 window closed five hours ago.** That determines the whole shape of the output before any analysis happens.

### 1. Bias and DOL

**Bearish**, and Daily, 4H and 1H all agree.

Daily shows five consecutive down-closing bars with no meaningful retest — roughly 1,240 points off the 30339.75 high down to 29099.5. On 4H the structure is a clean sequence of lower highs and lower lows, and the bearish FVG at **29785.5–29820** has never been returned to; each rally tops lower than the last (30339 → 29759 → 29688 → 29492 → 29399). The 1H repeats it at smaller scale: a 29480.5 high, then 29283, then 29249, with a fresh low at **28947.75** that has not been revisited.

Near DOL: **28947.75**, the session's unmitigated 1H/4H swing low. Far structural DOL: the Daily swing low near **27200** — multi-day direction, not a target for one trade.

SMT against ES was not obtained. `quote_get` with an explicit symbol returned the Nasdaq contract's data regardless, so the cross-check would have required switching the chart. Stated as missing rather than guessed at.

### 2. Key levels

Deferred. Levels marked at 16:00 ET are stale by the next open — the overnight session will build new structure, and a level chosen now would be justified by data that no longer describes the market at 09:30. Better to re-read 15m/30m/1H shortly before the next session.

### 3. iFVG confirmation status

Not applicable. No manipulation leg is being tracked because no key level has been committed to.

### 4. Trade plan

None. The window is closed, and the validation gate fails on three counts: no key level, no leg, no body close.

### 5. Daily limit status

Outside 09:30–11:00 ET. Trading is done for the day; the next window opens tomorrow at 09:30 ET. Economic calendar for the next session not yet checked — flag to do that before the open.

### 6. Disclaimer

Not financial advice, no performance claims, all decisions the user's own.

---

## Example B — complete flow to a plan

**Constructed numbers**, not a real session. Shown to demonstrate the full four steps and how justification is written.

### Context

MNQ1!, 09:52 ET, inside the window. Bias carried in from the pre-market read: **bearish**, near DOL at the overnight low **29150**, with 4H and 1H both making lower highs and price closing through bullish gaps without holding them.

Economic calendar: nothing high-impact scheduled before 11:00.

### 2. Key level

**15m bearish FVG, 29330–29375, unmitigated.** Formed on the drop out of the pre-market range and untouched since. Price rallied off the open and pushed into it, topping at **29360** — inside the gap, which is what makes this the level to watch rather than a level that has already failed.

A second level sat lower at the 5m CISD near 29240, kept as a fallback for a limit entry. Nothing beyond those two — a third would have been noise.

### 3. iFVG confirmation

Manipulation leg: **29150 → 29360**, the rally that reached the key level.

Inside that leg there are gaps on 1m, 2m and 3m. No 4m or 5m gap exists within it, so the **3m gap at 29290–29310** is the highest available and therefore the one that governs. The 1m gaps are ignored despite triggering earlier — taking them would be trading a lower-reliability signal for a better fill, which the model explicitly rejects.

At 09:52 a 3m candle **body closed at 29285**, below the gap. Confirmed.

### 4. Trade plan

**Entry 29285**, at the close of the confirming candle. The alternative — a limit back into the 3m gap at 29300 — would improve R:R by 15 points at the cost of possibly never filling; taking the close is the default and the reason to deviate did not exist here.

**Stop 29365**, five points above the 29360 swing high that formed the leg. That high is the level that has to break for the bearish read to be wrong, which is what makes it the right stop rather than a tighter body-based one. Risk: 80 points.

**Target 29155**, just ahead of the 29150 overnight low — the nearest low-hanging fruit in the trade's direction, and the near DOL from step 1 happening to coincide. Reward 130 points, **R:R roughly 1.6:1**, inside the 1:1–1:3 band.

The stop does not move after entry. If 29150 gives way cleanly there is a case for a runner, but that is a separate decision requiring its own backtest, not a mid-trade improvisation.

### 5. Daily limit status

First trade of the day, inside the window. One more available if this one loses *and* a genuinely fresh key level appears. A win closes the day.

### 6. Disclaimer

As in Example A.

---

## What these examples are demonstrating

Example A refuses to produce a plan and names exactly why — closed window, no committed level, no leg, no body close — instead of assembling something plausible from an honest bias read. Example B reaches a plan, and every number in it carries the reason it sits where it does.

The failure mode this skill exists to prevent is the middle ground: a confident-sounding entry built on a bias that was real and a confirmation that was not.
