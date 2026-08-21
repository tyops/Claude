# 03 — VISUAL DESIGN SPECIFICATION
### A restrained, methodology-focused TradingView workspace

> **Design thesis.** The chart should read as *one instrument panel*, not as a collection of
> alerts. A trader glancing at it should be able to answer, in under two seconds and without
> clicking anything: *what state am I in, where is price relative to it, did anything happen, and
> if I were in a trade, where is my protection right now?*
>
> Everything below serves that. Nothing below exists because it looks impressive.

---

## TABLE OF CONTENTS

1. [Design principles](#1-design-principles)
2. [The methodology story the chart must tell](#2-the-methodology-story-the-chart-must-tell)
3. [Visual layers](#3-visual-layers)
4. [The five visual layers in detail](#4-the-five-visual-layers-in-detail)
5. [Display modes](#5-display-modes)
6. [Anti-clutter heuristic](#6-anti-clutter-heuristic)
7. [Signal-quality visualisation](#7-signal-quality-visualisation)
8. [State and position visualisation](#8-state-and-position-visualisation)
9. [Event grammar](#9-event-grammar)
10. [Virtual trade-lifecycle visualisation](#10-virtual-trade-lifecycle-visualisation)
11. [The dashboard](#11-the-dashboard)
12. [Debug Mode](#12-debug-mode)
13. [Colour, contrast and accessibility](#13-colour-contrast-and-accessibility)
14. [Typography and glyph inventory](#14-typography-and-glyph-inventory)
15. [What is deliberately not drawn](#15-what-is-deliberately-not-drawn)

---

## 1. DESIGN PRINCIPLES

| # | Principle | Consequence |
|---|---|---|
| P1 | **Candles stay readable.** | No opaque backgrounds, no full-height zones by default, no bar colouring that destroys the green/red distinction the method depends on (SRC-102 needs colour to be legible) |
| P2 | **One idea, one place.** | State appears in the dashboard and optionally as a thin ribbon — never also as a background *and* a box *and* a label |
| P3 | **Density scales with intent, not with capability.** | Clean Mode shows less because the user asked for less, not because the indicator can do less |
| P4 | **Approximate things must look approximate.** | The `~` marker is part of the label, not a footnote |
| P5 | **Provisional things must look provisional.** | Dotted outline, reduced opacity, `PROV` tag; never the same weight as a confirmed marker |
| P6 | **Confirmed-later things must not lie about when they were known.** | Pivot-derived marks are drawn on the confirmation bar with a `[c]` tag and, where useful, a leader line back to the originating bar |
| P7 | **The chart never scores in secret.** | Every quality tier is decomposable in Debug Mode into the exact factors that produced it |
| P8 | **Meaning never depends on colour alone.** | Every colour-coded state also carries a shape, a glyph or text |
| P9 | **Nothing implies execution.** | The word "virtual" or the `⟨v⟩` marker appears on every lifecycle object |
| P10 | **Bounded by construction.** | Every drawing collection has a hard cap; the indicator degrades gracefully rather than silently dropping the newest object |

---

## 2. THE METHODOLOGY STORY THE CHART MUST TELL

```
STATE → POSITION → EVENT → SETUP QUALITY → ENTRY → ADD → ACTIVE STOP → PROFIT/EXIT CONTEXT
```

This is the manual's own funnel (SRC-020, C1) extended through the management rules. The chart
tells it **through placement and persistence, not through a drawn flowchart**:

| Stage | Where it lives on screen | Why there |
|---|---|---|
| STATE | Dashboard row 1 + optional 3-px ribbon at the top of the pane | Ambient, always-on, never competes with price |
| POSITION | Dashboard row 2 + optional zone boxes anchored at the right edge | Positional by nature — it belongs next to the price axis |
| EVENT | A marker **on the bar** | Events are bar-local facts |
| SETUP QUALITY | A single chip attached to the qualifying event marker | It qualifies the event, so it travels with it |
| ENTRY | A horizontal reference line + marker at the entry bar | Persistent while the lifecycle lives |
| ADD | A smaller marker on the add bar, same line family | Visually subordinate to the entry |
| ACTIVE STOP | One live line that steps as it rolls, labelled with the current method | The single most operationally important object |
| EXIT CONTEXT | A marker plus a dashboard row | Context, not an instruction |

**Explicitly rejected:** drawing the funnel as connected boxes across the chart. It would occupy
the price plane permanently to convey information that changes once per bar and belongs in a table.

---

## 3. VISUAL LAYERS

| Layer | Name | Contents | Clean | Analysis | Full | Debug |
|---|---|---|---|---|---|---|
| L1 | **Foundation** | 20 SMA, 200 SMA, MA bands, 8 SMA (situational) | ● | ● | ● | ● |
| L2 | **Context** | State ribbon, position badge, trend, warnings, leg/push context | ◐ | ● | ● | ● |
| L3 | **Qualified event** | Elephant, tail, colour change, gap-fill, takeout markers | ◐ | ● | ● | ● |
| L4 | **Setup & lifecycle** | Developing setup, entry, add, initial stop, trailing stop, stop-method change, exit context | ◐ | ● | ● | ● |
| L5 | **Diagnostics** | Pass/fail reasoning, blockers, approximation status, rule IDs, confirmation status | ○ | ○ | ○ | ● |

● full · ◐ high-priority subset only · ○ hidden

---

## 4. THE FIVE VISUAL LAYERS IN DETAIL

### L1 · Foundation

| Object | Spec | Source |
|---|---|---|
| 20 SMA | 1 px solid, accent-neutral | SRC-030/031 |
| 200 SMA | 2 px solid, heavier weight | SRC-030/031 — the manual's own convention (p.2: "Thin line the 20 SMA · Thick line the 200 SMA") |
| MA bands | Translucent fill ±`bandAtrMult × ATR` around each average, ~8% opacity | SRC-042/003 — the manual renders averages as bands for exactly this reason |
| 8 SMA | 1 px dashed, **hidden unless CON-086 has escalated** | SRC-033 — the 8 is situational, so it should not be permanently on the chart |

The band is the single most important foundation decision. It exists because the source repeatedly
insists an average is a zone (SRC-042), and because a visible band stops the reader treating a
one-tick penetration as a break. The manual itself adopts this rendering (SRC-003), and its
colophon (p.71) restates it.

### L2 · Context

| Object | Spec | Notes |
|---|---|---|
| State wash ("ribbon") | Pane-wide tint at **6% opacity**; hue by state | **Pine limitation, stated rather than glossed:** a script cannot anchor a fixed-height strip to the top of the price pane, because the visible price range is not exposed to Pine. The intended 3 px ribbon is therefore realised as a very light wash. Because a wash cannot carry a *pattern*, the narrow **grade is carried by the dashboard text (`NARROW G1/G2/G3`), not by the wash** — so P8 is still satisfied, just in a different place |
| Position badge | Dashboard `POSITION` row + the right-anchored zone-box text (`POSITION 1 ~` / `POSITION −1 ~`) | **Delivered through the dashboard and zone boxes, not as a separate chart label.** Keeping it off the price plane is the point of §2 |
| Trend chip | Dashboard `TREND` row: `20 up` / `20 down` / `20 flat` + `200 flat/sloping` | Drives the T2 filter |
| Warning chips | Dashboard `POSITION` row (`(PLUTO LAND)`) and `EXTENSION` row (`EXTREME ~`, `3 FINGERS ~`) | Only when active; warning-coloured **and** worded (P8) |
| Leg / push context | Dashboard `LEG / PUSH` row; push flag markers on the chart | Push markers and the dashboard value both carry `[c]` |

### L3 · Qualified event

Markers sit **on the bar**. Placement alternates above/below by direction: bullish events below
the bar, bearish above — matching how a reader scans for support and resistance.

Offset from the bar is `labelOffsetAtr × ATR` (default 0.5) so markers scale with volatility rather
than with zoom.

### L4 · Setup and lifecycle

Drawn only for the **active or most recent** virtual lifecycle (see §10). Historical lifecycles
leave a compact residue: entry marker, exit marker, and nothing else.

### L5 · Diagnostics

Debug Mode only. See §12.

---

## 5. DISPLAY MODES

**Display modes change visibility and density only. They never change which signals exist, when
they fire, or what the alerts do.** That is controlled solely by the fidelity layer
(`02_CONCEPT_TO_CODE_MAP.md §14`).

### 5.1 Clean Mode *(default)*

**Intent:** an elegant working chart a person can watch all day.

| Shown | Hidden |
|---|---|
| 20 SMA, 200 SMA, bands | 8 SMA unless escalated |
| Dashboard (compact, 8 rows) | Position zone boxes |
| State ribbon | Event markers below `SETUP` quality |
| Confirmed entries, adds and exits of the **active** lifecycle | Historical lifecycle stops |
| The **active** virtual stop line only | Retracement ladder |
| Elephant + colour-change combinations; elephant bars at `HIGH QUALITY`+ | Snowman diagnostics, blockers, rule IDs |
| Warnings via the dashboard rows | Zone boxes, colour-change reference lines |

**Density budget:** Clean Mode creates **no text labels at all** — every marker is a `plotshape`,
and all context is delivered by the dashboard. Live drawing objects in Clean Mode: 0, plus the
one-off non-standard-chart warning. This is enforced structurally (`not modeClean` gates the
label-creation block), not by convention.

### 5.2 Analysis Mode

Adds interpretation without adding history.

Adds: aggregated event text labels (with the confluence tier appended) · colour-change body-extreme
reference lines · position zone boxes · stop-method change ticks · all event classifications
including tails, colour changes and takeouts.

**Density budget:** one aggregated label per bar at most, spaced by `minLabelGap` (default 3);
capped at 120 event labels, 60 reference lines, 30 stop ticks, 60 zone boxes.

### 5.3 Full Velez Mode

**Every supported detected event within a bounded recent window.**

- The window is `historyCap` bars (default 300, max 1000) — **not** the whole chart.
- The dashboard shows `HISTORY: last 300 bars` so the bound is never mistaken for completeness.
- When the number of qualifying objects exceeds the cap, the **oldest** are recycled and the
  dashboard shows `TRIMMED: n`.

**This mode does not and cannot render unbounded history.** Pine caps labels, lines and boxes at
500 each; the design treats that as a first-class constraint rather than something to discover at
runtime.

Adds over Analysis: all events regardless of confluence tier · the retracement ladder with each
level carrying its **attributed** source claim · smaller label text so density stays readable.

**Density budget:** 170 labels + 80 lines + 60 boxes worst case — 66%/84%/88% headroom under
Pine's 500-per-type caps. See `04_INDICATOR_ARCHITECTURE.md §10.3`. `trimmedCount` is surfaced on
the dashboard `HISTORY` row, so recycling is visible rather than silent.

### 5.4 Debug Mode

**Intent:** answer "why did / didn't this fire?" for a bounded window.

Shows, for the **latest bar** and for a selectable **inspected bar** (via a bar-offset input):

- Full funnel trace: state (+grade) → position → event → quality → gates
- Each gate as `PASS` / `FAIL` with the SRC and CON reference that defines it
- Every blocking reason, ordered
- Which approximations were active and the value each threshold compared against
- Confirmation status of every displayed object (`CONFIRMED` / `PROVISIONAL` / `CONFIRMED-LATER`)
- The live checklist (SRC-028/029/158) with computed values
- The attributed source-claims panel (CON-121)
- Resource counters: labels / lines / boxes in use versus cap

Debug never emits alerts and never changes detection.

---

## 6. ANTI-CLUTTER HEURISTIC

**This specification does not promise that labels never visually overlap. Pine Script has no
access to rendered pixel geometry, so screen-space collision avoidance is not achievable.** What
follows is a documented, deterministic heuristic that keeps density low enough that overlap is
rare, and is honest that it is a heuristic.

### R1 — Same-bar aggregation
At most **one label per side of a bar**. When several events qualify on one bar they are merged
into a single compact label:

```
ELEPHANT + CHANGE ~        (not two stacked labels)
▲ ENTRY · SETUP [impl]     (entry and quality in one)
```

### R2 — Priority ranking
When aggregation is not possible, the highest-priority item wins the slot and the rest are
suppressed (and listed in Debug, so nothing is lost silently).

| Rank | Item |
|---|---|
| 1 | Lifecycle transitions: `ENTRY`, `ADD`, `STOP-OUT`, `EXIT CONTEXT`, `INVALIDATED` |
| 2 | `ELEPHANT + CHANGE` (SRC-107) |
| 3 | `ELEPHANT` (igniting / exhaustion variants) |
| 4 | Tail bars |
| 5 | Colour changes |
| 6 | Red/green bar takeouts |
| 7 | State transitions |
| 8 | Leg / reset / push context |
| 9 | Warning chips |
| 10 | Diagnostics |

### R3 — Minimum bar spacing
A label of a given family may not print within `minLabelSpacing` bars of the previous label of the
same family (default 3; 8 in Clean Mode). Suppressed items are counted and the count is shown in
Debug.

### R4 — ATR-scaled offsets
Vertical offset is `labelOffsetAtr × ATR`, never a fixed price or tick amount, so spacing survives
zoom and instrument changes.

### R5 — Alternating placement
Consecutive same-family labels alternate between a near offset and a far offset (`1×` and `1.8×`
the base) to reduce stacking in clusters.

### R6 — Clean-mode suppression
In Clean Mode, anything below quality `SETUP` and every diagnostic is suppressed entirely.

### R7 — Bounded retention
Ring buffers with hard caps per object family (§ `04_INDICATOR_ARCHITECTURE.md §10`). Oldest out,
newest in. The dashboard reports trimming.

### R8 — Off-chart overflow
Detail that does not earn chart space goes to the **dashboard** or to **Data Window plots**
(`plot(..., display = display.data_window)`) — quality score, space rank, gap in ATR, push count,
bars in lifecycle. This is the pressure valve that lets the chart stay quiet.

---

## 7. SIGNAL-QUALITY VISUALISATION

Tiers: `WATCH` · `DEVELOPING` · `SETUP` · `HIGH QUALITY` · `ELITE SETUP`.

**Every rendering of a tier carries the `[impl]` marker**, and the dashboard carries a permanent
row reading `CONFLUENCE (impl layer)`. The full weight table lives in
`02_CONCEPT_TO_CODE_MAP.md §11 W23` and is reproduced in Debug Mode.

| Tier | Chip | Non-colour cue | Clean Mode |
|---|---|---|---|
| `WATCH` | outline only | 1 pip `·` | hidden |
| `DEVELOPING` | outline | 2 pips `··` | hidden |
| `SETUP` | filled, light | 3 pips `···` | shown |
| `HIGH QUALITY` | filled | 4 pips `····` | shown |
| `ELITE SETUP` | filled + border | 5 pips `·····` | shown |

The pip count means the tier is legible in greyscale and to a colour-blind reader without reading
the text (P8).

**The score never gates.** A candidate that passes the source's own gates but scores `WATCH` is
still a valid candidate under the method — it is simply low-confluence. This is stated in the
dashboard tooltip so a low tier is never read as a rejection.

---

## 8. STATE AND POSITION VISUALISATION

### 8.1 Options evaluated

| Option | Readability | Information density | Candle safety | Verdict |
|---|---|---|---|---|
| Heavy background tint by state | High at a glance | Low | **Poor** — washes out candle colour, which the method depends on | **Rejected as default** (available as `showBg`, transparency floor 88) |
| Thin ribbon at the top of the pane | High | Medium (hue + pattern) | Excellent | **Selected in principle, but NOT achievable in Pine** — see §4 L2. Delivered as a 6%-opacity wash instead, with the grade moved to the dashboard |
| Compact dashboard row | High | **Highest** — carries grade, gap value, mode | Perfect | **Selected** |
| Zone boxes for all seven positions, drawn historically | Medium | High | Poor — seven boxes across history is exactly the clutter this design avoids | **Rejected** |
| Position zone boxes anchored at the right edge only | High | Medium | Good | **Selected**, Analysis+ |
| Lightweight position badge at the last bar | High | Low | Excellent | **Selected** |

### 8.2 Chosen default composition

- **State:** dashboard row (authoritative — carries the grade, the measured gap and the `~`
  marker) **+** the 6%-opacity wash (ambient). Wash hue = state; **grade lives in the dashboard**,
  since a wash cannot carry a pattern.
- **Position:** dashboard row **+** zone boxes from Analysis Mode up, drawn only over the last
  `zoneBoxBars` bars (default 30) and anchored to the right edge.
- **Never both a wash and a background tint.** Enabling `showBg` disables the wash in code, not
  merely by convention.

### 8.3 Label discipline

State, position and warning labels are **≤ 18 characters**. No prose. Explanation lives in the
tooltip and in Debug. Examples of what is *not* permitted on the chart:

> ~~"Narrow state grade 1 detected — all three items flat, this is the creme de la creme"~~

Rendered instead as: ribbon solid + dashboard `STATE: NARROW G1 ~`, with the full source phrasing
available in Debug.

---

## 9. EVENT GRAMMAR

A restrained, systematic vocabulary. **Not every large bar is loud.**

| Event | Marker | Bar treatment | Text | Modes |
|---|---|---|---|---|
| Elephant (unclassified) | Small triangle, direction-oriented | Thin outline on the bar body | `ELEPHANT ~` | Analysis+ (Clean only at `HIGH QUALITY`+) |
| Igniting elephant | Triangle + upward tick | Outline **plus** a subtle body fill | `IGNITING ELEPHANT ~` | Analysis+ |
| Exhaustion elephant | Triangle + a bar across the apex | Outline only, **muted** | `EXHAUSTION ELEPHANT ~` | Analysis+ |
| Bottoming tail | Small circle below the bar | Tail segment emphasised with a 2 px line | `BOTTOMING TAIL` | Analysis+ |
| Topping tail | Small circle above | Same, above | `TOPPING TAIL` | Analysis+ |
| Colour change | Diamond | **Dashed reference line at the marked body extreme** | `COLOUR CHANGE` | Analysis+ |
| Elephant + change | Diamond inside triangle | Outline + fill + reference line | `ELEPHANT + CHANGE ~` | **Clean+** |
| Red/green bar takeout | Small chevron | Dashed line at the isolated bar's extreme | `RED BAR TAKEOUT` | Analysis+ |
| Gap fill | Ghost box spanning the gap | — | `GAP FILL: BAR ONE` | Analysis+ |
| Snowman (near-miss) | Hollow circle, muted | — | `SNOWMAN (NOT A TAIL)` | **Debug only** |

**Restraint rules**
- Bar outlining is 1 px and uses the bar's own direction hue at reduced saturation — it must never
  overpower the green/red the method reads from.
- Only the combination event (SRC-107, the source's own "more powerful than either alone") gets
  both an outline and a fill.
- Exhaustion elephants are drawn **more quietly** than igniting ones, because the source's point
  is that they are a reason not to act (SRC-235, SRC-287).
- The colour-change reference line is the single most useful event visual in the whole design: it
  shows the *body* extreme (SRC-105, "ignore the tails, use the body"), which is exactly the detail
  the manual says people get wrong.

---

## 10. VIRTUAL TRADE-LIFECYCLE VISUALISATION

> **Naming, enforced everywhere.** This is a **virtual methodology lifecycle**. Not a trade, not a
> position, not a fill, not a backtest. Every lifecycle object carries the `⟨v⟩` (ASCII `(v)`)
> marker, and the dashboard row reads `VIRTUAL LIFECYCLE`.

### 10.1 What is drawn for the active lifecycle

| Object | Implemented as | Spec |
|---|---|---|
| Entry marker | `plotshape` label | `ENTRY` label above/below the entry bar |
| Entry reference | `plot` (`style_linebr`) | Flat 1 px line held at `lc.entryRef` for the lifecycle's life |
| Initial stop reference | `plot` (`style_linebr`) | Thin warning-coloured line **frozen** at its original level; **hidden in Clean Mode** |
| Current stop reference | `plot` (`style_linebr`), 2 px | Steps as it rolls. **The hero object — visible in every mode including Clean** |
| Stop-method change | `label` (Analysis+) | A tick at the bar where the method changed, naming it (`FAT BAR`, `PIVOT`, `20 MA`, `8 MA`, `COLOUR ADJUST`, `BAR-BY-BAR`) with the `(v)` marker |
| Add | `plotshape` cross | Placed at an ATR-scaled offset on the lifecycle's side |
| Stop-out | `plotshape` xcross at `lc.stop` | Terminal marker |
| Push | `plotshape` flag, carries `[c]` | Confirmed-later by construction |
| Exit context / phase / invalidation reason | Dashboard rows | Kept off the price plane deliberately (§2). Reasons: `3RD PUSH` · `NEW HIGH RIGHT` · `NARROW->WIDE` · `WINDOW ELAPSED` · `STOP-OUT` · `STOP-OUT (GAP-THROUGH)` · `SAME-BAR AMBIGUITY` |

Using plots rather than line objects for the three horizontal references is deliberate: they cost
no drawing budget, they extend automatically, and they cannot leak past the lifecycle's end.

### 10.2 Historical lifecycles

Only entry and terminal markers survive. **No historical stop lines.** A chart with fifty
historical stop ladders is unreadable and the information has no forward use.

### 10.3 Concurrency

Default: **one active lifecycle**. When one is active, an opposite-direction candidate is drawn as
an event and a quality chip but **not** as an entry, and the dashboard shows
`BLOCKED: LIFECYCLE ACTIVE`. This is a `PLATFORM_SAFEGUARD` — the source never addresses
concurrency (SRC-300) — and is configurable.

### 10.4 The stop line is the hero object

Of everything on the chart, the current stop line is the one thing a trader needs continuously and
cannot reconstruct by eye, because it is the running maximum of up to six rotating references
(SRC-148). It therefore gets: the heaviest lifecycle weight, a persistent method label, and
visibility in **every** mode including Clean.

---

## 11. THE DASHBOARD

Compact, calm, optional (`showDashboard`), position-selectable (9 anchor points), size-selectable
(Small / Normal / Large).

### 11.1 Layout

```
┌──────────────────────────────────────────────┐
│  VELEZ MARKET MAP            ES · 2m  ⟨v⟩    │   ← title + symbol/TF + virtual marker
├──────────────────────────────────────────────┤
│  STATE          NARROW G1 ~      gap 0.62a   │
│  POSITION       +1  (POSITION 1)             │
│  TREND          20 ↑   200 → (flat)          │
│  LOCATION       ABOVE                        │
├──────────────────────────────────────────────┤
│  EVENT          ELEPHANT + CHANGE ~          │
│  LEG / PUSH     LEG 1 · PUSH 2 [c]           │
│  SETUP          SURGE OFF 200 ~              │
│  CONFLUENCE     HIGH QUALITY ···· [impl]     │
│  EXTENSION      SPACE p78 · not extreme      │
├──────────────────────────────────────────────┤
│  LIFECYCLE      VIRTUAL LONG ACTIVE ⟨v⟩      │
│  STOP           1.4a below · FAT BAR         │
│  ADD            DUE — first colour change    │
│  EXIT CONTEXT   —                            │
├──────────────────────────────────────────────┤
│  FIDELITY       SOURCE + APPROXIMATIONS      │
│  APPROX ACTIVE  state, position, elephant… 7 │
│  TF LESSON      2-min → V2 V4 V6 V7          │
│  HISTORY        last 300 bars · trimmed 0    │
└──────────────────────────────────────────────┘
```

### 11.2 Row inventory

| Row | Source | Clean | Analysis | Full | Debug |
|---|---|---|---|---|---|
| `STATE` (+grade, +measured gap) | SRC-050/055-057 | ● | ● | ● | ● |
| `POSITION` | SRC-070 | ● | ● | ● | ● |
| `TREND` (20 slope, 200 flatness) | SRC-035/036/040 | ● | ● | ● | ● |
| `LOCATION` | SRC-064 | ○ | ● | ● | ● |
| `EVENT` | SRC-090 | ● | ● | ● | ● |
| `LEG / PUSH` | SRC-201/170 | ○ | ● | ● | ● |
| `SETUP` | D10 | ● | ● | ● | ● |
| `CONFLUENCE` | impl layer | ● | ● | ● | ● |
| `EXTENSION` (space rank, Pluto, three-finger) | SRC-073/079/209 | ◐ | ● | ● | ● |
| `LIFECYCLE` | project | ● | ● | ● | ● |
| `STOP` (distance + method) | SRC-148 | ● | ● | ● | ● |
| `ADD` (due / count) | SRC-123 | ● | ● | ● | ● |
| `EXIT CONTEXT` | SRC-170-180 | ● | ● | ● | ● |
| `FIDELITY` | project | ◐ | ● | ● | ● |
| `APPROX ACTIVE` (count + names) | project | ○ | ● | ● | ● |
| `TF LESSON` | SRC-240 | ○ | ● | ● | ● |
| `HISTORY` (cap + trimmed) | project | ○ | ○ | ● | ● |
| `HTF 20` | SRC-248 | ○ | ● (if enabled) | ● | ● |
| `BLOCKERS` | SRC-029 | ○ | ○ | ○ | ● |

### 11.3 Rules

- **One `var table`, created once**, cells written only when `barstate.islast` (or on the last
  historical bar for replay). Never recreated per bar.
- Content is assembled into local strings first, then written — one `table.cell` call per row per
  update, not per condition.
- **Never dominates:** default Small size, default anchor top-right, monospaced numerals.
- **Non-colour cues everywhere:** `↑ ↓ →` for slope, `···` pips for confluence, explicit words for
  state. A greyscale screenshot of the dashboard loses nothing.
- **`~` on every approximation-dependent value.**
- **`⟨v⟩` in the title bar** so the virtual nature is visible without reading a row.
- In Debug Mode a `MODEL` row appears showing the active approximation models
  (`state=ATR · elephant=AvgMult · cc=Nearest · pos=ATR`).

---

## 12. DEBUG MODE

### 12.1 The funnel trace panel

```
┌─ FUNNEL TRACE · bar −0 ────────────────────────────────┐
│ 1 STATE      NARROW G1 ~     PASS   gap 0.62a ≤ 1.00a  │  CON-010 / SRC-051
│ 2 POSITION   +1              PASS   d 0.71a ≤ 1.00a    │  CON-020 / SRC-071
│ 3 EVENT      ELEPHANT+CC ~   PASS   rng 2.31× ≥ 1.80×  │  CON-035 / SRC-107
│ ─ GATES                                                 │
│   with the 20                PASS   slope +0.11a/bar   │  CON-074 / SRC-224
│   not inside state           PASS                       │  CON-022 / SRC-075
│   not Pluto land             PASS                       │  CON-023 / SRC-079
│   stop outside state         PASS   stop 4412.25        │  CON-028 / SRC-077
│   stop ladder                RUNG 1  base fits max loss │  CON-054 / SRC-151
│ ─ QUALITY  13 → ELITE SETUP [impl]                      │
│   narrow G1 +3 · pos1 +3 · eleph+cc +3 · with20 +2 ·    │
│   flat200 +2 · clear-left +1 · leg1 +1  = 15 → cap 13   │
│ ─ CONFIRMATION                                          │
│   event      CONFIRMED (bar close)                      │
│   push count CONFIRMED-LATER [c] (pivLen 3)             │
│ ─ APPROXIMATIONS ACTIVE (7)                             │
│   state · position · elephant · flatness · band ·       │
│   clear-left · near-20                                  │
└─────────────────────────────────────────────────────────┘
```

### 12.2 Blocked-candidate trace

When a candidate fails, the same panel shows the **first** failing gate prominently and lists the
rest — so "why didn't this fire?" is answerable without guesswork:

```
│ 2 POSITION   +3              FAIL   d 3.42a > 2.50a    │  CON-023 / SRC-079
│   BLOCKED: PLUTO LAND — with-trend entries not         │
│   permitted from position three (SRC-284, p.56)        │
```

### 12.3 The live checklist

The manual's own "BEFORE CLICKING BUY" checklist (SRC-028, p.66) rendered with computed values:

```
☑ State said out loud            NARROW G1 ~
☑ Position                       +1
☑ Event real                     ELEPHANT + CHANGE ~
☑ If tail — is most of it tail?  n/a (not a tail bar)
☑ With the direction of the 20   yes (20 ↑)
☑ Stop outside the state         yes — 4412.25 < 4418.10
☐ Stop fits max loss             UNSET — enter maxLossPerUnit
☑ Leg one or leg two             LEG 1
```

### 12.4 Inspected-bar offset

`debugBarOffset` (default 0) lets the trace target a historical bar within `historyCap`, so a user
can ask "why did nothing fire 40 bars ago?" without scrolling the whole chart.

### 12.5 Resource counters

```
labels 61/500 · lines 34/500 · boxes 12/500 · trimmed 0
```

---

## 13. COLOUR, CONTRAST AND ACCESSIBILITY

### 13.1 Palette

The manual uses green and red as its only accent colours "because they are the only two colours in
the method" (p.71). This design follows that, with a neutral third for structure.

| Role | Light theme | Dark theme | Notes |
|---|---|---|---|
| Bullish accent | `#1B7F5A` | `#3FBF8F` | ≥ 4.5:1 against both chart backgrounds |
| Bearish accent | `#A32B23` | `#E8635A` | ≥ 4.5:1 |
| Neutral / structure | `#5A6169` | `#9AA3AC` | 20 SMA, structure lines |
| Deep neutral | `#2B3138` | `#D6DBE0` | 200 SMA |
| Band fill | accent @ 8% | accent @ 10% | Never above 12% |
| Warning | `#B7791F` | `#E0A93B` | Distinct from both accents in luminance |
| Muted / suppressed | 55% opacity of base | 55% | Exhaustion elephants, provisional marks |

Colours are exposed as inputs; theme presets `Auto / Light / Dark` are provided.

### 13.2 Non-colour redundancy (P8)

| Meaning | Colour | **Also encoded as** |
|---|---|---|
| Bullish / bearish | green / red | marker orientation (`▲` / `▼`) and label text |
| State | hue | ribbon **pattern** + dashboard word |
| Narrow grade | — | ribbon pattern solid/dashed/dotted + `G1/G2/G3` text |
| Confluence tier | fill weight | `···` pip count + tier word |
| Approximate | — | **`~` character** (never colour-only) |
| Provisional | reduced opacity | dotted outline + `PROV` text |
| Confirmed-later | — | `[c]` / `⌛` marker |
| Blocked | warning hue | `■` glyph + reason text |
| Virtual | — | `⟨v⟩` / `(v)` marker |

**A greyscale screenshot of this indicator loses no meaning.** That is the acceptance test.

### 13.3 Deuteranopia consideration

Green and red are the method's own vocabulary and cannot be abandoned. Mitigations: (a) the accent
pair differs in **luminance** as well as hue, (b) every directional marker carries an orientation
glyph, (c) an optional `colorBlindSafe` preset substitutes blue/orange for the *indicator's* accents
while leaving candle colours untouched.

---

## 14. TYPOGRAPHY AND GLYPH INVENTORY

- Dashboard and Debug panels use `font.family_monospace` so numeric columns align.
- Chart labels use the default family at `size.small` (Clean/Analysis) or `size.tiny` (Full).

| Glyph | ASCII fallback | Meaning |
|---|---|---|
| `~` | `~` | Approximation-dependent (always ASCII — never emoji) |
| `⌛` | `[c]` | Confirmed later (pivot-derived) |
| `⟨v⟩` | `(v)` | Virtual lifecycle object |
| `▲` `▼` | `^` `v` | Long / short direction |
| `■` | `#` | Blocked |
| `···` | `...` | Confluence pips |
| `↑ ↓ →` | `U D -` | Slope |
| `☑ ☐` | `[x] [ ]` | Checklist |

`useEmoji = false` switches every glyph to its ASCII fallback. **No meaning is ever carried by an
emoji alone** — the `~` and the text labels are always present regardless.

---

## 15. WHAT IS DELIBERATELY NOT DRAWN

| Not drawn | Why |
|---|---|
| A flowchart of the funnel across the chart | Occupies the price plane for information that belongs in a table (§2) |
| All seven position zones across history | Clutter with no forward use (§8.1) |
| Historical trailing-stop ladders | Fifty stop ladders are unreadable and have no forward use (§10.2) |
| Head-and-shoulders pattern overlays or necklines | The source supplies no tolerances and explicitly warns against mechanising it (CON-069, SRC-237) |
| Any probability, win-rate or expectancy number as an output | All source figures are self-reported (SRC-004) and the manual instructs against using them to size (SRC-273) |
| Any P&L, R-multiple, equity curve or fill marker | Not a strategy (P9) |
| Buy/sell arrows on every qualifying bar | The explicit anti-goal of this project |
| The 13 bar types / 14 events | Not in the source (SRC-091) |
| A 13-period moving average | Referenced but never specified (SRC-034) |
| Intrabar entry timing marks (1:20–1:40 into the bar) | Not observable at bar close (§10.4 of the rulebook) |
| Backdated pivot markers on their originating bar without a `[c]` tag | Would misrepresent when the information was available (P6) |
