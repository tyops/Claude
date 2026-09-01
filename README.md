# Trade Daddy — Pro Indicator Suite (Pine Script v6)

TradingView rebuild of the suite, one script per indicator, matched to the descriptions
in the indicator book and the reference charts.

| File | Pane | Stars | What it is |
|------|------|-------|------------|
| `pine/gold_regime.pine` | overlay | ✅ | Flagship regime filter + trigger system. Recolours candles, fires bull/bear triggers |
| `pine/carbon_structure.pine` | overlay | — | Dynamic band around price: direction, pressure, expansion vs compression |
| `pine/uranium_band.pine` | overlay | — | Balance-zone hunter: green panel with white rails at the zone high/low, extends while in balance, stops on a confirmed break. No stars — it is a structure tool |
| `pine/control_line.pine` | overlay | — | The dusty-rose baseline. Above it favours buyers, below favours sellers |
| `pine/air_pocket.pine` | overlay | — | Unfilled gaps, boxed and held open until price comes back and fills them |
| `pine/bark_or_bite.pine` | overlay | — | Trend filter with a memory of past pullbacks: noise (bark) vs structural break (bite) |
| `pine/bb_trend_avg.pine` | overlay | — | Trend-duration dashboard: Trend, Real length, Probable, Bull/Bear averages |
| `pine/mcdx_td.pine` | sub-pane | — | Participation lens — banker, hot money and retail tiers as a histogram |
| `pine/volume_planes.pine` | overlay | — | Horizontal volume profile — high volume nodes, value area, POC |
| `pine/neon_stack.pine` | sub-pane | ✅ | Momentum alignment meter — three RSI lengths as a stack, banded |
| `pine/iron_momentum.pine` | sub-pane | ✅ | Squeeze momentum vs its baseline, gold = buyers control, gray = sellers |
| `pine/silver_flow.pine` | sub-pane | ✅ | Banker band (gold) vs hot money (white), 0-100 with a control zone |
| `pine/sauls_watch.pine` | overlay | — | Multi-timeframe BULL / BEAR / NEUTRAL strip from the Gold Regime engine |
| `pine/regime_volume.pine` | sub-pane | — | The `Vol` pane in the suite palette |

MA Stack is not included — you already have it.

## Install

Pine Editor → *Open* → *New indicator* → paste one file → **Save** → **Add to chart**.
Repeat per file. Overlay scripts land on price; the rest open their own pane.

Reference layout, top to bottom: price (Gold Regime + Carbon Structure + Control Line +
Uranium Band + Volume Planes + Saul's Watch + your MA Stack), Vol, Iron Momentum,
Neon Stack, Silver Flow.

## Defaults

Every script ships configured the way it should be run. Nothing needs setting up before you
add it, and the optional filters are off on purpose — each one only ever *removes* signals,
so leaving them on by default would give you a different chart than the reference.

| Script | The settings that matter | Off by default |
|---|---|---|
| Gold Regime | MACD 12/26/9, trend 50, confirmation 10, ATR 14, sensitivity 5, swing filter on (lookback 20, age 5), cooldown 8, push 0.25 ATR, sequential confirmation on (window 4) | RSI side, volume, VWAP side, internals, HTF sync, regime agreement, streak table |
| Carbon Structure | Hull band, HMA, length **55**, lag 2 bars, cloud transparency 30 | Two-baseline mode, centre line, compression fade |
| Control Line | SMA 50, extra smoothing 3, dusty rose `#C0707E`, width 3 | Side tinting |
| Uranium Band | Window 14, persist 4, range 2.5-15%, 4 centre crossings, cooldown 25, max overlap 35%, break 2 closes, rails track the true high/low, green panel on | Volume-gated breaks, rail recolour, projecting after a break |
| Air Pocket | Fair value gap (3-bar), min 0.30% **and** 0.10 ATR, fully-closed fill, 20 open pockets | Session-only gaps, keeping filled pockets, size labels |
| BB Trend Avg | Bollinger 20/2, flips on a close beyond the band, table top-right | The bands themselves — this script contributes the table |
| Bark or Bite | ATR 14, 8 pullbacks remembered, bite at 1.4× the average, floor 2.0 ATR, volume required, memory clears on a bite | Bark markers, bar tinting |
| Volume Planes | Lookback 300, 60 bins, longest plane 140 bars, anchored to the window start, POC extended | Value-area highlight |
| Saul's Watch | 1H / 4H / 1D / 1W / 1M, Gold Regime engine, duration row on | — |
| Neon Stack | RSI 7 / 14 / 28, smoothing **3**, compression threshold 2.0 | Slow line, 30/50/70 levels |
| Iron Momentum | Squeeze momentum, BB 20/2 inside KC 20/1.5, smoothing **3**, baseline 9, squeeze dots on | MACD and Hull engines |
| Silver Flow | MCDX bands, banker RSI 21 base 50 at 1.5, hot money RSI 40 base 30 at 0.7, ceiling 20, readout on | Volume-flow mode, 50 level |
| MCDX TD | Banker RSI 50 base 50 at 1.5, hot money RSI 40 base 30 at 0.7, retail RSI 30 base 20 at 0.4, ceiling 20, heavy accumulation at 50% | — |
| Vol | Coloured by bar direction, average 20, spike at 2.0× | Gold Regime colouring |

Stars default to **Tiny** on all four scripts that draw them.

## Matching the reference charts

| What you see | Where it comes from |
|---|---|
| Gold/slate cloud hugging price | Carbon Structure fills the space between a Hull average and its own value two bars back — one fill, gold when the Hull is rising, slate when it is falling |
| Cloud pinches to a waist and flips colour | The Hull turning over: the gap to its own lag goes to nothing at every inflection, which is where the tone flips |
| Faint lines running through the cloud | Your own MA Stack showing through the transparent fill — Carbon Structure draws no inner lines |
| Dusty-rose line running through price | Control Line, SMA 50 by default with light extra smoothing |
| Cyan stars well below the bars, magenta well above | Gold Regime, *Star offset (ATR)* — 1.5 by default |
| Dark green panel with a white rail top and bottom | Uranium Band. The panel is drawn behind the chart so candles sit on top of it; no side borders. It extends while price stays in balance and stops at the confirmed break |
| White horizontal bars along the left edge | Volume Planes, anchored to the window start, brighter as the node gets bigger |
| `1H 4H 1D 1W 1M` strip with one gold cell | Saul's Watch, a 5-column horizontal strip, gold for BULL |
| Green ribbon with a pale halo and a white line through it | Neon Stack: bright core band, faded halo, mid line on top |
| Gold/slate blob breathing around zero | Iron Momentum: baseline ± the gap to the squeeze throttle, coloured by who has control of momentum |
| Dots on the zero line under a quiet blob | Iron Momentum marking every bar the Bollinger Bands sit inside the Keltner Channels — the market coiled |
| Gold zone under the gold line, near-black when white leads | Silver Flow's control zone, which also fades a step when control is slipping |
| Stars sitting just under each oscillator ribbon | The band pinches at every flip, so a fixed gap under the lower edge lands them in a neat row |

## How the stars work

Each indicator owns its own trigger — that is why the panes light up on different bars,
and why the book tells you to look for **2 of 3 confirming on the daily or higher, with at
least 1 confirming on the weekly or higher**.

| Indicator | ★ cyan | ★ magenta |
|-----------|--------|-----------|
| Gold Regime | Momentum flips up **and** price closes above the short confirmation baseline | Mirror image |
| Neon Stack | Stack flips into bullish alignment (fast > mid > slow) | Stack flips into bearish alignment |
| Iron Momentum | Fast momentum takes control over its baseline | Baseline takes control back |
| Silver Flow | Whale flow crosses above hot money | Hot money crosses above whale flow |

Neon usually fires first, Iron and Gold confirm, Silver tends to confirm last.

**Star size** is an input on every script that draws them — Auto, Tiny (default), Small,
Normal. Tiny is the smallest fixed size Pine offers; Auto is the only thing smaller, and it
scales the star to the bar width so it shrinks as you zoom out. Pine requires a constant for
character size, so each size is its own hidden plot and the input picks between them; you
will see the unused sizes listed in the Style tab, which is normal. Gold Regime also has a
*Star offset (ATR)* input to push the stars off the bar, and a triangle-marker mode.

> **If the stars still look oversized after you paste a new version in, the code is not the
> problem — your chart is.** TradingView keeps an indicator instance's *saved* inputs when
> the script behind it is edited, so a new default never reaches an instance you already
> added. Open the indicator's Settings, go to **Defaults → Reset settings**, and it will
> pick up every default in this release. The same applies to any other default that looks
> unchanged after an update.

## What each script does

### Gold Regime
Regime is a latch: it flips gold when momentum and trend both turn up, gray when both turn
down, and holds otherwise — so it rides a trend through pullbacks instead of flickering.
Triggers are separate from the regime: a momentum flip only counts when price closes beyond
the **short** confirmation baseline (`confirmation length`), which is what lets a bear star
print at a high while the candles are still gold. Optional RSI-side and volume-participation
filters, an optional "trigger must agree with regime" switch, and a cooldown.

### Gold Regime — what feeds it
MACD drives the momentum flip and ATR sets the offsets and the push threshold. RSI, volume,
VWAP side, and market internals (Put/Call ratio, VIX, McClellan level and divergence, on a
2-of-3 vote) are all wired in as filters — each behind its own switch, and each one only
ever removes triggers, never adds them. Internals and VWAP default off so the read stays
symbol-specific; RSI and volume default off so the star set matches what the reference
charts print.

**Sensitivity** (1-10, default 5) scales three gates at once: how far price must push past
the confirmation baseline, the cooldown between same-side triggers, and how close to a swing
extreme a trigger must sit. 5 leaves them exactly as configured; lower tightens all three,
higher loosens them. The individual inputs remain the baseline it scales, so calibrate there
and steer with the one dial.

### Confidence

Two kinds of claim sit behind these scripts, and they don't deserve the same trust.

**Usage rules** — what to do with the output — are published, so they're near-certain.
**Trigger rules** — what makes each script fire — are the protected part, and they leave
almost no visual fingerprint. A three-bar confirmation window and a five-bar one produce
charts you cannot tell apart. Treat anything below 40% as a starting point for testing.

| Script | Contents | Trigger |
|---|---|---|
| Saul's Watch | 100% | inherits Gold Regime |
| Neon Stack | 95% | 25% on lengths |
| Silver Flow | 95% | 30% on params |
| Gold Regime | 90% | 70% structure, 25% specifics |
| Uranium Band | 90% | 40% |
| Carbon Structure | 90% | 65% |
| Volume Planes | 75% | 35% |
| Control Line | 55% | 30% |
| Iron Momentum | 50% | 40% |

Cheap tests that would resolve the biggest unknowns: overlay SMA 200, SMA 100 and HMA 100
on a chart carrying Control Line — one will trace it exactly. Load RSI at 7/14/28 beside
Neon Stack and compare shape. Both are twenty-minute jobs and worth more than any amount
of further inference from screenshots.

### Carbon Structure
A Hull average compared against its own value two bars back, with the space between filled
— gold when rising, slate when falling. Length 55 and the Hull construction are the parts
its author has stated outright, which makes this the best-supported trigger in the suite.
The band widens as the Hull accelerates and pinches to a waist at every inflection.

The earlier fast/slow cloud is still available under *Band construction* — it widens as the
baselines separate (trend strength) and pinches as they converge (momentum fading), flipping
gold ↔ slate when they cross, with double smoothing on for a rounded shape. Under either
construction, *Fade the cloud while compressed* dims the fill when band width drops under
the compression percentile; it is off by default.

Carbon Structure draws **no inner lines**. Faint striations inside the cloud on a reference
chart are the MA Stack showing through the transparent fill, not part of this script.

### Uranium Band
Balance is declared when the window's range sits between the min and max range percentages,
price has crossed the window's midpoint at least *N* times, and the average bar range is a
real fraction of the band height (filters dead tape). Two horizontal rails are drawn at the
window's high and low over a solid dark green panel drawn behind the chart, and they extend
bar by bar while balance holds. While the zone is live the rails track its true high and low,
so a wick or a single close outside widens the zone rather than leaving a rail that no longer
contains price; the moment a bar closes outside, the rails lock and the break clock starts. A
break needs *N* consecutive closes outside, optionally past a buffer, and on confirmation the
zone stops. Old zones are kept up to the limit you set, then deleted.

**If the zone vanishes when you mouse over it.** Hovering never re-runs a Pine script, so
nothing in the logic can cause this — only the rendering can, and `behind_chart = true` is
the one flag in this suite that moves drawings onto a different layer. A layer below the
chart can be occluded by the hover/crosshair layer TradingView paints on top. Two ways to
confirm in about ten seconds: flip `behind_chart = true` to `false` in the `indicator()`
call, save, then remove and re-add the indicator; or just turn off *Draw the green panel* in
Style, which leaves the rails and centre line as ordinary foreground drawings. If the rails
stay put while the panel was disappearing, it is the panel's layer. Report which one it was
rather than living with it — the diagnosis above is reasoning about TradingView's renderer,
not something verified from here.

### Volume Planes
Bins the lookback window by price, spreads each bar's volume across the bins its range
touches, and draws every bin as a plane whose length is its share of the peak. Value area
grows out from the POC until it covers the set percentage; POC optionally extends across the
chart. Anchor the planes to the window start or to the right edge.

### Neon Stack
Three RSI **lengths** — 7, 14 and 28 — not three smoothings of one RSI. Its author says to
use a triple RSI if you don't want his, so three lengths it is. Bullish alignment is
fast > mid > slow, and the midline is wrapped in a band of `|fast − slow| × width` with a
floor, coloured by the stack state. Wide = impulse, tight = compression. Line smoothing
defaults to 3: a raw RSI(7) prints jagged, and the reference panes are smooth.

### Iron Momentum
Squeeze momentum by default — a linear regression of price against its own range midpoint,
with Bollinger Bands measured inside Keltner Channels (20 / 2 / 20 / 1.5) detecting the
squeeze directly rather than inferring it from band width. The predecessor charts run
SQZMOM_LB in this pane, which is what settled it. Band = baseline ± `|throttle − baseline| ×
width`, gold when buyers have control of momentum, iron-gray when sellers do; a dot on the
zero line marks every coiled bar. MACD (chart), MACD (higher timeframe) and Hull deviation
remain selectable under *Engine*. Line smoothing defaults to 3 for the same reason as Neon.

### Silver Flow
MCDX bands by default. Each band is an RSI on its own period, offset by a base level, scaled
by a sensitivity, clamped into a 0-20 ceiling, then read as a percentage of that ceiling —
which is why a raw 17.8 displays as 89%, and reproducing those published readouts is what
identified the construction. Gold is the banker band (RSI 21), white is hot money (RSI 40).
The zone between them tones down when control is slipping (gold falling while gold leads, or
gold rising while white leads). Stars are the crossings. The earlier size-filtered volume
model is still available under *Construction*.

Crossings are regime hints, **not entries** — its author is explicit about that, and it is
the most misused thing in the suite. Nor can this see institutional order flow; nothing
available to a chart script can. It is a price-derived proxy, and "banker" is a label on an
oscillator rather than a fact about anyone's book.

Market internals — Put/Call ratio, VIX, McClellan breadth — live in **Gold Regime**, behind
its *Require internals confirmation* switch, and are off by default so the read stays
symbol-specific. Feeds your plan does not carry (`USI:PCC`, `CBOE:VIX`, `USI:ADVN.NY`,
`USI:DECL.NY`) return `na` and drop out of the vote instead of breaking anything.

### Air Pocket
A gap is a price range that never traded, and it stays a magnet until it does. Each gap
that clears both size filters (percentage of price *and* a fraction of ATR, so the filter
travels across symbols) gets a box: blue for a gap up, leaving unfilled air below price;
magenta for a gap down, leaving air above. The box extends right, bar after bar, until
price comes back through it — then it is spent and disappears. *First touch* ends a pocket
as soon as price reaches its near edge; the default *Fully closed* holds it until the whole
gap is traded through.

### BB Trend Avg
A Bollinger breakout sets the trend — a close beyond the upper band turns it bullish,
beyond the lower band bearish — and it holds until the other side is taken. The table
reports how long the current run has lasted against how long runs usually last on this
symbol: **Real length**, **Probable** (the average for the side you're on), and both
averages, showing `n/a` until one completes.

Read Probable as context, not a countdown. Streak lengths are close to memoryless, so a
trend reaching its average is no more likely to end than one that hasn't.

### Bark or Bite
Every trend pulls back; the question is whether this pullback resembles the ones the trend
has already absorbed. The indicator keeps a rolling memory of completed pullback depths,
measured in ATR so it travels across symbols, and compares the live pullback against it. A
**bark** sits inside that history — noise, bias holds. A **bite** exceeds the remembered
average by the set multiple *and* carries above-average volume — structural, bias flips.
Memory clears on a flip by default, so each new trend learns its own character. The status
box shows the live pullback against the current threshold and how much memory has built up.

### MCDX TD
Three tiers of participation, each an RSI on its own period, offset by a base level, scaled
by a sensitivity, clamped to the tier ceiling and read as a percentage of *that* ceiling.
Banker (red) is the slow institutional tier, hot money (yellow) the fast speculative one,
retail (green) the crowd. Rising red above the heavy-accumulation line is the reading that
matters; retail leading with no red under it is thin support. Defaults follow the published
Multi-Color Dragon values — banker RSI 50 base 50 at 1.5, hot money RSI 40 base 30 at 0.7.

The three are **independent readings, not shares of one pie**. The reference charts print
62 / 18 / 0 and 74 / 46 / 19, which sum to 80 and 139 — that is what ruled out the
stacked-to-100 reading an earlier version of this script used. Bars are drawn tallest-first
so the shorter tiers stay visible in front.

### Saul's Watch
Runs the Gold Regime engine on five configurable timeframes via `request.security` and
labels each BULL, BEAR, or NEUTRAL. Neutral means momentum and trend disagree on that
timeframe — the chop warning.

## If an oscillator lands on the price pane

Neon Stack, Iron Momentum, Silver Flow, MCDX TD and Vol are all `overlay = false` and
belong in their own box under the chart. TradingView decides an indicator's pane when you *add* it
and never moves an instance afterwards — so if you paste one of these into a Pine Editor
tab that already has a price-pane script on the chart, the new code inherits that pane and
the ribbon smears across price.

Fix either way:

- Remove it from the chart, open a **new** Pine Editor tab, paste, then *Add to chart*, or
- Right-click the indicator's title on the chart → **Move to → New pane below**.

Give each script its own editor tab and this cannot happen.

## Notes

- Written against the **Pine Script v6** spec. Not compiled from this environment — paste
  into the Pine Editor and it will tell you immediately if your build disagrees.
- Alerts ship on every script. Create them as **Once Per Bar Close**; triggers evaluate on
  the close, so intrabar they can appear and disappear before the bar finishes.
- Volume-derived pieces (Silver Flow, Volume Planes, Vol) need a symbol with volume data.
- This is analysis tooling, not trading advice, and not a substitute for entries, stops,
  sizing, and exits.
