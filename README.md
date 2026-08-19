# ThetaDaddy — Carbon Structure Trading System (Pine Script v6)

A four-part TradingView system that rebuilds the overlay + oscillator stack from the
reference charts: **Gold Regime candles, Uranium Band, Control Line, Volume Planes,
MA Stack, Neon Stack, Iron Momentum, Silver Flow**, and the blue/purple ★ signals that
run through every pane.

All four scripts share one **star engine**, so a star that prints on the chart prints in
the oscillators on the same bar.

| # | File | Pane | What it draws |
|---|------|------|----------------|
| 1 | `pine/01_carbon_structure_main.pine` | Overlay | Gold Regime barcolor, Uranium Band, Control Line, Volume Planes, MA Stack, ★ signals, status table |
| 2 | `pine/02_neon_stack_iron_momentum.pine` | Sub-pane | Neon Stack (RSI ribbon) + Iron Momentum (MACD ribbon), ★ signals |
| 3 | `pine/03_silver_flow.pine` | Sub-pane | Whale flow (gold) vs reactive flow (white) + delta fill, ★ signals |
| 4 | `pine/04_regime_volume.pine` | Sub-pane | The gold/gray volume histogram that matches the regime |

## Install

1. TradingView → **Pine Editor** → *Open* → *New indicator*.
2. Paste one file, **Save**, **Add to chart**. Repeat for each file.
3. To reproduce the reference layout exactly:
   - Add **Carbon Structure** (overlay).
   - Add **Regime Volume** (own pane).
   - Add **Neon / Iron** twice — set one copy's *Display* to `Iron Momentum`, the other to
     `Neon Stack`. That gives the two separate ribbons in the screenshots. Leaving the
     default `Both (normalised 0-100)` puts them in one pane on a shared scale.
   - Add **Silver Flow** (own pane).

## The signals

**Blue ★ = bullish momentum / control change. Purple ★ = bearish.** They are context —
a shift in who is in control — not automatic entries.

A blue star needs *all* of:

- RSI crosses **up** through its level (default 50)
- MACD line above its signal line
- Close above the **Control Line**
- Market internals agree — at least *N* of 3 votes (default 2): Put/Call ratio below its
  average, VIX below its average, McClellan oscillator positive **or** showing a bullish
  divergence against price
- Optional: regime agreement (`Require regime agreement`, off by default)

A purple star is the mirror image. A per-side **cooldown** (default 5 bars) stops clusters
of stars firing on the same swing.

## The pieces

### Gold Regime (barcolor)
A latching state machine, so the regime holds instead of flickering bar to bar:

- flips **gold** when close is above both the trend MA and the Control Line, the MA is
  rising, and volume participates (fast volume EMA above the volume average × multiple,
  or OBV rising)
- flips **gray** on the mirror condition, and otherwise holds its last state

Candles are painted with `barcolor()`; `Solid bodies` adds a `plotcandle()` overlay if you
want filled bodies instead of hollow ones.

### Uranium Band
`EMA(hlc3, band length) ± ATR(atr length) × multiple`.
Close **outside** the band = a real move; price **inside** = balance/chop. The band edge
lights up on a break, and the cloud fades when the width percentile drops under 25
(pinched = compression) — `linewidth` has to be a constant in `plot()`, so brightness
carries that information instead of thickness. Triangles mark the bar a break starts.

### Control Line
One baseline: price above it favours buyers, below favours sellers. Four modes —
**Rolling VWAP** (default, `Σ(hlc3·vol)/Σ(vol)` over the VWAP length), **Session VWAP**
(anchored, hand-rolled so it never errors on symbols without volume), **VWMA**, **EMA**.
Default colour matches the mauve baseline in the reference chart.

### Volume Planes
A VPVR-lite: the lookback window is split into price bins, each bar's volume is spread
across the bins its range touches, and the highest-volume nodes are drawn as horizontal
lines extending right. The POC is solid and thick, the rest dotted. Neighbouring bins are
knocked out after each pick so the planes don't cluster on one shelf. Drawn on the last
bar only, so it costs nothing on history.

### Neon Stack & Iron Momentum
- **Neon Stack** — four EMAs of RSI (2/4/7/12 by default) with fills between them. Pinched
  ribbon = compression, fanned = momentum. Reacts first.
- **Iron Momentum** — smoothed MACD against its signal, gold body when bullish, iron-gray
  when not. Confirms later; it is the trend-strength read.

### Silver Flow
- **Gold line — whale flow.** Only bars whose volume exceeds the baseline average ×
  multiple count; their signed body-ratio flow is smoothed, normalised 0-100, then blended
  with the internals (inverted Put/Call, inverted VIX, McClellan breadth) using the weights
  in the *Flow weights* group. Weights re-normalise automatically and any feed missing on
  the current symbol drops out of the blend.
- **White line — reactive flow.** Fast money: MFI/RSI money flow with short smoothing.
- **The fill is the delta.** Gold body (gold over white) = institutional control. Silver
  body = reactive flow leading, which makes rallies lower quality until gold catches up.

## Data feeds

The internals default to `USI:PCC` (CBOE total put/call), `CBOE:VIX`, and
`USI:ADVN.NY` / `USI:DECL.NY` (McClellan is built from advances − declines as
`EMA(19) − EMA(39)`). Every request uses `ignore_invalid_symbol = true`:

- If your TradingView plan doesn't carry a feed, that component returns `na`, drops out of
  the vote and out of the Silver Flow blend — nothing breaks, the filter just gets looser.
- Swap in your own symbols in the *Market internals* group (e.g. `TVC:VIX`,
  `USI:PCCE`, `USI:ADVN.NQ` / `USI:DECL.NQ` for Nasdaq breadth).
- Leave *Internals timeframe* blank to follow the chart. Set it to `D` only if you want a
  fixed daily read on an intraday chart.
- `ta.mfi` and the volume terms need a symbol with volume data. On volume-less symbols the
  Control Line falls back to an EMA automatically.

## Inputs you'll actually touch

Every lookback the system runs on is exposed: **RSI** length + cross levels, **MACD**
fast/slow/signal, **ATR** length + band multiple, **VWAP** length and mode, volume
averages, McClellan EMAs and divergence lookback, internals vote threshold, star cooldown,
and all colours.

## Alerts

Each script ships `alertcondition()` entries: blue/purple star, regime flips, Uranium
breakouts, Neon compression/expansion, Iron cross, and whale/reactive control changes.
Create them with **Once Per Bar Close** — the star engine evaluates on the bar's close, so
intrabar alerts can appear and disappear before the bar finishes.

## Notes

- Written against the **Pine Script v6** spec (`//@version=6`). It has not been run through
  TradingView's compiler from this environment — paste it into the Pine Editor and it will
  tell you instantly if your build disagrees on anything.
- The stars use `plotchar()` — `plotshape()` has no star style, and `plotchar` renders the
  ★ glyph exactly as in the reference charts. Star size must stay a constant
  (`size.tiny` / `size.small` / `size.normal` / `size.large`); edit it in the source.
- This is analysis tooling, not trading advice. Nothing here manages risk or position size.
