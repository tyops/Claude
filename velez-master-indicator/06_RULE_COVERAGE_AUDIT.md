# 06 — RULE COVERAGE AUDIT
### Forward and reverse traceability between the source, the documentation, and the code

> **What "coverage" means here.** Coverage means every concept present in the PDF has a
> documented disposition, and every behaviour present in the code has a traceable origin. It does
> **not** mean the indicator reproduces the methodology, that the methodology works, that the
> source's claims are true, or that anything here is endorsed. See §10.

---

## TABLE OF CONTENTS

1. [Page-coverage result](#1-page-coverage-result)
2. [Forward traceability](#2-forward-traceability-source--doc--code)
3. [Reverse traceability](#3-reverse-traceability-code--source)
4. [Candidate-callout audit result](#4-candidate-callout-audit-result)
5. [Source-term versus UI-label audit](#5-source-term-versus-ui-label-audit)
6. [Disposition counts](#6-disposition-counts)
7. [Referenced-but-unspecified concepts](#7-referenced-but-unspecified-concepts)
8. [Intentionally omitted concepts](#8-intentionally-omitted-concepts-and-exact-reasons)
9. [Source contradictions and unresolved ambiguities](#9-source-contradictions-and-unresolved-ambiguities)
10. [What coverage is not](#10-what-coverage-is-not)

---

## 1. PAGE-COVERAGE RESULT

| Metric | Result |
|---|---|
| Pages in the document | **71** (verified by `pdfinfo` and PyMuPDF; the upload metadata's "25 pages" was wrong) |
| Pages reviewed as extracted text | **71 / 71** |
| Pages reviewed as rendered images | **71 / 71** (8 at full resolution, all 71 via legible contact sheets, 1 additional region at 300 dpi) |
| Pages that were unreadable | **0** |
| Pages that were image-only / required OCR | **0** (0 embedded raster images across the whole document) |
| Pages where extraction was incomplete or malformed | **0** |
| Pages flagged low-confidence | **1** — page 15, for a diagram-label inconsistency (SRC-152) |
| Pages containing no rule-bearing content | 8 — pp. 3, 4, 18, 22, 57, 58, 61, 71 (contents, dividers, colophon); all still reviewed |

Full per-page checklist: `01_MASTER_RULEBOOK.md §2`.

---

## 2. FORWARD TRACEABILITY (SOURCE → DOC → CODE)

Every material source proposition, traced to its concept, its implementation rule, and its QA
coverage. `—` means the concept has no code presence by design (its disposition explains why).

### 2.1 Core framework

| SRC | Page | Proposition | CON | RULE | Src class | Impl class | Status | Module | QA |
|---|---|---|---|---|---|---|---|---|---|
| SRC-020 | 5 | State → position → event, "not optional" | — (governs all) | RULE-110, 171–173 | DIRECT | EXACT | Implemented | §15, §17 | QA-206 |
| SRC-021 | 5 | Valid event in the wrong position is the wrong trade | CON-023 | RULE-037, 111–119 | DIRECT | EXACT | Implemented | §10, §15 | QA-213 |
| SRC-022 | 5 | Timeframe- and market-agnostic | CON-114 | RULE-218 | DIRECT | EXACT | Implemented as **advisory only** | §22 | QA-327 |
| SRC-025 | 5 | No event = no trade | CON-049 | RULE-066, 111 | DIRECT | EXACT | Implemented (hard blocker) | §11, §15 | QA-249 |
| SRC-028/029 | 66 | The pre-entry checklist and the five reasons to do nothing | CON-113 | RULE-230+ | DIRECT | EXACT | Implemented in Debug | §23 | QA-326 |

### 2.2 Moving averages

| SRC | Page | Proposition | CON | RULE | Src class | Impl class | Status | Module | QA |
|---|---|---|---|---|---|---|---|---|---|
| SRC-030/031 | 6 | 20 and 200, simple, on close, permanent | CON-001/002 | RULE-009, 200 | DIRECT | EXACT | Implemented | §05, §20 | QA-101, QA-301 |
| SRC-032 | 6 | Simple beats EMA/WMA | CON-009 | — | DIRECT (claim) | DOCS | Documented only | — | — |
| SRC-033 | 6, 29 | The 8 is a trailing tool only | CON-003/086 | RULE-010, 131, 201 | DIRECT | EXACT | Implemented; hidden unless escalated | §05, §16, §20 | QA-214, QA-286 |
| SRC-034 | 6 | A 13-period short partner exists "in some setups" | CON-007 | — | **UNSUPPORTED** | NOT_IMPLEMENTED | **Referenced-unspecified** | — | QA-105 |
| SRC-035/036 | 6–7 | The 20 is strongest sloping; the 200 strongest flat | CON-005 | RULE-014 | DIRECT / SUBJECTIVE | APPROX | Implemented (W2) | §06 | QA-103, QA-201 |
| SRC-040 | 7, 45 | The 20's slope defines the trend | CON-006 | RULE-016 | DIRECT | APPROX | Implemented | §06 | QA-104 |
| SRC-042/003 | 7, 2 | Zones, not lines | CON-004 | RULE-011, 202 | DIRECT / SUBJECTIVE | APPROX | Implemented (W3) | §05, §20 | QA-102, QA-302 |
| SRC-043 | 7 | Judge breaks by eye and follow-through | — | — | **SUBJECTIVE** | DOCS | Not mechanised (source resists it) | — | — |

### 2.3 State

| SRC | Page | Proposition | CON | RULE | Src class | Impl class | Status | Module | QA |
|---|---|---|---|---|---|---|---|---|---|
| SRC-050/051 | 8 | Three states; "eyeball it, do not measure" | CON-010 | RULE-027 | DIRECT / SUBJECTIVE | APPROX | Implemented (W1) | §09 | QA-201–203 |
| SRC-055/056/057 | 9, 53 | Three grades of narrow | CON-011/012/013 | RULE-028 | DIRECT | APPROX | Implemented | §09 | QA-204 |
| SRC-058 | 9 | Grade 1 is high-frequency | — | — | UNSUPPORTED as measurable | DOCS | Not used as a calibration target | — | — |
| SRC-059/060 | 9 | Regular and wide defined | CON-010 | RULE-027 | DIRECT | APPROX | Implemented | §09 | QA-202 |
| SRC-061 | 9, 54 | Mode by state | CON-015 | RULE-033 | DIRECT | EXACT | Implemented | §09, §22 | QA-206 |
| SRC-062/180 | 9, 52 | Narrow → wide is an exit signal by itself | CON-014/105 | RULE-182 | DIRECT | EXACT | Implemented | §17 | QA-205, QA-231 |
| SRC-063 | 28 | Only two spots matter | CON-015 | RULE-033 | DIRECT | EXACT | Implemented | §09 | QA-206 |
| SRC-064 | 48 | State ≠ location | CON-019 | RULE-032 | DIRECT | EXACT | Implemented as separate fields | §09, §22 | QA-209 |
| SRC-207 | 33 | The sleepy state — all three flat, not two of three | CON-016 | RULE-029 | DIRECT | APPROX | Implemented | §09 | QA-207 |
| SRC-198 | 39 | The merge | CON-017 | RULE-030 | DIRECT | APPROX | Implemented | §09 | QA-208 |

### 2.4 Position

| SRC | Page | Proposition | CON | RULE | Src class | Impl class | Status | Module | QA |
|---|---|---|---|---|---|---|---|---|---|
| SRC-070/071 | 10 | The seven-position ladder | CON-020 | RULE-035 | DIRECT / SUBJECTIVE | APPROX | Implemented (W4) | §10 | QA-210, QA-211 |
| SRC-072/073 | 10, 54 | Position one is the best zone; 80%+ target | CON-021/029 | RULE-035 | DIRECT / behavioural | APPROX / DOCS | Zone implemented; the 80% figure is reference text only | §10 | QA-210 |
| SRC-074 | 10 | Bias by position | CON-024/025 | RULE-111–119 | DIRECT | EXACT | Implemented as gates | §15 | QA-213 |
| SRC-075 | 10 | Do not trade inside the state | CON-022 | RULE-036, 111 | DIRECT | EXACT | Implemented (hard blocker) | §10, §15 | QA-212 |
| SRC-076 | 54 | Full long/short mirroring | CON-021 | throughout | DIRECT | EXACT | Implemented symmetrically | all | QA-350 |
| SRC-077/078 | 11, 55 | Never stop inside position one; opening bell is the exception | CON-028/081 | RULE-175 | DIRECT | EXACT | Implemented incl. the exception | §14, §15 | QA-216, QA-232 |
| SRC-079 | 11 | Pluto land | CON-023 | RULE-037, 111–119 | DIRECT | APPROX | Implemented as a blocker | §10, §15 | QA-213 |
| SRC-209/323 | 34–35 | The box and the three-finger spread | CON-026/027 | RULE-038, 039 | DIRECT | APPROX | Implemented | §10 | QA-215 |

### 2.5 Events

| SRC | Page | Proposition | CON | RULE | Src class | Impl class | Status | Module | QA |
|---|---|---|---|---|---|---|---|---|---|
| SRC-090 | 12 | Three events taught publicly | CON-030/032/034 | RULE-055–064 | DIRECT | EXACT | Implemented (plus SRC-133's fourth) | §11 | QA-220+ |
| SRC-091 | 12, 20 | The 13 bar types / 14 events are **never enumerated** | CON-039 | — | **UNSUPPORTED** | NOT_IMPLEMENTED | **Referenced-unspecified; surfaced in Debug** | §23 | QA-231 |
| SRC-092 | 12 | Elephant bar = visibly larger than neighbours | CON-030 | RULE-055 | DIRECT / SUBJECTIVE | APPROX | Implemented (W5) | §11 | QA-220, QA-303 |
| SRC-094 | 12 | ~80% follow-through, position-conditioned | — | RULE-239 | DIRECT (claim) | DOCS | Quoted with attribution only | §23 | QA-329 |
| SRC-095 | 12 | Two elephant entry methods | CON-046/047 | RULE-064 | DIRECT | NOT_IMPL / EXACT | Method 1 omitted (intrabar); method 2 implemented | §11 | QA-247, QA-248 |
| SRC-096/097 | 12 | Tail bar strict definition; the snowman counterfeit | CON-032/033 | RULE-057, 058 | DIRECT | APPROX / EXACT | Implemented; snowman as a Debug diagnostic | §11, §23 | QA-223, QA-224, QA-304 |
| SRC-098 | 12 | Bottoming = up, topping = down | CON-032 | RULE-057 | DIRECT | EXACT | Implemented | §11 | QA-223 |
| SRC-102/104/105/106 | 13 | Colour change: body extreme, not adjacent, one tick beyond | CON-034 | RULE-059, 209 | DIRECT | EXACT | Implemented incl. the reference line | §11, §21 | QA-225, QA-226 |
| SRC-107 | 13 | Elephant + change outranks either alone | CON-035 | RULE-060, 121 | DIRECT | EXACT | Implemented (rank + score) | §11, §15 | QA-227 |
| SRC-108 | 13 | Colour-change validity by state/location | CON-034 | RULE-111–119 | DIRECT | EXACT | Implemented | §15 | QA-225 |
| SRC-120/121 | 13, 30 | Gap fill; oversized gap invalidation | CON-037/038 | RULE-062, 063 | DIRECT | EXACT | Implemented; session-gated | §11 | QA-229, QA-230 |
| SRC-132/133 | 50 | Little red bar takeout; promoted to a standalone entry | CON-036 | RULE-061 | DIRECT | EXACT | Implemented, both directions | §11 | QA-228 |
| SRC-235 | 26 | Igniting vs exhaustion decided by location | CON-031 | RULE-056 | DIRECT / SUBJECTIVE | APPROX | Implemented, with a visible UNCLASSIFIED outcome | §11 | QA-222 |

### 2.6 Entries, adds, setups

| SRC | Page | Proposition | CON | RULE | Src class | Impl class | Status | Module | QA |
|---|---|---|---|---|---|---|---|---|---|
| SRC-123/126 | 13, 30 | Add on the first colour change — mandatory; continued adds near the 20 | CON-043/044 | RULE-177 | DIRECT | EXACT | Implemented; `ADD: DUE` surfaced | §17, §22 | QA-244, QA-245 |
| SRC-127 | 34 | 1A/1B versus two trades | CON-045 | RULE-087 | DIRECT / SUBJECTIVE | APPROX | Implemented (W19) | §12 | QA-246 |
| SRC-129 | 30, 55 | Position path 1 → 2 → 3 as the profit map | CON-106 | RULE-041 | DIRECT | EXACT | Tracked | §10 | QA-299 |
| SRC-130 | 34 | Hourly entry timing (last 15–20 min) | CON-048 | — | DIRECT | **NOT_IMPLEMENTED** | Intrabar; reason documented | — | QA-247 |
| SRC-190/191/195/196 | 37–38 | Surge off the 200 incl. **the costume rule** | CON-050 | RULE-082 | DIRECT | APPROX | Implemented; tail bars qualify as surges | §12 | QA-250, QA-251 |
| SRC-192/324 | 37, 59 | Clear blue skies to the left | CON-051 | RULE-080 | DIRECT / SUBJECTIVE | APPROX | Implemented (W8) | §12 | QA-252 |
| SRC-193 | 38 | Entry at 1:20–1:40 into a 2-minute bar | — | — | DIRECT | **NOT_IMPLEMENTED** | Intrabar; reason documented | — | QA-247 |
| SRC-198/199 | 39 | Merge, purge, surge; all three = strongest | CON-052/053 | RULE-081, 083 | DIRECT | APPROX / EXACT | Implemented | §12 | QA-253, QA-254 |
| SRC-151/197 | 38 | The stop ladder; **no fourth option** | CON-054 | RULE-084, 111–119 | DIRECT | EXACT | Implemented incl. the SKIP rung | §12, §15 | QA-255, QA-256 |
| SRC-201/202 | 38 | Legs, resets, leg-two demotion | CON-055/056 | RULE-022, 034 | DIRECT / SUBJECTIVE | APPROX | Implemented (W9) | §08, §09 | QA-257, QA-258 |
| SRC-205/206/208 | 33 | Igniting and pullback swings; the best pairing | CON-057/058 | RULE-085, 086 | DIRECT | APPROX | Implemented | §12 | QA-259, QA-260 |
| SRC-210/211/212 | 48–52 | The full opening-bell sequence and its window | CON-059/077/078 | RULE-088–092 | DIRECT | EXACT | Implemented | §12, §17 | QA-261–265 |
| SRC-213 | 51 | Hidden green / hidden red | CON-060 | RULE-093 | DIRECT / SUBJECTIVE | APPROX | **Bar-close form only** — limitation stated on-chart and in alerts | §12 | QA-266 |
| SRC-228 | 45 | Dual space reversal | CON-061 | RULE-094 | DIRECT | APPROX | Implemented | §12 | QA-267 |
| SRC-216/217/218/269/271 | 46 | Retracement ladder, scalp definition, scalp-vs-trade | CON-062–065 | RULE-095–098 | DIRECT levels / claims | EXACT / DOCS | Levels implemented; odds quoted with attribution | §12, §21 | QA-268, QA-269 |
| SRC-220/221/227 | 41–43 | Space; near add, away pare; the space extreme | CON-071/072/073 | RULE-018, 019 | DIRECT / SUBJECTIVE | EXACT / APPROX | Implemented (W14) | §07 | QA-275–277 |
| SRC-224/225 | 43 | Never trade against the 20; three-bucket screen | CON-006/074 | RULE-016, 017, 111–119 | DIRECT | APPROX | Implemented as the T2 gate | §06, §15 | QA-220 |
| SRC-231/232/233 | 23–25 | First warning drop; the 50% deep-drop test; Market Law Four | CON-066/067/068 | RULE-099–101 | DIRECT / SUBJECTIVE | APPROX | Implemented as warnings | §12 | QA-270–272 |
| SRC-230/234/236/237 | 23–27 | The full six-stage H&S with neckline | CON-069 | — | DIRECT narrative / UNSUPPORTED tolerances | **NOT_IMPLEMENTED** | Reason documented in code and docs | — | QA-273 |

### 2.7 Stops, exits, management

| SRC | Page | Proposition | CON | RULE | Src class | Impl class | Status | Module | QA |
|---|---|---|---|---|---|---|---|---|---|
| SRC-140/148/158 | 14 | Never leave the stop; rotate to the most protective | CON-088/095 | RULE-178 | DIRECT | EXACT | Implemented, monotonic | §17 | QA-288, QA-289, QA-293 |
| SRC-142 | 14 | Big bar / fat bar stop | CON-082 | RULE-132 | DIRECT / SUBJECTIVE | APPROX | Implemented | §16 | QA-282 |
| SRC-143 | 14, 51 | Colour-adjust stop, both directions | CON-083 | RULE-133 | DIRECT | EXACT | Implemented per the **text**, not the p.15 diagram label | §16 | QA-283 |
| SRC-144/320 | 14, 60 | Pivot stop with the hand-back to the 20 | CON-084 | RULE-134 | DIRECT / SUBJECTIVE | APPROX | Implemented; CONFIRMED-LATER | §16 | QA-284, QA-305 |
| SRC-145 | 14, 29 | Trail the 20; escalate to the 8 | CON-085/086 | RULE-131, 135, 136 | DIRECT / SUBJECTIVE | EXACT / APPROX | Implemented (W12) | §16 | QA-285, QA-286 |
| SRC-146/147 | 14, 29 | Bar-by-bar once separated; selection by distance | CON-087/089 | RULE-130, 137 | DIRECT / SUBJECTIVE | APPROX | Implemented (W13) | §16 | QA-287, QA-290 |
| SRC-149 | 14, 35 | Pivots alone give back too much | CON-093 | — | DIRECT | DOCS | Rationale for the rotation | — | — |
| SRC-150 | 14 | Two named reference sets | CON-094 | RULE-138 | DIRECT | SAFE | Implemented as a selector; off-timeframe policy disclosed | §16 | QA-292 |
| SRC-153/154 | 33 | Igniting- and pullback-swing stop references | CON-091/092 | RULE-174 | DIRECT | EXACT | Both source options exposed | §14 | QA-291 |
| SRC-155/327 | 49, 60 | The one-bar risk budget | CON-090/077 | RULE-091 | DIRECT | EXACT | Implemented | §12 | QA-262 |
| SRC-170/171/172 | 16 | Three pushes; the push-two right | CON-100/101/102 | RULE-179, 181 | DIRECT / **mechanics absent** | APPROX | Implemented (W10); CONFIRMED-LATER | §17 | QA-294–296 |
| SRC-173 | 16 | New high / new low is a right, not an obligation | CON-103 | RULE-180 | DIRECT / SUBJECTIVE | APPROX | Implemented as context; never forces a close | §17 | QA-297 |
| SRC-174 | 16 | Stop-out profit take | CON-104 | RULE-185 | DIRECT | EXACT | Implemented | §17 | QA-298 |
| SRC-175/176 | 16, 51 | Place the exit ahead of the market | CON-107 | — | DIRECT | **NOT_IMPLEMENTED** | Order routing; the observation it depends on is surfaced | — | QA-247 |
| SRC-177/178/179 | 16, 30 | Snatching at your roses; the expectancy argument | CON-108 | — | DIRECT | DOCS | README | — | — |

### 2.8 Statistics, gaps and editorial content

| SRC | Page | Content | Disposition |
|---|---|---|---|
| SRC-004, SRC-250–273 | 2, 19–20 | Every percentage | **DOCS only.** Rendered in the Debug claims panel and the retracement labels, always attributed, never as an output. The three that are mechanical *levels* (25%, 50%, 65–70%) are implemented as levels |
| SRC-272, SRC-200 | 17, 39 | Three size tiers | **DOCS only.** No sizing is computed — SRC-300 records that the source supplies none |
| SRC-300–306 | 67–69 | The manual's own five gaps and its "what holds up" list | **DOCS.** GAP FOUR (SRC-303) is the governing constraint on the whole project |
| SRC-010, SRC-011 | 36, 52, 56 | Commercial content; the anecdotal remark | **NOT reproduced.** Recorded in the rulebook, absent from the code |
| SRC-246/247 | 32–33 | Six pairs; why forex | **NOT_IMPLEMENTED** — multi-symbol screening is outside a single-chart indicator |

---

## 3. REVERSE TRACEABILITY (CODE → SOURCE)

Every category of behaviour in `05_VELEZ_MASTER_INDICATOR.pine`, walked and assigned an origin.
Anything that could not be traced was removed during the static review passes (§3.9).

### 3.1 Numeric constants in executable code

The file contains **no unexplained magic numbers**. Every numeric literal in executable code falls
into exactly one of these five buckets:

| Bucket | Count | Examples | Traceability |
|---|---|---|---|
| **Input defaults** | 47 | `narrowMult 1.0`, `eleRangeMult 1.8`, `pivLen 3` | Each has a tooltip declaring `SOURCE-BACKED` / `APPROXIMATION` / `PLATFORM` / `EXPERIMENTAL`, and a full provenance row in `02_CONCEPT_TO_CODE_MAP.md §2.2` |
| **Constant-family members** | 44 | `ST_NARROW 0`, `EV_COMBO 5`, `BLK_PLUTO 4` | Enumerations and bitmask bits. RULE-001 |
| **Named internal constants** | 11 | `SENTINEL_BIG`, `SNOWMAN_MIN_BODY`, `ALT_OFFSET_MULT`, `RET_25/50/75`, `EXP_NARROW_PCT`, `EXP_DENSITY_LEN`, drawing caps | Declared in §00 with a comment giving the reason; `RET_*` are source-backed (SRC-216) |
| **Source-backed inline levels** | 4 | `rng / 3.0` (SRC-151 "bottom third"), `50.0` in the scalp verdict (SRC-270), `100.0` percentage conversions | Cited inline |
| **Rendering values** | ~60 | Colour transparencies (`92`, `94`, `70`, `55`, `35`…), table dimensions (`2, 22`, `1, 14`), row indices (`0…21`), label offsets (`+3`, `+5`, `−40`) | Presentation only. They alter no detection, no gate, no score and no alert. Enumerated as a class here rather than individually, because each is a coordinate or an alpha channel |

Input-default provenance summary (from `02_CONCEPT_TO_CODE_MAP.md §2.2`): **3 anchored ·
2 cross-context anchors · 5 semi-anchored · 19 pure additions · 3 platform safeguards**, plus
display-only controls.

### 3.2 Formulas

| Formula | Origin | Class |
|---|---|---|
| `ta.sma(close, 20/200/8)` | SRC-031 | EXACT |
| `band = bandMult × atr` | SRC-042 concept, width added | APPROX (W3) |
| `gapN = |sma20 − sma200| / unit` | SRC-023 concept, unit added | APPROX (W1) |
| `slopeN(s) = (s − s[n]) / n / unit` | SRC-035 concept, threshold added | APPROX (W2) |
| `priceSandwiched = highest(close,n) ≤ stateTop and lowest(close,n) ≥ stateBot` | SRC-055 "sandwiched between them" — a containment statement | APPROX |
| `dUp/dDn` position distance | SRC-071 structure, boundaries added | APPROX (W4) |
| Elephant: `rng ≥ mult × sma(rng,n)[1] and body ≥ frac × rng` | SRC-092 + SRC-191 | APPROX (W5) |
| Tail: `tail ≥ frac × rng and body ≤ frac × rng and tail > opposite tail` | SRC-096 | APPROX (W6) |
| Colour change: body extreme ± `mintick`, non-adjacent, prior-bar reference | SRC-102/104/105/106 | EXACT |
| Takeout: `red[2] and not red[1] and green and high ≥ high[2] + tick` | SRC-132 | EXACT |
| Clear left: `close > highest(high, n)[1]` | SRC-192 purpose statement | APPROX (W8) |
| Purge: clear-left **and** `low ≤ leftHigh` (one bar spanned it) | SRC-198 "in one motion" | APPROX |
| Stop ladder thirds: `low + rng/3` | SRC-151 | EXACT |
| Rotation: monotonic max/min over six candidates | SRC-148 | EXACT |
| Retracement levels 25/50/75/100% | SRC-216 | EXACT |
| Scalp verdict at 50% / `scalpTradePct` | SRC-218/269 | EXACT |
| Deep drop at `deepDropPct` | SRC-232 level + **cross-context 65% inference** | APPROX (W15) |
| `space = |close − sma20|`; `percentrank` | SRC-322 binding + SRC-221 framing | EXACT / APPROX (W14) |
| Acceleration: `Δclose > mult × Δsma20` | SRC-145 | APPROX (W12) |
| Push count: confirmed higher pivots | SRC-170 rule, **mechanics entirely absent** | APPROX (W10) |
| Confluence score | **Implementation-defined**, weights published | SAFE |

### 3.3 Input defaults

All 47 material input defaults are traced in `02_CONCEPT_TO_CODE_MAP.md §2.2` and carry an
in-app tooltip stating their classification. **Audit result: 0 inputs without a provenance
tooltip; 0 inputs whose default is presented as source-derived when it is not.**

### 3.4 Detection conditions

| Detector | RULE | Origin |
|---|---|---|
| `isElephant`, `elephantBull/Bear` | RULE-055 | SRC-092 |
| `isIgniting`, `isExhaustion` | RULE-056 | SRC-235 |
| `bottomingTail`, `toppingTail` | RULE-057 | SRC-096/098 |
| `isSnowman` | RULE-058 | SRC-097 |
| `ccBull`, `ccBear`, `comboBull/Bear` | RULE-059, 060 | SRC-102–107 |
| `takeoutBull/Bear` | RULE-061 | SRC-132/133 |
| `gapUp/Dn`, `gapTooLarge`, `gapTrigger*` | RULE-062, 063 | SRC-120/121 |
| `clearLeft*`, `purge*` | RULE-080, 081 | SRC-192/198 |
| `surgeLong/Short`, `mergePurgeSurge` | RULE-082, 083 | SRC-190–199 |
| `igniteLong/Short`, `pullback*` | RULE-085, 086 | SRC-205–208 |
| `obQual*`, `obEntry*` | RULE-088–092 | SRC-210 |
| `hiddenGreenShort`, `hiddenRedLong` | RULE-093 | SRC-213 |
| `dualShort/Long` | RULE-094 | SRC-228 |
| `isDeepDrop`, `lowerTop`, `higherBottom`, `firstWarningDrop` | RULE-099–101 | SRC-231/232/233 |
| `ma20Halt` | RULE-042 | SRC-239 |

**0 detection conditions without a source origin.**

### 3.5 State transitions

Every transition in §17 is listed in `04_INDICATOR_ARCHITECTURE.md §6.2` with its guard and its
classification. Terminal-state reset happens at the **start** of the following bar (RULE-170a),
which is also how the code avoids the illegal `lc.state[1]` UDT-history access.

### 3.6 Plots, shapes, drawings and table fields

| Object class | Count | Traceability |
|---|---|---|
| `plot()` | 17 | RULE-200–204; 7 are `display.data_window` only |
| `plotshape()` | 15 | RULE-205, 206; each maps to one CON |
| `fill()` | 2 | RULE-202 (the MA bands, SRC-042) |
| `bgcolor()` | 2 | RULE-207 (state wash / background tint) |
| `label.new()` sites | 5 | RULE-208 (event), RULE-209 area (stop-method tick), retracement label, non-standard-chart warning |
| `line.new()` sites | 2 | RULE-209 (colour-change body reference, SRC-105); retracement ladder |
| `box.new()` sites | 1 | Position zone boxes (RULE-208 area, SRC-071) |
| `table` | 2 | RULE-210–219 (dashboard), RULE-230–239 (Debug); both `var`, created once |
| Dashboard rows | 22 | Each row maps to a SRC or is explicitly `[impl]` — see `03_VISUAL_DESIGN_SPEC.md §11.2` |

**0 drawing objects created outside the bounded arrays**, except the single one-off
non-standard-chart warning, which is intentionally unbounded-by-one and is created only on
`barstate.islast`.

### 3.7 Alerts

**26 `alertcondition()` families + 1 dynamic `alert()`.** Every one:
- fires on a **rising edge**, never on a persistent state;
- is gated by `gateA` (which is `barstate.isconfirmed` under the default alert mode);
- carries the string `Study tool - not a recommendation` or an equivalent disclaimer;
- maps to a CON in `04_INDICATOR_ARCHITECTURE.md §9.2`.

Audit result: **0 alerts that fire on a level rather than a transition; 0 alerts without a
disclaimer; 0 alerts that assert a probability or a recommendation.**

### 3.8 Timeframe, session, repaint and reset behaviour

| Behaviour | Traceability |
|---|---|
| Session gating (`timeframe.isintraday`, `session.isfirstbar_regular`) | RULE-008, PLATFORM. Opening-bell and gap-fill self-disable where no regular session exists |
| Timeframe advisory | RULE-218 / SRC-240. **Advisory only** — never blocks, because SRC-022 says the framework is timeframe-agnostic |
| One `request.security`, `lookahead_off`, `[1]`/`[2]` only | RULE-102 / SRC-248. Off by default |
| Confirmation classes | `04_INDICATOR_ARCHITECTURE.md §5.2`; surfaced in the Debug `CONFIRM` row |
| Warm-up | RULE-005, visible as `WARMING UP n/m` |
| Lifecycle reset | RULE-170a |
| Leg / pullback counters reset on a return to narrow | RULE-034, RULE-101 |
| Colour-change reference re-anchor | RULE-059, same-bar policy S10 |
| Gap arming cleared on trigger or new session | RULE-062 |

### 3.9 Behaviours removed during the audit

These existed in the first draft and were **removed** because they could not be traced or were
defective. Recorded here rather than quietly deleted:

| Removed | Reason |
|---|---|
| `atrUnit = useAtrBasis or true ? atrRaw : atrRaw` | Dead ternary with identical branches |
| `bool actNow = ... ? true : true` | Dead |
| `blockers := blockers + 0` | A no-op masquerading as a gate; the real dumb-stop gate now runs **before** `gatesPass` is computed |
| `eligFatBar = spaceN <= near20Mult or true` | Always-true expression; replaced by a comment explaining that SRC-142 attaches no distance condition |
| `lastLcState/Dir/Entry/Bar` | Assigned, never read |
| `darkTheme` | Pine cannot detect the chart theme; the variable was inert |
| `pushIncrement` (first draft) | Was assigned but unused; now genuinely used by the push marker |
| A second redundant gap-arm reset block | Unreachable given the preceding `if newSession` |

### 3.10 Defects found and fixed during the static review

| # | Defect | Fix |
|---|---|---|
| D1 | `lc.state[1]` and `lc.pushes[1]` — Pine has **no `[]` history operator on UDT fields** | Terminal reset moved to the start of the bar; `pushIncrement`, `lcLongActive`, `lcShortActive` added as plain bool mirrors |
| D2 | `trimmedCount += 1` inside functions — **Pine functions cannot assign to globals** | Trim helpers now return a count; the caller accumulates |
| D3 | Six `float r = rotateStop(...)` declarations in one scope — redeclaration | Replaced with an `applyStop()` tuple-returning helper and a six-step chain of uniquely named results |
| D4 | `float[]` / `string[]` — legacy array syntax | Changed to `array<float>` / `array<string>` |
| D5 | `plotshape(bool, location.absolute)` for add / stop-out / push — would plot at 0/1 | Now passes an explicit float price series |
| D6 | Integer-division bit test `m / BIT % 2 >= 1` | Replaced with a modulo `hasBit()` helper |
| D7 | `int expDensity = math.round(...)` — type mismatch | Changed to `float` |
| D8 | Dumb-stop blocker applied **after** `gatesPass` was computed | Initial-stop selection moved into §14, before the gate block |
| D9 | `ta.percentrank` called inside a ternary — can desynchronise series state | Called unconditionally; only the use is gated |
| D10 | `bar_index − 40` / `− zoneBoxBars` could go negative on short charts | Wrapped in `math.max(..., 0)` |
| D11 | Six behavioural magic numbers | Promoted to named constants with reasons |
| D12 | The visual spec claimed a 3 px pane-anchored ribbon, which Pine cannot draw | Implemented as a 6%-opacity wash; the spec now states the limitation and moves the grade cue to the dashboard |

---

## 4. CANDIDATE-CALLOUT AUDIT RESULT

Full table: `02_CONCEPT_TO_CODE_MAP.md §12`. Result across the 34 proposed labels:

| Outcome | Count | Labels |
|---|---|---|
| **Accepted** (source term, or a disclosed shortening) | 26 | ELEPHANT BAR, IGNITING/EXHAUSTION ELEPHANT, TAIL BAR, BOTTOMING/TOPPING TAIL, COLOUR CHANGE, HIDDEN GREEN, GAP FILL, POSITION 1/2/3, NARROW/WIDE STATE, SURGE, CLEAR BLUE SKIES, LEG 1/2, THREE PUSHES, ADD, ENTRY, STOP, BIG BAR STOP, FAT BAR, BAR-BY-BAR STOP, 20 MA TRAIL, 8 MA TRAIL |
| **Replaced with the source's own term** | 4 | `FLAT 200 SETUP` → `SURGE OFF 200` (the source explains on p.38 *why* it is not named for the elephant bar); `POSSIBLE EXHAUSTION` → `EXHAUSTION ELEPHANT`; `STATE SHIFT` → `STATE CHANGE [impl]`; `PROFIT MANAGEMENT` → retained only as a section heading |
| **Rejected outright** | 3 | `200 MA REJECTION` (implies a detector the source never defines); `TOO EXTENDED` and `LATE ENTRY WARNING` (both flatten three distinct source concepts — Pluto land, the three-finger spread, and the space extreme — into one vague phrase) |
| **Restricted** | 1 | `PROFIT MANAGEMENT` — UI heading only, never a chart marker |

---

## 5. SOURCE-TERM VERSUS UI-LABEL AUDIT

| Class | Count | Verification |
|---|---|---|
| Strings rendered that are **source terms** | 33 | Each verified against the glossary (pp.59–61) or the body text. British spelling preserved (`COLOUR`, not `COLOR`) |
| Strings rendered that are **UI shortenings** of source terms | 9 | Each listed in `01_MASTER_RULEBOOK.md §3` and `02_CONCEPT_TO_CODE_MAP.md §13`; full term available in the dashboard/Debug |
| Strings rendered that are **implementation-defined** | 10 | `WATCH`, `DEVELOPING`, `SETUP`, `HIGH QUALITY`, `ELITE SETUP`, `CONFLUENCE`, `STATE CHANGE`, `EXIT CONTEXT`, `VIRTUAL LIFECYCLE`, `BLOCKED`. **Every one carries `[impl]` or an equivalent marker** |
| Rule identifiers shown | C##, V#.# | Presented as **the manual's** navigation scheme (SRC-002), never as "Velez's rule numbers" |
| Approximation marking | `~` | Applied to every threshold-dependent value; ASCII, never colour-only, never emoji |

**Audit result: 0 unsupported terms presented as source-defined.**

---

## 6. DISPOSITION COUNTS

| Disposition | Concepts | Notes |
|---|---|---|
| **Implemented exactly** (`EXACT_TRANSLATION`) | **38** | Deterministic translations of fully-specified source rules |
| **Implemented as documented approximation** (`PROGRAMMABLE_APPROXIMATION`) | **37** | Each has a worksheet (W1–W22) and configurable inputs |
| **Experimental optional module** | **3** | CON-130/131/132. Off by default **twice over** — the fidelity layer *and* the individual toggle must both be on |
| **Visual context / documentation only** | **11** | Including all 24 statistics |
| **Not implemented** | **6** | CON-046, 048, 069, 107, 123, 124 |
| **Referenced but unspecified** | **2** | CON-007 (the 13-period partner), CON-039 (the 13 bar types / 14 events) |
| **Platform safeguards** | **14** | PS-01…PS-14 |

Approximation worksheets written: **22** (W1–W22) plus the confluence-layer worksheet (W23).
Traceability identifiers present in the code: **107 RULE ids · 77 CON ids · 114 SRC ids**.

---

## 7. REFERENCED-BUT-UNSPECIFIED CONCEPTS

The source names these and provides insufficient mechanics. **No rule was created for any of
them.**

| Concept | SRC | What the source says | What is missing |
|---|---|---|---|
| The 13 bar types | SRC-091, 263 | "the market repeats a limited alphabet of 13 bar types" | "**Never enumerated in these eight videos. Only four are named.**" |
| The 14 actionable events | SRC-091, 264 | "he teaches professional traders 14 actionable events" | "**The other eleven are not in this material.**" |
| A 13-period short partner | SRC-034 | "He also uses 13 and 8 as the short partner in some setups" | Which setups, how, and what it replaces |
| The third reason traders lose | SRC-268 | Opens by naming three reasons, then attributes 95% to two | "**The third is never separately enumerated in the captured transcript.**" |
| Maximum loss per trade | SRC-300 | The stop ladder requires one | No percentage of capital, no derivation. Exposed as a user input that stays `UNSET` until filled |
| Position-sizing mathematics | SRC-300 | Three size tiers only | "no discussion of what percentage of capital a maximum loss should represent, how to size when the stop distance varies, how many positions to hold at once, or what happens to sizing after a losing streak" |
| Trade concurrency | SRC-300 | — | Explicitly listed as absent. Handled by a disclosed platform policy (`maxLifecycles = 1`) |
| The content of the transcript gaps | SRC-007 | Videos 2, 3, 7, 8 have named unrecovered ranges | Unknown by construction |

The Debug panel surfaces the first two on-chart, so a user is never left assuming full coverage.

---

## 8. INTENTIONALLY OMITTED CONCEPTS AND EXACT REASONS

| CON | Concept | Exact reason |
|---|---|---|
| CON-046 | Elephant entry method 1 — "buy into the bar before it finishes trading" (SRC-095) | Intrabar. A bar-close indicator cannot express a position inside a forming bar in a way that survives the close; an intrabar evaluation would repaint. **The source's own alternative (method 2, "the next bar that clears its high") is implemented instead, so nothing is lost that the source does not also offer.** |
| CON-048 | Hourly entry timing, "the last 15 to 20 minutes of the hourly bar" (SRC-130) | Same reason. Intrabar clock position |
| CON-069 | The full six-stage head-and-shoulders with a neckline (SRC-230/234/236) | The source supplies **no tolerance** for shoulder equality, **no bar window**, and **no neckline slope allowance**, and explicitly warns against mechanising it: *"Treating it as a mechanical checklist … the shoulders do not have to be equal, usually but not always. The pattern is probabilistic. A slanted right shoulder still counts"* (SRC-237, p.27). Building a detector would require inventing every tolerance against a direct instruction not to. **The three components the source does define comparably — the 50% deep-drop test, the first-warning-drop comparison, and the lower-high structure — are implemented as approximations instead** |
| CON-107 | "Place the exit order ahead of the market on push two" (SRC-175) | An order-routing instruction. An indicator has no orders. The underlying observation it depends on — that push two has completed — **is** surfaced |
| CON-123 | Risk-sizing mathematics | SRC-300: the source contains none. Inventing risk mathematics would be the single most consequential unsupported addition possible |
| CON-124 | Multi-symbol watchlist screening (SRC-246, SRC-225) | Requires scanning symbols other than the chart's. Outside a single-chart indicator's scope. The **single-symbol** form of the three-bucket screen (SRC-225) *is* implemented as the T2 directional gate |
| — | The 13 bar types / 14 events | §7 |
| — | The 13-period partner | §7 |
| — | Commercial content (SRC-010) | Contains no trading instruction |
| — | The anecdotal demographic remark (SRC-011) | The manual itself records that "nothing in the method depends on it" |

---

## 9. SOURCE CONTRADICTIONS AND UNRESOLVED AMBIGUITIES

Carried forward from `01_MASTER_RULEBOOK.md §10.2–§10.3`. **None has been resolved by inventing
precedence.**

### 9.1 Contradictions

| # | Contradiction | Handling |
|---|---|---|
| T1 | "No third option" / "there is no third spot" (SRC-052, SRC-063) versus the explicitly named **middling/regular** state with its own trading mode (SRC-059/061, V2.3) | Both recorded. Reading adopted: three *states* exist, two *spots* are named tradable. The indicator reports all three states and marks regular-state signals distinctly. No precedence invented |
| T2 | Stop "beyond the state" (SRC-077) versus "sometimes under the bar, sometimes under the state" (p.55) versus the one-bar opening-bell stop (SRC-078) | **The manual reconciles this itself** on p.55: the invariant is that the stop "must never sit inside the zone you entered from". Adopted as the gate; both placements allowed outside it; the opening bell is the source's own named exception |
| T3 | Win rates of 80–87% versus "two or three out of ten will not work" (SRC-257) | Recorded unresolved, exactly as the manual instructs (SRC-273: "READ THE CONTRADICTION, DO NOT SMOOTH IT OVER"). Neither figure is used anywhere in code |
| T4 | The p.15 diagram labels a group "GREEN, RED, RED" while illustrating a long-side rising sequence; that is the **short-side** colour-adjust pattern (SRC-152) | The **text** wins: the rule is stated twice, in both directions, on pp.14 and 51. The diagram inconsistency is disclosed in a code comment, in the rulebook, and here — not smoothed over |
| T5 | "Buy into the elephant bar before it finishes trading" versus what a bar-close indicator can observe | Method 1 not implemented; the source's own method 2 is. Disclosed |

### 9.2 Ambiguities with no source resolution (17)

A1 which opposite-colour bar supplies the colour-change level · A2 what constitutes a push ·
A3 where "the initial move" ends · A4 how close is "close together" · A5 position band boundaries ·
A6 elephant bar size · A7 flatness · A8 clear-space lookback · A9 reset duration · A10
"accelerating faster than the 20 can track" · A11 "separated from the 20 and lost that support" ·
A12 which trailing set applies off the source's own timeframes · A13 tie-breaking between equally
protective stop references · A14 "unusually large" space · A15 what makes a prior move "strong" ·
A16 how far apart 1A and 1B must be · A17 which higher timeframe.

Each is resolved by a **documented, configurable** policy tagged `PROGRAMMABLE_APPROXIMATION` or
`PLATFORM_SAFEGUARD`, with the alternatives and the failure modes written out in the worksheets.

### 9.3 One inference this project made that the source does not state

**The 65% deep-drop threshold (W15).** SRC-232 says a deep drop "severely breaks" the 50% halfway
point and never quantifies "severely". The default borrows 65% from the source's *own* V6.5
quantification of a significant break of the same 50% line (SRC-269, "toward 65 or 70%"). **The
source does not state 65% in the Lesson 1 context.** This is a cross-context inference by this
project, and it is labelled as such in the input tooltip, in the code comment, in the concept map
and here. It is the only place in the project where a number was carried from one lesson's context
into another's.

---

## 10. WHAT COVERAGE IS NOT

**Coverage is not profitability.** Nothing in this audit says any rule makes money. The source's
own figures are self-reported (SRC-004) and its compiler's assessment is that their consistency
across unrelated methods is itself a reason for caution (SRC-273).

**Coverage is not validation.** No threshold in this project has been validated against anything.
It could not be: every worked example in all eight videos is a setup that worked (SRC-302), so the
examples cannot validate a detector, and the source states its framework "cannot be backtested"
(SRC-303).

**Coverage is not official endorsement.** This is an independent, unofficial,
methodology-inspired study tool. It is not affiliated with, endorsed by, or authorised by Oliver
Velez or any related entity, and uses no proprietary logo, artwork or branding.

**Coverage is not completeness of the methodology.** Ten of thirteen bar types and eleven of
fourteen actionable events are absent **from the source material itself** (SRC-091). The manual
also declares unrecovered transcript ranges in four of the eight lessons (SRC-007).

**Coverage is not fidelity.** The source states its definitions are deliberately loose and
instructs the reader to eyeball rather than measure (SRC-051, SRC-303). Every numeric threshold
in this indicator is therefore an **addition**, not a translation — which is why 37 concepts are
classified `PROGRAMMABLE_APPROXIMATION` rather than `EXACT_TRANSLATION`, and why every one of
them is marked `~` on the chart.
