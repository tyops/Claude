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
| `pine/mcdx_td.pine` | sub-pane | — | Participation lens — banker, hot money and retail tiers as a histogram |
| `pine/volume_planes.pine` | overlay | — | Horizontal volume profile — high volume nodes, value area, POC |
| `pine/neon_stack.pine` | sub-pane | ✅ | Momentum alignment meter — three speed layers as a stack, banded |
| `pine/iron_momentum.pine` | sub-pane | ✅ | Fast momentum vs its baseline, gold = buyers control, gray = sellers |
| `pine/silver_flow.pine` | sub-pane | ✅ | Whale flow (gold) vs reactive money (white), 0-100 with a control zone |
| `pine/sauls_watch.pine` | overlay | — | Multi-timeframe BULL / BEAR / NEUTRAL strip from the Gold Regime engine |
| `pine/regime_volume.pine` | sub-pane | — | The `Vol` pane in the suite palette |
| `pine/ma_stack.pine` | overlay | — | EMA 8/21/34 over SMA 50/100/200, labelled at the right edge, SMA 200 tinted by regime |
| `pine/dma_wma_200.pine` | overlay | — | Just the two long ones: 200-day and 200-week, each from its own timeframe |

MA Stack and 200 DMA / 200 WMA are your own scripts, not part of the Trade Daddy suite —
they live here so they are versioned alongside everything else on the chart.

## Install

Pine Editor → *Open* → *New indicator* → paste one file → **Save** → **Add to chart**.
Repeat per file. Overlay scripts land on price; the rest open their own pane.

Reference layout, top to bottom: price (Gold Regime + Carbon Structure + Control Line +
Uranium Band + Volume Planes + Saul's Watch + MA Stack), Vol, Iron Momentum,
Neon Stack, Silver Flow.

## Matching the reference charts

| What you see | Where it comes from |
|---|---|
| Gold/slate cloud hugging price | Carbon Structure fills the space between two double-smoothed baselines — one fill, gold when fast leads, slate when it doesn't |
| Cloud pinches to a waist and flips colour | The baselines converging and crossing; a small ATR floor keeps the waist visible |
| Faint lines running through the cloud | MA Stack showing through the transparent fill — Carbon Structure draws no inner lines |
| Dusty-rose line running through price | Control Line, SMA 50 by default with light extra smoothing |
| Cyan stars well below the bars, magenta well above | Gold Regime, *Star offset (ATR)* — 2.0 by default |
| Dark green panel with a white rail top and bottom | Uranium Band. The panel is drawn behind the chart so candles sit on top of it; no side borders. It extends while price stays in balance and stops at the confirmed break |
| White horizontal bars along the left edge | Volume Planes, anchored to the window start, brighter as the node gets bigger |
| `1H 4H 1D 1W 1M` strip with one gold cell | Saul's Watch, a 5-column horizontal strip, gold for BULL |
| Green ribbon with a pale halo and a white line through it | Neon Stack: bright core band, faded halo, mid line on top |
| Gold/slate blob breathing around zero | Iron Momentum: baseline ± the gap to the fast line, coloured by who has control |
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

### Carbon Structure
Two smoothed baselines; the cloud is drawn between them with an ATR floor so it never
collapses to a line. It widens as the baselines separate (trend strength) and pinches as
they converge (momentum fading), flipping gold ↔ slate when they cross. Width percentile
under the compression threshold fades the cloud. Double smoothing is on by default for the
rounded shape in the reference charts.

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

### Air Pocket
A gap is a price range that never traded, and it stays a magnet until it does. Each gap
that clears both size filters (percentage of price *and* a fraction of ATR, so the filter
travels across symbols) gets a box: blue for a gap up, leaving unfilled air below price;
magenta for a gap down, leaving air above. The box extends right, bar after bar, until
price comes back through it — then it is spent and disappears. *First touch* ends a pocket
as soon as price reaches its near edge; the default *Fully closed* holds it until the whole
gap is traded through.

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
by a sensitivity and clamped into a 0-20 band so they read on one scale. Banker (red) is the
slow institutional tier, hot money (yellow) the fast speculative one, retail (green) the
crowd. Rising red above the heavy-accumulation line is the reading that matters; retail
leading with no red under it is thin support. Defaults follow the published Multi-Color
Dragon values — banker RSI 50 base 50 at 1.5, hot money RSI 40 base 30 at 0.7.

### Saul's Watch
Runs the Gold Regime engine on five configurable timeframes via `request.security` and
labels each BULL, BEAR, or NEUTRAL. Neutral means momentum and trend disagree on that
timeframe — the chop warning.

### MA Stack
Not part of the suite — your own script, kept here so it is versioned with the rest.

Six averages on price — EMA 8/21/34 as the fast layer, SMA 50/100/200 as the slow one —
each tagged with a label at the right edge. SMA 200 is drawn heavier and, with *SMA 200
Regime Bias* on, takes the bull colour while price closes above it. Everything can run on a
higher timeframe through the *Calculation Timeframe* input.

**The fade** runs right to left: solid at the live edge, thinning out as the lines travel
back into history, so the working end of the stack reads first and old crosses sit back in
the chart. *Visible range* (default) spans the gradient across whatever is on screen — it
re-scales as you zoom and follows you when you scroll back, because reading the chart's
visible-range variables makes TradingView recalculate the script on every scroll and zoom.
*Fixed bars* measures it back from the last bar over a set number of bars instead, so the
faded region stays put. *Fade curve* above 1 keeps the near bars solid and does the fading
further out; below 1 starts dropping away immediately.

**Switching an average off:** use the toggles in the *Visibility* group of the **Inputs**
tab, not the checkboxes in the Style tab. Pine cannot read Style-tab visibility — that
switch is applied by TradingView after the script has run, so it hides the plotted line and
leaves the label sitting there. The Visibility toggles hide the line and its label together.

### 200 DMA / 200 WMA
Not part of the suite either — MA Stack cut down to the two lines that matter on the big
picture, for when the six-average version is too much on the chart.

The 200 DMA is a 200-period SMA requested from the **daily** timeframe, the 200 WMA a
200-period SMA requested from the **weekly** one, so both sit at the same price no matter
what timeframe the chart is on — put it on a 15-minute chart and the daily and weekly lines
are still the daily and weekly lines. Both timeframes are inputs if you want something else.

Regime tint is on for the daily line and off for the weekly by default, so the two are never
the same shade at once: gold above, gray below on the daily; a constant pale line for the
weekly. Visibility toggles, labels and the fade work exactly as they do in MA Stack.

200 weekly bars is roughly four years, so on a symbol younger than that the weekly line has
nothing to average and does not draw.

## If an oscillator lands on the price pane

Neon Stack, Iron Momentum, Silver Flow and Vol are all `overlay = false` and belong in
their own box under the chart. TradingView decides an indicator's pane when you *add* it
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
