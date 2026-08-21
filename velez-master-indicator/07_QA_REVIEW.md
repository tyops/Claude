# 07 — QA REVIEW
### Three verification passes, findings, fixes, and what remains unverified

> **Verification status, stated once and plainly:**
>
> **Static review completed; TradingView compilation and live-chart verification were not
> performed in this environment.**
>
> There is no Pine Script compiler, no TradingView access and no market data in the authoring
> environment. Every claim below is the result of reading, reasoning and scripted inspection of
> the source file — not of execution. Where a check could only be performed by running the script,
> it is listed in §5 as a **procedure for the user to run**, not as a result.

---

## TABLE OF CONTENTS

1. [QA Pass 1 — PDF fidelity review](#1-qa-pass-1--pdf-fidelity-review)
2. [QA Pass 2 — Quantitative logic review](#2-qa-pass-2--quantitative-logic-review)
3. [QA Pass 3 — Pine Script and visual UX review](#3-qa-pass-3--pine-script-and-visual-ux-review)
4. [Audits: repaint, state machine, resources](#4-audits)
5. [Manual TradingView test procedure](#5-manual-tradingview-compile-and-replay-procedure)
6. [Defect log](#6-defect-log)
7. [Limitations](#7-limitations)
8. [What was verified and what was not](#8-what-was-verified-and-what-was-not)

**Method note.** Checks marked **[scripted]** were performed by a program run against the source
file and their output is reproduced. Checks marked **[reasoned]** were performed by reading the
code against the rulebook. Checks marked **[not run]** require execution.

---

## 1. QA PASS 1 — PDF FIDELITY REVIEW

### 1.1 Was every page reviewed?

**[scripted] PASS.** 71 of 71 pages extracted as text with per-page character, image and vector
counts; 71 of 71 reviewed visually (8 pages at full resolution, all 71 via contact sheets at
legible scale, plus one 300 dpi region crop of the p.15 diagram). Zero pages were unreadable,
image-only, or required OCR — the document contains **0 embedded raster images**.

**Finding F1-1 (metadata, not content).** The upload metadata declared 25 pages. `pdfinfo` and
PyMuPDF both report **71**. All 71 were reviewed. Recorded in `01_MASTER_RULEBOOK.md §1.1`.

### 1.2 Does every visible signal have source evidence?

**[reasoned] PASS.** Every detector in §11–§12 of the script carries a `SRC-###` + page citation
in a code comment, and the reverse audit (`06_RULE_COVERAGE_AUDIT.md §3.4`) walks all 15 detector
families. **0 detectors without a source origin.**

### 1.3 Does every label use supported terminology?

**[scripted + reasoned] PASS with disclosure.** 33 rendered strings are source terms; 9 are
disclosed UI shortenings; 10 are implementation-defined and **every one carries `[impl]`**.
Rule identifiers (`C##`, `V#.#`) are presented as the manual's navigation scheme, never as
"Velez's rule numbers" (SRC-002).

**Finding F1-2.** Three proposed labels were rejected because they had no source basis or
flattened distinct source concepts: `200 MA REJECTION`, `TOO EXTENDED`, `LATE ENTRY WARNING`.
Four were replaced with the source's own term. Recorded in `02_CONCEPT_TO_CODE_MAP.md §12`.

### 1.4 Were examples turned into universal rules?

**[reasoned] PASS.** All 13 worked examples in the manual are enumerated in
`01_MASTER_RULEBOOK.md §8` alongside an explicit statement of what each does **not** establish.
**No threshold anywhere in this project was derived from an example.**

This is not a stylistic choice: SRC-302 records that every worked example in all eight videos is
a setup that **worked**, so the example set cannot validate a detector even in principle.

### 1.5 Is editorial commentary falsely attributed?

**[reasoned] PASS.** Eight provenance layers (L1–L8) are separated in
`01_MASTER_RULEBOOK.md §1.2`. The manual's §3.5 critique, its rule identifiers, its glossary, its
checklists and its statistics table are all recorded as **the compiler's additions**, per the
manual's own disclosure on p.70. Part One is recorded as editorial synthesis, not as how any
single video presents the material.

### 1.6 Are source gaps silently filled?

**[reasoned] PASS.** Eight referenced-but-unspecified concepts are enumerated in
`06_RULE_COVERAGE_AUDIT.md §7` with **no rule created for any of them**. Two are surfaced
on-chart: the Debug panel states that 10 of 13 bar types and 11 of 14 events are absent from the
source, so a user is never left assuming full coverage.

### 1.7 Are missing or undefined events invented?

**[reasoned] PASS.** The four detectable events implemented (elephant, tail, colour change, bar
takeout) are exactly the four the source names. The 13-bar alphabet and the 14-event set are
**not** populated. The 13-period moving average referenced in SRC-034 is **not** implemented.

### 1.8 Is any self-reported statistic presented as a promise?

**[scripted] PASS.** Every one of the 24 statistics is `DOCUMENTATION_ONLY`. They appear in
exactly two places, both explicitly attributed:
- the Debug claims panel, prefixed *"SOURCE CLAIMS (attributed, NOT outputs) … All self-reported,
  unpublished, no stated sample"*;
- the retracement ladder labels, each reading *"the source claims 9 of 10 (V6, self-reported)"*
  with a tooltip stating the levels are computed and the odds are **quoted, not asserted**.

No statistic is used as a score weight, a threshold, a default, or an output. The three figures
that are mechanical *levels* rather than statistics — 25%, 50% and 65–70% — are implemented as
levels, and that distinction is recorded in `01_MASTER_RULEBOOK.md §9`.

### 1.9 Pass 1 findings summary

| ID | Finding | Severity | Resolution |
|---|---|---|---|
| F1-1 | Page count metadata wrong (25 vs 71) | Informational | All 71 reviewed; recorded |
| F1-2 | 3 proposed labels unsupported, 4 mis-worded | Would have misattributed terminology | Rejected / replaced; audited |
| F1-3 | p.15 diagram label contradicts the text rule it illustrates (SRC-152) | Source defect | Text implemented; contradiction disclosed in code, rulebook and audit — **not smoothed over**, per the manual's own instruction |
| F1-4 | The deep-drop severity threshold has no in-context source value | Would have been an invented number | 65% borrowed from the source's own V6.5 quantification, and the **cross-context borrowing is labelled as an inference** in four places |

---

## 2. QA PASS 2 — QUANTITATIVE LOGIC REVIEW

### 2.1 Long/short symmetry

**[reasoned] PASS.** SRC-076 states the mirror is not optional. Every detector, gate, stop
reference and lifecycle path has both directional forms: `elephantBull/Bear`,
`bottomingTail/toppingTail`, `ccBull/ccBear`, `takeoutBull/takeoutBear`, `surgeLong/Short`,
`igniteLong/Short`, `obEntryLong/Short`, `hiddenRedLong/hiddenGreenShort`, `dualLong/Short`,
`colourAdjLong/Short`, `pivotStopLong/Short`, `accelLong/Short`, `deepDropLong/Short`,
`lowerTop/higherBottom`.

**Finding F2-1.** SRC-215 records a stated short-side preference ("more money is generally made on
the downside"). It is deliberately **not** implemented as an asymmetric gate, because doing so
would break SRC-076's mirroring requirement. It is README text only.

### 2.2 Warm-up

**[reasoned] PASS.** `ready` requires `bar_index ≥ warmupNeeded`, a non-`na` ATR, and a positive
measurement unit. `warmupNeeded` is the max of the 200 SMA length and every lookback, plus the
experimental percentile lookback when that module is on. While not ready, every detector returns
false, no drawing is created, no alert can fire, and the dashboard reads
`WARMING UP <bar_index>/<warmupNeeded>` — **visible, not silent**.

The 200-bar floor is deliberate: the 200 SMA is the source's non-negotiable second average
(SRC-030), so a chart with fewer bars cannot express the method at all.

### 2.3 `na` behaviour

**[reasoned] PASS.** Every arithmetic input that can be `na` passes through `nz()` or an explicit
`na()` test. `ccUseHi`/`ccUseLo` are `na` until the first opposite-colour bar and are guarded by
`not na(...)`. Pivot values are `na` except on confirmation bars and are guarded everywhere.
`lc.stop` is `na` between lifecycles; `applyStop` handles `na(cur)` as its first branch.

### 2.4 Zero-range candles and division safety

**[scripted] PASS.** A precise scan found **exactly three division sites** in executable code:

```
L645  : n / d          — inside safeDiv(), guarded by `na(d) or d == 0`
L1240 : rng / 3.0      — constant divisor (SRC-151, "bottom third")
L1241 : rng / 3.0      — constant divisor
```

Every other ratio in the script routes through `safeDiv`. `rngOk = rng > 0` gates the elephant,
tail and snowman detectors, so a zero-range bar can never be classified as an event.

### 2.5 Dojis

**[reasoned] PASS.** `isDoji = close == open` is neither green nor red. A doji cannot supply a
colour-change reference (`if isRed` / `if isGreen` both skip it) and cannot be a trigger bar
(`not isDoji` in both `ccBull` and `ccBear`). The source never addresses dojis; this is recorded
as `PLATFORM_SAFEGUARD` PS-06.

### 2.6 Gaps

**[reasoned] PASS.** Three distinct gap behaviours are handled:
- **Gap-fill setup** (SRC-120): armed only from a prior narrow state, cancelled if the gap opens
  into a wide state (SRC-121), cleared on trigger or on the next session.
- **Gap through the stop** (same-bar policy S7): the lifecycle completes and the recorded reason
  is `STOP-OUT (GAP-THROUGH)` — **the indicator never claims a level price did not trade**.
- **SMA slope spike after a gap**: noted as a known behaviour in §7, not a defect.

### 2.7 Thin and illiquid bars

**[reasoned] PASS with a caveat.** `rngOk` excludes single-print bars from all bar-pattern
detection. In `LocalMax` elephant mode an additional `rng ≥ atrUnit` floor prevents the largest
bar of a dead range from being labelled an institutional footprint — which would directly
contradict the source's own rationale (SRC-093).
**Caveat:** on a genuinely illiquid instrument the ATR itself becomes unstable; the dashboard
shows the measured gap so the user can see when the unit is behaving oddly.

### 2.8 Session boundaries

**[reasoned] PASS.** `sessionOk = timeframe.isintraday` and
`newSession = session.isfirstbar_regular`. On symbols or timeframes without a regular session
(24 h crypto/forex, daily and above), the opening-bell and gap-fill modules **disable themselves**
rather than producing meaningless output.

### 2.9 Chart-timeframe compatibility

**[reasoned] PASS with disclosure.** The framework is timeframe-agnostic per SRC-022, so nothing
is blocked by timeframe. The dashboard `TF LESSON` row maps the chart to the manual's §1.9 table
and reads `not mapped in the source` on any other timeframe — advisory, never a gate.

**Finding F2-2 (important, disclosed to the user).** `clearLookback` is timeframe-sensitive in a
way the source's "the recent past" phrasing hides: 20 bars is 40 minutes on a 2-minute chart and
20 months on a monthly chart. The input tooltip says so explicitly.

### 2.10 Non-standard chart types

**[reasoned] PASS.** `not chart.is_standard` raises a persistent warning label and a dashboard row
reading `NON-STANDARD - distorted`. Detection is **not** disabled — the user is informed and left
in control — and the warning cannot be suppressed by a display mode.

### 2.11 State transitions and position boundaries

**[reasoned] PASS.** State is a pure function of `gapN` against two thresholds, so transitions are
deterministic and hysteresis-free. Position is a pure function of signed distance from the state
envelope against two thresholds.

**Finding F2-3 (behavioural, documented).** Neither classifier has hysteresis. A `gapN` hovering
at exactly `narrowMult` will flip state between adjacent bars. This is intentional — adding
hysteresis would introduce a *third* invented parameter on top of a threshold the source already
refuses to supply. The consequence is disclosed in the limitations section.

### 2.12 Approximation threshold boundaries

**[reasoned] PASS.** Every comparison uses a consistent inclusive/exclusive convention:
`≤` for the "still narrow" and "still position one" side, `≥` for the "already wide" and "already
qualifying" side. A value exactly on a boundary resolves to the **more conservative** class in
each case (still narrow, still position one, still an elephant).

### 2.13 Conflicting conditions

**[reasoned] PASS.** Eleven same-bar cases (S1–S11) are enumerated in
`04_INDICATOR_ARCHITECTURE.md §7` and each is implemented:
- S1 entry + stop on one bar → `INVALIDATED`, reason `SAME-BAR AMBIGUITY`
- S2/S11 stop evaluated **before** any favourable roll
- S3/S6 conflicting long and short evidence → neither entry (`anyEntryLong and not entryShortRaw`)
- S5 highest `EV_*` rank wins the marker
- S7 gap-through recorded explicitly
- S10 the colour-change reference is the one that existed at the **previous** bar's close, so a
  bar can never take out its own reference

### 2.14 Reset and invalidation

**[reasoned] PASS.** Verified reset paths: lifecycle → `IDLE` at the **start** of the bar after a
terminal state; `legCount` → 0 on a return to narrow; `maxPriorPullback` → 0 on a return to
narrow; gap arming cleared on trigger or new session; opening-bell state re-armed each session;
hidden-play arming cleared on trigger; colour-change reference re-anchored on each new
opposite-colour bar; `sleepyRun` → 0 the moment grade-1 is lost.

### 2.15 Virtual lifecycle transitions

**[reasoned] PASS.** All transitions in `04_INDICATOR_ARCHITECTURE.md §6.2` are implemented.

**Finding F2-4 (fixed).** The first draft attempted `lc.state[1]` to detect a terminal state on
the previous bar. **Pine has no `[]` history operator on user-defined-type fields.** Fixed by
resetting at the *start* of the bar, which reads the previous bar's closing state directly from
the persistent object and needs no history access at all.

### 2.16 Stop-method rotation

**[reasoned] PASS.** The rotation is a monotonic max (long) / min (short) over six candidates
applied in a fixed chain, so the stop can never loosen. Verified properties:
- the initial stop holds on the entry bar (`rotOk` requires `bar_index > lc.entryBar`);
- the pivot reference is released once the 20 climbs past it, exactly as SRC-144 states;
- the 8 engages only while the acceleration test is true and disengages on `close < sma8`,
  exactly as SRC-145 states;
- bar-by-bar engages only once `spaceN ≥ barByBarSpace`, exactly as SRC-146 states;
- the displayed *method name* follows a fixed precedence, disclosed as `PLATFORM_SAFEGUARD` PS-03
  because the source does not address ties.

**Finding F2-5.** Method-label flapping is possible when `spaceN` oscillates around
`barByBarSpace`. The *stop level* cannot flap because the rotation is monotonic; only the label
can. Rate-limited by the `minLabelGap` spacing rule.

### 2.17 Alert de-duplication

**[scripted] PASS.** 26 of 26 `alertcondition` calls are gated by `gateA`; 25 of 25 parsed message
blocks contain the disclaimer string. Every alert fires on a **rising edge**. Four per-family
cooldown trackers suppress repeats within `alertCooldownBars`.

### 2.18 Source-specific exceptions

**[reasoned] PASS.** Three named exceptions are implemented as exceptions rather than being
flattened away:
- the opening-bell one-bar stop bypasses the never-stop-inside-position-one gate (SRC-078);
- an oversized gap cancels the gap-fill setup (SRC-121);
- a surge may present as a **tail bar**, not only an elephant bar — the costume rule (SRC-196),
  which the manual explicitly says people miss.

### 2.19 Comparison against source examples

**[not run] NOT PERFORMED, and deliberately so.** SRC-302 records that every worked example in
all eight videos is a setup that worked. Tuning thresholds until the indicator reproduced those
examples would be fitting to a survivorship-biased sample and would produce a false impression of
validation. **No such comparison was made and none should be inferred.**

---

## 3. QA PASS 3 — PINE SCRIPT AND VISUAL UX REVIEW

### 3.1 Syntax and typing

**[reasoned, not compiled] REVIEWED.** Line-by-line reading against Pine v6 semantics.
Twelve defects were found and fixed (§6). Constructs deliberately avoided to reduce compile risk
in an environment with no compiler:

| Avoided | Reason |
|---|---|
| `enum` | Cannot be verified here; `const int` families are equivalent and universally supported |
| `[]` on UDT fields | Not supported by Pine |
| Global assignment inside functions | Not supported by Pine |
| Legacy `type[]` array syntax | Superseded by `array<type>` |
| `ta.*` inside a ternary | Can desynchronise series state |
| Conditional `plot()` / `alertcondition()` | Must be at global scope |
| Declaration-level `timeframe=` | Conflicts with drawing objects |
| Variable-length `ta.highest/lowest` | Avoided by tracking running extremes manually |

### 3.2 Scope correctness

**[scripted] PASS.** All 15 `plot()`, 15 `plotshape()`, 2 `fill()`, 2 `bgcolor()` and 26
`alertcondition()` calls are at global scope. A scripted forward-reference scan across 561 tracked
identifiers returned **9 candidates, all of which are matches inside tooltip string literals**,
not code references. **0 genuine forward references.**

### 3.3 Plot-count and drawing-object budget

**[scripted] PASS.** 17 plots + 15 plotshapes = **32 of ~64 slots (50% headroom)**. Drawing
worst case: **170 labels / 80 lines / 60 boxes** against Pine's 500-per-type caps — 66% / 84% /
88% headroom. Full inventory in `04_INDICATOR_ARCHITECTURE.md §10`.

### 3.4 Label, line and box cleanup

**[reasoned] PASS.** Six bounded arrays with FIFO trim helpers. The helpers **return** a trim count
which the caller accumulates, because Pine functions cannot assign to globals (§6 D2).
`trimmedCount` is surfaced on the dashboard `HISTORY` row and in the Debug resource counter, so
recycling is visible rather than silent.

One object is intentionally outside the pools: the non-standard-chart warning, created once on
`barstate.islast`.

### 3.5 Table behaviour

**[reasoned] PASS.** Two `var` tables, each created exactly once. Cells are written only under
`barstate.islast`. Both are explicitly cleared when their owning toggle is off, so a disabled
panel leaves no residue. `font.family_monospace` keeps numeric columns aligned.

### 3.6 Array and loop performance

**[reasoned] PASS.** No `while` loops. Three `for` loops exist, all bounded:
the three trim helpers (at most a few iterations per bar, since at most a few objects are pushed
per bar) and the 4-iteration retracement ladder, which runs only on `barstate.islast`. Worst-case
iterations per bar: well under 20.

### 3.7 Future-data leak

**[scripted] PASS.** Zero negative history offsets, zero `barmerge.lookahead_on`. Exactly one
`request.security` call, which requests `[1]` and `[2]` — **closed higher-timeframe bars only** —
with `lookahead = barmerge.lookahead_off` stated explicitly rather than defaulted.
`barstate.isconfirmed` does **not** appear inside the request expression (a known anti-pattern
that does not deliver higher-timeframe confirmation).

### 3.8 Higher-timeframe behaviour

**[reasoned] PASS.** Off by default. When enabled it contributes **context only**; using it as a
hard gate is a separate experimental module requiring two toggles. The cost — up to one
higher-timeframe bar of delay — is marked `[c]` on the dashboard row.

### 3.9 Confirmed versus provisional

**[reasoned] PASS.** Four confirmation classes are defined and each is honoured:
`CF_CONFIRMED` (bar close), `CF_PROVISIONAL` (off by default; never alerts, never advances the
lifecycle, never scores), `CF_LATER` (pivot-derived, drawn on the confirmation bar, tagged `[c]`),
`HTF-DELAY` (tagged `[c]`). With provisional preview off — the default — historical and real-time
rendering of confirmed objects are identical.

### 3.10 Alert semantics

**[scripted] PASS.** §2.17. No alert asserts a probability, a recommendation, or an execution.

### 3.11 Display-mode readability

**[reasoned] PASS.**
- **Clean** creates **zero text labels** — enforced structurally by `not modeClean` on the
  label-creation block, not by convention. All context is delivered by the dashboard.
- **Analysis** adds aggregated labels (one per bar maximum, `minLabelGap`-spaced), reference lines
  and zone boxes.
- **Full Velez** shows every supported event within `historyCap` bars and states the bound on the
  dashboard, so a bounded window is never mistaken for completeness.
- **Debug** answers "why did / didn't this fire?" with a 14-row trace naming the failing gate, its
  measured value, its threshold, and its `CON-###` / `SRC-###`.

### 3.12 Colour contrast and non-colour cues

**[reasoned] PASS.** Nine meanings carry a non-colour encoding: `~` (approximate, always ASCII),
`[c]` (confirmed later), `(v)` (virtual), marker orientation (direction), `···` pips (confluence
tier), `[x]`/`[ ]` (checklist), explicit words for state, position and blockers.

**Acceptance test: a greyscale screenshot loses no meaning.** Green/red are the method's own
vocabulary and cannot be abandoned; a `colorBlindSafe` preset substitutes blue/orange for the
*indicator's* accents while leaving candle colours untouched.

### 3.13 Readability under dense historical conditions

**[reasoned] PASS by construction.** Density is bounded by the per-family caps, the `minLabelGap`
spacing rule, one-aggregated-label-per-bar, and `historyCap`. Markers use `plotshape` (no label
budget), which is what makes Full Velez Mode possible at all.

**Explicitly not claimed:** that labels never visually overlap. Pine has no access to rendered
pixel geometry, so screen-space collision avoidance is impossible. The heuristic makes overlap
rare; it cannot prevent it. This is stated in the input tooltip and in
`03_VISUAL_DESIGN_SPEC.md §6`.

---

## 4. AUDITS

### 4.1 Repaint / confirmation audit

| Component | Class | Behaviour | Verified |
|---|---|---|---|
| SMAs, bands, state, position, space | `CONFIRMED` | Final at close | [reasoned] |
| Elephant, tail, colour change, takeout, gap fill | `CONFIRMED` | Final at close | [reasoned] |
| Surge, ignite, pullback, opening bell, hidden, dual space | `CONFIRMED` | Final at close | [reasoned] |
| Legs, resets, pushes, retracement legs, deep drop, Market Law Four, first warning drop, pivot stop | **`CONFIRMED-LATER`** | Known only `pivLen` bars after the originating bar; drawn **on the confirmation bar**; tagged `[c]` | [reasoned] |
| Higher-timeframe 20 direction | `HTF-DELAY` | Closed HTF bars only; tagged `[c]` | [scripted] |
| Intrabar preview | `PROVISIONAL` | **Off by default**; never alerts, never advances the lifecycle, never scores | [reasoned] |

**Honest statement of the limit.** This project does **not** claim to be "non-repainting" as a
blanket property. It claims something narrower and checkable: with `allowProvisional = false`
(the default) no confirmed object changes after its bar closes, and every value that depends on
future bars is drawn on its confirmation bar and visibly tagged. Whether that holds in live
rendering has **not** been observed here — see §5.

### 4.2 State-machine transition audit

| From | Trigger | To | Guard verified | Reset verified |
|---|---|---|---|---|
| `COMPLETED`/`INVALIDATED` | next bar | `IDLE` | ✅ start-of-bar | ✅ dir, stop, method, adds, pushes, initPeak, initDone, lastPushExt, reason |
| `IDLE` | state supports a mode | `WATCHING` | ✅ | — |
| `WATCHING` | position supports | `DEVELOPING` | ✅ | — |
| `DEVELOPING` | event + all gates | `QUALIFIED` | ✅ `evEntryEligible and gatesPass` | — |
| `QUALIFIED`/any | entry trigger | `*_ACTIVE` | ✅ `canOpen` (PS-01) | ✅ all fields initialised |
| `*_ACTIVE` | first colour change | add, `adds = 1` | ✅ mandatory (SRC-123) | — |
| `*_ACTIVE` | further event near the 20 | add | ✅ `adds < maxAdds` (PS-02) | — |
| `*_ACTIVE` | better reference | stop rolled | ✅ monotonic | — |
| `*_ACTIVE` | push 3 / new high / narrow→wide / window | `EXIT_CONTEXT` | ✅ **does not force a close** (SRC-173) | — |
| `EXIT_CONTEXT` | conditions clear | `*_ACTIVE` | ✅ | ✅ reason cleared |
| `*_ACTIVE`/`EXIT_CONTEXT` | stop broken | `COMPLETED` | ✅ evaluated before rolls (S2) | — |
| entry bar + stop broken | S1 | `INVALIDATED` | ✅ | ✅ reason recorded |

**Unreachable-state check [reasoned]:** every state has at least one inbound and one outbound
transition. No state is a sink except through the start-of-bar reset.

### 4.3 Resource-budget audit

| Resource | Limit | Worst case | Headroom | Verified |
|---|---|---|---|---|
| Labels | 500 | 170 + 1 | 66% | [scripted] |
| Lines | 500 | 80 | 84% | [scripted] |
| Boxes | 500 | 60 | 88% | [scripted] |
| Plot slots | ~64 | 32 | 50% | [scripted] |
| Tables | — | 2, `var` | — | [scripted] |
| `request.security` | — | 1 | — | [scripted] |
| Loop iterations / bar | — | < 20 | — | [reasoned] |
| `max_bars_back` | 5000 | declared | — | [scripted] |

---

## 5. MANUAL TRADINGVIEW COMPILE AND REPLAY PROCEDURE

**Because compilation and live verification were not performed here, run this before relying on
the script.** Each step names what to look for and what a failure looks like.

### Step 1 — Compile
1. TradingView → Pine Editor → **Open → New blank indicator**.
2. Paste the entire contents of `05_VELEZ_MASTER_INDICATOR.pine`.
3. **Save**, then **Add to chart**.
4. **Expected:** compiles with no errors. **If it does not**, note the exact line and message —
   §6 lists the twelve defects already fixed, and the constructs in §3.1 were avoided precisely
   to reduce this risk, but no compiler validated the result.

### Step 2 — Warm-up
5. Open a chart with **fewer than 200 bars**.
6. **Expected:** dashboard `STATE` reads `WARMING UP n/m`; no markers; no alerts.

### Step 3 — Foundation
7. Switch to a liquid symbol with ample history (e.g. a large-cap equity, 2-minute).
8. **Expected:** thin 20, thick 200, two soft bands. `20 SMA STATUS` / `200 SMA STATUS` sensible.

### Step 4 — Display modes
9. Cycle Clean → Analysis → Full Velez → Debug.
10. **Expected:** Clean shows **no text labels**; Analysis adds labels, reference lines and zone
    boxes; Full adds the retracement ladder; Debug shows the 14-row trace.
11. **Critical check:** the *set of shapes* must not change between modes for the same bars —
    only their density and their text. If a signal appears in one mode and not another, that
    violates the mode guarantee and is a defect.

### Step 5 — Repaint check (the most important one)
12. Note a `COLOUR CHANGE` marker on the most recent closed bar.
13. Let several bars close.
14. **Expected:** the marker stays on its original bar and does not move or vanish.
15. Note a `PUSH` marker. **Expected:** it appears `pivLen` bars *after* its swing, carrying `[c]`.
    It must **never** appear retroactively on the swing bar without that tag.

### Step 6 — Bar Replay
16. Open **Bar Replay**, rewind ~300 bars, step forward one bar at a time.
17. **Expected:** confirmed markers appear only at bar close and never disappear afterwards.
18. **Expected:** the virtual stop line steps monotonically in the protective direction and never
    loosens.
19. **Failure signature:** a marker that appears mid-bar and vanishes at close means provisional
    preview is on; check input 18.

### Step 7 — Same-bar policy
20. Find a bar where an entry marker and a stop-out coincide.
21. **Expected:** the lifecycle reads `INVALIDATED` with `SAME-BAR AMBIGUITY`, not a completed win.

### Step 8 — Session modules
22. Switch to a 24 h symbol (crypto), then to a daily chart.
23. **Expected:** opening-bell and gap-fill produce nothing; nothing errors.

### Step 9 — Non-standard chart
24. Switch the chart type to Heikin Ashi.
25. **Expected:** a persistent warning label plus the dashboard `CHART TYPE: NON-STANDARD`.

### Step 10 — Resources
26. In Debug Mode, read the `RESOURCES` row after scrolling back several thousand bars.
27. **Expected:** all three counters well under 500; `trimmed` may be non-zero (that is the ring
    buffer working correctly, not an error).

### Step 11 — Alerts
28. Create an alert on a specific condition, and one on **"Any alert() function call"**.
29. Set **Once Per Bar Close**.
30. **Expected:** alerts fire at close only; payloads carry state, position, setup, quality tier,
    stop, fidelity and the non-recommendation line.
31. **Remember:** alerts must be **recreated** after any change to the script or its inputs.

### Step 12 — Fidelity layers
32. Switch to **Strict source mechanics only**.
33. **Expected:** more raw events, no state/position gating, dashboard `FIDELITY: STRICT SOURCE
    ONLY`, and elephant-driven entries suppressed while elephant bars still draw as context.
34. Switch to **Include experimental modules**.
35. **Expected:** nothing changes until an individual experimental toggle is also switched on.

---

## 6. DEFECT LOG

All twelve were found by static review and fixed before delivery. Recorded rather than quietly
corrected, because a defect log with nothing in it is not credible.

| # | Defect | Would have caused | Fix | Pass |
|---|---|---|---|---|
| D1 | `lc.state[1]`, `lc.pushes[1]` — Pine has no `[]` on UDT fields | Compile error | Start-of-bar reset; `pushIncrement`, `lcLongActive`, `lcShortActive` bool mirrors | 3 |
| D2 | `trimmedCount += 1` inside functions — Pine functions cannot assign to globals | Compile error | Helpers return a count; caller accumulates | 3 |
| D3 | Six `float r = ...` declarations in one scope | Redeclaration error | `applyStop()` tuple helper + six uniquely named results | 3 |
| D4 | `float[]` / `string[]` legacy array syntax | Compile error on v6 | `array<float>` / `array<string>` | 3 |
| D5 | `plotshape(bool, location.absolute)` | Markers drawn at price 0/1 | Explicit float price series | 3 |
| D6 | `m / BIT % 2 >= 1` bit test | Wrong blocker names in Debug (integer-division semantics) | `hasBit()` modulo helper | 3 |
| D7 | `int expDensity = math.round(...)` | Type mismatch | Changed to `float` | 3 |
| D8 | Dumb-stop blocker applied **after** `gatesPass` was computed | **A stop inside position one could have passed the gates** — a direct violation of SRC-077 | Initial-stop selection moved to §14, before the gate block | 2 |
| D9 | `ta.percentrank` called inside a ternary | Desynchronised series state → wrong percentile | Called unconditionally; only the use is gated | 2 |
| D10 | `bar_index − 40` / `− zoneBoxBars` unguarded | Runtime error on short charts | `math.max(..., 0)` | 2 |
| D11 | Six behavioural magic numbers | Untraceable behaviour | Promoted to named constants with reasons | 1 |
| D12 | Visual spec claimed a 3 px pane-anchored ribbon | Documentation asserting something Pine cannot do | Implemented as a 6% wash; spec now states the limitation and moves the grade cue to the dashboard | 3 |

Eight further items were **removed** as dead or untraceable code — listed in
`06_RULE_COVERAGE_AUDIT.md §3.9`.

---

## 7. LIMITATIONS

### 7.1 The governing limitation

**The source states its own definitions are deliberately loose and instructs the reader to eyeball
rather than measure** (SRC-051, SRC-303). Every numeric threshold in this indicator is therefore an
**addition by this project**, not a translation of the method. The manual goes further: *"A method
built on judgement calls cannot be backtested … two traders applying the same lesson will take
different trades. Be aware you are buying a discretionary framework, not a system"* (SRC-303, p.68).

That is why 37 concepts are classified `PROGRAMMABLE_APPROXIMATION` rather than
`EXACT_TRANSLATION`, and why every one of them renders with `~`.

### 7.2 Not verified in this environment

- Pine compilation
- Live or historical chart rendering
- Bar Replay behaviour
- Alert delivery
- Actual on-screen label density or overlap
- Behaviour on any specific symbol, timeframe or broker feed

### 7.3 Structural limitations

| Limitation | Detail |
|---|---|
| **Pivot-derived values are known late** | Legs, pushes, retracement levels, deep drops, Market Law Four and the pivot stop are all `CONFIRMED-LATER` by `pivLen` bars. This is inherent to swing detection, not a bug — but it means the deep-drop warning arrives *after* the drop, which is the opposite of the source's stated intent that you train on catching it forming (SRC-231) |
| **"Push" has no source definition** | The three-push exit is the source's most-repeated exit rule and its mechanics are absent from all 71 pages. The implementation is a defensible proxy, nothing more |
| **The hidden-green entry is a different event** | The source's trigger is the intrabar instant green is erased; only the bar-close form is observable. Disclosed on-chart, in the alert text, and in worksheet W21 |
| **Two intrabar entry timings are not implemented** | "Into the bar before it finishes" and "the last 15–20 minutes of the hourly bar". The source's own fallback (next bar clears the high) is implemented instead |
| **The full head-and-shoulders is not implemented** | The source supplies no tolerances and warns against mechanising it (SRC-237) |
| **No hysteresis on state or position** | A value sitting exactly on a threshold will flip between bars (F2-3). Adding hysteresis would mean inventing a *third* parameter atop a threshold the source already refuses to supply |
| **`clearLookback` is timeframe-sensitive** | 20 bars means very different things on a 2-minute and a monthly chart (F2-2) |
| **Label overlap cannot be prevented** | Pine cannot see rendered geometry. The heuristic makes overlap rare, not impossible |
| **Full Velez Mode is bounded** | `historyCap` bars, not the whole chart. Pine's 500-per-type caps make unbounded rendering impossible |
| **No sizing, no risk mathematics** | The source contains none (SRC-300). `maxLossPerUnit` is a user input that stays `UNSET` until filled |
| **Ten of thirteen bar types and eleven of fourteen events are missing from the source itself** | Not an implementation gap (SRC-091) |
| **Four of eight lessons have declared transcript gaps** | Videos 2, 3, 7, 8 (SRC-007) |

### 7.4 Claims deliberately not made

Not official · not endorsed · not affiliated · not a strategy · not a backtest · no fills ·
no P&L · no probability output · no win rate · no performance claim · not validated against the
source's examples (and could not be — SRC-302) · not blanket non-repainting · not investment
advice.

---

## 8. WHAT WAS VERIFIED AND WHAT WAS NOT

### Verified here

| Check | Method |
|---|---|
| All 71 pages reviewed, text and visually | scripted + visual |
| 0 detectors without a source citation | reasoned + scripted |
| 0 unsupported terms presented as source-defined | reasoned |
| 0 statistics used as outputs, weights or defaults | scripted |
| 0 negative history offsets; 0 `lookahead_on` | scripted |
| 0 `strategy.*` / backtest surface | scripted |
| Exactly 1 `request.security`, closed bars only, lookahead off | scripted |
| Exactly 3 division sites, all guarded | scripted |
| 26/26 alerts gated by `gateA`; 25/25 messages carry a disclaimer | scripted |
| All plots, shapes and alert conditions at global scope | scripted |
| 0 genuine forward references across 561 identifiers | scripted |
| Resource budgets: 32/64 plots, 170/500 labels, 80/500 lines, 60/500 boxes | scripted |
| All 12 state-machine transitions and all reset paths implemented | reasoned |
| All 11 same-bar ambiguity cases implemented | reasoned |
| 12 defects found and fixed; 8 dead-code items removed | reasoned |

### Not verified here

**Static review completed; TradingView compilation and live-chart verification were not performed
in this environment.**

Specifically not done: compilation, rendering, replay, alert delivery, visual density inspection,
and any behaviour on real market data. §5 is the procedure for closing that gap.
