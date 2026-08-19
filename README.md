# Trade Daddy — Pro Indicator Suite (Pine Script v6)

TradingView rebuild of the suite, one script per indicator, matched to the descriptions
in the indicator book and the reference charts.

| File | Pane | Stars | What it is |
|------|------|-------|------------|
| `pine/gold_regime.pine` | overlay | ✅ | Flagship regime filter + trigger system. Recolours candles, fires bull/bear triggers |
| `pine/carbon_structure.pine` | overlay | — | Dynamic band around price: direction, pressure, expansion vs compression |
| `pine/uranium_band.pine` | overlay | — | Balance-zone hunter: horizontal band, extends while in balance, stops on a confirmed break |
| `pine/volume_planes.pine` | overlay | — | Horizontal volume profile — high volume nodes, value area, POC |
| `pine/neon_stack.pine` | sub-pane | ✅ | Momentum alignment meter — three speed layers as a stack, banded |
| `pine/iron_momentum.pine` | sub-pane | ✅ | Fast momentum vs its baseline, gold = buyers control, gray = sellers |
| `pine/silver_flow.pine` | sub-pane | ✅ | Whale flow (gold) vs reactive money (white), 0-100 with a control zone |
| `pine/sauls_watch.pine` | overlay | — | Multi-timeframe BULL / BEAR / NEUTRAL table from the Gold Regime engine |
| `pine/regime_volume.pine` | sub-pane | — | The `Vol` pane in the suite palette |

MA Stack is not included — you already have it.

## Install

Pine Editor → *Open* → *New indicator* → paste one file → **Save** → **Add to chart**.
Repeat per file. Overlay scripts land on price; the rest open their own pane.

Reference layout, top to bottom: price (Gold Regime + Carbon Structure + Uranium Band +
Volume Planes + your MA Stack), Vol, Iron Momentum, Neon Stack, Silver Flow.

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

**Star size** is an input on every script that draws them — Tiny (default), Small, Normal.
Pine requires a constant for character size, so each size is its own hidden plot and the
input picks between them; you will see the unused sizes listed in the Style tab, which is
normal. Gold Regime also has a *Star offset (ATR)* input to push the stars off the bar, and
a triangle-marker mode.

## What each script does

### Gold Regime
Regime is a latch: it flips gold when momentum and trend both turn up, gray when both turn
down, and holds otherwise — so it rides a trend through pullbacks instead of flickering.
Triggers are separate from the regime: a momentum flip only counts when price closes beyond
the **short** confirmation baseline (`confirmation length`), which is what lets a bear star
print at a high while the candles are still gold. Optional RSI-side and volume-participation
filters, an optional "trigger must agree with regime" switch, and a cooldown.

### Carbon Structure
Two smoothed baselines; the cloud is drawn between them with an ATR floor so it never
collapses to a line. It widens as the baselines separate (trend strength) and pinches as
they converge (momentum fading), flipping gold ↔ slate when they cross. Width percentile
under the compression threshold fades the cloud. Double smoothing is on by default for the
rounded shape in the reference charts.

### Uranium Band
Balance is declared when the window's range sits between the min and max range percentages,
price has crossed the window's midpoint at least *N* times, and the average bar range is a
real fraction of the band height (filters dead tape). The band then extends bar by bar. A
break needs *N* consecutive closes outside — optionally past a buffer — and on confirmation
the band freezes and recolours green (up) or red (down). Old bands are kept up to the limit
you set, then deleted.

### Volume Planes
Bins the lookback window by price, spreads each bar's volume across the bins its range
touches, and draws every bin as a plane whose length is its share of the peak. Value area
grows out from the POC until it covers the set percentage; POC optionally extends across the
chart. Anchor the planes to the window start or to the right edge.

### Neon Stack
Three EMAs of RSI. Bullish alignment = fast > mid > slow. The midline is wrapped in a band
of `|fast − slow| × width` with a floor, coloured by the stack state. Wide = impulse,
tight = compression.

### Iron Momentum
Smoothed MACD line vs its smoothed signal. Band = baseline ± `|fast − baseline| × width`,
gold when the fast line has control, iron-gray when it doesn't. The compression percentile
marks the tight-then-expanding flips the book calls the best ones.

### Silver Flow
Whale line: only bars whose volume clears the baseline average × multiple contribute, their
signed body-ratio flow is smoothed and normalised to 0-100. Reactive line: MFI, lightly
smoothed. The zone between them tones down when control is slipping (gold falling while gold
leads, or gold rising while white leads). Stars are the crossings.

Market internals — Put/Call ratio, VIX, McClellan breadth — are wired in but **off by
default**, so the default read stays symbol-specific. Turn on *Blend PCR / VIX / breadth*
to fold them into the gold line with the weights provided; feeds your plan does not carry
(`USI:PCC`, `CBOE:VIX`, `USI:ADVN.NY`, `USI:DECL.NY`) return `na`, drop out of the blend and
re-normalise the remaining weights instead of breaking anything.

### Saul's Watch
Runs the Gold Regime engine on five configurable timeframes via `request.security` and
labels each BULL, BEAR, or NEUTRAL. Neutral means momentum and trend disagree on that
timeframe — the chop warning.

## Notes

- Written against the **Pine Script v6** spec. Not compiled from this environment — paste
  into the Pine Editor and it will tell you immediately if your build disagrees.
- Alerts ship on every script. Create them as **Once Per Bar Close**; triggers evaluate on
  the close, so intrabar they can appear and disappear before the bar finishes.
- Volume-derived pieces (Silver Flow, Volume Planes, Vol) need a symbol with volume data.
- This is analysis tooling, not trading advice, and not a substitute for entries, stops,
  sizing, and exits.
