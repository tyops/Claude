# 04 — INDICATOR ARCHITECTURE
### Implementation architecture for `05_VELEZ_MASTER_INDICATOR.pine`

> Written **before** the Pine implementation. The script follows this document; where the two
> disagree, this document is the defect.

---

## TABLE OF CONTENTS

1. [Data flow](#1-data-flow)
2. [Module inventory](#2-module-inventory)
3. [Types, constants and state variables](#3-types-constants-and-state-variables)
4. [Pine Script v6 engineering constraints](#4-pine-script-v6-engineering-constraints)
5. [Confirmation and repaint policy](#5-confirmation-and-repaint-policy)
6. [Virtual lifecycle state machine](#6-virtual-lifecycle-state-machine)
7. [Same-bar ambiguity policy](#7-same-bar-ambiguity-policy)
8. [Precedence and platform-safeguard register](#8-precedence-and-platform-safeguard-register)
9. [Alerts](#9-alerts)
10. [Resource budget](#10-resource-budget)
11. [Error handling, guards and warm-up](#11-error-handling-guards-and-warm-up)
12. [Input organisation](#12-input-organisation)
13. [Traceability metadata](#13-traceability-metadata)

---

## 1. DATA FLOW

```
                       ┌─────────────────────────────────────┐
                       │  §00 HEADER · indicator() declaration │
                       └──────────────────┬────────────────────┘
                                          │
   ┌──────────────────────────────────────▼──────────────────────────────────────┐
   │  §01 INPUTS  — 19 groups, fidelity layer, display mode, every threshold      │
   └──────────────────────────────────────┬──────────────────────────────────────┘
                                          │
   ┌──────────────────────────────────────▼──────────────────────────────────────┐
   │  §02 GUARDS  — warm-up, na, zero-range, chart-type, mintick, session         │
   └──────────────────────────────────────┬──────────────────────────────────────┘
                                          │
   ┌──────────────────────────────────────▼──────────────────────────────────────┐
   │  §03 BASE     — atrUnit, bar geometry (rng, body, tails), colour, mintick    │
   └──────────────────────────────────────┬──────────────────────────────────────┘
                                          │
   ┌──────────────────────────────────────▼──────────────────────────────────────┐
   │  §04 MA       — sma20, sma200, sma8, bands, stateTop/stateBot                │
   └──────────────────────────────────────┬──────────────────────────────────────┘
                    ┌─────────────────────┼─────────────────────┐
                    │                     │                     │
   ┌────────────────▼──────┐  ┌───────────▼─────────┐  ┌────────▼──────────────┐
   │ §05 SLOPE / FLATNESS  │  │ §06 SPACE           │  │ §07 SWINGS (pivots)   │
   │ CON-005, CON-006      │  │ CON-071, CON-073    │  │ CON-055 legs, pushes  │
   └────────────────┬──────┘  └───────────┬─────────┘  └────────┬──────────────┘
                    └─────────────────────┼─────────────────────┘
                                          │
   ┌──────────────────────────────────────▼──────────────────────────────────────┐
   │  §08 STATE      CON-010…019   narrow / regular / wide + grade + transitions  │
   └──────────────────────────────────────┬──────────────────────────────────────┘
                                          │
   ┌──────────────────────────────────────▼──────────────────────────────────────┐
   │  §09 POSITION   CON-020…029   +3…−3, location, box / three-finger           │
   └──────────────────────────────────────┬──────────────────────────────────────┘
                                          │
   ┌──────────────────────────────────────▼──────────────────────────────────────┐
   │  §10 EVENTS     CON-030…039   elephant · tail · colour change · takeout ·    │
   │                                gap-fill · snowman diagnostic                 │
   └──────────────────────────────────────┬──────────────────────────────────────┘
                                          │
   ┌──────────────────────────────────────▼──────────────────────────────────────┐
   │  §11 SETUPS     CON-050…079   surge · swing · opening bell · hidden ·        │
   │                                dual space · retracement · reversal warnings  │
   └──────────────────────────────────────┬──────────────────────────────────────┘
                                          │
   ┌──────────────────────────────────────▼──────────────────────────────────────┐
   │  §12 GATES + QUALITY   CON-111/112   hard gates → blockers → additive score  │
   └──────────────────────────────────────┬──────────────────────────────────────┘
                                          │
   ┌──────────────────────────────────────▼──────────────────────────────────────┐
   │  §13 STOP RESOLVER     CON-080…095   six references → rotation → one level   │
   └──────────────────────────────────────┬──────────────────────────────────────┘
                                          │
   ┌──────────────────────────────────────▼──────────────────────────────────────┐
   │  §14 EXIT RESOLVER     CON-100…106   pushes · new high · narrow→wide · stop  │
   └──────────────────────────────────────┬──────────────────────────────────────┘
                                          │
   ┌──────────────────────────────────────▼──────────────────────────────────────┐
   │  §15 LIFECYCLE FSM     CON-110       11 states, deterministic transitions    │
   └──────────────────────────────────────┬──────────────────────────────────────┘
                                          │
   ┌──────────────────────────────────────▼──────────────────────────────────────┐
   │  §16 VISIBILITY RESOLVER  mode × fidelity × priority × spacing → draw or not │
   └──────────────────────────────────────┬──────────────────────────────────────┘
                                          │
        ┌─────────────────┬───────────────┼───────────────┬──────────────────┐
        │                 │               │               │                  │
┌───────▼──────┐ ┌────────▼──────┐ ┌──────▼──────┐ ┌──────▼──────┐ ┌─────────▼────────┐
│ §17 PLOTS    │ │ §18 DRAWINGS  │ │ §19 TABLE   │ │ §20 DEBUG   │ │ §21 ALERTS       │
│ global scope │ │ ring buffers  │ │ var table   │ │ trace panel │ │ alertcondition + │
│              │ │ bounded       │ │ islast only │ │             │ │ alert() payloads │
└──────────────┘ └───────────────┘ └─────────────┘ └─────────────┘ └──────────────────┘
```

**Strict separation.** Detection (§05–§15) never draws and never alerts. Rendering (§16–§20) never
computes a detection. Alerts (§21) read only booleans already produced by detection. This makes
the display-mode guarantee — *modes change visibility, never logic* — structural rather than a
promise.

---

## 2. MODULE INVENTORY

| § | Module | Responsibility | Depends on | Key outputs |
|---|---|---|---|---|
| §00 | `HEADER` | `indicator()` declaration, resource caps, disclaimer comment | — | — |
| §01 | `INPUTS` | All user inputs, grouped; fidelity + mode resolution | — | `fidelity`, `mode`, all thresholds |
| §02 | `GUARDS` | Warm-up, chart-type, session availability, na/zero guards | §01 | `ready`, `chartWarn` |
| §03 | `BASE` | Bar geometry primitives | §02 | `atrUnit`, `rng`, `body`, `upTail`, `dnTail`, `isGreen`, `isRed`, `isDoji`, `tick` |
| §04 | `MA` | Averages, bands, state envelope | §03 | `sma20`, `sma200`, `sma8`, `band`, `stateTop`, `stateBot` |
| §05 | `SLOPE` | Slope normalisation, flatness, trend | §04 | `flat20`, `flat200`, `priceSandwiched`, `trendDir` |
| §06 | `SPACE` | Price↔20 distance and its historical rank | §04 | `spaceN`, `spaceRank`, `spaceExtreme`, `near20` |
| §07 | `SWING` | Confirmed pivots, legs, resets, push counting | §03 | `pivHi`, `pivLo`, `legCount`, `pushCount`, `lastLeg*` |
| §08 | `STATE` | State classification, grade, transitions, merge | §05 | `state`, `narrowGrade`, `stateChanged`, `isMerge` |
| §09 | `POSITION` | Position band, location, box, three-finger | §08 | `pos`, `location`, `inBox`, `threeFingers` |
| §10 | `EVENT` | All primitive event detectors | §03, §09 | `evElephant*`, `evTail*`, `evColour*`, `evTakeout*`, `evGap*`, `evSnowman` |
| §11 | `SETUP` | Named setup qualification | §08–§10 | `suSurge`, `suIgnite`, `suPullback`, `suOpening*`, `suHidden*`, `suDual`, `warnDeep*` |
| §12 | `QUALITY` | Hard gates, blocker list, additive confluence | §08–§11 | `gatesPass`, `blockMask`, `qScore`, `qTier` |
| §13 | `STOP` | Six trailing references, eligibility, rotation | §07, §10, §15 | `stopLevel`, `stopMethod`, `stopChanged`, `ladderRung` |
| §14 | `EXIT` | Exit-context detection | §07, §08, §15 | `exitReason`, `exitCtx` |
| §15 | `LIFECYCLE` | The finite-state machine | §11–§14 | `lcState`, `lcDir`, `lcEntry`, `lcAdds`, `lcBars` |
| §16 | `VISIBILITY` | Mode × fidelity × priority × spacing resolver | §01, §12 | `showX(...)` helpers |
| §17 | `PLOTS` | Global-scope plots and fills | §04, §16 | — |
| §18 | `DRAW` | Label/line/box creation, ring buffers, recycling | §16 | — |
| §19 | `TABLE` | Dashboard renderer | §08–§15 | — |
| §20 | `DEBUG` | Funnel trace, checklist, counters, claims panel | all | — |
| §21 | `ALERT` | `alertcondition` set + dynamic `alert()` payloads | §10–§15 | — |
| §22 | `META` | Traceability strings, version, build ID | — | — |

---

## 3. TYPES, CONSTANTS AND STATE VARIABLES

### 3.1 Enumeration strategy — a deliberate engineering decision

Pine Script v6 offers user-defined types (`type`). This project uses **UDTs for structured state**
and **named `const int` families for enumerations**, rather than the `enum` keyword.

**Rationale, stated honestly:** no Pine compiler is available in this environment
(`07_QA_REVIEW.md §5`). `const int` families are supported across every Pine version from v4
onward and carry zero compile risk, while `enum` is a newer language feature whose availability
this project cannot verify by execution. Named integer constants with a documented family prefix
provide the same readability and the same exhaustive-switch discipline. This is a
`PLATFORM_SAFEGUARD` choice and is recorded as such.

### 3.2 Constant families

```pine
// ── State ────────────────────────────────────────────────────────────────
const int ST_NARROW  = 0
const int ST_REGULAR = 1
const int ST_WIDE    = 2

// ── Narrow grade (SRC-055/056/057) ───────────────────────────────────────
const int NG_NONE = 0, NG_G1 = 1, NG_G2 = 2, NG_G3 = 3

// ── Direction ────────────────────────────────────────────────────────────
const int DIR_NONE = 0, DIR_LONG = 1, DIR_SHORT = -1

// ── Event rank (drives same-bar priority, VIS §6 R2) ──────────────────────
const int EV_NONE = 0
const int EV_TAKEOUT  = 1
const int EV_COLOUR   = 2
const int EV_TAIL     = 3
const int EV_ELEPHANT = 4
const int EV_COMBO    = 5   // elephant + colour change (SRC-107) — highest

// ── Stop method (display precedence order, §8) ────────────────────────────
const int SM_NONE = 0, SM_INITIAL = 1, SM_PIVOT = 2, SM_FATBAR = 3
const int SM_COLOUR = 4, SM_MA20 = 5, SM_MA8 = 6, SM_BARBYBAR = 7

// ── Lifecycle (§6) ────────────────────────────────────────────────────────
const int LC_IDLE = 0, LC_WATCHING = 1, LC_DEVELOPING = 2, LC_QUALIFIED = 3
const int LC_LONG_ACTIVE = 4, LC_SHORT_ACTIVE = 5
const int LC_EXIT_CONTEXT = 6, LC_INVALIDATED = 7, LC_COMPLETED = 8

// ── Fidelity layer ────────────────────────────────────────────────────────
const int FID_STRICT = 0, FID_APPROX = 1, FID_EXPERIMENTAL = 2

// ── Display mode ──────────────────────────────────────────────────────────
const int MODE_CLEAN = 0, MODE_ANALYSIS = 1, MODE_FULL = 2, MODE_DEBUG = 3

// ── Confirmation status ───────────────────────────────────────────────────
const int CF_CONFIRMED = 0, CF_PROVISIONAL = 1, CF_LATER = 2

// ── Blocker bitmask (§12) ─────────────────────────────────────────────────
const int BLK_NO_EVENT      = 1
const int BLK_INSIDE_STATE  = 2
const int BLK_PLUTO         = 4
const int BLK_AGAINST_20    = 8
const int BLK_DUMB_STOP     = 16
const int BLK_LADDER_SKIP   = 32
const int BLK_THREE_FINGERS = 64
const int BLK_LIFECYCLE     = 128
const int BLK_WARMUP        = 256
const int BLK_HTF_OPPOSED   = 512   // experimental (CON-132)
```

### 3.3 User-defined types

```pine
// One qualified event candidate on one bar.
type EventCandidate
    int    rank       = EV_NONE     // EV_* — drives same-bar priority
    int    dir        = DIR_NONE
    float  refLevel   = na          // body extreme / bar extreme used as the trigger reference
    string label      = ""
    bool   approx     = false       // true if any threshold-dependent test contributed
    int    confirm    = CF_CONFIRMED

// The current virtual methodology lifecycle. Never a real position.
type Lifecycle
    int    state      = LC_IDLE
    int    dir        = DIR_NONE
    int    entryBar   = na
    float  entryRef   = na
    float  initStop   = na
    float  stop       = na
    int    stopMethod = SM_NONE
    int    adds       = 0
    int    pushes     = 0
    int    entryState = ST_REGULAR   // for the narrow→wide exit (SRC-062)
    bool   isOpening  = false        // opening-bell lifecycles bypass CON-028 (SRC-078)
    float  initPeak   = na           // for the new-high right (SRC-173)
    string exitReason = ""

```

**Drawing collections are module-level `var array<...>` globals, not a UDT.** A `DrawPool` type
was considered and rejected: Pine functions cannot assign to global variables, so a pool object
would still need its trim helpers to return counts for the caller to accumulate — the type would
add a layer without removing the constraint. Six plain arrays with a `trimmedCount` accumulator
express the same bound more directly. Recorded here so the code and this document agree.

### 3.4 Persistent state

| Variable | Type | Purpose | Reset condition |
|---|---|---|---|
| `lc` | `Lifecycle` | The active virtual lifecycle | At the **start** of the bar after a terminal transition (see below) |
| `evLabels` `evLines` `lcLabels` `ctxBoxes` `ctxLines` `ctxLabels` | `array<...>` | Bounded drawing collections | Never (FIFO recycled) |
| `trimmedCount` | `int` | Objects recycled, reported on the dashboard | Never |
| `dash` / `dbg` | `table` | Dashboard and Debug panel, each created once | Never |
| `ccRefHi` / `ccRefLo` | `float` | Current colour-change reference levels | On each new opposite-colour bar |
| `lastPivHi` / `lastPivLo` | `float` | Most recent confirmed pivots | On new confirmed pivot |
| `legCount` | `int` | Legs since the state emerged | On state change to NARROW |
| `sessionBar` | `int` | Bar index within the session | On new session |
| `bar1Hi` / `bar1Lo` | `float` | Opening-bell bar-one geometry | On new session |
| `alertFired[]` | `array<int>` | Cooldown bookkeeping per alert family | On cooldown expiry |

---

## 4. PINE SCRIPT V6 ENGINEERING CONSTRAINTS

### 4.1 Declaration

```pine
//@version=6
indicator(
     title             = "Velez Method Master Indicator (Unofficial Study Tool)",
     shorttitle        = "VELEZ MAP",
     overlay           = true,
     max_labels_count  = 500,
     max_lines_count   = 500,
     max_boxes_count   = 500,
     max_bars_back     = 5000)
```

- `overlay = true` — required; every object is price-anchored.
- **No declaration-level `timeframe=`.** A declaration timeframe conflicts with drawing objects
  and would silently change every bar-relative computation. Higher-timeframe context is obtained
  explicitly via `request.security` in one place only (§5.4).
- **No `strategy(...)`, no `strategy.*` calls anywhere.** Verified by the reverse audit.

### 4.2 Language discipline

| Rule | Reason |
|---|---|
| All functions declared before first use | Pine has no forward references |
| `plot()`, `plotshape()`, `fill()`, `alertcondition()`, `bgcolor()` at **global scope only** | Pine requires it; conditional visibility is achieved with `na`/`color.new(...,100)` series, not by conditional calls |
| No `var` inside a function that is called conditionally | Would produce surprising persistence |
| `nz()` on every arithmetic input that can be `na` | Warm-up safety |
| Every division guarded (`d != 0 ? n/d : na`) | Zero-range bars, zero ATR on flat instruments |
| Ternaries used for series selection; `switch` for constant families | Readability |
| No `while` loops; all `for` loops have compile-time-bounded counts | Execution-time safety |
| No undocumented or deprecated APIs | Stability |
| Labels/lines/boxes always created through the `DRAW` module | Guarantees the cap is respected |

### 4.3 Conditional-visibility pattern

Because plots must be global, visibility is series-driven:

```pine
// Correct: one global plot whose value or colour becomes na/transparent.
plot(showMa8 ? sma8 : na, "8 SMA", color = c8, linewidth = 1, style = plot.style_linebr)
```

Never a `plot()` inside an `if`.

### 4.4 `syminfo.mintick` generalisation

The source says "one penny" (SRC-106, SRC-132, SRC-155, SRC-210). One penny is an
instrument-specific increment. Everywhere the source says a penny, the implementation uses
`syminfo.mintick`, and a `tickMultiplier` input (default 1) allows instruments where a single tick
is unhelpfully small. This is a `PLATFORM_SAFEGUARD` translation and is disclosed in the tooltip.

### 4.5 Non-standard chart types

Heikin Ashi, Renko, Kagi, Point & Figure, Line Break and Range bars synthesise OHLC. Every event
detector in this project reads bar geometry directly (SRC-092, SRC-096, SRC-102 all depend on real
open/high/low/close), so synthetic bars distort them severely.

```pine
bool nonStandard = not chart.is_standard
```

When true, a one-time warning label is drawn and the dashboard shows
`CHART TYPE: NON-STANDARD — bar logic distorted`. Detection is **not** disabled — the user is
informed and left in control — but the warning cannot be dismissed by a display mode.

---

## 5. CONFIRMATION AND REPAINT POLICY

### 5.1 The default

**Every actionable output — setup qualification, entry, add, stop change, exit context and state
transition — is evaluated on chart-bar close.** All default alerts are gated by
`barstate.isconfirmed`.

```pine
bool confirmed = barstate.isconfirmed
```

### 5.2 The four confirmation classes

| Class | Applies to | Behaviour |
|---|---|---|
| `CF_CONFIRMED` | Everything derived from the current closed bar and history | Final at close; never changes |
| `CF_PROVISIONAL` | Optional intrabar preview (`allowProvisional`, **off** by default) | Dotted, muted, tagged `PROV`. **Never alerts. Never advances the lifecycle. Never contributes to the quality score.** May disappear before close |
| `CF_LATER` | Everything pivot-derived: CON-055 legs, CON-062 retracement legs, CON-066/067/068 reversal warnings, CON-084 pivot stop, CON-100 pushes | Drawn **on the confirmation bar**, tagged `[c]`. Optionally a thin leader line back to the originating bar — the line makes the delay visible rather than hiding it |
| `HTF-DELAY` | CON-115 higher-timeframe context (off by default) | One-HTF-bar confirmation delay, tagged `⌛` |

### 5.3 What this project will not do

- **No future-bar references.** No `[-n]` offsets, no `barmerge.lookahead_on`.
- **No backpainted entry markers.** An entry marker is never placed on a bar earlier than the bar
  on which its condition became true from closed data.
- **No hidden pivot confirmation.** A pivot-derived object never appears on the originating bar
  without the `[c]` tag.
- **No `barstate.isconfirmed` inside a `request.security()` expression.** That is a well-known
  anti-pattern: `barstate` inside the request refers to the requested context and does not deliver
  higher-timeframe confirmation. Confirmation is achieved by requesting the **previous** HTF value
  (§5.4).

### 5.4 The single higher-timeframe request

```pine
// CON-115 / SRC-248 — optional, OFF by default.
// Requests the PREVIOUS higher-timeframe value with lookahead explicitly off.
[htfSma20, htfSma20Prev] = request.security(
     syminfo.tickerid, htfTf,
     [ta.sma(close, 20)[1], ta.sma(close, 20)[2]],
     lookahead = barmerge.lookahead_off)
```

- Requesting `[1]` and `[2]` means only **closed** higher-timeframe bars are used.
- `lookahead_off` is explicit, not defaulted.
- The cost is a documented delay of up to one higher-timeframe bar. The dashboard shows `⌛` on
  that row so the delay is visible.
- **Multi-timeframe logic is not added anywhere else.** The one HTF feature that exists is
  source-backed (SRC-248, the manual's own pre-open checklist item) and is off by default.

### 5.5 Historical versus real-time equivalence

On historical bars every bar is closed, so `barstate.isconfirmed` is true and the confirmed path is
the only path. In real time the same path executes on the close of each bar. With
`allowProvisional = false` (the default), **historical and real-time rendering of confirmed objects
are identical.** With provisional preview enabled they are not, and the README says so.

---

## 6. VIRTUAL LIFECYCLE STATE MACHINE

> **Naming.** `Lifecycle` models a *virtual methodology lifecycle*. It has no fills, no size, no
> P&L and no broker. Every rendered object carries `⟨v⟩`.

### 6.1 States

| State | Meaning |
|---|---|
| `LC_IDLE` | Nothing of interest; state/position do not support a candidate |
| `LC_WATCHING` | State supports a candidate (narrow, or wide-for-reversion) but position does not yet |
| `LC_DEVELOPING` | State **and** position support a candidate; waiting on an event |
| `LC_QUALIFIED` | Event present, all hard gates passed, a valid stop exists — armed |
| `LC_LONG_ACTIVE` | Virtual long lifecycle running |
| `LC_SHORT_ACTIVE` | Virtual short lifecycle running |
| `LC_EXIT_CONTEXT` | Active, and at least one exit condition has appeared (still running — the source's exits are rights and contexts, not forced closes, SRC-173) |
| `LC_INVALIDATED` | Terminated without the stop being reached (setup broke before or after arming) |
| `LC_COMPLETED` | Terminated at the stop reference |

`ADD_ELIGIBLE` and `STOP_UPDATED` are modelled as **flags/events on an active state**, not as
states. Making them states would produce a machine where "active" is ambiguous; the source treats
adding and stop-rolling as things that happen *while* in a trade (SRC-123, SRC-140), which is a
flag, not a phase.

### 6.2 Transition table

| From | Event | Guard | To | Side effects |
|---|---|---|---|---|
| `IDLE` | state supports a mode | — | `WATCHING` | — |
| `WATCHING` | position ±1 (or ±3 for reversion) | — | `DEVELOPING` | — |
| `WATCHING` | state no longer supports | — | `IDLE` | — |
| `DEVELOPING` | qualifying event | all §8 gates pass **and** a valid stop exists | `QUALIFIED` | compute `entryRef`, `initStop`, `ladderRung` |
| `DEVELOPING` | position leaves the zone | — | `WATCHING` | — |
| `QUALIFIED` | entry trigger (event bar close, or takeout of the event bar's extreme) | `confirmed` | `LONG_ACTIVE` / `SHORT_ACTIVE` | record `entryBar`, `entryState`, `entryRef`, `stop = initStop` |
| `QUALIFIED` | gate fails / state changes | — | `INVALIDATED` | reason recorded |
| `*_ACTIVE` | first colour change in trade direction | `adds == 0` | `*_ACTIVE` | `adds++`, emit `ADD` (SRC-123 — **mandatory**) |
| `*_ACTIVE` | further qualifying event with `near20` | `adds < maxAdds` | `*_ACTIVE` | `adds++` (SRC-126) |
| `*_ACTIVE` | any trailing reference improves | monotonic | `*_ACTIVE` | `stop = rotate(...)`, `stopMethod` updated, emit `STOP MOVED` |
| `*_ACTIVE` | push 3 / new-high right / narrow→wide | — | `EXIT_CONTEXT` | reason recorded; **lifecycle continues** |
| `*_ACTIVE` / `EXIT_CONTEXT` | stop reference broken | — | `COMPLETED` | emit `STOP-OUT` |
| `EXIT_CONTEXT` | exit conditions all clear | — | `*_ACTIVE` | reason cleared |
| `COMPLETED` / `INVALIDATED` | next bar | — | `IDLE` | drawings demoted to residue (VIS §10.2) |

### 6.3 Documented policies

| Question | Policy | Classification |
|---|---|---|
| One or many concurrent lifecycles? | **One.** `maxLifecycles = 1` | `PLATFORM_SAFEGUARD` — source silent (SRC-300) |
| Opposite signal while active? | Ignored for entry purposes; drawn as an event only. `allowReversal` (default **off**) permits immediate flip | `PLATFORM_SAFEGUARD` |
| Add limit? | `maxAdds = 3` | `PLATFORM_SAFEGUARD` — source says adds are mandatory and continued adds are allowed, but never caps them (SRC-123, SRC-126) |
| Re-entry after completion? | Permitted from the next bar; a fresh setup must qualify from scratch | `PLATFORM_SAFEGUARD` |
| Does an exit context force a close? | **No.** The source's exits are a "right, not an obligation" (SRC-173) and a plan (SRC-170) | `EXACT_TRANSLATION` |
| Stop-method precedence when equally protective? | Fixed order `SM_PIVOT > SM_FATBAR > SM_COLOUR > SM_MA20 > SM_MA8 > SM_BARBYBAR` for the **displayed name** only; the *level* is always the max/min | `PLATFORM_SAFEGUARD` (§10.2 A13) |
| Object cleanup? | Terminal transition demotes drawings to residue; ring buffers recycle | `PLATFORM_SAFEGUARD` |

---

## 7. SAME-BAR AMBIGUITY POLICY

OHLC data cannot establish intrabar sequence. Every ambiguous case below is resolved
**deterministically and conservatively**, and every resolution is recorded in Debug with an
`AMBIGUOUS` tag.

| # | Situation | Resolution | Rationale |
|---|---|---|---|
| S1 | Entry trigger and stop reference both touched on the same bar | **Treat as stopped.** Lifecycle → `INVALIDATED`, tagged `SAME-BAR AMBIGUITY` | Risk-first. The optimistic reading cannot be justified from OHLC |
| S2 | Stop reference and a further favourable trailing reference on the same bar | **Stop first.** Evaluate the stop break before rolling | Never allows a roll to rescue a bar that already broke the stop |
| S3 | Entry and an opposite qualifying event on the same bar | **No entry.** Conflicting evidence on one bar is not a signal | Matches SRC-025's spirit — the event must be unambiguous |
| S4 | Add trigger and stop change on the same bar | **Add first, then stop.** The add is an event; the stop is a state derived from all events including that bar | Deterministic ordering |
| S5 | Multiple event candidates on one bar | Highest `EV_*` rank wins the marker; all are recorded in Debug; the combination (SRC-107) is a distinct higher rank, not a tie | Source ranks the combination explicitly |
| S6 | Long and short setups both qualify on one bar | **Neither.** Blocker `CONFLICTING SETUPS` | Conservative |
| S7 | Price gaps **through** the stop reference | Lifecycle → `COMPLETED`; the recorded reference is the **gap open**, flagged `GAP-THROUGH — reference not achievable` | Never claims a level that price never traded |
| S8 | Reversal vs invalidation on one bar | **Invalidation.** A reversal requires a fresh lifecycle on a subsequent bar unless `allowReversal` | Conservative |
| S9 | State transition and entry trigger on one bar | State is evaluated **before** the event (SRC-020's mandated order), so the new state gates the event | The source's own ordering rule |
| S10 | Colour-change reference re-anchors on the same bar it is taken out | The reference used is the one that existed at the **previous** bar's close | Prevents a self-referential trigger |
| S11 | An elephant bar is also the bar that breaks the stop | S2 applies — stop first | Risk-first |

**Statement required by the project brief and reproduced in the README:**
*No historical same-bar sequence rendered by this indicator represents an actual executable order
of events. Where sequence mattered and OHLC could not establish it, the conservative reading was
taken and the bar is tagged `AMBIGUOUS` in Debug Mode.*

---

## 8. PRECEDENCE AND PLATFORM-SAFEGUARD REGISTER

### 8.1 Hard gate order (evaluated in this order; first failure short-circuits)

1. `BLK_WARMUP` — insufficient history (§11)
2. `BLK_INSIDE_STATE` — position 0 (SRC-075)
3. `BLK_NO_EVENT` — no qualifying event (SRC-025)
4. `BLK_AGAINST_20` — direction opposes the 20's slope, when enforced (SRC-224)
5. `BLK_PLUTO` — with-trend entry from position ±3 (SRC-079, SRC-284)
6. `BLK_THREE_FINGERS` — swing-family setups only (SRC-292)
7. `BLK_DUMB_STOP` — computed stop sits inside position one (SRC-077); bypassed for opening-bell (SRC-078)
8. `BLK_LADDER_SKIP` — stop-ladder rung 3 (SRC-197)
9. `BLK_LIFECYCLE` — a lifecycle is already active (platform)
10. `BLK_HTF_OPPOSED` — experimental only (CON-132)

Gates 2–8 are source-derived. Gates 1, 9 and 10 are platform/experimental and are visually
distinguished in the blocker chips.

### 8.2 Complete platform-safeguard register

| ID | Safeguard | Reason |
|---|---|---|
| PS-01 | `maxLifecycles = 1` | Source silent on concurrency (SRC-300) |
| PS-02 | `maxAdds = 3` | Source never caps adds |
| PS-03 | Stop-method display tie-break order | Source silent (§10.2 A13) |
| PS-04 | `stopMethodSet = ALL` off-timeframe | Source names sets only for swing and opening bell (§10.2 A12) |
| PS-05 | `syminfo.mintick` for "one penny" | Instrument generalisation |
| PS-06 | Doji excluded as colour reference and trigger | Source silent |
| PS-07 | Zero-range bar guards | Division safety |
| PS-08 | Warm-up suppression | `na` safety |
| PS-09 | Ring-buffer caps and trimming | Pine object limits |
| PS-10 | Non-standard chart-type warning | Bar-geometry integrity |
| PS-11 | Same-bar conservative resolutions S1–S11 | OHLC cannot establish sequence |
| PS-12 | `const int` families instead of `enum` | Compile-risk avoidance in an environment without a compiler |
| PS-13 | Alert cooldown / de-duplication | Alert noise |
| PS-14 | Provisional preview excluded from scoring and alerts | Repaint containment |

---

## 9. ALERTS

### 9.1 Principles

- **Transitions only, never states.** `alertcondition` fires on the bar a condition *becomes*
  true, never while it remains true. Every alert boolean is wrapped in a rising-edge helper.
- **Confirmed by default.** `alertMode = Confirmed` gates on `barstate.isconfirmed`. A
  `Provisional` mode exists, is off by default, and is labelled as repaint-capable in its tooltip.
- **Per-event enablement.** Every alert family has its own toggle.
- **Cooldown.** `alertCooldownBars` (default 3) suppresses repeat firings of the same family.

### 9.2 Static alert families (`alertcondition`, global scope)

| ID | Family | Fires when |
|---|---|---|
| `AL-01` | State change | `state != state[1]` |
| `AL-02` | Narrow grade 1 formed | `narrowGrade == NG_G1` on a rising edge |
| `AL-03` | Elephant bar | rising edge, either direction |
| `AL-04` | Elephant + colour change | rising edge |
| `AL-05` | Tail bar | rising edge |
| `AL-06` | Colour change | rising edge |
| `AL-07` | Red/green bar takeout | rising edge |
| `AL-08` | Gap-fill bar-two trigger | rising edge |
| `AL-09` | Surge off the 200 | rising edge |
| `AL-10` | Igniting swing | rising edge |
| `AL-11` | Pullback swing | rising edge |
| `AL-12` | Opening-bell mark set | bar one complete and qualified |
| `AL-13` | Opening-bell entry trigger | mark crossed |
| `AL-14` | Opening-bell add trigger | little red bar takeout |
| `AL-15` | Hidden green / hidden red | rising edge |
| `AL-16` | Dual space reversal | rising edge |
| `AL-17` | Deep drop | rising edge |
| `AL-18` | Market Law Four candidate | rising edge |
| `AL-19` | Virtual entry | lifecycle → active |
| `AL-20` | Virtual add | `adds` increments |
| `AL-21` | Virtual stop moved | `stopLevel` changes |
| `AL-22` | Virtual stop-out | lifecycle → completed |
| `AL-23` | Exit context: third push | rising edge |
| `AL-24` | Exit context: narrow→wide | rising edge |
| `AL-25` | Space extreme / pare context | rising edge |
| `AL-26` | Quality tier reached `HIGH QUALITY`+ | rising edge |
| `AL-27` | Blocked candidate (diagnostic) | a candidate failed a gate |

### 9.3 Dynamic payload (`alert()`)

Used only for a single consolidated `Any qualifying event` alert where a rich payload is useful.
Message template:

```
VELEZ MAP | {sym} {tf} | {event} | dir={L|S} | px={price}
state={NARROW G1~} pos={+1} trend={20 up}
setup={SURGE OFF 200~} quality={HIGH QUALITY [impl]}
stop={4412.25 FAT BAR} fidelity={SOURCE+APPROX} confirm={CONFIRMED}
NOT A RECOMMENDATION — study tool, no execution implied.
```

Fields included: event ID, direction, price, state, position, setup, quality tier, current stop
reference and method, fidelity classification, and confirmation status. The final line is **not
optional** and cannot be disabled.

### 9.4 TradingView setup requirements (reproduced in the README)

1. `alertcondition` alerts appear individually in TradingView's *Condition* dropdown after adding
   the indicator to the chart.
2. The dynamic `alert()` message requires creating an alert on **"Any alert() function call"**.
3. **Alerts must be recreated after changing the script or any alert-affecting input.** TradingView
   binds an alert to the script version and settings that existed when it was created.
4. Recommended trigger: *Once Per Bar Close*. With `alertMode = Confirmed`, *Once Per Bar* would
   still only fire at close, but *Once Per Bar Close* makes the intent explicit.

### 9.5 What alerts never say

No alert asserts that a trade should be taken, that a level will be reached, that a probability
applies, or that any execution has occurred.

---

## 10. RESOURCE BUDGET

### 10.1 Declared limits

| Resource | Pine limit | Declared | Design budget | Headroom |
|---|---|---|---|---|
| Labels | 500 | 500 | **170** | 66% |
| Lines | 500 | 500 | **80** | 84% |
| Boxes | 500 | 500 | **60** | 88% |
| Tables | practical limit low | 2 (`var`, created once) | 2 | — |
| Plots + plotshapes | ~64 | **32** | 32 | 50% |
| `max_bars_back` | 5000 | 5000 | — | — |

### 10.2 Plot inventory (32 of ~64 slots)

**17 `plot()` calls**

| # | Plot | Purpose |
|---|---|---|
| 1–2 | `sma20`, `sma200` | Foundation. Source's own convention: thin = the 20, thick = the 200 (p.2) |
| 3 | `sma8` | Situational; `na` unless the trail has escalated |
| 4–7 | `ma20Hi/Lo`, `ma200Hi/Lo` | Band edges, transparent, consumed by two `fill()` calls |
| 8 | `lc.stop` (`style_linebr`) | The active virtual stop — visible in every mode |
| 9 | `lc.entryRef` (`style_linebr`) | Entry reference, held flat for the lifecycle's life |
| 10 | `lc.initStop` (`style_linebr`) | Frozen initial stop; hidden in Clean Mode |
| 11–17 | `qScore`, `spaceRank`, `pos`, `gapN`, `lc.pushes`, `legCount`, `expDensity` | `display.data_window` only — the anti-clutter pressure valve (VIS §6 R8) |

The three lifecycle references are plots rather than `line` objects so they cost no drawing
budget, extend automatically, and cannot leak past the lifecycle's end.

**15 `plotshape()` calls** — event and lifecycle markers. Shapes are used rather than labels
wherever text is not required, because `plotshape` consumes no label budget. That is precisely
what allows Full Velez Mode to render every supported event inside the bounded window.

Two `fill()` calls and two `bgcolor()` calls consume no additional plot slots.

### 10.3 Per-family object caps (as implemented)

| Array | Cap | Contents | Recycling |
|---|---|---|---|
| `evLabels` | 120 | Aggregated event labels (Analysis+) | FIFO |
| `evLines` | 60 | Colour-change body-extreme reference lines | FIFO |
| `lcLabels` | 30 | Stop-method change ticks | FIFO |
| `ctxBoxes` | 60 | Position zone boxes | FIFO |
| `ctxLines` | 20 | Retracement ladder lines | FIFO |
| `ctxLabels` | 20 | Retracement ladder labels | FIFO |

**Total worst case: 170 labels + 80 lines + 60 boxes**, plus the one-off non-standard-chart
warning label. Full Velez Mode raises `historyCap`, but the per-family caps are the binding
constraint, so these totals hold in every mode. `trimmedCount` is reported on the dashboard
`HISTORY` row and in the Debug resource counter, so recycling is never silent.

### 10.4 Loop and computation budget

- No `while` loops.
- The only `for` loops are: (a) the `priceSandwiched` containment check, bounded by `slopeLen`
  (default 10, max 50); (b) ring-buffer trimming, bounded by one element per bar; (c) the blocker
  bitmask decode, bounded at 10 iterations. **Worst case ≈ 61 iterations per bar.**
- `ta.percentrank` over `spaceLookback` (default 250, max 2000) is a single built-in call.
- One `request.security` call, and only when `useHtf` is enabled.

---

## 11. ERROR HANDLING, GUARDS AND WARM-UP

### 11.1 Warm-up

```pine
int warmupNeeded = math.max(200, slopeLen + 1, elephantLookback + 1, ATR_LEN + 1)
                 + (useSpaceRank ? spaceLookback : 0)
bool ready = bar_index >= warmupNeeded and not na(sma200) and not na(atrUnit) and atrUnit > 0
```

While `not ready`: every detector returns false, no drawing is created, no alert can fire, and the
dashboard shows `WARMING UP — n / m bars`. **Warm-up is explicit and visible, not silent.**

The 200-bar floor is a hard requirement: the 200 SMA is the source's non-negotiable second average
(SRC-030), so a chart with fewer than 200 bars cannot express the method at all. The dashboard says
so rather than showing a half-formed picture.

### 11.2 Guard inventory

| Condition | Guard | Behaviour |
|---|---|---|
| `na` averages | `not na(sma200)` in `ready` | Suppressed |
| `atrUnit == 0` or `na` (flat/illiquid instrument) | Explicit check | All ATR-normalised tests return false; dashboard shows `ATR UNAVAILABLE` |
| Zero-range bar (`high == low`) | `rng > 0` before any ratio | Bar cannot be an elephant, tail or snowman |
| Doji (`close == open`) | `isDoji` | Not a colour reference, not a trigger bar (PS-06) |
| Division by zero | Every ratio wrapped | Returns `na`, propagates to "not detected" |
| Gap-through stop | Explicit S7 handling | `COMPLETED` with `GAP-THROUGH` flag |
| Illiquid / single-print bars | `rng > 0` plus the elephant `rng ≥ atrUnit` floor in LocalMax mode | Suppressed |
| No session information (`session.ismarket` unavailable, e.g. crypto/forex 24h) | `sessionAvailable` check | Opening-bell and gap-fill modules disable themselves and the dashboard says `SESSION MODULES: N/A ON THIS SYMBOL` |
| Non-standard chart type | `not chart.is_standard` | Persistent warning (§4.5) |
| Insufficient history for `percentrank` | Folded into `ready` | `spaceRank` reported as `n/a` |
| `maxLossPerUnit` unset | Checklist shows `UNSET`; the stop ladder reports rung 1 as "base" without a fit test | Never silently assumes a value |

### 11.3 Failure philosophy

Every guard produces a **visible** degraded state, never a silent one. A user must never be able to
look at this indicator and be unable to tell that something was suppressed.

---

## 12. INPUT ORGANISATION

19 groups, in this order. Every threshold input carries a tooltip that states its
classification (`SOURCE-BACKED` / `APPROXIMATION` / `EXPERIMENTAL` / `PLATFORM`) and, for
approximations, the SRC reference for the concept and a note that the number is added.

| # | Group | Contents | Signal-affecting? |
|---|---|---|---|
| 1 | `GENERAL` | Theme, emoji/ASCII, `tickMultiplier`, `useEmoji` | display |
| 2 | `SOURCE FIDELITY` | `fidelity` (Strict / Source+Approx / +Experimental), `showApproxMarks`, `enforce20Direction`, `enforceDumbStop` | **yes** |
| 3 | `DISPLAY MODE` | `mode` (Clean / Analysis / Full / Debug), `historyCap`, `minLabelSpacing`, `labelOffsetAtr` | display |
| 4 | `MOVING AVERAGES` | lengths (locked to 20/200/8 with a note), `bandAtrMult`, `showBands`, `showMa8` | **yes** (band) |
| 5 | `MARKET STATE` | `stateBasis`, `narrowMult`, `wideMult`, `mergeMult`, `slopeLen`, `flatThresh`, `sleepyBars` | **yes** |
| 6 | `POSITION` | `posBasis`, `p1Max`, `p2Max`, `boxGapMult`, `boxPriceMult`, `zoneBoxBars` | **yes** |
| 7 | `ELEPHANT BARS` | `elephantModel`, `elephantLookback`, `elephantRangeMult`, `elephantBodyFrac`, `exhaustLegs`, `emergenceLookback` | **yes** |
| 8 | `TAIL BARS` | `tailMinFrac`, `tailBodyMaxFrac`, `tailRequireDominance`, `showSnowman` | **yes** |
| 9 | `COLOUR CHANGES` | `ccRefModel`, `requireTriggerColour`, `showCcRefLine` | **yes** |
| 10 | `GAPS` | `enableGapFill`, `gapMinAtr` | **yes** |
| 11 | `SPECIALISED SETUPS` | per-setup toggles; `clearLookback`, `originTolBars`, `resetBars`, `resetDriftMult`, `dualSpaceMult`, `strongMoveAtr`, `requireMonotonic`, `scalpTradePct`, `deepDropPct`, `mlfSignifMult`, `openWindowBars`, `enableHidden` | **yes** |
| 12 | `ENTRY SIGNALS` | `near20Model`, `near20Mult`, `abProximityBars`, `abModel`, `allowReversal` | **yes** |
| 13 | `TRADE MANAGEMENT` | `stopMethodSet`, `pivLen`, `accelLen`, `accelMult`, `barByBarSpaceMult`, `swingStopRef`, `pullbackStopRef`, `maxAdds`, `maxLossPerUnit` | **yes** |
| 14 | `PROFIT MANAGEMENT` | `push2LargeMult`, `showPushes`, `showOdds`, `scalpTargetPct` | **yes** |
| 15 | `VISUALS` | Colours, ribbon on/off, background on/off + opacity cap, zone boxes, event marker toggles, `colorBlindSafe` | display |
| 16 | `DASHBOARD` | `showDashboard`, position (9), size (3), per-row toggles | display |
| 17 | `ALERTS` | `alertMode`, per-family toggles, `alertCooldownBars` | alerts |
| 18 | `ADVANCED` | `ATR_LEN`, `spaceLookback`, `spacePct`, `warmupOverride`, `useHtf`, `htfTf`, `allowProvisional` | **yes** |
| 19 | `DEBUG` | `debugBarOffset`, `showChecklist`, `showSourceClaims`, `showCounters`, `showBlockers` | display |

**Display-only controls are never mixed into signal groups.** Groups 1, 3, 15, 16 and 19 contain
no signal-affecting input. Groups 2, 4–14 and 18 do, and every one of them is tooltipped with its
provenance.

**Tooltip template for an approximation input:**

> `[APPROXIMATION — CON-030 / SRC-092, p.12]` The source says an elephant bar is "visibly larger
> and taller than the bars around it" and gives no ratio. This multiple is added by this
> implementation, not taken from the source. See `02_CONCEPT_TO_CODE_MAP.md §11 W5`.

**No input exists purely to look configurable.** Every input in the list changes either a
documented approximation, a documented platform policy, or what is drawn.

---

## 13. TRACEABILITY METADATA

### 13.1 In-code comment convention

Every material block carries a one-line traceability comment:

```pine
// [RULE-014 | CON-034 | SRC-102/105/106 p.13 | DIRECT / EXACT_TRANSLATION]
// Colour change: one colour takes out the BODY extreme of the opposite-coloured bar.
```

Fields, in order: `RULE-###` (implementation rule) · `CON-###` (concept) · `SRC-###` + page ·
source classification / implementation classification.

### 13.2 RULE-ID allocation

| Range | Domain |
|---|---|
| `RULE-001…019` | Base data, guards, MA foundation |
| `RULE-020…039` | State |
| `RULE-040…054` | Position |
| `RULE-055…079` | Events |
| `RULE-080…109` | Setups |
| `RULE-110…129` | Gates and quality |
| `RULE-130…154` | Stops |
| `RULE-155…169` | Exits |
| `RULE-170…189` | Lifecycle |
| `RULE-190…209` | Visibility, drawing, dashboard |
| `RULE-210…229` | Alerts |
| `RULE-230…239` | Debug and metadata |

### 13.3 Runtime traceability

- Debug Mode prints the `CON-###` / `SRC-###` for every gate in the funnel trace.
- The dashboard's `APPROX ACTIVE` row names every approximation contributing to the current bar.
- The script header carries a `BUILD` string and a pointer to this repository's documents.

### 13.4 The rule

**No material code behaviour may exist without a traceability ID.** The reverse audit in
`06_RULE_COVERAGE_AUDIT.md §3` walks every constant, formula, plot, label, line, box, table field,
alert and state transition in the finished script and assigns each one an ID — or removes it.
