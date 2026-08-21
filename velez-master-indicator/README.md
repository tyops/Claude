# Velez Method Master Indicator
### An unofficial, methodology-inspired study workspace for TradingView

A Pine Script v6 **indicator** that renders the *state → position → event* framework described in
a 71-page third-party field manual titled *The Velez Method*, which reorganises eight public video
lessons attributed to Oliver Velez.

It is built to be read like an instrument panel, not like a stream of arrows. A glance should
answer four questions: **what state am I in, where is price relative to it, did anything happen,
and if I were in a trade, where is my protection right now?**

---

## ⚠️ Read this before anything else

**What this is not:**

- **Not official, endorsed, affiliated with, or authorised by** Oliver Velez or any related
  entity. No proprietary logo, artwork or branding is used. It is an independent study tool.
- **Not a strategy, backtest, broker simulator or execution engine.** There are no orders, no
  fills, no size, no position and no P&L. What it draws is a **virtual methodology lifecycle**,
  marked `(v)` everywhere it appears.
- **Not a claim of accuracy, profitability, win rate or performance.** Every percentage in the
  source manual is self-reported by its subject, unpublished, and carries no stated sample, date
  range or instrument set. None is used as an output, a score weight, or a default.
- **Not a faithful mechanisation of the method.** The source states its definitions are
  deliberately loose and instructs the reader to *"eyeball it rather than measure"*. **Every
  numeric threshold in this indicator is an addition by this project**, marked `~` on the chart.
- **Not investment advice.**

The manual's own assessment, quoted because it is the honest frame for this whole project:

> *"A method built on judgement calls cannot be backtested, which means you cannot know whether
> it works before risking money on it, and it means two traders applying the same lesson will
> take different trades. Be aware you are buying a discretionary framework, not a system."*
> — source manual, p.68

---

## What's in this directory

| File | What it is |
|---|---|
| `01_MASTER_RULEBOOK.md` | Evidence ledger. Every concept, rule, statistic, warning and gap in the PDF, with page citations. The source of truth for everything else |
| `02_CONCEPT_TO_CODE_MAP.md` | Per-concept disposition, plus 23 approximation worksheets showing exactly what was added and why |
| `03_VISUAL_DESIGN_SPEC.md` | Display modes, visual layers, the anti-clutter heuristic, dashboard layout |
| `04_INDICATOR_ARCHITECTURE.md` | Modules, data flow, repaint policy, same-bar policy, resource budget, input groups |
| **`05_VELEZ_MASTER_INDICATOR.pine`** | **The indicator** |
| `06_RULE_COVERAGE_AUDIT.md` | Forward and reverse traceability; what was omitted and why |
| `07_QA_REVIEW.md` | Three QA passes, the defect log, limitations, and the TradingView test procedure |

---

## Installing it

1. Open TradingView → **Pine Editor** → **Open → New blank indicator**.
2. Paste the entire contents of `05_VELEZ_MASTER_INDICATOR.pine`.
3. **Save**, then **Add to chart**.
4. Use a chart with **at least 200 bars of history** — the 200-period average is the source's
   non-negotiable second average, so with fewer bars the dashboard will correctly say
   `WARMING UP n/m` and show nothing else.

**Before relying on it, work through the 12-step test procedure in `07_QA_REVIEW.md` §5.**
It was written because compilation and live verification could not be performed in the authoring
environment — see [Verification status](#verification-status).

---

## The four display modes

Display modes change **visibility and density only**. They never change which signals exist, when
they fire, or what the alerts do. Only the *fidelity layer* does that.

| Mode | What you see | Use it for |
|---|---|---|
| **Clean** *(default)* | The two averages and their bands, the dashboard, the active virtual stop, and high-priority event shapes. **No text labels at all** | Watching all day without visual fatigue |
| **Analysis** | Adds aggregated event labels with a confluence tier, colour-change reference lines, position zone boxes, and stop-method change ticks | Reading the chart's story |
| **Full Velez** | Every supported event inside a bounded recent window, plus the retracement ladder with each level carrying its attributed source claim | Studying history |
| **Debug** | A 14-row funnel trace: state → position → event → gates → quality → confirmation → approximations → checklist → resources | Answering *"why did / didn't that fire?"* |

**Full Velez Mode is bounded**, not unlimited. TradingView caps labels, lines and boxes at 500
each, so the mode renders the last `historyCap` bars (default 300) and says so on the dashboard.

---

## What each visible signal means

### Foundation
| On chart | Meaning |
|---|---|
| Thin line | The 20-period simple moving average, on close. The source's convention: thin = the 20 |
| Thick line | The 200-period SMA. Thick = the 200 |
| Soft bands | The averages drawn as **zones, not lines** — because the source says twice that a small penetration is *"a lean, not a break"*. **The band width is this project's number, not the source's** |
| Dashed line (appears occasionally) | The 8-period SMA. It appears **only** while the trailing stop has escalated to it, because the source calls the 8 situational and trailing-only |

### Events
| Marker | Event | Source term? |
|---|---|---|
| ◆ Diamond | Elephant bar **and** colour change on one bar — the source rates this above either alone | Yes |
| ▲ / ▼ Triangle | Elephant bar. **Drawn muted when it reads as exhaustion**, because the source's point is that an exhaustion elephant is a reason *not* to act | Yes |
| ● Circle | Bottoming or topping tail bar (strict definition: most of the bar must be tail) | Yes |
| ✕ Cross | Colour change — one colour took out the **body** extreme of an opposite-coloured bar | Yes |
| ↑ / ↓ Arrow | Little red / green bar takeout | Shortened from the source's term |
| Dashed horizontal line | The colour-change reference level, drawn at the **body** extreme. The source is emphatic: *"Ignore the tails. Use the body."* This is the detail it says people get wrong | — |

### Virtual lifecycle — everything here is marked `(v)`
| On chart | Meaning |
|---|---|
| `ENTRY` label | A virtual lifecycle opened. **No order, no fill, no position** |
| Flat thin line | The entry reference level |
| Thin warning-coloured line | The **initial** stop, frozen where it started (hidden in Clean Mode) |
| **Thick stepped line** | **The current virtual stop.** The most important object on the chart — it is the running maximum of up to six rotating references and cannot be reconstructed by eye. Visible in every mode |
| Small label on the stop | Which method currently owns it: `FAT BAR`, `PIVOT`, `20 MA`, `8 MA`, `COLOUR ADJUST`, `BAR-BY-BAR` |
| `+` cross | A virtual add. The source calls adding on the first colour change **mandatory**, not optional |
| ✕ at the stop | The stop reference was reached. No fill is implied |
| ⚑ Flag | A push, carrying `[c]` because push counting needs future bars |

### The markers that matter most
| Marker | Meaning |
|---|---|
| **`~`** | This value depends on a **threshold this project added**, not one the source supplied. Always ASCII, never colour-only |
| **`[c]`** | **Confirmed later** — this needed future bars, and it is drawn on its confirmation bar, never backdated |
| **`(v)`** | **Virtual.** Not a trade |
| **`[impl]`** | **Implementation-defined.** Not Velez terminology |

---

## Reading the dashboard

22 rows, top to bottom, in the order the method itself applies:

| Row | What it tells you |
|---|---|
| `STATE` | Narrow (with grade G1/G2/G3), Regular or Wide, plus the measured gap. `~` because the source refuses to quantify "close together" |
| `POSITION` | +3 … −3, with `(POSITION 1)`, `(PLUTO LAND)` or `(IN THE STATE)` |
| `TREND` | The 20's direction and whether the 200 is flat. **The 20's direction is the hardest gate in the system** |
| `LOCATION` | Above / below / inside the state. The source keeps this **separate** from state, and so does this |
| `MODE BY STATE` | `EMERGENCE`, `NEAR THE 20` or `REVERSION` |
| `EVENT` | The highest-priority event on this bar |
| `LEG / PUSH` | Which leg you're in and, in a lifecycle, the push count |
| `SETUP` | Which named setup qualified |
| `CONFLUENCE` | The quality tier — **`[impl]`, not a Velez taxonomy** |
| `EXTENSION` | Space percentile, and whether it is extreme or three-finger wide |
| `VIRTUAL LIFECYCLE` | The state-machine phase |
| `ACTIVE STOP` | The level and its method |
| `ADD` | `DUE - first colour change` or the count so far |
| `EXIT CONTEXT` | Why an exit condition is present. **Context, not an instruction** |
| `SCALP / TRADE` | Whether the prior bounce cleared 50% by enough to make the next pullback a trade rather than a scalp |
| `FIDELITY` | Which layer is active |
| `APPROX ACTIVE` | How many approximations are contributing right now |
| `TF LESSON` | Which source lesson maps to this chart timeframe. **Advisory only** — it never blocks |
| `HISTORY` | The bounded window and how many objects have been recycled |
| `HTF 20` | Higher-timeframe context, if enabled |
| `CHART TYPE` | A warning if the chart type is non-standard |

---

## The fidelity layers

This is the only control that changes **which signals exist**.

### 1 · Strict source mechanics only
Only events whose **trigger** is fully specified by the source may open a lifecycle: colour
change, red/green bar takeout, strict tail bar, and the opening-bell bar-one sequence. Elephant
bars still *draw* but cannot drive an entry, because their size test needs an invented multiple.
**State and position are shown but do not gate anything**, since gating requires a threshold the
source explicitly declines to give.

*Trade-off, stated plainly:* noisier in raw events, but free of invented gating.

### 2 · Source + documented approximations *(default)*
The complete funnel. State and position gate everything, exactly as the source's ordering rule
requires. Quieter and much closer to the method's intent — but it depends on added thresholds.

*Trade-off:* fewer, better-qualified signals, at the cost of relying on numbers the source
did not supply.

### 3 · Include experimental modules
Layer 2 plus three clearly non-source extensions. **Each is off by default independently** — you
must switch both this layer *and* the individual module on before anything changes.

---

## Exact translations versus approximations

**38 concepts are exact translations.** The source specified them completely:

> the two averages and their length/type/source · the colour change (body extreme, need not be
> adjacent, entry one tick beyond) · the strict tail-bar definition and its snowman counterexample
> · elephant + colour change outranking either alone · the little red bar takeout · the full
> opening-bell sequence (bar one untouched, mark +1 tick, stop −1 tick) · the stop ladder's three
> rungs including "skip the trade" · the rotation to whichever reference is most protective · the
> pivot hand-back to the 20 · never stopping inside position one, and the opening-bell exception ·
> narrow → wide as a standalone exit · the retracement levels · the scalp-versus-trade decision

**37 concepts are approximations.** The source described them but gave no number:

> how close is "close together" · what counts as flat · how wide the MA band is · where the
> position bands sit · how large is "visibly larger" · exactly what fraction is "most of the bar"
> · how near is "near the 20" · how far back is "the recent past" · how long is a "brief pause" ·
> what a "push" is · where "the initial move" ends · what "accelerating faster than the 20 can
> track" means · what "separated and lost that support" means · what makes a space "unusually
> large" · what "severely breaks" the halfway point means · what makes a prior move "strong"

Each has a worksheet in `02_CONCEPT_TO_CODE_MAP.md §11` showing the source wording, why it is not
objective, the candidate models considered, the one chosen, the failure modes, and why it must
never be presented as official.

**Every approximation is configurable, and every one renders with `~`.**

---

## How the virtual lifecycle works

A deterministic finite-state machine. It is **virtual** — it models the methodology's shape, not
a trade.

```
IDLE → WATCHING → DEVELOPING → QUALIFIED → VIRTUAL LONG/SHORT ACTIVE
                                                    ↓
                                             EXIT CONTEXT  (does not force a close)
                                                    ↓
                                        COMPLETED / INVALIDATED → IDLE
```

- **One lifecycle at a time** by default. The source never addresses concurrency, so this is a
  disclosed platform policy, not a rule.
- **Adds are mandatory** on the first colour change after entry — the source states this twice and
  calls it a rule, not a choice. The dashboard shows `ADD: DUE` until it happens.
- **The stop never loosens.** It is the running maximum (long) or minimum (short) over six
  rotating references.
- **Exit contexts do not close anything.** The source calls the new-high exit *"a right and not an
  obligation"*, so the lifecycle continues and the dashboard simply reports the context.
- **Same-bar ambiguity resolves conservatively.** If entry and stop occur on one bar, the
  lifecycle is `INVALIDATED` with the reason `SAME-BAR AMBIGUITY` — never recorded as a win. OHLC
  cannot establish intrabar sequence, and this indicator does not pretend otherwise.

---

## Confirmation and repaint behaviour

**Default: everything actionable is evaluated at bar close, and all alerts require a confirmed
bar.**

| Class | Applies to | Behaviour |
|---|---|---|
| Confirmed | Averages, state, position, all four events, all named setups | Final at close; never changes |
| **Confirmed-later `[c]`** | Legs, resets, pushes, retracement levels, deep drop, Market Law Four, the pivot stop | Needs future bars. **Drawn on the confirmation bar, never backdated** |
| Higher-timeframe `[c]` | The optional HTF 20 direction | Closed HTF bars only; up to one HTF bar of delay |
| Provisional | Optional intrabar preview, **off by default** | May vanish before close. Never alerts, never advances the lifecycle, never scores |

**What is not claimed:** blanket "non-repainting". What *is* claimed, and is checkable: with
provisional preview off, no confirmed object changes after its bar closes, and everything
depending on future bars is drawn on its confirmation bar and visibly tagged.

No future-bar references. No `lookahead_on`. Exactly one `request.security` call, requesting only
closed bars, with lookahead explicitly off.

---

## Alerts

26 individual alert conditions plus one rich dynamic payload.

- **Transitions only**, never persistent states.
- **Confirmed-bar by default.** A provisional mode exists and is labelled repaint-capable.
- **Per-family toggles and a cooldown** to keep volume sane.
- Every message carries a non-recommendation disclaimer.

**To set up:**
1. Add the indicator to the chart.
2. For a specific alert: create an alert, choose the indicator, pick the condition from the
   dropdown.
3. For the rich payload: create an alert on **"Any alert() function call"**. It includes event,
   direction, price, state, position, setup, quality tier, current stop and method, fidelity
   layer, and confirmation status.
4. Set **Once Per Bar Close**.

> **Alerts must be recreated after you change the script or any alert-affecting input.**
> TradingView binds an alert to the script version and settings that existed when it was created.

---

## Session and timeframe limitations

- **Opening-bell and gap-fill modules require a regular session.** On 24-hour symbols
  (crypto, forex) and on daily-and-above charts they disable themselves rather than producing
  meaningless output.
- **Nothing is blocked by timeframe** — the source says the framework is timeframe-agnostic. The
  `TF LESSON` row is advisory.
- **`clearLookback` is timeframe-sensitive.** 20 bars is 40 minutes on a 2-minute chart and 20
  months on a monthly chart. The source only ever says "the recent past". Reconsider this input
  per chart.
- **Non-standard chart types distort everything.** Heikin Ashi, Renko, Kagi, P&F, Line Break and
  Range bars synthesise OHLC, and every event detector reads real bar geometry. A persistent
  warning appears; detection is not disabled, because that is your call, not the script's.

---

## Drawing retention limits

TradingView caps labels, lines and boxes at 500 each. Six bounded ring buffers hold worst-case
**170 labels, 80 lines and 60 boxes** — 66–88% headroom. Oldest objects are recycled first, and
the count is shown on the dashboard `HISTORY` row and in the Debug resource counter, so recycling
is never silent.

**Label overlap cannot be prevented.** Pine has no access to rendered pixel geometry, so
screen-space collision avoidance is impossible for any script. The anti-clutter heuristic — one
aggregated label per bar, minimum spacing, ATR-scaled offsets, alternating placement, priority
ranking, Clean-mode suppression — makes overlap rare. It cannot make it impossible, and this
project does not claim otherwise.

---

## Using Debug Mode

Set display mode to **Debug**. The panel answers *"why did / didn't that fire?"*:

```
1 STATE     NARROW G1   gap 0.62 vs narrow<=1.0 wide>=3.0   [CON-010 / SRC-051 p.8]
2 POSITION  +1          dUp 0.71 dDn 0.00  p1<=1.0 p2<=2.5  [CON-020 / SRC-071 p.10]
3 EVENT     ELEPHANT + CHANGE   rng x2.31 vs 1.8            [CON-030/032/034]
GATES       ALL PASS
BLOCKERS    none
QUALITY     13 -> ELITE SETUP   [impl layer - NOT a Velez taxonomy]
CONFIRM     event=CONFIRMED(bar close) · pivots=CONFIRMED-LATER [c]
MODELS      state=ATR · pos=ATR · elephant=Average multiple · cc=Nearest
APPROX (~)  state · position · MA band · flatness · elephant size · tail ratio · …
CHECKLIST   [x] state  [x] position  [x] event  [-] tail valid  [x] with the 20  [ ] UNSET max loss
RESOURCES   labels 61/500 · lines 34/500 · boxes 12/500 · trimmed 0
SOURCE CLAIMS (attributed, NOT outputs): 87% … 85% … 80% … All self-reported, unpublished
```

When a candidate is blocked, `BLOCKERS` names every failed gate with the rule that defines it —
`PLUTO LAND (SRC-079)`, `AGAINST THE 20 (SRC-224)`, `DUMB STOP (SRC-077)`, and so on.

The `CHECKLIST` row is the manual's own pre-entry checklist (p.66) with live values.

---

## Known limitations

The full list is in `07_QA_REVIEW.md §7`. The ones that matter most:

1. **Every threshold is added, not translated.** The source deliberately refuses to quantify its
   own definitions.
2. **"Push" is undefined in the source.** The three-push exit is its most-repeated exit rule and
   its mechanics appear nowhere in 71 pages. The implementation is a defensible proxy.
3. **Pivot-derived warnings arrive late.** The deep-drop warning in particular arrives *after* the
   drop — the opposite of the source's stated intent that you train on catching it forming.
4. **The hidden-green entry is a different event from the one taught.** The source's trigger is the
   intrabar instant green is erased; only the bar-close form is observable, and it will usually
   sit at a worse reference.
5. **Two intrabar entry timings are not implemented at all** — the source's own fallback is used
   instead.
6. **The full head-and-shoulders is not implemented.** The source supplies no tolerances and
   explicitly warns against mechanising it. The three components it *does* define — the 50%
   deep-drop test, the first-warning-drop comparison and the lower-high structure — are.
7. **No hysteresis.** A value sitting exactly on a threshold flips between bars.
8. **No sizing mathematics.** The source contains none; inventing it would be the single most
   consequential unsupported addition possible.
9. **Ten of thirteen bar types and eleven of fourteen events are absent from the source itself.**
   Not an implementation gap. Debug Mode says so on-chart.

---

## Verification status

> **Static review completed; TradingView compilation and live-chart verification were not
> performed in this environment.**

The authoring environment had no Pine compiler, no TradingView access and no market data.

**What was verified** (scripted inspection and line-by-line reading): all 71 pages reviewed; zero
detectors without a source citation; zero statistics used as outputs; zero future-data leaks;
zero `strategy.*` calls; exactly one `request.security` using closed bars with lookahead off;
exactly three division sites, all guarded; 26/26 alerts confirmed-gated and disclaimed; all plots
and alert conditions at global scope; zero forward references across 561 identifiers; resource
budgets at 50–88% headroom.

**Twelve defects were found and fixed during that review**, including three that would have
prevented compilation outright and one — a stop-validity gate evaluated after the gate mask was
computed — that would have let a stop inside position one pass the gates, directly violating the
source's most emphatic stop rule. The full log is in `07_QA_REVIEW.md §6`.

**What was not verified:** compilation, rendering, Bar Replay, alert delivery, on-screen density,
and behaviour on any real instrument. `07_QA_REVIEW.md §5` is a 12-step procedure for closing
that gap.

---

## Attribution and scope

This project analyses a third-party field manual as reference material. The manual itself
discloses that its rule identifiers, glossary, checklists, statistics table and critical
commentary are the compiler's editorial additions, and that its Part One arrangement "is not how
any single video presents it" — those distinctions are preserved throughout this documentation
rather than flattened.

The source's statistics are recorded because they are what was taught, and are treated exactly as
the manual instructs: *"Use them to rank setups against each other. Do not use them to size a
position."*

No content from the PDF has been reproduced wholesale. Short excerpts appear only as evidence for
specific propositions, with page citations.
