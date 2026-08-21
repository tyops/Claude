# 02 — CONCEPT-TO-CODE MAP
### Every source concept, its disposition, and the exact mechanics used

> **Reading order.** This document depends entirely on `01_MASTER_RULEBOOK.md`. Every `SRC-###`
> reference below resolves to an evidence record there. No concept may appear here without one.

---

## TABLE OF CONTENTS

1. [Classification keys and reading guide](#1-classification-keys-and-reading-guide)
2. [Normalisation basis and the default-provenance ledger](#2-normalisation-basis-and-the-default-provenance-ledger)
3. [Concept map — foundation layer](#3-concept-map--foundation-layer-con-001-009)
4. [Concept map — state, position, event, entries, stops, exits](#4-concept-map--state-con-010-019)
5. [Approximation worksheets](#11-approximation-worksheets)
6. [Candidate callout audit](#12-candidate-callout-audit)
7. [Source-term versus UI-label register](#13-source-term-versus-ui-label-register)
8. [Fidelity-layer membership](#14-fidelity-layer-membership)
9. [Disposition totals](#15-disposition-totals)

---

## 1. CLASSIFICATION KEYS AND READING GUIDE

**Source classification** — `DIRECT` · `IMPLIED` · `SUBJECTIVE` · `UNSUPPORTED`
(defined in `01_MASTER_RULEBOOK.md §5`).

**Implementation classification**

| Code | Meaning |
|---|---|
| `EXACT` | `EXACT_TRANSLATION` — deterministic Pine translation of a direct source rule |
| `APPROX` | `PROGRAMMABLE_APPROXIMATION` — an explicit, configurable mathematical proxy for a subjective or incomplete source concept |
| `EXPER` | `EXPERIMENTAL` — an optional, clearly non-source extension for exploration only. **Off by default** |
| `SAFE` | `PLATFORM_SAFEGUARD` — a non-methodological Pine/TradingView reliability decision |
| `DOCS` | `DOCUMENTATION_ONLY` — in the knowledge base, not rendered as a signal |
| `NONE` | `NOT_IMPLEMENTED` — intentionally omitted, reason given |

**Disposition** — the required six-way outcome:
`IMPL-EXACT` · `IMPL-APPROX` · `EXPERIMENTAL` · `VISUAL-CONTEXT` · `NOT-IMPLEMENTED` ·
`REFERENCED-UNSPECIFIED`.

**Repaint-risk vocabulary**

| Value | Meaning |
|---|---|
| `NONE` | Value is final when the bar closes and never changes afterwards |
| `PROVISIONAL` | Computed intrabar for preview; may change or vanish before close. Never alerts, never scores |
| `CONFIRMED-LATER` | Requires future bars (pivots). Drawn **only on the confirmation bar**, never backdated onto the originating bar |
| `HTF-DELAY` | Higher-timeframe value carries a documented one-HTF-bar confirmation delay |
| `N/A` | Not a series computation |

---

## 2. NORMALISATION BASIS AND THE DEFAULT-PROVENANCE LEDGER

### 2.1 The volatility unit

The manual supplies no unit of measurement for any of its distance concepts. Every distance in
this implementation is expressed in **ATR units**:

```
atrUnit = ta.atr(ATR_LEN)          // ATR_LEN default 20
```

**Provenance of that choice, stated plainly:**
- Using ATR at all is **this project's addition**. The manual never mentions ATR, average range,
  standard deviation, or any volatility measure. Its instruction is the opposite — "eyeball it"
  (SRC-051), "do not measure" (SRC-051, p.54).
- The **length 20** is borrowed from the source's own short average (SRC-031) purely to avoid
  introducing an unrelated magic number. The borrowing does not make ATR source-supported.
- A percent-of-price basis is offered as an alternative for instruments where ATR behaves poorly
  (very low-priced or very illiquid symbols).

### 2.2 Default-provenance ledger

Every material numeric default in the indicator, with an honest account of where it came from.

| Input | Default | Provenance | Source anchor (if any) |
|---|---|---|---|
| `ATR_LEN` | 20 | **Anchored** | Borrowed from the source's short SMA period (SRC-031) |
| `narrowMult` (state) | 1.00 ATR | **Pure addition** | None. The manual refuses to quantify (SRC-051) |
| `wideMult` (state) | 3.00 ATR | **Pure addition** | None (SRC-060) |
| `bandAtrMult` (MA zone half-width) | 0.25 ATR | **Pure addition** | Concept is DIRECT (SRC-042); width is never given |
| `slopeLen` | 10 bars | **Pure addition** | None |
| `flatThresh` | 0.05 ATR/bar | **Pure addition** | None (SRC-035, SRC-055) |
| `p1Max` (position 1 outer edge) | 1.00 ATR | **Pure addition** | Ladder structure is DIRECT (SRC-070); boundaries are not |
| `p2Max` (position 2 outer edge) | 2.50 ATR | **Pure addition** | Same |
| `elephantLookback` | 10 bars | **Pure addition** | "the bars around it" (SRC-092) gives no count |
| `elephantRangeMult` | 1.80× | **Pure addition** | "visibly larger" (SRC-092) |
| `elephantBodyFrac` | 0.50 | **Semi-anchored** | "large-bodied" (SRC-191) and the solid-body diagram (SRC-109) |
| `tailMinFrac` | 0.50 | **Semi-anchored** | "**Most** of the bar must be tail" (SRC-096) ⇒ more than half |
| `tailBodyMaxFrac` | 0.35 | **Pure addition** | "The body is the small part" (SRC-096) gives no ratio |
| `near20Mult` | 0.75 ATR | **Pure addition** | "A little above, a little below … He does not care which" (SRC-223) |
| `clearLookback` | 20 bars | **Pure addition** | "the recent past" (SRC-192) |
| `originTolBars` | 2 bars | **Anchored** | "can start **a bar or two** away from the 200" (SRC-195) |
| `resetBars` | 3 bars | **Pure addition** | "A brief pause" (SRC-201) |
| `resetDriftMult` | 1.00 ATR | **Pure addition** | Covers both "sideways" and "shallow drift" forms (SRC-201) |
| `pivLen` | 3 bars | **Pure addition** | The manual never defines a swing (SRC-144) |
| `exhaustLegs` | 3 | **Anchored** | "the market tends to pause, rest or reverse after **three to five** pushes … three happens more often" (SRC-265) |
| `deepDropPct` | 65% | **Anchored (cross-context)** | Borrowed from V6.5's "significant margin, **toward 65 or 70%**" (SRC-269) as the manual's only quantification of a significant break of the 50% line. **The manual does not state 65% in the V1 deep-drop context — the borrowing is this project's inference** |
| `scalpTradePct` | 65% | **Anchored** | Stated directly for this purpose (SRC-218, SRC-269) |
| `scalpTargetPct` | 25% | **Anchored** | Stated directly (SRC-271) |
| `abProximityBars` | 10 bars | **Anchored** | The source's own 8–12 bar average holding period (SRC-259) |
| `accelLen` / `accelMult` | 5 / 2.0× | **Pure addition** | "accelerates faster than the 20 can track" (SRC-145) |
| `barByBarSpaceMult` | 2.00 ATR | **Pure addition** | "separated from the 20 and lost that support" (SRC-146) |
| `spaceLookback` / `spacePct` | 250 bars / 90th pct | **Semi-anchored** | The "biggest space in 10 years" framing establishes *historical-relative* measurement (SRC-221); the numbers are additions |
| `strongMoveAtr` | 3.00 ATR | **Pure addition** | "a genuinely strong, one-directional prior move" (SRC-258) |
| `maxAdds` | 3 | `PLATFORM_SAFEGUARD` | The source caps adds nowhere (SRC-123, SRC-126) |
| `maxLifecycles` | 1 concurrent | `PLATFORM_SAFEGUARD` | Concurrency explicitly absent from the source (SRC-300) |
| `historyCap` | 300 bars | `PLATFORM_SAFEGUARD` | Drawing-object budget |

**Count: 3 anchored, 2 cross-context/semi-anchored numerals, 5 semi-anchored, 19 pure additions,
3 platform safeguards.** Every pure addition is exposed as an input, tooltipped as an
approximation, and surfaced in Debug Mode.

---

## 3. CONCEPT MAP — FOUNDATION LAYER (CON-001…009)

### Table A — identity
| CON | Source term | SRC evidence | Src class | Impl class | Purpose |
|---|---|---|---|---|---|
| CON-001 | 20-period simple moving average | SRC-030, 031, 035, 037, 039, 040 | DIRECT | `EXACT` | Foundation / trend reference |
| CON-002 | 200-period simple moving average | SRC-030, 031, 036, 038, 044 | DIRECT | `EXACT` | Foundation / context reference |
| CON-003 | 8-period average (situational) | SRC-033 | DIRECT | `EXACT` | Trailing escalation only |
| CON-004 | "Zones, not lines" — the MA band | SRC-003, 042, 043 | DIRECT (concept) / SUBJECTIVE (width) | `APPROX` | Display + state/position geometry |
| CON-005 | Flat versus sloping | SRC-035, 036, 039, 055–057 | DIRECT (concept) / SUBJECTIVE (threshold) | `APPROX` | Filter |
| CON-006 | "Keeps you honest" — trend by the 20's slope | SRC-040, 041, 224, 225 | DIRECT | `APPROX` | Hard directional filter |
| CON-007 | 13-period short partner | SRC-034 | UNSUPPORTED | `NONE` | — |
| CON-008 | The buddy system | SRC-030, glossary | DIRECT | `DOCS` | Design rationale |
| CON-009 | Simple beats exponential/weighted | SRC-032 | DIRECT (claim) | `DOCS` | Justifies SMA choice |

### Table B — mechanics
| CON | Preconditions | Inputs / data | Objective formula | Timing | Repaint risk |
|---|---|---|---|---|---|
| CON-001 | ≥20 bars | `close` | `ta.sma(close, 20)` | Bar close | `NONE` |
| CON-002 | ≥200 bars | `close` | `ta.sma(close, 200)` | Bar close | `NONE` |
| CON-003 | ≥8 bars; active lifecycle; acceleration test true (CON-086) | `close` | `ta.sma(close, 8)` | Bar close | `NONE` |
| CON-004 | CON-001, CON-002, ATR ready | `atrUnit` | `band = bandAtrMult × atrUnit`; `ma20Hi/Lo = sma20 ± band`; `ma200Hi/Lo = sma200 ± band` | Bar close | `NONE` |
| CON-005 | `slopeLen` bars of history | `sma20`, `sma200`, `close`, `atrUnit` | `slopeN(s) = (s − s[slopeLen]) / slopeLen / atrUnit`; `flat(s) = abs(slopeN(s)) ≤ flatThresh`. Price "flat and sandwiched" = every `close[0…slopeLen−1]` inside `[stateBot, stateTop]` | Bar close | `NONE` |
| CON-006 | CON-005 | `sma20` slope sign | `trendUp = slopeN(sma20) > flatThresh`; `trendDn = slopeN(sma20) < −flatThresh`; else `trendFlat` | Bar close | `NONE` |
| CON-007 | — | — | *(none — no rule exists to translate)* | — | `N/A` |
| CON-008 | — | — | — | — | `N/A` |
| CON-009 | — | — | — | — | `N/A` |

### Table C — behaviour, wiring and verification
| CON | Invalidation / reset | Conflict / precedence | Inputs & default provenance | Pine module | Visual mapping | Alert | QA |
|---|---|---|---|---|---|---|---|
| CON-001 | n/a | n/a | none | `§MA` | Thin plot + optional band fill | none | QA-101, QA-301 |
| CON-002 | n/a | n/a | none | `§MA` | Thick plot + optional band fill | none | QA-101, QA-301 |
| CON-003 | Hidden unless CON-086 active | Never used for entries (SRC-033) | `showMa8` (display) | `§MA` | Dashed plot, shown only while escalated | none | QA-214 |
| CON-004 | n/a | Band participates in state/position geometry | `bandAtrMult` = 0.25 ATR — **pure addition** | `§MA` | Two translucent bands | none | QA-102, QA-302 |
| CON-005 | n/a | Feeds CON-011/012/013 grading | `slopeLen` = 10, `flatThresh` = 0.05 — **pure additions** | `§CTX` | Dashboard `20 MA STATUS` / `200 MA STATUS` | none | QA-103, QA-201 |
| CON-006 | n/a | **T2 blocker** — outranks every setup except explicit reversion modes | reuses CON-005 inputs | `§CTX` | Dashboard `TREND`; blocker chip `AGAINST THE 20` | none | QA-104, QA-220 |
| CON-007 | — | — | — | — | none | none | QA-105 |
| CON-008 | — | — | — | — | README only | none | — |
| CON-009 | — | — | — | — | README only | none | — |

**CON-007 omission reason.** SRC-034 records that a 13-period partner is used "in some setups",
but the manual never says which setups, how it is used, or what it replaces. Creating a
13-period rule would be inventing a member of an unspecified set. **Disposition:
`REFERENCED-UNSPECIFIED`.**

---

## 4. CONCEPT MAP — STATE (CON-010…019)

### Table A
| CON | Source term | SRC evidence | Src class | Impl class | Purpose |
|---|---|---|---|---|---|
| CON-010 | State: narrow / regular / wide | SRC-050, 023, 052, 059, 060 | DIRECT (concept) / SUBJECTIVE (boundaries) | `APPROX` | Step 1 filter |
| CON-011 | Creme de la creme (narrow grade 1) | SRC-055, 058 | DIRECT | `APPROX` | Setup grading |
| CON-012 | Second best (narrow grade 2) | SRC-056 | DIRECT | `APPROX` | Setup grading |
| CON-013 | Least potent (narrow grade 3) | SRC-057 | DIRECT | `APPROX` | Setup grading |
| CON-014 | Get in near, get out away — narrow→wide exit | SRC-062, 180 | DIRECT | `EXACT` | Exit signal |
| CON-015 | Only two spots matter | SRC-063, 054, 061 | DIRECT | `EXACT` | Mode selection |
| CON-016 | Sleepy state | SRC-207 | DIRECT | `APPROX` | Swing-setup precondition |
| CON-017 | The merge | SRC-198 | DIRECT | `APPROX` | Surge-setup enhancer |
| CON-018 | Markets are trapped on rails | SRC-052, 053 | DIRECT | `DOCS` | Rationale |
| CON-019 | State versus location | SRC-064 | DIRECT | `EXACT` | Prevents axis conflation |

### Table B
| CON | Preconditions | Inputs / data | Objective formula | Timing | Repaint |
|---|---|---|---|---|---|
| CON-010 | CON-001/002/004 ready | `sma20`, `sma200`, `atrUnit` | `gapN = abs(sma20 − sma200) / atrUnit`; `NARROW` if `gapN ≤ narrowMult`; `WIDE` if `gapN ≥ wideMult`; else `REGULAR` | Bar close | `NONE` |
| CON-011 | CON-010 = NARROW | CON-005 | `flat(sma20) and flat(sma200) and priceSandwiched` | Bar close | `NONE` |
| CON-012 | CON-010 = NARROW, not CON-011 | CON-005 | `flat(sma200) and not flat(sma20)` | Bar close | `NONE` |
| CON-013 | CON-010 = NARROW, not CON-011/012 | CON-005 | `not flat(sma20) and not flat(sma200)` | Bar close | `NONE` |
| CON-014 | An active virtual lifecycle whose entry state was NARROW | CON-010, lifecycle | `entryState == NARROW and state == WIDE` → emit `EXIT CONTEXT: NARROW→WIDE` | Bar close | `NONE` |
| CON-015 | CON-010 | — | `EMERGENCE` mode if NARROW; `REVERSION` mode if WIDE; `NEAR-THE-20` mode if REGULAR | Bar close | `NONE` |
| CON-016 | CON-010 = NARROW | CON-005 | `flat(sma20) and flat(sma200) and priceFlatSideways` over `sleepyBars` (default = `slopeLen`) — i.e. CON-011 sustained | Bar close | `NONE` |
| CON-017 | — | `gapN` | `gapN ≤ mergeMult` (default 0.5 ATR — a tighter narrow) | Bar close | `NONE` |
| CON-018 | — | — | — | — | `N/A` |
| CON-019 | CON-010, price | — | `location = close > stateTop ? ABOVE : close < stateBot ? BELOW : INSIDE` | Bar close | `NONE` |

### Table C
| CON | Invalidation | Precedence | Inputs & provenance | Module | Visual | Alert | QA |
|---|---|---|---|---|---|---|---|
| CON-010 | Recomputed every bar | Gates everything (T0) | `narrowMult` 1.0, `wideMult` 3.0 — **pure additions**; `stateBasis` = ATR \| %price | `§STATE` | Dashboard `STATE`; optional ribbon; state-change marker | `alertcondition` on transition | QA-201, QA-202, QA-203 |
| CON-011 | State leaves NARROW | Highest grade | reuses CON-005 | `§STATE` | Dashboard `NARROW G1 ~`; grade pip in ribbon | via state alert | QA-204 |
| CON-012 | ″ | — | ″ | `§STATE` | `NARROW G2 ~` | — | QA-204 |
| CON-013 | ″ | Lowest grade; adds `WHIPPY` context chip (SRC-057) | ″ | `§STATE` | `NARROW G3 ~` | — | QA-204 |
| CON-014 | Lifecycle completes/invalidates | **Standalone exit — fires regardless of other conditions** (SRC-062) | none | `§EXIT` | `EXIT CONTEXT` marker + dashed line | `alertcondition` | QA-205, QA-231 |
| CON-015 | State change | Determines which setup family may arm | none | `§STATE` | Dashboard `MODE` | — | QA-206 |
| CON-016 | Any of the three ceases to be flat | Precondition for CON-057 only | `sleepyBars` = 10 — **pure addition** | `§SETUP` | `SLEEPY` zone box | — | QA-207 |
| CON-017 | `gapN > mergeMult` | Enhancer only; never a standalone signal | `mergeMult` 0.5 ATR — **pure addition** | `§SETUP` | `MERGE` chip in surge label | — | QA-208 |
| CON-018 | — | — | — | — | README | — | — |
| CON-019 | — | Must never be reported as "state" (SRC-064) | none | `§STATE` | Dashboard `LOCATION` | — | QA-209 |

---

## 5. CONCEPT MAP — POSITION (CON-020…029)

### Table A
| CON | Source term | SRC evidence | Src class | Impl class | Purpose |
|---|---|---|---|---|---|
| CON-020 | The seven-position ladder | SRC-070, 071, 074 | DIRECT (structure) / SUBJECTIVE (boundaries) | `APPROX` | Step 2 filter |
| CON-021 | Position one / negative one | SRC-072, 073, 076 | DIRECT | `APPROX` | Primary entry zone |
| CON-022 | The state (position zero) | SRC-075 | DIRECT | `EXACT` | No-trade zone |
| CON-023 | Pluto land | SRC-079, 282 | DIRECT | `APPROX` | Blocker / warning |
| CON-024 | Position two | SRC-074 | DIRECT | `APPROX` | Secondary entry zone |
| CON-025 | Position three | SRC-074, 284 | DIRECT | `APPROX` | Reversion-only zone |
| CON-026 | The box | SRC-209, 326 | DIRECT | `APPROX` | Swing entry zone |
| CON-027 | Three-finger spread | SRC-209, 323, 292 | DIRECT | `APPROX` | Late-entry blocker |
| CON-028 | Never stop inside position one ("the dumb stop") | SRC-077, 281 | DIRECT | `EXACT` | Stop-validity gate |
| CON-029 | The 80%+ position-one target | SRC-073, 254 | DIRECT (behavioural target) | `DOCS` | Reference text only |

### Table B
| CON | Preconditions | Inputs / data | Objective formula | Timing | Repaint |
|---|---|---|---|---|---|
| CON-020 | CON-010, CON-004 | `close`, `stateTop`, `stateBot`, `atrUnit` | `dUp = (close − stateTop)/atrUnit`; `dDn = (stateBot − close)/atrUnit`. Above: `+1` if `0 < dUp ≤ p1Max`; `+2` if `≤ p2Max`; `+3` otherwise. Mirror below. `0` if inside | Bar close | `NONE` |
| CON-021 | CON-020 | — | `pos == +1` (long) / `pos == −1` (short) | Bar close | `NONE` |
| CON-022 | CON-020 | — | `pos == 0` | Bar close | `NONE` |
| CON-023 | CON-020 | — | `abs(pos) == 3` | Bar close | `NONE` |
| CON-024 | CON-020 | — | `abs(pos) == 2` | Bar close | `NONE` |
| CON-025 | CON-020 | — | `abs(pos) == 3` (same band as CON-023; the two are the same zone under two source names) | Bar close | `NONE` |
| CON-026 | CON-010, CON-004 | `close`, `sma20`, `sma200`, `atrUnit` | `inBox = gapN ≤ boxGapMult and abs(close − sma20)/atrUnit ≤ boxPriceMult` | Bar close | `NONE` |
| CON-027 | CON-026 | — | `threeFingers = gapN > boxGapMult and abs(close − sma20)/atrUnit > boxPriceMult` — all three items mutually separated | Bar close | `NONE` |
| CON-028 | A proposed stop level and CON-020 | `stopLevel`, `stateBot`/`stateTop` | Long: valid iff `stopLevel < stateBot`. Short: iff `stopLevel > stateTop`. **Exception:** opening-bell lifecycles (SRC-078) bypass this gate | Bar close | `NONE` |
| CON-029 | — | — | — | — | `N/A` |

### Table C
| CON | Invalidation | Precedence | Inputs & provenance | Module | Visual | Alert | QA |
|---|---|---|---|---|---|---|---|
| CON-020 | Every bar | Gates events (T0) | `p1Max` 1.0, `p2Max` 2.5 ATR — **pure additions**; `posBasis` = ATR \| StateWidth | `§POS` | Dashboard `POSITION`; optional zone boxes at the last bar | — | QA-210, QA-211 |
| CON-021 | ″ | Highest-priority entry zone | — | `§POS` | `POSITION 1` badge | — | QA-210 |
| CON-022 | ″ | **Blocks all entries** (SRC-075) | — | `§POS` | Blocker chip `INSIDE THE STATE` | — | QA-212 |
| CON-023 | ″ | **Blocks with-trend entries** (SRC-079) | — | `§POS` | Blocker chip `PLUTO LAND` | `alertcondition` (warning) | QA-213 |
| CON-024 | ″ | Permitted, lower quality (SRC-074) | — | `§POS` | `POSITION 2` badge | — | QA-210 |
| CON-025 | ″ | Reversion-only (SRC-074, 284) | — | `§POS` | `POSITION 3` badge + reversion mode | — | QA-213 |
| CON-026 | Price leaves the box | Swing-family precondition | `boxGapMult` 1.5, `boxPriceMult` 1.0 ATR — **pure additions** | `§SETUP` | `THE BOX` shaded box | — | QA-215 |
| CON-027 | Returns to box | **Blocker** for new swing entries (SRC-292) | reuses CON-026 | `§SETUP` | Blocker chip `THREE FINGERS` | — | QA-215 |
| CON-028 | Stop recomputed | **Hard gate.** A lifecycle cannot arm with an invalid stop | `enforceDumbStop` (default **on**) | `§STOP` | Blocker chip `DUMB STOP` | — | QA-216, QA-232 |
| CON-029 | — | — | — | — | Debug reference panel; README | — | — |

**Note on CON-023/CON-025.** "Pluto land" (SRC-079) and "position three" (SRC-074) describe the
same region under two source names. They are kept as separate concept IDs because the manual uses
them in different roles — position three is a *directional-bias* statement, Pluto land is a
*warning* — but they resolve to one computed band. This is disclosed rather than silently merged.

---

## 6. CONCEPT MAP — EVENTS (CON-030…039)

### Table A
| CON | Source term | SRC evidence | Src class | Impl class | Purpose |
|---|---|---|---|---|---|
| CON-030 | Elephant bar | SRC-092, 093, 094, 101, 109 | DIRECT (concept) / SUBJECTIVE (size) | `APPROX` | Primary event detector |
| CON-031 | Igniting vs exhaustion elephant | SRC-235, 287 | DIRECT (rule) / SUBJECTIVE (early/late) | `APPROX` | Event qualifier |
| CON-032 | Tail bar (bottoming / topping) | SRC-096, 098, 100, 101, 109 | DIRECT | `APPROX` | Primary event detector |
| CON-033 | Snowman (counterfeit tail) | SRC-097, 283 | DIRECT | `EXACT` | Diagnostic guard |
| CON-034 | Colour change / colour game / bull 180 | SRC-102–106, 108, 222, 223 | DIRECT | `EXACT` | Primary event detector |
| CON-035 | Elephant + colour change combination | SRC-107 | DIRECT | `EXACT` | Quality enhancer |
| CON-036 | Little red bar takeout (and its mirror) | SRC-132, 133 | DIRECT | `EXACT` | Add trigger + standalone event |
| CON-037 | Gap fill | SRC-120, 122 | DIRECT | `EXACT` | Event substitution |
| CON-038 | Oversized-gap invalidation | SRC-121 | DIRECT | `EXACT` | Blocker |
| CON-039 | The 13 bar types / 14 actionable events | SRC-091, 263, 264 | UNSUPPORTED | `NONE` | — |

### Table B
| CON | Preconditions | Inputs / data | Objective formula | Timing | Repaint |
|---|---|---|---|---|---|
| CON-030 | `elephantLookback` bars of history | OHLC | `rng = high−low`; `body = abs(close−open)`; **AvgMultiple model (default):** `rng ≥ elephantRangeMult × sma(rng, elephantLookback)[1] and body ≥ elephantBodyFrac × rng`. **LocalMax model:** `rng > highest(rng, elephantLookback)[1] and body ≥ elephantBodyFrac × rng` | Bar close (`PROVISIONAL` intrabar if preview on) | `NONE` on close |
| CON-031 | CON-030, CON-020, leg counter | `legCount`, `pos`, `spaceRank` | `EXHAUSTION` if `legCount ≥ exhaustLegs or abs(pos) == 3 or spaceRank ≥ spacePct`; `IGNITING` if `legCount ≤ 1 and abs(pos) ≤ 2 and narrowWithin(emergenceLookback)`; else `UNCLASSIFIED` | Bar close | `NONE` |
| CON-032 | ≥1 bar | OHLC | `upTail = high − max(open,close)`; `dnTail = min(open,close) − low`. **Bottoming:** `dnTail ≥ tailMinFrac×rng and body ≤ tailBodyMaxFrac×rng and dnTail > upTail`. **Topping:** mirror | Bar close | `NONE` |
| CON-033 | ≥1 bar | OHLC | `body > 0.5×rng and max(upTail,dnTail) ≥ 0.15×rng` → the bar looks tail-like but fails CON-032 | Bar close | `NONE` |
| CON-034 | ≥1 opposite-colour bar in history | OHLC, `syminfo.mintick` | Reference = **body** extreme of the most recent opposite-colour bar (default) or of the whole opposite-colour run (alternative). Bull: `refHi = max(open,close)` of that red bar; trigger on a **green** bar where `high ≥ refHi + mintick`. Entry reference price = `refHi + mintick`. Bear: mirror with `refLo` | Bar close | `NONE` |
| CON-035 | CON-030 and CON-034 on the same bar | — | `elephantBull and colourChangeBull` | Bar close | `NONE` |
| CON-036 | ≥3 bars | OHLC, `mintick` | Long form: bar `n−2` is the first red after the reference point, bar `n−1` is **not** red, and bar `n` is green with `high ≥ high[2] + mintick`. Formally: `red[2] and not red[1] and green[0] and high ≥ high[2] + mintick`. Mirror for shorts | Bar close | `NONE` |
| CON-037 | Session-based symbol; new session bar; prior state NARROW | `open`, `close[1]`, session | `gapUp = open > high[1]` and `priorState == NARROW` → the gap is treated as elephant-bar-one; trigger = the first subsequent bar continuing in the gap direction (`close > open` for an up gap) | Bar close | `NONE` |
| CON-038 | CON-037 armed | CON-010 | `state == WIDE` on the gap bar → cancel; emit `GAP TOO LARGE` | Bar close | `NONE` |
| CON-039 | — | — | *(none)* | — | `N/A` |

### Table C
| CON | Invalidation | Precedence | Inputs & provenance | Module | Visual | Alert | QA |
|---|---|---|---|---|---|---|---|
| CON-030 | Per bar | Rank 2 in same-bar priority | `elephantModel`, `elephantLookback` 10, `elephantRangeMult` 1.8, `elephantBodyFrac` 0.5 | `§EVENT` | Bar highlight + `ELEPHANT ~` marker | `alertcondition` | QA-220, QA-221, QA-303 |
| CON-031 | Per bar | Modifies CON-030's label and quality | `exhaustLegs` 3 (**anchored** SRC-265), `emergenceLookback` 20 | `§EVENT` | Label becomes `IGNITING ELEPHANT ~` / `EXHAUSTION ELEPHANT ~` | separate alerts | QA-222 |
| CON-032 | Per bar | Rank 3 | `tailMinFrac` 0.5, `tailBodyMaxFrac` 0.35 | `§EVENT` | `BOTTOMING TAIL` / `TOPPING TAIL` marker | `alertcondition` | QA-223, QA-304 |
| CON-033 | Per bar | Suppresses nothing — it is purely informational | `showSnowman` (Debug only, default on in Debug) | `§EVENT` | Debug-only `SNOWMAN (NOT A TAIL)` | none | QA-224 |
| CON-034 | Reference re-anchors on each new opposite-colour bar | Rank 4; Rank 1 when combined with CON-030 | `ccRefModel` = Nearest \| RunExtreme; `requireTriggerColour` (default on) | `§EVENT` | `COLOUR CHANGE` marker + dashed reference line at the body extreme | `alertcondition` | QA-225, QA-226 |
| CON-035 | Per bar | **Rank 1** in same-bar priority (SRC-107) | none | `§EVENT` | `ELEPHANT + CHANGE` combined marker | `alertcondition` | QA-227 |
| CON-036 | Per bar | Rank 5; Rank 1 inside an active lifecycle (it is the add trigger) | none | `§EVENT` | `RED BAR TAKEOUT` / `GREEN BAR TAKEOUT` | `alertcondition` | QA-228 |
| CON-037 | Trigger found, or session ends, or CON-038 | Substitutes for CON-030 as "bar one" | `enableGapFill` (default on for session symbols) | `§EVENT` | Ghost box over the gap + `GAP FILL: BAR ONE` | `alertcondition` on the bar-two trigger | QA-229, QA-230 |
| CON-038 | New session | **Cancels** CON-037 | none | `§EVENT` | `GAP TOO LARGE` blocker chip | none | QA-230 |
| CON-039 | — | — | — | — | Debug note: "10 of 13 bar types and 11 of 14 events are absent from the source" | — | QA-231 |

**CON-039 omission reason.** SRC-091/263/264 record that the manual states these sets exist and
explicitly states their remaining members are **not in the material**. Creating detectors for them
would be fabricating source content. **Disposition: `REFERENCED-UNSPECIFIED`.** The indicator
surfaces this fact in Debug Mode so a user is never left assuming full coverage.

**Doji handling (CON-034).** A bar with `close == open` is neither green nor red. It does not
supply a colour-change reference and cannot be a trigger bar. The manual never addresses dojis;
this is a `PLATFORM_SAFEGUARD` and is disclosed in Debug.

---

## 7. CONCEPT MAP — ENTRIES AND ADDS (CON-040…049)

### Table A
| CON | Source term | SRC evidence | Src class | Impl class | Purpose |
|---|---|---|---|---|---|
| CON-040 | Emergence entry from a narrow state | SRC-054, 061, 063, 072 | DIRECT | `EXACT` | Primary entry |
| CON-041 | Near-the-20 continuation entry (regular state) | SRC-059, 061, 099, 108, 126 | DIRECT | `APPROX` | Secondary entry |
| CON-042 | Wide-state reversion entry | SRC-060, 061, 063, 074 | DIRECT | `EXACT` | Counter-trend entry |
| CON-043 | Mandatory add on the first colour change | SRC-123, 124, 125 | DIRECT | `EXACT` | Lifecycle add |
| CON-044 | Continued adds at/near the 20 | SRC-126 | DIRECT | `APPROX` | Lifecycle add |
| CON-045 | 1A and 1B | SRC-127 | DIRECT (rule) / SUBJECTIVE (proximity) | `APPROX` | Add-vs-new-trade decision |
| CON-046 | Entry method 1 — into the bar before it finishes | SRC-095, 193 | DIRECT | `NONE` | — |
| CON-047 | Entry method 2 — next bar clears the high | SRC-095, 131 | DIRECT | `EXACT` | Entry trigger |
| CON-048 | Hourly-bar entry timing (last 15–20 min) | SRC-130 | DIRECT | `NONE` | — |
| CON-049 | No event = no trade | SRC-025, 289 | DIRECT | `EXACT` | Hard blocker |

### Table B
| CON | Preconditions | Inputs / data | Objective formula | Timing | Repaint |
|---|---|---|---|---|---|
| CON-040 | `state == NARROW`, `abs(pos) == 1`, a qualifying event, CON-006 aligned | CON-010/020/030-036 | `narrowEntry = state==NARROW and abs(pos)==1 and anyEvent and trendAligned` | Bar close | `NONE` |
| CON-041 | `state == REGULAR`, `near20`, event, CON-006 aligned | CON-005, CON-034 | `regEntry = state==REGULAR and near20 and (colourChange or tailBar or elephant) and trendAligned` | Bar close | `NONE` |
| CON-042 | `state == WIDE`, `abs(pos) == 3`, **counter**-direction event | CON-010/020 | `revEntry = state==WIDE and abs(pos)==3 and counterEvent` — direction is *against* the prior move (SRC-074) | Bar close | `NONE` |
| CON-043 | Active lifecycle, no add yet | CON-034 | First `colourChange` in the lifecycle direction after entry | Bar close | `NONE` |
| CON-044 | Active lifecycle, `adds < maxAdds` | CON-034/030/032, `near20` | Subsequent qualifying event with `near20 == true` | Bar close | `NONE` |
| CON-045 | An ignite entry within `abProximityBars` | bar index | `barsSinceIgnite ≤ abProximityBars` → treat pullback as **1B add**; else → **second trade** | Bar close | `NONE` |
| CON-046 | — | — | *(intrabar; not observable at bar close)* | — | `N/A` |
| CON-047 | A prior qualifying event bar | `high`, `mintick` | `high ≥ eventBarHigh + mintick` on a later bar (long); mirror short | Bar close | `NONE` |
| CON-048 | — | — | *(intrabar clock position)* | — | `N/A` |
| CON-049 | — | CON-030–037 | `anyEvent == false` → block, regardless of state/position | Bar close | `NONE` |

### Table C
| CON | Invalidation | Precedence | Inputs & provenance | Module | Visual | Alert | QA |
|---|---|---|---|---|---|---|---|
| CON-040 | State leaves NARROW before trigger; or opposite event | Highest entry priority | none beyond upstream | `§ENTRY` | `ENTRY` marker + entry line | `alertcondition` (confirmed) | QA-240, QA-241 |
| CON-041 | Price leaves the near-20 zone | Below CON-040 | `near20Mult` 0.75 ATR — **pure addition** | `§ENTRY` | `ENTRY (REG) ~` marker | `alertcondition` | QA-242 |
| CON-042 | State leaves WIDE | Only entry type permitted from position 3 | none | `§ENTRY` | `REVERSION ENTRY` marker | `alertcondition` | QA-243 |
| CON-043 | Lifecycle ends | **Mandatory** — flagged if missed (SRC-123) | none | `§LIFECYCLE` | `ADD` marker; dashboard `ADD: DUE` when pending | `alertcondition` | QA-244 |
| CON-044 | `maxAdds` reached | After CON-043 | `maxAdds` 3 — `PLATFORM_SAFEGUARD` | `§LIFECYCLE` | `ADD` marker (smaller) | shares CON-043 alert | QA-245 |
| CON-045 | — | Decides lifecycle membership | `abProximityBars` 10 — **anchored** SRC-259 | `§LIFECYCLE` | `1A` / `1B` chips | — | QA-246 |
| CON-046 | — | — | — | — | Debug note explaining the omission | — | QA-247 |
| CON-047 | Event bar superseded | Fallback when the intrabar method is unavailable — which is always | none | `§ENTRY` | `TAKEOUT ENTRY` marker | shares entry alert | QA-248 |
| CON-048 | — | — | — | — | Debug note | — | QA-247 |
| CON-049 | — | **T1 blocker** | none | `§ENTRY` | Blocker chip `NO EVENT` | — | QA-249 |

**CON-046 / CON-048 omission reason.** Both are intrabar timing instructions ("buy into the bar
before it finishes trading"; "enter in the last 15 to 20 minutes of the hourly bar"). A bar-close
indicator cannot express a position within a forming bar in a way that survives the close, and an
intrabar evaluation would repaint. Rather than fabricate a proxy, both are recorded as
`NOT-IMPLEMENTED` with the reason shown in Debug Mode, and the source's own stated alternative
(CON-047, "take the next bar that clears its high", SRC-131) is implemented instead. Note the
manual itself supplies that fallback, so nothing is lost that the source does not also offer.

---

## 8. CONCEPT MAP — SPECIALISED SETUPS (CON-050…079)

### Table A
| CON | Source term | SRC evidence | Src class | Impl class | Purpose |
|---|---|---|---|---|---|
| CON-050 | Surge off the 200 | SRC-190, 191, 196 | DIRECT | `APPROX` | Named setup |
| CON-051 | Clear blue skies to the left | SRC-192, 324 | DIRECT (concept) / SUBJECTIVE (lookback) | `APPROX` | Setup qualifier |
| CON-052 | The purge | SRC-198 | DIRECT | `APPROX` | Setup enhancer |
| CON-053 | Merge + purge + surge | SRC-199 | DIRECT | `EXACT` | Highest-conviction composite |
| CON-054 | The stop ladder (thirds of the trigger bar) | SRC-151, 197 | DIRECT | `EXACT` | Stop validity |
| CON-055 | Legs and resets | SRC-201 | DIRECT (concept) / SUBJECTIVE (pause) | `APPROX` | Leg context |
| CON-056 | Leg-two pullback demotion | SRC-202 | DIRECT | `EXACT` | Quality gate |
| CON-057 | Igniting swing | SRC-205, 207, 208 | DIRECT | `APPROX` | Named setup |
| CON-058 | Pullback swing | SRC-206, 208 | DIRECT | `APPROX` | Named setup |
| CON-059 | Opening-bell sequence | SRC-210, 211 | DIRECT | `EXACT` | Named setup |
| CON-060 | Hidden green / hidden red play | SRC-213, 214 | DIRECT (concept) / SUBJECTIVE (intrabar erasure) | `APPROX` | Named setup |
| CON-061 | Dual space reversal | SRC-228, 229 | DIRECT | `APPROX` | Named setup |
| CON-062 | Retracement ladder (25/50/75/100%) | SRC-216, 258 | DIRECT levels / self-reported odds | `EXACT` levels, `DOCS` odds | Context overlay |
| CON-063 | Scalp definition (the 25% zone) | SRC-217, 271 | DIRECT | `EXACT` | Context classifier |
| CON-064 | Scalp-or-trade decision | SRC-218, 269, 295 | DIRECT | `EXACT` | Context classifier |
| CON-065 | Sucker play bounce | SRC-219, 321 | DIRECT | `EXACT` | Warning zone |
| CON-066 | Deep drop (50% halfway test) | SRC-232, 270 | DIRECT level / SUBJECTIVE severity | `APPROX` | Reversal warning |
| CON-067 | First warning drop | SRC-231 | DIRECT (concept) / SUBJECTIVE (magnitude) | `APPROX` | Reversal warning |
| CON-068 | Market Law Four (lower-high / lower-top) | SRC-233 | DIRECT (concept) / SUBJECTIVE (significance) | `APPROX` | Reversal warning |
| CON-069 | Full six-stage head and shoulders + neckline | SRC-230, 234, 236, 237 | DIRECT (narrative) / UNSUPPORTED (tolerances) | `NONE` | — |
| CON-070 | 20 MA halt | SRC-239, 325 | DIRECT (concept) / SUBJECTIVE | `APPROX` | Context marker |
| CON-071 | Space (price ↔ the 20) | SRC-220, 322 | DIRECT | `EXACT` | Measurement |
| CON-072 | Near add, away pare | SRC-227 | DIRECT | `APPROX` | Lifecycle context |
| CON-073 | Space extreme ("unusually large") | SRC-221, 227 | DIRECT (concept) / SUBJECTIVE (threshold) | `APPROX` | Pare context |
| CON-074 | Screening by the 20's direction | SRC-225 | DIRECT | `EXACT` (single-symbol form) | Directional filter |
| CON-075 | The colour game near the 20 (monthly form) | SRC-222, 223 | DIRECT | `EXACT` | Same detector as CON-034 |
| CON-076 | Tight picture of power | SRC-211 | DIRECT | `EXACT` | Composite context |
| CON-077 | Professional loss (one bar of risk) | SRC-155, 327 | DIRECT | `EXACT` | Stop geometry |
| CON-078 | The 18–20 minute window | SRC-212 | DIRECT | `EXACT` | Lifecycle context |
| CON-079 | Short-side preference | SRC-215 | DIRECT (preference) | `DOCS` | Not a gate |

### Table B
| CON | Preconditions | Inputs / data | Objective formula | Timing | Repaint |
|---|---|---|---|---|---|
| CON-050 | `flat(sma200)`, event bar is elephant **or** tail (SRC-196), origin near the 200, CON-051 | CON-002/005/030/032/051 | `surge = flat200 and (elephant or tail) and originNear200 and clearLeft` where `originNear200 = ` origin bar's extreme within the 200 band, or within `originTolBars` bars of such a bar | Bar close | `NONE` |
| CON-051 | `clearLookback` bars | `high`/`low` | Long: `close > highest(high, clearLookback)[1]`. Short: `close < lowest(low, clearLookback)[1]` | Bar close | `NONE` |
| CON-052 | CON-051 | — | Long: `low ≤ highest(high, clearLookback)[1] and close > highest(high, clearLookback)[1]` — one bar spanned it | Bar close | `NONE` |
| CON-053 | CON-017, CON-052, CON-050 | — | `merge and purge and surge` | Bar close | `NONE` |
| CON-054 | A trigger bar and a `maxLossPerUnit` input | OHLC, user max loss | `baseStop = low − mintick` (long). If `entryRef − baseStop ≤ maxLossPerUnit` → **rung 1**. Else `thirdStop = low + (high−low)/3`; if `entryRef − thirdStop ≤ maxLossPerUnit` → **rung 2**. Else → **rung 3 = SKIP** | Bar close | `NONE` |
| CON-055 | Confirmed pivots | `ta.pivothigh/low(pivLen,pivLen)` | A **leg** increments on each confirmed pivot extending the move. A **reset** = `resetBars` bars with `abs(close − close[resetBars]) ≤ resetDriftMult × atrUnit` and no new leg extreme. A reset increments `legCount` and re-arms the pullback | Confirmed `pivLen` bars later | `CONFIRMED-LATER` |
| CON-056 | CON-055 | `legCount` | `legCount ≥ 2` → pullback quality demoted; chip `LEG 2 PULLBACK ~` | Bar close | inherits `CONFIRMED-LATER` |
| CON-057 | CON-016 sleepy, elephant-sized bar preferred (SRC-207) | CON-016/030 | `ignite = sleepyPrev and (elephant or strongBar) and breaksOutOf(sleepyRange)` | Bar close | `NONE` |
| CON-058 | CON-034, `near20` | — | `pullbackSwing = colourChange and near20`; quality boosted if it follows an ignite within `abProximityBars` (SRC-208) | Bar close | `NONE` |
| CON-059 | Session symbol; new session; `state == NARROW` at the open; bar 1 opens outside the state with a qualifying elephant/tail | session, CON-010/020/030/032 | Bar 1: record `mark = high + mintick`, `stop = low − mintick`. No action on bar 1 (SRC-210 step 4). Entry on the first later bar with `high ≥ mark`. Session-gated to `openWindowBars` | Bar close | `NONE` |
| CON-060 | Session symbol; bar 1 opens **below** a narrow state and is green (or above and red) | OHLC, CON-010 | Bar-close form: mark `refLow = low` of that green bar; entry when a bar **closes red below** `refLow` (long-side mirror: closes green above `refHigh`). **The intrabar erasure instant is not represented** | Bar close | `NONE` (bar-close form) |
| CON-061 | Price, 20 and 200 all mutually separated | CON-010/071 | `dualSpace = spaceN ≥ dualSpaceMult and gapN ≥ wideMult`; trigger = counter-colour change; stop = beyond the recent counter-extreme; target = `sma20` (SRC-228) | Bar close | `NONE` |
| CON-062 | A confirmed prior leg ≥ `strongMoveAtr` | pivots | Levels at 25 / 50 / 75 / 100% of `legHigh − legLow` | `CONFIRMED-LATER` | `CONFIRMED-LATER` |
| CON-063 | CON-062 | — | The 25% band is labelled `THE SCALP` | Bar close | inherits |
| CON-064 | A completed prior bounce | pivots | `bounce% = (bounceHigh − legLow)/(legHigh − legLow)`. `< 50%` → `EXPECT CONTINUATION`; `≥ scalpTradePct` → `THIS IS A TRADE` ; between → `AMBIGUOUS` | `CONFIRMED-LATER` | `CONFIRMED-LATER` |
| CON-065 | CON-062 | — | The 50–100% band is labelled `SUCKER-PLAY ZONE` | Bar close | inherits |
| CON-066 | A confirmed up-leg then a pullback | pivots | `retrace% ≥ deepDropPct` → `DEEP DROP` | `CONFIRMED-LATER` | `CONFIRMED-LATER` |
| CON-067 | ≥2 prior pullbacks in the current leg sequence | pivots | Current pullback depth `>` every prior pullback depth in the sequence | `CONFIRMED-LATER` | `CONFIRMED-LATER` |
| CON-068 | A prior swing high, then a lower swing high, then a break of the intervening low | pivots | `lowerTop = pivotHigh < prevPivotHigh` and subsequently `close < interveningPivotLow` | `CONFIRMED-LATER` | `CONFIRMED-LATER` |
| CON-069 | — | — | *(no tolerances exist in the source)* | — | `N/A` |
| CON-070 | Price near the 20 with contracted range | CON-005 | `abs(close − sma20) ≤ near20Mult × atrUnit` for `haltBars` bars with `rng ≤ atrUnit` | Bar close | `NONE` |
| CON-071 | CON-001 | `close`, `sma20` | `space = abs(close − sma20)`; `spaceN = space / atrUnit` | Bar close | `NONE` |
| CON-072 | Active lifecycle | CON-071/073 | `near20` → `ADD CONTEXT`; `spaceExtreme` → `PARE CONTEXT` | Bar close | `NONE` |
| CON-073 | `spaceLookback` bars | CON-071 | `spaceRank = percentrank(space, spaceLookback) ≥ spacePct` | Bar close | `NONE` |
| CON-074 | CON-006 | — | `RISING` → long-only; `DECLINING` → short-only; `FLAT` → **discard** (SRC-225) | Bar close | `NONE` |
| CON-075 | — | — | Identical to CON-034 gated by `near20` | Bar close | `NONE` |
| CON-076 | CON-010, CON-030/032 | — | `state == NARROW and (elephant or tail)` | Bar close | `NONE` |
| CON-077 | CON-059 | — | `riskPerUnit = mark − stop` = one bar-one range + 2 ticks | Bar close | `NONE` |
| CON-078 | CON-059 active | bar index / session time | Elapsed since entry; warn past `≈18–20 min` equivalent in bars | Bar close | `NONE` |
| CON-079 | — | — | — | — | `N/A` |

### Table C (selected — full wiring for every CON in this domain)
| CON | Invalidation | Precedence | Inputs & provenance | Module | Visual | Alert | QA |
|---|---|---|---|---|---|---|---|
| CON-050 | 200 ceases flat; origin drifts beyond tolerance | Top-ranked setup (SRC-190) | `originTolBars` 2 (**anchored**) | `§SETUP` | `SURGE OFF 200 ~` label + origin marker | `alertcondition` | QA-250, QA-251 |
| CON-051 | New overhead/underfoot high/low | Required qualifier (SRC-192) | `clearLookback` 20 — **pure addition** | `§SETUP` | `CLEAR LEFT` chip + shaded left-scan region (Full/Debug) | — | QA-252 |
| CON-052 | Per bar | Enhancer | reuses `clearLookback` | `§SETUP` | `PURGE` chip | — | QA-253 |
| CON-053 | Any component fails | Marks `ELITE` eligibility | none | `§SETUP` | `MERGE+PURGE+SURGE` composite chip | `alertcondition` | QA-254 |
| CON-054 | New trigger bar | **Gate** — rung 3 blocks the setup entirely (SRC-197) | `maxLossPerUnit` (user, no source default) | `§STOP` | Trigger-bar thirds drawn; `OK` / `NOT YOUR TRADE` | — | QA-255, QA-256 |
| CON-055 | Move reverses past origin | Feeds CON-031, CON-056 | `resetBars` 3, `resetDriftMult` 1.0, `pivLen` 3 | `§CTX` | `LEG 1` / `RESET` / `LEG 2` chips | — | QA-257 |
| CON-056 | New reset | Demotes quality; never blocks outright (source says "questionable", not "forbidden") | none | `§QUALITY` | `LEG 2 PULLBACK ~` warning chip | — | QA-258 |
| CON-057 | Sleepy state broken without ignition | Swing family | `sleepyBars` 10 | `§SETUP` | `IGNITE` marker | `alertcondition` | QA-259 |
| CON-058 | Price leaves near-20 | Quality-boosted after CON-057 | `near20Mult` | `§SETUP` | `PULLBACK` marker | `alertcondition` | QA-260 |
| CON-059 | Session ends; window elapses; stop reference broken | Own lifecycle family; **bypasses CON-028** per SRC-078 | `openWindowBars` (default 10 bars ≈ 20 min on a 2-min chart) | `§SETUP` | Bar-1 box, mark line, stop line, `ENTER` marker | `alertcondition` ×3 | QA-261…QA-265 |
| CON-060 | Session ends without erasure | Alternative to CON-059 for wrong-colour opens | `enableHidden` (default on) | `§SETUP` | `HIDDEN GREEN ~` / `HIDDEN RED ~` + reference line | `alertcondition` | QA-266 |
| CON-061 | Space closes to the 20 | Reversion family only | `dualSpaceMult` 2.0 ATR — **pure addition** | `§SETUP` | `DUAL SPACE ~` + target line at the 20 | `alertcondition` | QA-267 |
| CON-062 | New leg supersedes | Context only — never an entry by itself | `strongMoveAtr` 3.0 — **pure addition**; `showOdds` | `§CTX` | Four dashed levels, each labelled with the **attributed** source claim | none | QA-268 |
| CON-063 | ″ | ″ | `scalpTargetPct` 25 (**anchored**) | `§CTX` | `THE SCALP` band | none | QA-268 |
| CON-064 | New bounce | Context classifier | `scalpTradePct` 65 (**anchored**) | `§CTX` | Dashboard `SCALP / TRADE` row | `alertcondition` | QA-269 |
| CON-065 | ″ | Warning only | none | `§CTX` | `SUCKER-PLAY ZONE` shaded band (Full/Debug) | none | QA-268 |
| CON-066 | New up-leg | Warning; contributes a blocker to with-trend longs | `deepDropPct` 65 (**cross-context anchor**) | `§REV` | `DEEP DROP ~` marker + 50% line | `alertcondition` | QA-270 |
| CON-067 | New leg sequence | Warning | none | `§REV` | `FIRST WARNING DROP ~` marker | `alertcondition` | QA-271 |
| CON-068 | New high above the prior top | Warning | `mlfSignifMult` 0.25 ATR — **pure addition** | `§REV` | `MARKET LAW FOUR ~` marker | `alertcondition` | QA-272 |
| CON-069 | — | — | — | — | Debug note explaining the omission | — | QA-273 |
| CON-070 | Price leaves the zone | Context | `haltBars` 3 — **pure addition** | `§CTX` | `20 MA HALT ~` chip (Analysis+) | none | QA-274 |
| CON-071 | — | Feeds CON-072/073, CON-087, CON-089 | none | `§CTX` | Optional space rung from price to the 20 | none | QA-275 |
| CON-072 | Lifecycle ends | Context only — never sizes anything | none | `§LIFECYCLE` | Dashboard `NEAR: ADD` / `AWAY: PARE` | `alertcondition` on pare context | QA-276 |
| CON-073 | Rank falls | Feeds CON-031 exhaustion and CON-072 | `spaceLookback` 250, `spacePct` 90 | `§CTX` | `SPACE EXTREME ~` chip | shares CON-072 alert | QA-277 |
| CON-074 | Slope sign changes | **T2 hard filter** | `enforce20Direction` (default **on**) | `§CTX` | Dashboard `TREND`; blocker `AGAINST THE 20` | none | QA-220 |
| CON-075 | — | Same detector as CON-034 | — | `§EVENT` | shares CON-034 visuals | shares | QA-225 |
| CON-076 | — | Feeds quality | none | `§QUALITY` | `TIGHT PICTURE OF POWER` chip | none | QA-278 |
| CON-077 | Lifecycle ends | Opening-bell only | none | `§STOP` | `1-BAR RISK` chip on the stop line | none | QA-262 |
| CON-078 | Window elapses | Context only — never force-closes a lifecycle | `openWindowBars` | `§LIFECYCLE` | Dashboard `WINDOW: n/10` | `alertcondition` on expiry | QA-263 |
| CON-079 | — | **Deliberately not a gate** — implementing an asymmetric preference would break SRC-076 mirroring | none | — | README only | none | QA-279 |

**CON-069 omission reason.** The manual describes the six-stage pattern narratively (SRC-230) but
supplies no tolerance for shoulder height equality, no bar-count window, no neckline slope
allowance, and explicitly warns against mechanising it: *"Treating it as a mechanical checklist …
the shoulders do not have to be equal, usually but not always. The pattern is probabilistic. A
slanted right shoulder still counts"* (SRC-237, p.27). Building a detector would require inventing
every one of those tolerances against a direct instruction not to. **Disposition:
`NOT-IMPLEMENTED`.** The three components the manual *does* quantify or define comparably —
the 50% deep-drop test (CON-066), the first-warning-drop comparison (CON-067) and the lower-high
structure of Market Law Four (CON-068) — are implemented as approximations instead. The neckline
(SRC-236) is not drawn, because it depends on identifying the two pullback lows of a pattern that
is not detected.

---

## 9. CONCEPT MAP — STOPS AND TRADE MANAGEMENT (CON-080…099)

### Table A
| CON | Source term | SRC evidence | Src class | Impl class | Purpose |
|---|---|---|---|---|---|
| CON-080 | Initial stop beyond the state | SRC-077, 153 | DIRECT | `EXACT` | Initial stop |
| CON-081 | The dumb stop (blocker) | SRC-077, 281 | DIRECT | `EXACT` | Validity gate |
| CON-082 | Big bar / fat bar stop | SRC-142 | DIRECT | `APPROX` | Trailing reference |
| CON-083 | Colour adjustment stop | SRC-143, 328 | DIRECT | `EXACT` | Trailing reference |
| CON-084 | Pivot stop + hand-back to the 20 | SRC-144, 320 | DIRECT (rule) / SUBJECTIVE (pivot) | `APPROX` | Trailing reference |
| CON-085 | Moving-average trail (the 20) | SRC-145 | DIRECT | `EXACT` | Trailing reference |
| CON-086 | The 8 escalation | SRC-033, 145 | DIRECT (rule) / SUBJECTIVE (acceleration) | `APPROX` | Trailing reference |
| CON-087 | Bar-by-bar stop | SRC-146 | DIRECT (rule) / SUBJECTIVE (separation) | `APPROX` | Trailing reference |
| CON-088 | The rotation rule | SRC-140, 148, 158 | DIRECT | `EXACT` | Stop resolver |
| CON-089 | Method selection by distance from the 20 | SRC-147 | DIRECT (rule) / SUBJECTIVE (distances) | `APPROX` | Method eligibility |
| CON-090 | Opening-bell one-bar stop | SRC-155, 078 | DIRECT | `EXACT` | Initial stop (exception) |
| CON-091 | Igniting-swing stop references | SRC-153 | DIRECT | `EXACT` | Initial stop |
| CON-092 | Pullback-swing stop references ("the V") | SRC-154 | DIRECT | `EXACT` | Initial stop |
| CON-093 | Why pivots alone fail | SRC-149 | DIRECT | `DOCS` | Rationale for CON-088 |
| CON-094 | Named reference sets (swing vs opening bell) | SRC-150 | DIRECT | `SAFE` | Method-set selection |
| CON-095 | Never leave the stop static | SRC-140, 157, 290 | DIRECT | `EXACT` | Lifecycle invariant |

### Table B
| CON | Preconditions | Inputs / data | Objective formula | Timing | Repaint |
|---|---|---|---|---|---|
| CON-080 | Arming lifecycle | `stateBot`/`stateTop` | Long: `stateBot − mintick`. Short: `stateTop + mintick` | Bar close | `NONE` |
| CON-081 | A candidate stop | CON-020 | Long: reject if `stop ≥ stateBot`. Short: reject if `stop ≤ stateTop`. Bypassed for CON-059 lifecycles | Bar close | `NONE` |
| CON-082 | An elephant-sized bar since entry | CON-030 | Long: `low − mintick` of the most recent qualifying bar | Bar close | `NONE` |
| CON-083 | Pattern `counter, dir, dir` | OHLC | Long: `red[2] and green[1] and green[0]` → `low[2] − mintick`. Short: `green[2] and red[1] and red[0]` → `high[2] + mintick` | Bar close | `NONE` |
| CON-084 | A confirmed pivot that price has moved away from | `ta.pivotlow(pivLen,pivLen)` | Long: `pivotLow − mintick`, adopted on the **confirmation bar**. Released once `sma20 > pivotLow` (SRC-144) | `CONFIRMED-LATER` | `CONFIRMED-LATER` |
| CON-085 | — | `sma20` | Long: `sma20 − band` | Bar close | `NONE` |
| CON-086 | Acceleration test true | `sma8`, `sma20`, `close` | `accel = (close − close[accelLen]) > accelMult × (sma20 − sma20[accelLen])` and both positive → use `sma8 − band`; release when `close < sma8` | Bar close | `NONE` |
| CON-087 | `spaceN ≥ barByBarSpaceMult` | OHLC | Long: `low[1] − mintick`, updated every bar | Bar close | `NONE` |
| CON-088 | ≥1 eligible reference | all of CON-082…087 | Long: `stop = max(currentStop, max(eligible references))` — monotonic, never loosens. Short: `min(...)` | Bar close | inherits worst of contributors |
| CON-089 | CON-071 | `spaceN` | `spaceN ≤ near20Mult` → fat-bar eligible; `spaceN ≥ barByBarSpaceMult` → bar-by-bar eligible; `accel` → the 8 eligible. The 20, pivot and colour-adjust are always eligible | Bar close | `NONE` |
| CON-090 | CON-059 | bar-1 OHLC | Long: `bar1Low − mintick` | Bar close | `NONE` |
| CON-091 | CON-057 | sleepy range | Either `min(sleepyLow, sma200 − band) − mintick` (whole base) or `igniteBarLow − mintick` (tighter) | Bar close | `NONE` |
| CON-092 | CON-058 | pullback low | `pullbackLow − mintick` or the pivot | Bar close | `NONE` |
| CON-093 | — | — | — | — | `N/A` |
| CON-094 | — | — | Swing set = {pivot, big bar, MA}; opening-bell set = {fat bar, colour adjust}. `ALL` = every eligible reference | Bar close | `N/A` |
| CON-095 | Active lifecycle | — | Assert `stop` is monotonic in the protective direction for the lifecycle's life | Bar close | `NONE` |

### Table C
| CON | Invalidation | Precedence | Inputs & provenance | Module | Visual | Alert | QA |
|---|---|---|---|---|---|---|---|
| CON-080 | Superseded by trailing | Initial only | none | `§STOP` | Solid stop line, labelled `INITIAL STOP` | none | QA-280 |
| CON-081 | — | **Hard gate** | `enforceDumbStop` (on) | `§STOP` | Blocker chip `DUMB STOP` | none | QA-232, QA-281 |
| CON-082 | New qualifying bar | Display precedence rank 3 | reuses CON-030 inputs | `§STOP` | `FAT BAR` on the stop label | `alertcondition` (stop moved) | QA-282 |
| CON-083 | New pattern | Rank 4 | none | `§STOP` | `COLOUR ADJUST` | shares | QA-283 |
| CON-084 | Hand-back to the 20 when `sma20` passes it | Rank 2 | `pivLen` 3 — **pure addition** | `§STOP` | `PIVOT` + a marker on the originating pivot bar drawn on the confirmation bar | shares | QA-284, QA-305 |
| CON-085 | — | Rank 5 (default) | none | `§STOP` | `20 MA` | shares | QA-285 |
| CON-086 | `close < sma8` | Rank 6 | `accelLen` 5, `accelMult` 2.0 — **pure additions** | `§STOP` | `8 MA` + the 8 becomes visible | shares | QA-286 |
| CON-087 | `spaceN` falls back | Rank 7 | `barByBarSpaceMult` 2.0 — **pure addition** | `§STOP` | `BAR-BY-BAR` | shares | QA-287 |
| CON-088 | Lifecycle ends | **Resolver** — output is the single displayed stop | `stopMethodSet` = ALL \| Swing \| OpeningBell | `§STOP` | One current-stop line + method name; step markers on change | `alertcondition` | QA-288, QA-289 |
| CON-089 | Per bar | Controls which references enter CON-088 | reuses above | `§STOP` | Debug list of eligible/ineligible methods | none | QA-290 |
| CON-090 | Lifecycle ends | Replaces CON-080 for opening-bell only | none | `§STOP` | `1-BAR RISK` stop line | none | QA-262 |
| CON-091 | ″ | Swing family | `swingStopRef` = WholeBase \| IgniteBar | `§STOP` | Labelled stop line | none | QA-291 |
| CON-092 | ″ | Swing family | `pullbackStopRef` = V \| Pivot | `§STOP` | Labelled stop line | none | QA-291 |
| CON-093 | — | — | — | — | README | none | — |
| CON-094 | — | `PLATFORM_SAFEGUARD` — the source does not say which set applies off its own timeframes (§10.2 A12) | `stopMethodSet` default `ALL` | `§STOP` | Dashboard `STOP SET` | none | QA-292 |
| CON-095 | — | Invariant | none | `§STOP` | — | none | QA-293 |

---

## 10. CONCEPT MAP — EXITS, LIFECYCLE, PLATFORM AND EXPERIMENTAL (CON-100…135)

### Table A
| CON | Source term | SRC evidence | Src class | Impl class | Purpose |
|---|---|---|---|---|---|
| CON-100 | Push counting | SRC-170, 265 | DIRECT (concept) / UNSUPPORTED (mechanics) | `APPROX` | Exit context |
| CON-101 | Three-push exit | SRC-170, 172 | DIRECT | `APPROX` | Exit context |
| CON-102 | Push-two conditional right | SRC-171 | DIRECT (rule) / SUBJECTIVE ("unusually large") | `APPROX` | Exit context |
| CON-103 | New high / new low right | SRC-173 | DIRECT (rule) / SUBJECTIVE ("the initial move") | `APPROX` | Exit context |
| CON-104 | Stop-out profit take | SRC-174 | DIRECT | `EXACT` | Exit context |
| CON-105 | Narrow→wide exit | SRC-062, 180 | DIRECT | `EXACT` | Exit context |
| CON-106 | Position 1 → 2 → 3 progression | SRC-129 | DIRECT | `EXACT` | Exit context |
| CON-107 | Place the exit order ahead of the market | SRC-175, 176 | DIRECT | `NONE` | — |
| CON-108 | Snatching at your roses | SRC-177, 178, 179 | DIRECT | `DOCS` | Behavioural |
| CON-110 | Virtual methodology lifecycle | — (project construct) | n/a | `SAFE` | State machine |
| CON-111 | Confluence / signal-quality layer | — (project construct) | n/a | `SAFE` | Ranking |
| CON-112 | Blocking-reason engine | SRC-029, 280–296 | DIRECT (the blockers) | `EXACT` | Diagnostics |
| CON-113 | Live checklist | SRC-028, 029, 158 | DIRECT | `EXACT` | Diagnostics |
| CON-114 | Timeframe-to-lesson advisory | SRC-240 | DIRECT | `EXACT` | Context |
| CON-115 | Higher-timeframe 20-direction context | SRC-248 | DIRECT | `APPROX` (optional, off by default) | Context |
| CON-116 | Non-standard chart-type warning | — | n/a | `SAFE` | Reliability |
| CON-117 | Warm-up guard | — | n/a | `SAFE` | Reliability |
| CON-118 | Anti-clutter resolver | — | n/a | `SAFE` | Rendering |
| CON-119 | Drawing lifecycle manager | — | n/a | `SAFE` | Rendering |
| CON-120 | Alert bridge | — | n/a | `SAFE` | Alerts |
| CON-121 | Source-statistics reference panel | SRC-250–272 | DIRECT (as attributed claims) | `DOCS` | Reference |
| CON-122 | Three size tiers | SRC-272, 200 | DIRECT | `DOCS` | Reference |
| CON-123 | Risk-sizing mathematics | SRC-300 | UNSUPPORTED | `NONE` | — |
| CON-124 | Watchlist screening across symbols | SRC-246, 225 | DIRECT (as a routine) | `NONE` | — |
| CON-125 | Psychology, discipline and maxims | SRC-011, 297, 177 | DIRECT | `DOCS` | Reference |
| CON-130 | **EXPERIMENTAL** — percentile-rank state model | — | n/a | `EXPER` | Alternative state math |
| CON-131 | **EXPERIMENTAL** — confluence density meter | — | n/a | `EXPER` | Exploration |
| CON-132 | **EXPERIMENTAL** — HTF 20-direction as a hard gate | — | n/a | `EXPER` | Exploration |

### Table B
| CON | Preconditions | Inputs / data | Objective formula | Timing | Repaint |
|---|---|---|---|---|---|
| CON-100 | Active lifecycle, confirmed pivots | `ta.pivothigh/low(pivLen,pivLen)` | A push increments when a confirmed pivot in the trade direction exceeds the previous such pivot since entry | `CONFIRMED-LATER` | `CONFIRMED-LATER` |
| CON-101 | CON-100 | `pushCount` | `pushCount ≥ 3` → `EXIT CONTEXT: THIRD PUSH` | ″ | ″ |
| CON-102 | `pushCount == 2` | leg sizes | `push2Size ≥ push2LargeMult × push1Size` → `PUSH 2 RIGHT ~` | ″ | ″ |
| CON-103 | Active lifecycle | `initialPeak` | Long: `high > initialPeak` where `initialPeak` = highest high from entry to the first confirmed counter-pivot | ″ | ″ |
| CON-104 | Active lifecycle | CON-088 | `low ≤ stop` (long) while the lifecycle is in profit vs entry reference | Bar close | `NONE` |
| CON-105 | Entry state was NARROW | CON-010 | `state == WIDE` | Bar close | `NONE` |
| CON-106 | Active lifecycle | CON-020 | Position moved 1 → 2 → 3 since entry | Bar close | `NONE` |
| CON-107 | — | — | *(order routing)* | — | `N/A` |
| CON-110 | — | all detectors | See `04_INDICATOR_ARCHITECTURE.md §6` | Bar close | `NONE` |
| CON-111 | all upstream | weighted additive model, §11.W23 | See §11 worksheet W23 | Bar close | inherits |
| CON-112 | all upstream | — | Ordered list of failed gates for the current candidate | Bar close | `NONE` |
| CON-113 | — | all upstream | Each checklist item mapped to a computed boolean or `n/a` | Bar close | `NONE` |
| CON-114 | — | `timeframe.period` | Map the chart TF to the manual's §1.9 row | On last bar | `N/A` |
| CON-115 | `useHtf == true` | `request.security(syminfo.tickerid, htfTf, ta.sma(close,20)[1], lookahead=barmerge.lookahead_off)` | Slope sign of the confirmed HTF SMA20 | HTF close | `HTF-DELAY` |
| CON-116 | — | `chart.is_heikinashi` etc. | Warn if a non-standard chart type is active | On last bar | `N/A` |
| CON-117 | — | `bar_index` | Suppress all signals until `bar_index ≥ warmupBars` (default 200 + `slopeLen` + `spaceLookback` when space is used) | Bar close | `NONE` |
| CON-118 | — | — | See `03_VISUAL_DESIGN_SPEC.md §6` | Bar close | `N/A` |
| CON-119 | — | — | Ring buffers with hard caps | Bar close | `N/A` |
| CON-120 | `barstate.isconfirmed` by default | — | See `04_INDICATOR_ARCHITECTURE.md §9` | Bar close | `NONE` |
| CON-121 | — | — | Static attributed text | On last bar | `N/A` |
| CON-122 | — | — | Static attributed text | On last bar | `N/A` |
| CON-130 | `fidelity == EXPERIMENTAL` | `gapN` | `percentrank(gapN, expStateLookback)` bucketed into narrow/regular/wide | Bar close | `NONE` |
| CON-131 | ″ | event flags | Rolling count of supported events over `expDensityLen` | Bar close | `NONE` |
| CON-132 | ″ and `useHtf` | CON-115 | Blocks entries whose direction opposes the HTF 20 slope | HTF close | `HTF-DELAY` |

### Table C
| CON | Invalidation | Precedence | Inputs & provenance | Module | Visual | Alert | QA |
|---|---|---|---|---|---|---|---|
| CON-100 | Lifecycle ends | Feeds CON-101/102 | `pivLen` 3 | `§EXIT` | `PUSH 1/2/3` markers | none | QA-294 |
| CON-101 | ″ | Exit context, never a forced close | none | `§EXIT` | `EXIT CONTEXT: 3RD PUSH ~` | `alertcondition` | QA-295 |
| CON-102 | ″ | Advisory | `push2LargeMult` 1.5 — **pure addition** | `§EXIT` | `PUSH 2 RIGHT ~` | none | QA-296 |
| CON-103 | ″ | Advisory (a right, not an obligation — SRC-173) | none | `§EXIT` | `NEW HIGH RIGHT ~` | `alertcondition` | QA-297 |
| CON-104 | ″ | Terminal transition | none | `§EXIT` | `STOP-OUT` marker; lifecycle → `COMPLETED` | `alertcondition` | QA-298 |
| CON-105 | ″ | **Fires regardless of anything else** | none | `§EXIT` | `EXIT: NARROW→WIDE` | `alertcondition` | QA-205 |
| CON-106 | ″ | Context | none | `§EXIT` | Dashboard `POSITION PATH 1→2→3` | none | QA-299 |
| CON-107 | — | — | — | — | Debug note | none | QA-247 |
| CON-108 | — | — | — | — | README | none | — |
| CON-110 | — | Governs all lifecycle visuals | `maxLifecycles` 1, `allowReversal` off | `§LIFECYCLE` | Active-lifecycle panel | — | QA-231…QA-239 |
| CON-111 | — | Never gates; only ranks | full weight table exposed | `§QUALITY` | Quality tier with `[impl]` marker | `alertcondition` per tier | QA-320…QA-324 |
| CON-112 | — | — | none | `§DEBUG` | Blocker chips + Debug list | none | QA-325 |
| CON-113 | — | — | `showChecklist` | `§DEBUG` | Debug checklist panel | none | QA-326 |
| CON-114 | — | Advisory only — **never blocks**, because C3 says the framework is TF-agnostic (SRC-022) | `tfAdvisory` (on) | `§CTX` | Dashboard `TF LESSON` row | none | QA-327 |
| CON-115 | — | Context only unless CON-132 enabled | `useHtf` (**off**), `htfTf` | `§CTX` | Dashboard `HTF 20` row with `⌛` delay marker | none | QA-306, QA-328 |
| CON-116 | — | — | none | `§GUARD` | One-time warning label | none | QA-307 |
| CON-117 | — | Blocks all output | `warmupBars` | `§GUARD` | `WARMING UP` dashboard state | none | QA-308 |
| CON-118 | — | — | see spec | `§DRAW` | — | none | QA-330…QA-334 |
| CON-119 | — | — | `historyCap` 300 | `§DRAW` | — | none | QA-335 |
| CON-120 | — | — | `alertMode`, per-event toggles, `cooldownBars` | `§ALERT` | — | — | QA-340…QA-345 |
| CON-121 | — | Reference only | `showSourceClaims` (Debug) | `§DEBUG` | Attributed claims panel | none | QA-329 |
| CON-122 | — | Reference only | — | `§DEBUG` | Attributed text | none | QA-329 |
| CON-123 | — | — | — | — | README §Known limitations | none | QA-273 |
| CON-124 | — | — | — | — | README | none | QA-273 |
| CON-125 | — | — | — | — | README | none | — |
| CON-130 | — | **Off by default**; replaces CON-010 math when on | `expStateLookback` 500 | `§EXP` | Dashboard tagged `EXPERIMENTAL` | none | QA-350 |
| CON-131 | — | **Off by default**; never gates | `expDensityLen` 20 | `§EXP` | Density row | none | QA-351 |
| CON-132 | — | **Off by default**; gates entries when on | — | `§EXP` | Blocker chip `HTF OPPOSED [exp]` | none | QA-352 |

**CON-107 omission reason.** "Put the sell order into the market ahead of price" (SRC-175) is an
order-routing instruction. An indicator has no orders. **Disposition: `NOT-IMPLEMENTED`.** The
underlying *observation* it depends on — that push two has completed — is surfaced (CON-102), so
the user retains the information the rule acts on.

**CON-123 / CON-124 omission reasons.** SRC-300 records that the manual contains no position-sizing
mathematics at all, and SRC-246 describes a multi-symbol watchlist routine. Implementing either
would require inventing risk mathematics the source disclaims having, or scanning symbols outside
the chart. Both are `NOT-IMPLEMENTED` with the reason surfaced in the README.

---

## 11. APPROXIMATION WORKSHEETS

Every subjective, implied or operationally incomplete concept that this project nevertheless
renders gets a worksheet. Format is fixed: **what the source says → why it is not objective →
candidate models → chosen default → observable behaviour → failure modes → why it is not official.**

---

### W1 · APPROX-01 — "Relatively close together" (state width)
**Source (SRC-051, pp.8, 54):** *"Are the 20 and the 200 relatively close together? If yes, the
state is narrow. They do not have to touch. Velez is emphatic that this is an eyeball judgement
and warns against measuring the gap with a tool."* And p.54: *"Do not get a geometric tool out to
measure the gap … Close does not mean touching."*

**Why not objective:** The source does not merely omit a threshold — it contains an explicit
instruction **not to create one**. Any number here is a deliberate departure.

**Candidates**

| Model | Formula | Behaviour | Trade-off |
|---|---|---|---|
| **M1 · ATR-normalised gap** | `abs(sma20−sma200)/atr ≤ k` | Adapts to volatility regime; comparable across instruments and timeframes | ATR is itself a foreign concept to the source; behaves oddly on very illiquid bars |
| M2 · Percent-of-price gap | `abs(sma20−sma200)/close ≤ k%` | Simple, scale-free across price levels | Ignores volatility regime — a 1% gap is enormous on a bond, trivial on a small-cap |
| M3 · Percentile rank of the gap | `percentrank(gap, N) ≤ p` | "Narrow *for this instrument*", closest in spirit to the "biggest space in 10 years" framing (SRC-221) | Requires a long lookback; guarantees a fixed share of bars are "narrow" regardless of whether any truly are — which the source's frequency claim (SRC-058) does not support |

**Chosen default: M1 with `narrowMult = 1.0`, `wideMult = 3.0`.** Selected because it is the only
candidate that is simultaneously volatility-aware, instrument-comparable and free of the
guaranteed-hit-rate artefact of M3. M2 is offered as `stateBasis = %price`; M3 is offered as the
experimental CON-130 so its artefact is visible rather than hidden in the default.

**Inputs:** `stateBasis`, `narrowMult`, `wideMult`, `ATR_LEN`.

**Observable behaviour:** The dashboard `STATE` row changes between `NARROW ~ / REGULAR ~ / WIDE ~`.
Raising `narrowMult` produces more narrow states and therefore more position-one candidates.

**Failure modes:** (a) During a volatility collapse ATR shrinks and a previously-wide gap can be
reclassified narrow with no change in the averages — visible as a state flip on a quiet bar.
(b) Immediately after a gap or a limit move ATR spikes and genuinely narrow states read as
regular. (c) On the first bars after warm-up the ATR is unstable; CON-117 suppresses output.

**Why it must not be presented as official:** The manual instructs the reader not to measure this.
A measured threshold is the opposite of what the source teaches, however useful it is for a chart.
Always rendered with `~`.

---

### W2 · APPROX-02 — "Flat" versus "sloping"
**Source (SRC-035/036/055, pp.6–7, 53–54):** the 20 is *"strongest when sloping, ideally near 45°"*
and *"weakest when flat"*; the 200 is the reverse; grade-1 narrow needs *"20 flat, 200 flat … and
the stock itself flat"*.

**Why not objective:** "Flat" has no numeric definition anywhere. "Near 45°" is a rendering
artefact of chart scaling, not a property of the data, and is therefore unusable (§10.4).

**Candidates**

| Model | Formula | Behaviour | Trade-off |
|---|---|---|---|
| **M1 · ATR-normalised per-bar slope** | `abs(s − s[n])/n/atr ≤ t` | Volatility-aware; symmetric; cheap | Sensitive to `n`; a slow curve reads flat over a short `n` |
| M2 · Linear-regression slope + R² | `abs(slope) ≤ t and r² < r` | Distinguishes "flat" from "noisy but trending" | Much more expensive; two thresholds instead of one; harder to explain |
| M3 · Range containment | `(highest(s,n) − lowest(s,n))/atr ≤ t` | Directly captures "has not gone anywhere" | Insensitive to a steady slow drift, which visually reads as sloping |

**Chosen default: M1, `slopeLen = 10`, `flatThresh = 0.05` ATR/bar.** Chosen for explainability —
a single threshold with a stated meaning ("the average moves less than 5% of one ATR per bar").
M3 is used *in addition* for the third item in grade-1 narrow, because "the stock itself flat and
**sandwiched between them**" (SRC-055) is a containment statement, not a slope statement, and is
implemented as containment within the state band.

**Inputs:** `slopeLen`, `flatThresh`.

**Observable behaviour:** Drives the `20 MA STATUS` / `200 MA STATUS` dashboard rows and the
narrow-state grade. A higher `flatThresh` makes grade-1 narrow states more common.

**Failure modes:** (a) A moving average rolling over through a top is momentarily "flat" at the
turn — grade may briefly read G1 on a genuine reversal. (b) On very long timeframes `slopeLen = 10`
spans a long calendar period and rarely reads flat. (c) Immediately after a gap the SMA step
creates a one-off slope spike.

**Why not official:** The source gives no threshold and its one quantitative hint ("45°") is not
computable. Rendered with `~`.

---

### W3 · APPROX-03 — Moving-average band half-width
**Source (SRC-042/003, pp.7, 37, 71):** *"if he built his own platform he would shade a coloured
band on each side of the line so traders stopped misreading blips as breaks."* The manual adopts
this rendering itself.

**Why not objective:** The concept is DIRECT and stated twice. The **width** is never given —
not by Velez, not by the manual.

**Candidates:** M1 `band = k × atr` (volatility-scaled) · M2 `band = k% × price` · M3 `band =
k × stdev(close − sma, n)` (statistical dispersion of price around the average).

**Chosen default: M1, `bandAtrMult = 0.25`.** M1 keeps one normalisation basis across the whole
indicator, which matters because the band participates in state, position and stop geometry — a
second basis would create inconsistent zone edges. M3 is arguably the most principled but adds a
second volatility concept and a second lookback for no gain in explainability.

**Inputs:** `bandAtrMult`, `showBands`.

**Observable behaviour:** Band width on the chart; position-zone edges move with it; `CON-028`
stop validity depends on it.

**Failure modes:** A wide band swallows position one on quiet instruments, pushing entries into
position two. Disclosed in the tooltip.

**Why not official:** The width is this project's number entirely.

---

### W4 · APPROX-04 — Position band boundaries
**Source (SRC-071, p.10):** +1 *"immediately above"*, +2 *"not close, not far"*, +3 *"wide"*.
The ladder diagram draws them as stacked bands of roughly equal height, but SRC-005 records that
diagram geometry is illustrative and cannot be used to derive measurements.

**Why not objective:** Three ordinal descriptions, no distances.

**Candidates:** M1 fixed ATR multiples from the state edge · M2 multiples of the **state width**
itself (`gap`) · M3 percentile rank of the distance over a lookback.

**Chosen default: M1, `p1Max = 1.0`, `p2Max = 2.5` ATR.** M2 is conceptually attractive — position
scales with the state that produced it — but it degenerates badly: in a very tight narrow state
(the highest-quality state, SRC-055) the state width approaches zero and position one collapses to
a sliver, which inverts the source's intent that the best states produce the best position-one
entries. M2 is offered as `posBasis = StateWidth` for users who want it, with that caveat in the
tooltip.

**Inputs:** `posBasis`, `p1Max`, `p2Max`.

**Observable behaviour:** The `POSITION` dashboard row and the optional zone boxes; determines
whether an event is tradable, secondary, or Pluto land.

**Failure modes:** A single wide bar can jump price from position one to position three, skipping
two — the lifecycle records the skip rather than interpolating.

**Why not official:** The bands are stated only as words.

---

### W5 · APPROX-05 — "Visibly larger and taller than the bars around it"
**Source (SRC-092, p.12):** the elephant-bar definition. SRC-191 adds *"often a large-bodied
elephant bar"*.

**Why not objective:** No ratio, no neighbourhood size, no separation of range from body.

**Candidates**

| Model | Formula | Behaviour | Trade-off |
|---|---|---|---|
| **M1 · Average multiple** | `rng ≥ m × sma(rng, n)[1]` | Smooth, tolerant of one prior outlier; predictable frequency | After one huge bar the average rises, briefly suppressing the next elephant |
| M2 · Local maximum | `rng > highest(rng, n)[1]` | Literal reading of "larger than the bars around it"; very clean | Fires on the largest bar of any quiet stretch, including tiny ones — needs a floor |
| M3 · Percentile rank | `percentrank(rng, N) ≥ p` | Instrument-relative | Guarantees a fixed hit rate whether or not any bar is genuinely large |

**Chosen default: M1 with `elephantRangeMult = 1.8`, `elephantLookback = 10`, plus
`elephantBodyFrac = 0.5`.** M1 is chosen over the more literal M2 specifically because M2 has no
absolute floor and would label the biggest bar in a dead range an institutional footprint —
directly contradicting the source's rationale that only size capable of moving price creates one
(SRC-093). M2 is offered as `elephantModel = LocalMax` with an added `rng ≥ atr` floor. The body
fraction is included because both the text ("large-bodied", SRC-191) and the diagram (SRC-109)
show solid bodies, and because without it a wide-range doji — the opposite of a conviction bar —
would qualify.

**Inputs:** `elephantModel`, `elephantLookback`, `elephantRangeMult`, `elephantBodyFrac`.

**Observable behaviour:** Bar highlight and `ELEPHANT ~` markers. Raising the multiple reduces
count sharply.

**Failure modes:** (a) The bar after a volatility expansion is under-detected because the
lookback average has already risen. (b) On instruments with frequent gaps the range includes the
gap and inflates detection at the open. (c) Illiquid bars with one print produce degenerate ranges;
CON-117 and a `rng > 0` guard handle the zero case.

**Why not official:** "Visibly" is the source's word. 1.8× is this project's.

---

### W6 · APPROX-06 — "Most of the bar must be tail"
**Source (SRC-096, pp.12, 55):** *"Most of the bar must be tail. The body is the small part. A bar
that is mostly body with a wick hanging off it is not a tail bar."*

**Why not fully objective:** This is the **most** specified of the event definitions — "most"
carries a defensible literal reading of >50%, and the counterexample is explicit. What is missing
is the exact ratio for "the body is the small part" and whether the *other* tail matters.

**Candidates:** M1 tail-fraction only (`tail ≥ f × rng`) · **M2 tail-fraction + body-cap +
dominance** · M3 tail-to-body ratio (`tail ≥ r × body`).

**Chosen default: M2 — `tailMinFrac = 0.50`, `tailBodyMaxFrac = 0.35`, and the signal tail must
exceed the opposite tail.** M1 alone admits a long-legged doji with two large tails, which is a
balance bar, not a rejection bar. M3 explodes toward infinity as body → 0 and needs a special case.
M2 encodes all three of the source's statements: "most of the bar is tail" (0.50), "the body is
the small part" (0.35), and the directional reading that the *bottoming* tail is the one below
(dominance).

**Inputs:** `tailMinFrac`, `tailBodyMaxFrac`, `tailRequireDominance`.

**Observable behaviour:** `BOTTOMING TAIL` / `TOPPING TAIL` markers; the CON-033 snowman diagnostic
shows near-misses in Debug so the boundary is inspectable.

**Failure modes:** A bar with `rng == 0` (a flat print) is excluded by guard. On instruments with
tick-level noise, tails at the extremes of the session can qualify frequently.

**Why not official:** 0.50 and 0.35 are chosen numbers, even if they are the most defensible in
the whole project.

---

### W7 · APPROX-07 — "Near the 20"
**Source (SRC-223, p.42):** *"The signal only counts near the 20. A little above, a little below,
or right on it are all fine. **He does not care which.**"*

**Why not objective:** The source states the condition and then explicitly declines to bound it.

**Candidates:** M1 `abs(close − sma20) ≤ k × atr` · M2 `close` inside the 20's band (CON-004) ·
M3 `abs(close − sma20)` below its own rolling median.

**Chosen default: M1, `near20Mult = 0.75` ATR.** M2 is the tightest and most internally consistent
(it reuses the band), but at `bandAtrMult = 0.25` it is far stricter than "a little above, a little
below" and would reject most of the signals the manual's own p.42 diagram marks `VALID`. M1 at
0.75 ATR is deliberately looser to match that diagram's evident tolerance. M2 is offered as
`near20Model = Band`.

**Inputs:** `near20Model`, `near20Mult`.

**Observable behaviour:** Gates CON-041, CON-044, CON-058, CON-075. Visible as the `NEAR 20` chip.

**Failure modes:** In a fast trend the 20 lags and price is rarely "near" it, suppressing
regular-state entries — which is arguably correct behaviour but can read as the indicator being
silent.

**Why not official:** The source says it does not care which; this project had to pick.

---

### W8 · APPROX-08 — "Clear blue skies to the left"
**Source (SRC-192/324, pp.37, 59):** *"No significant price data immediately to the left of the
surge … He acknowledges that going back far enough always finds something. He means the recent
past."*

**Why not objective:** "The recent past" and "significant" are both unbounded.

**Candidates:** **M1 fixed bar lookback** with a "clears the highest high" test · M2 lookback
scaled to the state's age (bars since the state became narrow) · M3 volume-profile-style test of
how much price time was spent overhead.

**Chosen default: M1, `clearLookback = 20` bars.** M2 is appealing because it ties the scan window
to the setup that produced it, but it makes the qualifier unstable — the same chart geometry
qualifies or fails depending on how long the state had been narrow, which the source does not
suggest. M3 requires per-price-level accounting that Pine can do only crudely and expensively, and
the source says "price data", not "volume".

**Inputs:** `clearLookback`.

**Observable behaviour:** The `CLEAR LEFT` chip on surge labels; in Full/Debug the scanned region
is shaded so the user can see exactly what "the recent past" meant.

**Failure modes:** A 20-bar window on a monthly chart is 20 months, which is not "immediately to
the left" in any intuitive sense — the tooltip warns that this input is timeframe-sensitive and
should be reconsidered per chart.

**Why not official:** The source explicitly refuses to bound it.

---

### W9 · APPROX-09 — The reset / pause
**Source (SRC-201, p.38):** *"A brief pause acts as a reset … The pause can be sideways or a
shallow drift down at roughly 45 degrees. He is explicit that both forms accomplish the same
thing."*

**Why not objective:** "Brief" is unbounded; "roughly 45 degrees" is a rendering artefact (§10.4).

**Candidates:** **M1 net-displacement containment** over `resetBars` · M2 range containment
(`highest−lowest ≤ k×atr`) · M3 consecutive-bar-range contraction.

**Chosen default: M1, `resetBars = 3`, `resetDriftMult = 1.0` ATR** — i.e. over three bars price
has net-moved less than one ATR. M1 is chosen precisely because it captures **both** forms the
source insists are equivalent: a sideways pause and a shallow drift both have small net
displacement, whereas M2's range test rejects a drifting pause and M3 rejects a pause made of
ordinary-sized bars.

**Inputs:** `resetBars`, `resetDriftMult`.

**Observable behaviour:** `RESET` chip; increments `LEG` count; re-arms pullback eligibility;
feeds CON-031's igniting/exhaustion classification.

**Failure modes:** A sharp two-bar consolidation inside a strong trend registers as a reset and
promotes the move to leg two, demoting the next pullback (CON-056) earlier than a discretionary
reader would.

**Why not official:** Duration and displacement are both this project's numbers.

---

### W10 · APPROX-10 — What is a "push"
**Source (SRC-170, pp.16, 29, 51):** *"Count pushes from entry. Come out on the third."*

**Why not objective:** **Nowhere in 71 pages is a push mechanically defined.** This is the single
largest definitional hole in an otherwise well-specified exit rule. The manual's p.16 diagram
shows three visually obvious up-legs and no measurement.

**Candidates**

| Model | Formula | Behaviour | Trade-off |
|---|---|---|---|
| **M1 · Confirmed higher-pivot count** | Each confirmed pivot in the trade direction exceeding the previous one | Robust against noise; matches the diagram's visual reading | Confirmed `pivLen` bars late — push three is recognised after the fact |
| M2 · Consecutive same-colour runs | Each run of ≥k same-colour bars | Immediate, no confirmation lag | Extremely noisy; a choppy advance counts five pushes where a reader sees one |
| M3 · ZigZag on an ATR threshold | Reversal of ≥k ATR defines a leg | Tunable, immediate | Introduces a second, independent swing definition alongside CON-084's pivots — two different "swings" on one chart |

**Chosen default: M1 with `pivLen = 3`.** M1 is selected for consistency: the same pivot definition
already serves CON-084's pivot stop, CON-055's legs and CON-062's retracement legs, so the chart
never shows two contradictory notions of a swing. The cost — recognition lag — is disclosed
explicitly, and push markers are drawn on the confirmation bar, never backdated.

**Inputs:** `pivLen`, `showPushes`.

**Observable behaviour:** `PUSH 1/2/3` markers; the `EXIT CONTEXT` transition at push three.

**Failure modes:** (a) On a straight-line advance with no intervening pivots, push count stays at
zero and the three-push exit never triggers — the narrow→wide exit (CON-105) and the trailing stop
remain the operative exits, which is consistent with the source's three-exit design. (b) A `pivLen`
that is too small counts micro-swings as pushes.

**Why not official:** The source supplies the rule and none of the mechanics.

---

### W11 · APPROX-11 — "Beyond the initial move"
**Source (SRC-173, p.16):** *"Once price makes a fresh peak beyond the initial move (or a fresh
trough when short), you have earned the right to take money off the table."*

**Why not objective:** "The initial move" is not delimited.

**Candidates:** **M1 entry → first confirmed counter-pivot** · M2 entry → first close against the
trade direction · M3 a fixed bar count from entry.

**Chosen default: M1.** M2 triggers on the first ordinary red bar in a healthy advance, making
"the initial move" one or two bars long and the "new high right" nearly immediate — which drains
the rule of meaning. M3 is arbitrary in a way the source gives no basis for. M1 reuses the existing
pivot machinery.

**Inputs:** `pivLen` (shared).

**Observable behaviour:** `NEW HIGH RIGHT ~` chip once exceeded.

**Failure modes:** Inherits pivot confirmation lag.

**Why not official:** The boundary is inferred.

---

### W12 · APPROX-12 — "Accelerates faster than the 20 can track"
**Source (SRC-145, pp.14, 29):** *"If price accelerates faster than the 20 can track, switch to the
8-period average and ride until price breaks the 8."*

**Why not objective:** "Faster than the 20 can track" is descriptive.

**Candidates:** **M1 comparative displacement** (`Δclose > m × Δsma20` over `n`) · M2 space
expansion for `k` consecutive bars · M3 `close` above `sma8` and `sma8` above `sma20` with both
rising.

**Chosen default: M1, `accelLen = 5`, `accelMult = 2.0`.** M1 is the most literal rendering of the
sentence — price is outrunning the average — and it needs no second concept. M3 is cheaper but
describes a *configuration*, not an *acceleration*, and would engage the 8 in any ordinary trend,
which contradicts the source's framing of the 8 as situational (SRC-033).

**Inputs:** `accelLen`, `accelMult`.

**Observable behaviour:** The 8 SMA becomes visible; the stop label changes to `8 MA`; it
disengages when `close < sma8`, exactly as the source states.

**Failure modes:** A single gap bar can satisfy M1 spuriously; a `close`-based test rather than a
`high`-based one limits this.

**Why not official:** Both numbers are additions.

---

### W13 · APPROX-13 — "Separated from the 20 and lost that support"
**Source (SRC-146, pp.14, 29):** bar-by-bar trailing engages *"once price has separated from the 20
and lost that nearby support … it is dangling in mid-air."*

**Candidates:** **M1 space threshold** (`spaceN ≥ k`) · M2 space percentile rank · M3 bars since
price last touched the 20's band.

**Chosen default: M1, `barByBarSpaceMult = 2.0` ATR.** M2 duplicates CON-073's machinery for a
different purpose and would make the stop method depend on a 250-bar history, which is fragile
early on a chart. M3 is attractive and cheap but says nothing about *how far* — price can hover
just above the band for many bars without dangling.

**Inputs:** `barByBarSpaceMult`.

**Observable behaviour:** Stop method switches to `BAR-BY-BAR`; the stop tightens sharply.

**Failure modes:** Oscillating around the threshold causes method flapping between bars; the
rotation rule's monotonic `max` (CON-088) prevents the *stop* from loosening even when the
*method label* flaps, and the label change markers are rate-limited by CON-118.

**Why not official:** The distance is this project's.

---

### W14 · APPROX-14 — "An unusually large space"
**Source (SRC-221/227, pp.41–43):** *"the space between Apple and its monthly 20 was larger than
any space produced in the previous ten years. He treats that as actionable."*

**Why not objective:** The source establishes that space is judged **against that instrument's own
history**, but gives no lookback and no threshold. "Larger than any in ten years" is one worked
example, not a rule (§8).

**Candidates:** **M1 percentile rank over a lookback** · M2 absolute ATR multiple · M3 strict
all-time maximum over the lookback (the literal reading of the Apple example).

**Chosen default: M1, `spaceLookback = 250`, `spacePct = 90`.** M3 is the most literal but fires
so rarely as to be useless as a live context field, and SRC-302 warns that the single example
shown cannot be used to calibrate. M2 ignores the instrument-relative framing the source is
explicit about. M1 preserves the framing and is tunable toward M3 by raising `spacePct` to 100.

**Inputs:** `spaceLookback`, `spacePct`.

**Observable behaviour:** `SPACE EXTREME ~` chip; `AWAY: PARE` dashboard context; contributes to
CON-031's exhaustion classification.

**Failure modes:** A 250-bar lookback is meaningless before bar 250; CON-117 suppresses until then.
On a monthly chart 250 bars is ~20 years, which exceeds the example's ten.

**Why not official:** Lookback and percentile are both chosen.

---

### W15 · APPROX-15 — "Severely breaks the halfway point"
**Source (SRC-232, p.24):** *"Take the up-move and split it in half. If the pullback severely
breaks that halfway point, it is a deep drop."* Described by the manual as *"the only quantified
rule in this lesson"* — but the quantification is the **50% level**, not the severity.

**Candidates:** M1 a fixed ATR buffer beyond 50% · **M2 a retracement-percentage threshold** ·
M3 close-based confirmation beyond 50% for `k` bars.

**Chosen default: M2 with `deepDropPct = 65%`.** The 65% figure is **borrowed from the manual's
own V6.5 statement** that a bounce clearing 50% *"by a significant margin, toward 65 or 70%"*
changes the read (SRC-269) — the only place in 71 pages where the manual quantifies a significant
break of the 50% line. Using it here is a cross-context inference by this project and is labelled
as such wherever it appears. M1 makes the threshold scale-dependent in a way that a percentage of
the move is not; M3 adds a time dimension the source does not mention.

**Inputs:** `deepDropPct`.

**Observable behaviour:** `DEEP DROP ~` marker plus a drawn 50% line on the measured move.

**Failure modes:** The measured move depends on confirmed pivots, so a deep drop is labelled after
confirmation, not as it happens — which is precisely the opposite of the source's intent that you
train on *catching it forming* (SRC-231). This limitation is stated in the tooltip and the README.

**Why not official:** The manual never states 65% in this context.

---

### W16 · APPROX-16 — "A genuinely strong, one-directional prior move"
**Source (SRC-258, p.19; SRC-217, p.46):** the retracement odds *"require a genuinely strong,
one-directional prior move."*

**Candidates:** **M1 leg size in ATR** · M2 leg size plus a monotonicity test (no counter-move
within the leg exceeding x%) · M3 leg size relative to recent legs.

**Chosen default: M1, `strongMoveAtr = 3.0`.** M2 is a closer reading of "one-directional" and is
offered as `requireMonotonic` (off by default) because it substantially reduces the number of legs
that qualify, and the source gives no basis for how clean is clean enough.

**Inputs:** `strongMoveAtr`, `requireMonotonic`.

**Observable behaviour:** Whether the retracement ladder is drawn at all.

**Failure modes:** On low-volatility instruments almost every leg qualifies; on high-volatility
ones almost none.

**Why not official:** "Genuinely strong" is the source's phrase; 3 ATR is this project's.

---

### W17 · APPROX-17 — Which opposite-colour bar supplies the colour-change level
**Source (SRC-104, p.13):** *"It does not need to be back to back. Two or three reds can print
first. What counts is the first green bar that takes out **a** red high."*

**Why ambiguous:** With three reds printed, there are three candidate levels and the source says
"a red high" without saying which.

**Candidates:** **M1 nearest** (most recent opposite-colour bar's body extreme) · M2 run extreme
(the highest body high across the whole red run) · M3 the first red of the run.

**Chosen default: M1.** Rationale: the source says the *first green bar that takes out a red high*.
In a declining sequence the most recent red has the lowest body high, so M1 is the level that is
taken out first — making M1 the reading that satisfies "first" for the widest range of chart
shapes. M2 is materially stricter (it waits for the whole run to be reclaimed) and is offered as
`ccRefModel = RunExtreme` for users who want confirmation over immediacy. M3 has no support at all
and is not offered.

**Inputs:** `ccRefModel`, `requireTriggerColour`.

**Observable behaviour:** The dashed reference line drawn at the marked body extreme, and therefore
when the `COLOUR CHANGE` marker prints.

**Failure modes:** In a choppy sideways range M1 produces frequent colour changes; the state and
position gates (which the source requires anyway, SRC-108) suppress most of them from becoming
entries.

**Why not official:** The source leaves the choice open; this project made one.

---

### W18 · APPROX-18 — Igniting versus exhaustion elephant
**Source (SRC-235, p.26):** *"Nothing about the candle itself tells you which one you are looking
at. Only its location in the trend does."* The manual's illustration contrasts *"flat state to its
left. Nothing above it"* with *"same candle, five legs into the move."*

**Why not objective:** "Early" and "late" are relational; "five legs" appears in an illustration
caption (and in C2's failure-mode description), not as a rule.

**Candidates:** **M1 composite: leg count OR position OR space rank** · M2 leg count alone ·
M3 bars-since-narrow-state alone.

**Chosen default: M1 with `exhaustLegs = 3`.** M1 is chosen because the source's own two
illustrations differ on *three* dimensions at once — legs, what is overhead, and distance from the
state — so a single-dimension test would misclassify the very examples the manual uses. The leg
threshold is anchored to SRC-265 ("three to five pushes … three happens more often"), which is the
nearest thing to a stated count; note the illustration says five, so the default of 3 is the more
conservative (earlier-warning) choice and the input allows 5.

**Inputs:** `exhaustLegs`, `emergenceLookback`.

**Observable behaviour:** The elephant label reads `IGNITING ELEPHANT ~`, `EXHAUSTION ELEPHANT ~`,
or plain `ELEPHANT ~` when neither test is satisfied. **Deliberately, "unclassified" is a visible
outcome** rather than forcing every bar into one of the two categories.

**Failure modes:** Immediately after a reset the leg count increments, so a genuine fresh ignition
off a second base can read as exhaustion. The `~` marker and the Debug breakdown expose which of
the three tests fired.

**Why not official:** Location is the source's criterion; the thresholds are this project's.

---

### W19 · APPROX-19 — 1A/1B proximity
**Source (SRC-127, p.34):** *"If the pullback happens close to the igniting move, take both and
treat the second as an add … If they are far apart, treat them as trade one and trade two."*

**Candidates:** **M1 bar count** · M2 whether price stayed inside the box (CON-026) · M3 whether a
reset occurred between them.

**Chosen default: M1, `abProximityBars = 10`**, anchored to the source's own 8–12 bar average
holding period (SRC-259) as the nearest available notion of "one trade's worth of bars". M2 is
conceptually attractive and is offered as `abModel = Box`; M3 conflates two separate source
concepts.

**Observable behaviour:** `1B` chip on an add versus a new `ENTRY` marker starting a second
lifecycle.

**Failure modes:** On a chart where the ignite→pullback distance sits near 10 bars, small
parameter changes flip an add into a separate trade.

**Why not official:** Proximity is the source's word; 10 bars is an inference.

---

### W20 · APPROX-20 — Origin tolerance from the 200
**Source (SRC-195, p.38):** *"The move can start a bar or two away from the 200 and still qualify,
as long as the origin is not far from the zone. Originating directly at the 200 is preferred and
stronger."*

**Why partly objective:** "A bar or two" **is** a number — one of the few the manual supplies. What
is missing is "not far from the zone" as a price distance.

**Chosen default:** `originTolBars = 2` (**anchored** directly to the text) combined with the
requirement that the qualifying bar's extreme fell inside the 200's band (CON-004). Preference is
recorded: origins directly at the 200 receive a quality bonus, matching "preferred and stronger".

**Failure modes:** The band width (W3) drives what "at the 200" means, so this inherits W3's
uncertainty.

**Why not official:** The bar count is the source's; the price tolerance is the band's, which is
this project's.

---

### W21 · APPROX-21 — The hidden green erasure point
**Source (SRC-213, p.51):** *"The moment that green is fully erased by red, **whether inside the
same bar or on a later bar**, that erasure point becomes the entry."*

**Why not objective at bar close:** The source's own emphasis is on watching a bar *form* — "one
minute in, less green; one minute fifteen, less green". A bar-close indicator sees only the
finished bar. The intrabar erasure instant is unobservable and any intrabar approximation would
repaint.

**Candidates:** **M1 bar-close-only** (the bar closes red below the marked reference) · M2 intrabar
`PROVISIONAL` marker that may vanish · M3 lower-timeframe `request.security` reconstruction of the
intrabar path.

**Chosen default: M1**, with M2 available strictly as a labelled `PROVISIONAL` preview that never
alerts and never advances the lifecycle. M3 is rejected outright: reconstructing the intrabar path
would introduce a second data request, a second confirmation regime, and a strong repaint surface
for a marginal gain, and the source's precision here cannot be recovered anyway.

**Observable behaviour:** `HIDDEN GREEN ~` prints on the close of the erasing bar, with a note in
Debug that the true source entry is intrabar.

**Failure modes:** The bar-close entry reference will differ — usually unfavourably — from the
intrabar erasure point the source describes. **This is stated in the label tooltip and the README
rather than hidden.**

**Why not official:** The implemented trigger is a different event from the one taught.

---

### W22 · APPROX-22 — Pivot length
**Source:** none. SRC-144 uses "a new swing low"; the manual never defines a swing.

**Chosen default: `pivLen = 3`**, applied uniformly to CON-055, CON-062, CON-066, CON-084 and
CON-100 so that one notion of "swing" governs the whole chart. There is **no source anchor
whatsoever**; 3 is chosen as the smallest length that resists single-bar noise while remaining
responsive on the 8–12 bar holding horizon the source cites (SRC-259).

**Repaint consequence:** every pivot-derived object is `CONFIRMED-LATER` by construction. All such
objects are drawn on the confirmation bar and carry a `⌛` (ASCII `[c]`) marker. **No pivot marker
is ever backdated onto the originating bar without that marker.**

---

### W23 · APPROX-23 — The confluence / signal-quality layer *(implementation-defined)*

**This is not a Velez taxonomy and is never presented as one.** The manual ranks setups
qualitatively (grades of narrow, C9; position bias, C13; ignite-then-pullback, V3.4; merge+purge+
surge, V4.6; size tiers, V4.7) but never defines a score. This layer is a transparent additive
model built **only** from source-supported dimensions.

**Weights — published in full, adjustable, and shown in Debug:**

| Factor | Source | Points |
|---|---|---|
| State = NARROW grade 1 | SRC-055 | +3 |
| State = NARROW grade 2 | SRC-056 | +2 |
| State = NARROW grade 3 | SRC-057 | +1 |
| State = REGULAR with `near20` | SRC-059, 061 | +1 |
| Position = ±1 | SRC-072 | +3 |
| Position = ±2 | SRC-074 | +1 |
| Event = elephant **and** colour change | SRC-107 | +3 |
| Event = elephant | SRC-092 | +2 |
| Event = tail bar | SRC-096 | +2 |
| Event = colour change | SRC-102 | +1 |
| Event = red/green bar takeout | SRC-133 | +1 |
| Aligned with the 20's direction | SRC-224 | +2 |
| Origin at a flat 200 (surge) | SRC-190 | +2 |
| Clear space to the left | SRC-192 | +1 |
| Merge + purge + surge together | SRC-199 | +2 |
| Pullback follows an ignition | SRC-208 | +1 |
| Leg 1 (rather than leg 2+) | SRC-202 | +1 |

**Gates (a gate makes the candidate ineligible, it does not subtract points):**
no event (SRC-025) · inside the state (SRC-075) · against the 20 (SRC-224, when enforced) ·
with-trend entry from position 3 (SRC-284) · stop inside position one (SRC-077) ·
stop ladder rung 3 (SRC-197) · three-finger spread for swing setups (SRC-292).

**Tiers:** `WATCH` 1–3 · `DEVELOPING` 4–6 · `SETUP` 7–9 · `HIGH QUALITY` 10–12 ·
`ELITE SETUP` 13+.

**Tie-breaking (deterministic):** higher state grade → better position → higher event rank →
earlier leg → lower bar index. Published so two runs on the same data always agree.

**Why it must not be presented as official:** the weights are invented. Velez never scores a
setup numerically. Every tier string is rendered with the `[impl]` marker and the dashboard
carries a `CONFLUENCE (impl layer)` label. **The score never gates anything** — it only ranks
what the source's own gates have already allowed.

---

## 12. CANDIDATE CALLOUT AUDIT

Every label the brief proposed, tested against the manual before use.

| # | Candidate label | Concept supported? | Exact term in source? | Verdict | Rendered as |
|---|---|---|---|---|---|
| 1 | `ELEPHANT BAR` | ✅ SRC-092 | ✅ "Elephant bar" | **Use** | `ELEPHANT ~` |
| 2 | `IGNITING ELEPHANT` | ✅ SRC-235 | ✅ "Igniting elephant" | **Use** | `IGNITING ELEPHANT ~` |
| 3 | `EXHAUSTION ELEPHANT` | ✅ SRC-235 | ✅ "Exhaustion elephant" | **Use** | `EXHAUSTION ELEPHANT ~` |
| 4 | `TAIL BAR` | ✅ SRC-096 | ✅ "Tail bar" | **Use** | as `BOTTOMING`/`TOPPING TAIL` |
| 5 | `BOTTOMING TAIL` | ✅ SRC-098 | ✅ | **Use** | `BOTTOMING TAIL` |
| 6 | `TOPPING TAIL` | ✅ SRC-098 | ✅ | **Use** | `TOPPING TAIL` |
| 7 | `COLOUR CHANGE` | ✅ SRC-102 | ✅ (British spelling preserved) | **Use** | `COLOUR CHANGE` |
| 8 | `HIDDEN GREEN` | ✅ SRC-213 | ✅ "Hidden green play" | **Use** (shortened) | `HIDDEN GREEN ~` |
| 9 | `GAP FILL` | ✅ SRC-120 | ✅ "Gap fill" | **Use** | `GAP FILL: BAR ONE` |
| 10 | `POSITION 1` | ✅ SRC-072 | ✅ "Position one" | **Use** | `POSITION 1` |
| 11 | `POSITION 2` | ✅ SRC-074 | ✅ "Position two" | **Use** | `POSITION 2` |
| 12 | `POSITION 3` | ✅ SRC-074 | ✅ "Position three" | **Use** | `POSITION 3` |
| 13 | `NARROW STATE` | ✅ SRC-050 | ✅ "Narrow state" | **Use** | `NARROW ~` |
| 14 | `WIDE STATE` | ✅ SRC-060 | ✅ "Wide state" | **Use** | `WIDE ~` |
| 15 | `STATE SHIFT` | ✅ concept (SRC-052, 062) | ❌ **not a source term** | **Use with disclosure** — the transition is source-supported, the phrase is not | `STATE CHANGE [impl]` |
| 16 | `SURGE` | ✅ SRC-190 | ✅ "Surge off the 200" | **Use** (shortened) | `SURGE OFF 200 ~` |
| 17 | `FLAT 200 SETUP` | ✅ SRC-190 | ❌ the source names it "the surge off the 200" | **Replace** with the source's own name (SRC-196 explains *why* he named it that) | `SURGE OFF 200 ~` |
| 18 | `200 MA REJECTION` | ⚠️ partially — SRC-038 says the 200 is a floor/ceiling | ❌ not a source term; "rejection" implies a distinct detector the source never defines | **Do not use as an event label** | *(not rendered)* |
| 19 | `CLEAR BLUE SKIES` | ✅ SRC-192 | ✅ "Clear blue skies to the left" | **Use** (shortened) | `CLEAR LEFT` |
| 20 | `LEG 1` | ✅ SRC-201 | ✅ "Leg one" | **Use** | `LEG 1` |
| 21 | `LEG 2` | ✅ SRC-201 | ✅ "Leg two" | **Use** | `LEG 2` |
| 22 | `THREE PUSHES` | ✅ SRC-170 | ✅ "Three pushes" | **Use** | `PUSH 1/2/3` |
| 23 | `POSSIBLE EXHAUSTION` | ⚠️ the *concept* exists (SRC-235) | ❌ not a source phrase; "possible" is hedging language the source does not use | **Replace** — use the source's own term | `EXHAUSTION ELEPHANT ~` |
| 24 | `ADD` | ✅ SRC-123 | ✅ "Adding", "the add" | **Use** | `ADD` |
| 25 | `ENTRY` | ✅ SRC-095 | ✅ "Entry" | **Use** | `ENTRY` |
| 26 | `STOP` | ✅ SRC-140 | ✅ | **Use** | `STOP` + method name |
| 27 | `PROFIT MANAGEMENT` | ✅ SRC-170–179 | ⚠️ the manual's heading is "Taking profits"; "profit management" is not a source phrase | **Use only as a UI section heading**, never as a chart marker | dashboard section |
| 28 | `BIG BAR STOP` | ✅ SRC-142 | ✅ "Big bar stop" | **Use** | `FAT BAR` (also a source term) |
| 29 | `FAT BAR` | ✅ SRC-142 | ✅ "fat bar stop" | **Use** | `FAT BAR` |
| 30 | `BAR-BY-BAR STOP` | ✅ SRC-146 | ✅ "Bar by bar stop" | **Use** | `BAR-BY-BAR` |
| 31 | `20 MA TRAIL` | ✅ SRC-145 | ⚠️ the source says "moving average trailing stop" / "trail under the 20" | **Use** — a faithful compression | `20 MA` |
| 32 | `8 MA TRAIL` | ✅ SRC-145, SRC-033 | ⚠️ compression of "switch to the 8-period average" | **Use** | `8 MA` |
| 33 | `TOO EXTENDED` | ⚠️ concept exists (SRC-079 Pluto land, SRC-209 three fingers, SRC-203) | ❌ **not a source term** and it flattens three distinct source concepts into one vague phrase | **Do not use.** Render the specific source concept that actually fired | `PLUTO LAND` / `THREE FINGERS` / `SPACE EXTREME ~` |
| 34 | `LATE ENTRY WARNING` | ⚠️ concept exists (SRC-203, SRC-209) | ❌ not a source term | **Do not use** — same flattening problem | `THREE FINGERS` / `LEG 2 PULLBACK ~` |

**Summary: 26 accepted as-is or as disclosed shortenings · 4 replaced with the source's own term ·
3 rejected outright (`200 MA REJECTION`, `TOO EXTENDED`, `LATE ENTRY WARNING`) · 1 restricted to a
section heading (`PROFIT MANAGEMENT`).**

**Emoji policy.** Emoji appear only as optional decoration. Every glyph has an ASCII fallback and
`useEmoji` can be switched off entirely:
`~` approximate (no emoji equivalent — always ASCII) · `⌛`/`[c]` confirmed-later ·
`▲`/`^` long · `▼`/`v` short · `■`/`#` blocked. **No meaning is carried by an emoji alone.**

---

## 13. SOURCE-TERM VERSUS UI-LABEL REGISTER

| UI string rendered | Status | Basis |
|---|---|---|
| `ELEPHANT`, `BOTTOMING TAIL`, `TOPPING TAIL`, `COLOUR CHANGE`, `BULL 180`, `SNOWMAN`, `PLUTO LAND`, `POSITION 1/2/3`, `NARROW`, `WIDE`, `REGULAR`, `MERGE`, `PURGE`, `RESET`, `LEG 1/2`, `THE BOX`, `SLEEPY`, `PIVOT`, `FAT BAR`, `COLOUR ADJUST`, `BAR-BY-BAR`, `DEEP DROP`, `MARKET LAW FOUR`, `20 MA HALT`, `GAP FILL`, `SPACE`, `NEAR: ADD`, `AWAY: PARE`, `IGNITING ELEPHANT`, `EXHAUSTION ELEPHANT`, `HIDDEN GREEN`, `HIDDEN RED`, `THREE FINGERS` | **Source term** | Glossary pp.59–61 and body |
| `CLEAR LEFT`, `NARROW G1/G2/G3`, `DUAL SPACE`, `IGNITE`, `PULLBACK`, `RED BAR TAKEOUT`, `1-BAR RISK`, `SUCKER-PLAY ZONE`, `SURGE OFF 200` | **UI shortening of a source term** | Full term in dashboard/Debug |
| `WATCH`, `DEVELOPING`, `SETUP`, `HIGH QUALITY`, `ELITE SETUP`, `CONFLUENCE`, `STATE CHANGE`, `EXIT CONTEXT`, `VIRTUAL LIFECYCLE`, `BLOCKED` | **Implementation-defined** | Always carries `[impl]`; never attributed to the source |
| `~` prefix | **Approximation marker** | Anything depending on an added threshold |
| `[c]` / `⌛` | **Confirmed-later marker** | Anything pivot-derived |

---

## 14. FIDELITY-LAYER MEMBERSHIP

| Layer | What is emitted as an actionable marker | What is still computed |
|---|---|---|
| **1 · Strict source mechanics only** | Only events whose **trigger** is fully specified by the source may open a lifecycle: colour change (CON-034), red/green bar takeout (CON-036), strict tail bar (CON-032), and the opening-bell bar-1 sequence (CON-059). **The elephant detector is excluded from entries here**, because its size test needs an invented multiple — elephant bars are still *drawn* as context. Also active: narrow→wide exit (CON-105), the stop-ladder thirds (CON-054), retracement levels (CON-062). **State and position are computed and displayed but do not gate signals**, because gating requires a threshold the source refuses to supply | Everything — shown as context with `~` |
| **2 · Source + documented approximations** *(default)* | The complete funnel. State and position gate everything per C1/SRC-020. All `APPROX` detectors active | Everything |
| **3 · + experimental** | Layer 2 plus CON-130/131/132 | Everything |

**Layer 1 is deliberately *noisier in raw events and free of invented gating*; Layer 2 is quieter
and more faithful to the method's intent but depends on added thresholds.** That trade-off is
stated in the README so the choice is informed rather than cosmetic.

**Display modes never alter signal logic.** Switching between Clean / Analysis / Full / Debug
changes only what is drawn. The fidelity layer is the only control that changes which signals
exist.

---

## 15. DISPOSITION TOTALS

| Disposition | Count | Concept IDs |
|---|---|---|
| `IMPL-EXACT` | 38 | CON-001, 002, 003, 014, 015, 019, 022, 028, 033, 034, 035, 036, 037, 038, 040, 042, 043, 047, 049, 053, 054, 056, 059, 062(levels), 063, 064, 065, 071, 074, 075, 076, 077, 078, 080, 081, 083, 085, 090, 091, 092, 095, 104, 105, 106, 112, 113, 114 |
| `IMPL-APPROX` | 37 | CON-004, 005, 006, 010, 011, 012, 013, 016, 017, 020, 021, 023, 024, 025, 026, 027, 030, 031, 032, 041, 044, 045, 050, 051, 052, 055, 057, 058, 060, 061, 066, 067, 068, 070, 072, 073, 082, 084, 086, 087, 088, 089, 100, 101, 102, 103, 115 |
| `EXPERIMENTAL` | 3 | CON-130, 131, 132 |
| `VISUAL-CONTEXT` / `DOCS` | 11 | CON-008, 009, 018, 029, 079, 093, 108, 121, 122, 125, plus the D12 statistics set |
| `NOT-IMPLEMENTED` | 6 | CON-046, 048, 069, 107, 123, 124 |
| `REFERENCED-UNSPECIFIED` | 2 | CON-007 (13-period partner), CON-039 (the 13 bar types / 14 events) |
| `PLATFORM_SAFEGUARD` | 9 | CON-094, 110, 116, 117, 118, 119, 120, plus `maxAdds` and `maxLifecycles` policies |

*(Counts include concepts appearing in more than one row where a concept has both an exact and an
approximate component — e.g. CON-062, whose levels are exact and whose odds are documentation-only.
The authoritative per-concept disposition is the Table C "Final disposition" column above and the
reverse audit in `06_RULE_COVERAGE_AUDIT.md`.)*
