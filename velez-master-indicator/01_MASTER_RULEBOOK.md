# 01 — MASTER RULEBOOK
### Authoritative methodology database extracted from *The Velez Method* field manual

> **What this document is.** A forensic, page-traceable record of every concept, rule, definition,
> warning, statistic, example, exception and gap contained in the reference PDF. It is the single
> source of truth for every downstream artefact in this repository. No rule appears anywhere in
> this project that is not recorded here with a page citation.
>
> **What this document is not.** It is not a reproduction of the PDF, not a trading course, not a
> claim that the PDF contains the complete original methodology, and not an endorsement by or
> affiliation with Oliver Velez or any associated entity. This is an independent, unofficial,
> methodology-inspired engineering artefact.

---

## TABLE OF CONTENTS

1. [Source scope and provenance](#1-source-scope-and-provenance)
2. [Page-coverage summary](#2-page-coverage-summary)
3. [Terminology glossary (normalised)](#3-terminology-glossary-normalised)
4. [Concept dependency map](#4-concept-dependency-map)
5. [Evidence ledger](#5-evidence-ledger)
6. [Rule hierarchy](#6-rule-hierarchy)
7. [Direct rules versus discretionary guidance](#7-direct-rules-versus-discretionary-guidance)
8. [Examples versus universal rules](#8-examples-versus-universal-rules)
9. [Source statistics and their limitations](#9-source-statistics-and-their-limitations)
10. [Missing, ambiguous and non-programmable concepts](#10-missing-ambiguous-and-non-programmable-concepts)
11. [Source terminology normalisation rules](#11-source-terminology-normalisation-rules)
12. [What this indicator can and cannot claim](#12-what-this-indicator-can-and-cannot-claim)

---

## 1. SOURCE SCOPE AND PROVENANCE

### 1.1 The artefact analysed

| Property | Value |
|---|---|
| Internal PDF title | `The Velez Method` |
| Cover subtitle | "Oliver Velez Trading · Eight lessons, rebuilt as a working manual" |
| File analysed | `82688dfa-TheVelezMethodFieldManual.pdf` (uploaded copy) |
| Page count | **71 pages** (verified by `pdfinfo` and PyMuPDF) |
| Page size | 612 × 792 pt (US Letter) |
| Producer / Creator | `Skia/PDF m141` / `Chromium` — i.e. an HTML document printed to PDF |
| Embedded raster images | **0** on every page |
| Text layer | Present and complete on all 71 pages; no OCR required |
| Diagrams | 30 vector diagrams, drawn for the manual (stated p.70) |

**Requested-path note.** The task named `C:\Users\matth\Downloads\The-Velez-Method-Field-Manual.pdf`.
That path does not exist in this Linux execution environment. The attached upload of the same
manual was used. **A separate discrepancy is recorded:** the upload metadata declared 25 pages;
the document is 71 pages. All 71 were reviewed.

### 1.2 Provenance layers *inside* the PDF — these must not be flattened

The manual is explicit and unusually careful about distinguishing what is Velez's teaching from
what is the compiler's editorial work. This project preserves that distinction as a first-class
property of every evidence record (the **Source type** field).

| Layer | What it is | Where the manual says so | Treatment here |
|---|---|---|---|
| **L1 — Directly taught methodology** | Rules attributed to Velez from the eight video transcripts | Part Two chapters; p.70 "Everything in part two traces to the source transcripts" | Eligible for implementation |
| **L2 — Editorial synthesis** | Part One's arrangement of rules into one framework | p.70: "Part one is a synthesis. The individual rules are Velez's; **the arrangement into one framework is not how any single video presents it**" | Implementable, but the *arrangement* is credited to the compiler, not to Velez |
| **L3 — Editorial apparatus** | Rule identifiers C1–C24 / V1.1–V8.6, the glossary, checklists, the statistics table, §3.5 | p.70 "What this manual added" | Used as navigation aids; the **IDs themselves are the compiler's, not Velez's** |
| **L4 — Editorial critique** | §3.5 "What the videos leave out"; the editorial box on p.44; the contradiction note p.20 | p.68: "**This section is not Velez.** It is an assessment of the material as a system" | Recorded as evidence *about* the source, never as methodology |
| **L5 — Self-reported statistics** | All percentages | p.2 "VELEZ'S STATISTICS ARE HIS OWN"; p.68 GAP TWO | Documentation-only. Never converted into indicator behaviour |
| **L6 — Marketing / commercial content** | Funded-programme sales pitches at the end of videos 3, 7, 8 | p.36, p.56, p.68 GAP FIVE | Recorded; explicitly "contains no trading instruction" |
| **L7 — Transcript gaps** | Portions of videos 2, 3, 7, 8 not captured | p.70 §3.6 table | Recorded as bounded unknowns. **Nothing invented to fill them** |
| **L8 — Anecdotal generalisation** | The remark on trader demographics (p.52) | p.52: "Recorded as stated. It is an anecdotal generalisation, not a finding, and nothing in the method depends on it" | Recorded; not implemented; not repeated in user-facing output |

### 1.3 Transcript coverage declared by the manual (p.70, §3.6)

| # | Lesson | Duration | Declared transcript coverage |
|---|---|---|---|
| 01 | The first warning a trend is about to die | 13:53 | Complete |
| 02 | The fastest timeframe to learn trading | 31:53 | Complete through the closing section on additional plays near the 20; final moments not captured |
| 03 | The swing trading strategy that made me the most money | 1:39:05 | Complete to ~58:21. Missing ~58:21–1:02:30. Excerpts recovered ~1:02:30–1:07:29 and ~1:27:29–1:31:29. Gaps between and after remain. Recovered material is promotional |
| 04 | How to use the 200 SMA | 21:55 | Complete |
| 05 | Give me 30 minutes, I'll fix your trading | 22:16 | Complete (ends near 22:08 of 22:16) |
| 06 | How to scalp like a pro | 22:49 | Complete |
| 07 | Why I trade only the first 20 minutes | 48:57 | Captured to roughly 43–45 min. Final 4–5 minutes not captured |
| 08 | Trading is easier than counting to 3 | 1:20:44 | Complete to ~58:20. Excerpts at ~1:02:29–1:07:29 and ~1:13:40–1:18:04. Missing ~58:20–1:02:29, ~1:07:30–1:13:39, and 1:18:05 to end |

**Consequence for this project.** The manual states (p.70) that video 8's gap "begins mid-drill,
right as he starts running live chart examples of the three-step process. Those examples are lost.
The framework they demonstrate is fully captured before the gap." Therefore: the *framework* is
well-evidenced; a portion of the *worked examples* is not available for calibration. This is one
reason no threshold in this project is presented as validated against source examples.

---

## 2. PAGE-COVERAGE SUMMARY

Every page was reviewed twice: (a) full text extraction, (b) visual render. Pages carrying
rule-bearing diagrams were additionally rendered at full resolution and inspected individually.

**Legend — Confidence:** `H` = text and diagram unambiguous · `M` = text clear, diagram
illustrative/approximate · `L` = internal inconsistency or under-specification noted.

| Page | Content | Reviewed (text) | Reviewed (visual) | Confidence | Notes |
|---|---|---|---|---|---|
| 1 | Cover; "single picture" diagram; 8-lesson list; 8/3/2/30 counts | ✅ | ✅ full | H | Establishes the narrow-state + position-one + institutional-bar core image |
| 2 | How to use this manual; rule-ID scheme; diagram legend; **caution on numbers** | ✅ | ✅ full | H | Declares MA-as-band rendering convention; declares statistics as Velez's own |
| 3 | Contents / navigation | ✅ | ✅ sheet | H | Contents page numbers are partially placeholder ("3", "1", "66") — navigational only, no rules |
| 4 | Part One divider; "the claim underneath all of it" | ✅ | ✅ sheet | H | Small-vocabulary argument |
| 5 | §1.1 three-step funnel; C1, C2, C3 | ✅ | ✅ sheet | H | The ordering rule |
| 6 | §1.2 two moving averages; C4, C5 | ✅ | ✅ sheet | H | SMA on close; opposite-power table |
| 7 | MA condition diagrams; C6 zones-not-lines; C7 keeps-you-honest | ✅ | ✅ full | H | Four-panel strongest/weakest diagram; lean-vs-break diagram |
| 8 | §1.3 State; state cycle diagram; C8, C9 | ✅ | ✅ sheet | H | Narrow↔wide rails; three grades |
| 9 | Grades-of-narrow detail table; C10, C11 | ✅ | ✅ sheet | H | Regular/wide definitions; get-in-near/get-out-away |
| 10 | §1.4 Position; seven-position ladder diagram; C12, C13 | ✅ | ✅ full | H | Ladder confirms positions are **bands**, measured from the state band |
| 11 | C14 stop placement; C15 Pluto land | ✅ | ✅ sheet | H | Names the opening-bell exception to C14 |
| 12 | §1.5 Event; eight-shape diagram; C16, C17, C18 | ✅ | ✅ full | H | Canonical event grammar incl. the "not a tail bar" counterfeit |
| 13 | Colour changes detail; C19 gap handling; C20 adding | ✅ | ✅ sheet | H | Body-not-tail marking; takeout price = one tick above |
| 14 | §1.6 Managing the trade; C21; five-methods table; rotation rule | ✅ | ✅ sheet | H | The five trailing methods in one table |
| 15 | Stop-rotation diagrams (opening-bell version and swing version) | ✅ | ✅ full + zoom | **L** | Diagram label "GREEN, RED, RED" is the *short-side* colour-adjust pattern drawn on a rising chart — see SRC-152 |
| 16 | §1.7 Taking profits; C22, C23, C24; three-pushes diagram | ✅ | ✅ sheet | H | |
| 17 | Sizing — three size tiers | ✅ | ✅ sheet | H | Low text density; content fully captured |
| 18 | §1.8 divider for the statistics table | ✅ | ✅ sheet | H | Divider only |
| 19 | Statistics table part 1 | ✅ | ✅ sheet | H | 87 / 85 / 85 / 80 / 80+ / 80+ / 8-of-10 / 2–3-of-10 / 9,6,2,1-of-10 / 8–12 bars / 80-20 ×3 / 13 |
| 20 | Statistics table part 2; **read-the-contradiction box** | ✅ | ✅ sheet | H | 14 events, 3–5 pushes, 23%, 60% |
| 21 | §1.9 timeframe-to-lesson map; choosing; why short works | ✅ | ✅ sheet | H | Six-speed mapping |
| 22 | Part Two divider; margin-icon legend | ✅ | ✅ sheet | H | |
| 23 | Lesson 1 opener; topping-pattern diagram; V1.1 stages 1–5 | ✅ | ✅ sheet | H | |
| 24 | V1.2 psychology table; V1.3 deep-drop definition; halfway diagram; V1.4 | ✅ | ✅ sheet | H | The only quantified rule in lesson 1 |
| 25 | Bottoms (mirror); inverse H&S diagram; V1.5 Market Law Four | ✅ | ✅ sheet | H | |
| 26 | V1.6 where to enter; V1.7 igniting vs exhaustion + diagram; charts used | ✅ | ✅ sheet | H | Location decides meaning |
| 27 | Lesson 1 warnings (4); practice assignment | ✅ | ✅ sheet | H | |
| 28 | Lesson 2 opener; V2.1 battle charts; V2.2 tools; V2.3 three states | ✅ | ✅ sheet | H | |
| 29 | V2.4 13 bars; V2.5 trailing; V2.6 profits | ✅ | ✅ sheet | H | Selection rule = distance from the 20 |
| 30 | Management preference; V2.7 gaps; V2.8 adding; warnings | ✅ | ✅ sheet | H | |
| 31 | Lesson 2 takeaways (6); source note | ✅ | ✅ sheet | H | |
| 32 | Lesson 3 opener + source note; V3.1 four styles; V3.2 why forex | ✅ | ✅ sheet | H | |
| 33 | V3.3 six pairs; V3.4 two setups + sleepy state strict definition | ✅ | ✅ sheet | H | Sleepy state = all three flat |
| 34 | Sleepy-state diagram; V3.5 1A/1B; V3.6 entry timing; V3.7 the box | ✅ | ✅ sheet | H | |
| 35 | Box / three-finger diagram; V3.8 rotating stop; V3.9 win rate | ✅ | ✅ full | H | Three references: 20, pivot, big bar |
| 36 | Lesson 3 warnings; commercial content; takeaways (5) | ✅ | ✅ sheet | H | |
| 37 | Lesson 4 opener; V4.1 zone not line; V4.2 flat 200; setup diagram; V4.3 rules 1–2 | ✅ | ✅ sheet | H | |
| 38 | V4.3 rules 3–6; stop-ladder diagram; V4.4 costumes; V4.5 legs and resets | ✅ | ✅ full | H | Stop ladder: base → bottom third → skip |
| 39 | Legs/resets diagram; V4.6 merge/purge/surge + diagram; V4.7 sizing | ✅ | ✅ sheet | H | |
| 40 | Sizing precision; charts used; warnings (4); his reasoning | ✅ | ✅ sheet | H | |
| 41 | Lesson 5 opener; V5.1 three tools; V5.2 space | ✅ | ✅ sheet | H | |
| 42 | Breathing/space diagram; V5.3 colour game + near-the-20 condition + diagram | ✅ | ✅ full | H | Space measured price↔20; valid/not-yours diagram |
| 43 | V5.4 loss reason one + screening method; V5.5 loss reason two; V5.6 near-add-away-pare | ✅ | ✅ sheet | H | |
| 44 | Lesson 5 warnings (3); practice; **editorial box on the 85% claim** | ✅ | ✅ sheet | H | |
| 45 | Lesson 6 opener; V6.1 toolkit/buddy system; V6.2 dual space; V6.3 odds intro | ✅ | ✅ sheet | H | |
| 46 | Odds table + ladder diagram; V6.4 definition; V6.5 scalp-or-trade | ✅ | ✅ sheet | H | 65–70% figure appears here |
| 47 | 50%-line decision diagrams; sucker-play warning; takeaways (5) | ✅ | ✅ sheet | H | |
| 48 | Lesson 7 opener + source note; V7.1 five tools; V7.2 state vs location | ✅ | ✅ sheet | H | |
| 49 | V7.3 power bars; V7.4 the sequence steps 1–7; professional loss | ✅ | ✅ sheet | H | |
| 50 | Full opening-sequence diagram; V7.5 the add + two add diagrams | ✅ | ✅ full | H | Every price defined in advance |
| 51 | V7.6 exit; V7.7 trailing two-part rotation; V7.8 hidden green + 3-panel diagram | ✅ | ✅ sheet | H | |
| 52 | V7.9 discipline; two asides; warnings (4) + gap caveat | ✅ | ✅ sheet | H | Contains the anecdotal L8 remark |
| 53 | Lesson 8 opener + source note; V8.1 framing; V8.2 state in full + grades table | ✅ | ✅ sheet | H | |
| 54 | Eyeball-it box; three-grade diagrams; why it works; V8.3 position in full | ✅ | ✅ sheet | H | |
| 55 | V8.4 the dumb stop; V8.5 event + real-estate analogy; V8.6 drills; later examples | ✅ | ✅ sheet | H | |
| 56 | Later examples cont.; commercial content; warnings (4); his reasoning | ✅ | ✅ sheet | H | |
| 57 | Part Three divider | ✅ | ✅ sheet | H | Divider only |
| 58 | §3.1 glossary intro | ✅ | ✅ sheet | H | Divider only |
| 59 | Glossary A–L (bar-by-bar → location) | ✅ | ✅ sheet | H | |
| 60 | Glossary L–2 (location → 20 MA halt) | ✅ | ✅ sheet | H | |
| 61 | Glossary continuation — "20 MA halt" definition | ✅ | ✅ sheet | H | Low text density; content complete |
| 62 | §3.2 index of every rule — core rules C1–C24 | ✅ | ✅ sheet | H | |
| 63 | Index — lesson rules V1.1–V4.6 | ✅ | ✅ sheet | H | |
| 64 | Index — lesson rules V4.7–V8.6 | ✅ | ✅ sheet | H | |
| 65 | §3.3 where each concept recurs | ✅ | ✅ sheet | H | Recurrence counts = load-bearing signal |
| 66 | §3.4 checklists (6 blocks) | ✅ | ✅ sheet | H | Operationalises the framework |
| 67 | "The one number worth tracking yourself" | ✅ | ✅ sheet | H | |
| 68 | §3.5 what the videos leave out — gaps 1–5 (**editorial**) | ✅ | ✅ sheet | H | Explicitly not Velez |
| 69 | Gap five cont.; "what holds up" (**editorial**) | ✅ | ✅ sheet | H | |
| 70 | §3.6 sources and gaps; how gaps were handled; what this manual added | ✅ | ✅ sheet | H | Provenance disclosure |
| 71 | Colophon | ✅ | ✅ sheet | H | Confirms the band-rendering convention |

**Coverage result: 71 / 71 pages reviewed. 0 unreadable. 0 image-only. 0 requiring OCR. 0 pages
with content that could not be extracted.**
One page (15) is flagged low-confidence for a diagram-label inconsistency; see SRC-152.

---

## 3. TERMINOLOGY GLOSSARY (NORMALISED)

Reproduced from the manual's glossary (pp.59–61) in condensed paraphrase, with the manual's own
video attributions, plus this project's normalisation decision for each term. **"UI label"** is the
string this project is permitted to render on a chart. Where the source term is long, the
normalisation column records the shortening as a UI decision, not as a source term.

| Source term | Manual's definition (paraphrase) | Attributed | Normalised UI label | Status |
|---|---|---|---|---|
| Bar by bar stop | Trailing under every single bar; used only once price has separated from the 20 and lost that support | V2 | `BAR-BY-BAR` | Source term |
| Big bar stop / fat bar stop | Move protection under every sizeable solid bar as it prints | V2, V3, V7 | `FAT BAR` | Source term (both spellings used by source) |
| The box | Zone near the origin of a move where instrument, 20 and 200 are all still relatively close; the valid entry zone | V3 | `THE BOX` | Source term |
| Boxing ropes | Image for a moving average; a minor penetration is a lean, not a break | V8 | *(not rendered)* | Rationale only |
| Buddy system | Never one MA alone; always a short paired with a long | V6 | *(not rendered)* | Design rationale |
| Bull 180 | His name for a bullish colour change | V8 | `BULL 180` | Source term |
| Clear blue skies to the left | No significant recent price data immediately left of a surge | V4 | `CLEAR LEFT` | **UI shortening** of a source term |
| Colour game / colour change | One colour taking out the high/low of an opposite-coloured bar; need not be back to back; ignore the tails, use the body | V2, V5, V7, V8 | `COLOUR CHANGE` | Source term |
| Colour adjustment stop | After one counter-colour bar followed by two bars in your direction, stop just beyond that isolated bar | V7 | `COLOUR ADJUST` | Source term (source itself uses "colour adjust") |
| Core trading | Weeks-to-months holding for the sweet spot of a macro move | V3 | *(not rendered)* | Context only |
| Creme de la creme | Best grade of narrow: 20 flat, 200 flat, close together, instrument flat and sandwiched | V8 | `NARROW G1` | **UI shortening**; full term shown in dashboard tooltip |
| Deep drop | A pullback that severely breaks the halfway point of the move that preceded it | V1 | `DEEP DROP` | Source term |
| Dual space reversal | Price separated from the 20 *and* the 20 separated from the 200 simultaneously | V6 | `DUAL SPACE` | **UI shortening** of a source term |
| Elephant bar | A bar visibly larger and taller than those around it; institutional footprint | V2, V7, V8 | `ELEPHANT` | Source term |
| Exhaustion elephant | The same large bar appearing late in a move | V1 | `EXHAUSTION ELEPHANT` | Source term |
| Gap fill | Mentally treating a gap as if price traded through it, so the gap becomes elephant bar one | V2, V8 | `GAP FILL` | Source term |
| Get in near, get out away | Enter when 20, 200 and price are close; exit once separated | V7 | *(dashboard phrase)* | Source term |
| Hidden green play | A first bar starting green in the wrong location, fading, erased by red; erasure point is the entry. Mirrors for hidden red | V7, V8 | `HIDDEN GREEN` / `HIDDEN RED` | Source term |
| Igniting elephant | A large bar early in a move, starting or confirming a trend | V1 | `IGNITING ELEPHANT` | Source term |
| Igniting swing | Explosive move erupting out of a sleepy state, off either the 20 or the 200 | V3 | `IGNITE` | **UI shortening** of a source term |
| Last of the Mohicans | The final group of buyers or sellers available | V1 | *(not rendered)* | Psychology; not codeable |
| Leg one, leg two | The first move off a base and the continuation after a reset | V4 | `LEG 1` / `LEG 2` | Source term |
| Little red bar takeout | One weak red bar that fails to produce a second red and is then removed by green | V7 | `RED BAR TAKEOUT` | **UI shortening**; mirror = `GREEN BAR TAKEOUT` (mirror is source-supported, see SRC-131) |
| Location | Above or below the state — distinct from state itself | V7 | `LOCATION` | Source term |
| Market Law Four | A head and shoulders without the head; a double top with a lower second top | V1 | `MARKET LAW FOUR` | Source term |
| The merge | The 20 and the 200 converging tightly together | V4 | `MERGE` | Source term |
| Narrow state | 20 and 200 relatively close together, judged by eye | All | `NARROW` | Source term |
| Near add, away pare | Add on colour-game signals near the 20; reduce when stretched into unusually large space | V5 | `NEAR: ADD` / `AWAY: PARE` | Source term |
| Neckline | The line through the two pullback lows of a head and shoulders | V1 | *(not rendered)* | Not implemented — see §10 |
| Not every dollar has your name on it | Discipline maxim | V3 | *(not rendered)* | Discipline; not codeable |
| Pivot | A swing low price has since moved away from, used as a trailing reference until the 20 climbs past it | V3 | `PIVOT` | Source term |
| Pluto land | The zone far past position one where traders chase bars unrelated to the state that produced them | V8 | `PLUTO LAND` | Source term |
| Position one | The zone immediately above a narrow state for longs, below for shorts. A zone, not a point | V2, V8 | `POSITION 1` / `POSITION −1` | Source term |
| Professional loss | A loss limited to one bar of adverse movement | V7 | `1-BAR RISK` | **UI shortening** of a source term |
| The purge | A move that clears out every nearby price to the left in one motion | V4 | `PURGE` | Source term |
| Pullback swing / rebound swing | A colour change at or near the 20; best when it follows an igniting move | V3 | `PULLBACK` | **UI shortening** of a source term |
| Reset | A brief pause after leg one, sideways or a shallow ~45° drift | V4 | `RESET` | Source term |
| Sleepy state | The swing version of a narrow state: instrument, 20 and 200 all flat and sideways at once | V3 | `SLEEPY` | Source term |
| Snatching at your roses | Taking gains too fast | V2 | *(not rendered)* | Behavioural |
| Snowman | A bar with a wick mistaken for a tail bar; mostly body, so it does not qualify | V8 | `SNOWMAN (NOT A TAIL)` | Source term — rendered only in Debug |
| Space | The distance between price and the 20 | V5, V6 | `SPACE` | Source term |
| State | Narrow, regular or wide, defined by separation between 20 and 200 | All | `STATE` | Source term |
| Sucker play bounce | A bounce bought too high by someone expecting a 50/75/100% retracement | V6 | `SUCKER-PLAY ZONE` | **UI shortening** of a source term |
| Surge off the 200 | A strong move originating at or near a flat 200 with clear space to the left | V4 | `SURGE OFF 200` | **UI shortening** of a source term |
| Tail bar | A bar mostly tail with a small body. Bottoming tails signal upward continuation, topping tails the opposite | V2, V7, V8 | `BOTTOMING TAIL` / `TOPPING TAIL` | Source term |
| Three-finger spread | Instrument, 20 and 200 all separated from each other; too late to enter | V3 | `THREE FINGERS` | **UI shortening** of a source term |
| Three pushes | The profit-taking count | V2, V7 | `PUSH 1/2/3` | Source term |
| Tight picture of power | A narrow state combined with an elephant or tail bar | V7 | *(dashboard phrase)* | Source term |
| Wide state | 20 and 200 meaningfully separated; the high-volatility extreme | All | `WIDE` | Source term |
| 20 MA halt | Price pausing or holding at the 20-period MA, often coinciding with the right shoulder of an inverse H&S | V1 | `20 MA HALT` | Source term |

---

## 4. CONCEPT DEPENDENCY MAP

Read top-down. Nothing below a line may be evaluated before everything above it on its path.

```
                     ┌──────────────────────────────────────┐
                     │  PRICE DATA (standard OHLC bars)     │
                     └───────────────┬──────────────────────┘
                                     │
                     ┌───────────────▼──────────────────────┐
                     │  SMA20 (close)   SMA200 (close)      │  C4, V2.2, V5.1, V6.1, V7.1
                     │  SMA8 (situational, trailing only)   │  C4, V2.2
                     └───────────────┬──────────────────────┘
                                     │
        ┌────────────────────────────┼────────────────────────────┐
        │                            │                            │
┌───────▼────────┐        ┌──────────▼──────────┐      ┌──────────▼──────────┐
│ MA ZONE/BAND   │        │  MA SLOPE / FLATNESS│      │  SPACE (|px − 20|)  │
│  C6, V4.1      │        │  C5, C7, V4.2       │      │  V5.2, V6.2         │
└───────┬────────┘        └──────────┬──────────┘      └──────────┬──────────┘
        └────────────┬───────────────┘                            │
                     │                                            │
        ┌────────────▼───────────────┐                            │
        │  STEP 1 · STATE            │  C8, C9, C10, V2.3, V8.2    │
        │  narrow(G1/G2/G3)/reg/wide │                            │
        └────────────┬───────────────┘                            │
                     │                                            │
        ┌────────────▼───────────────┐                            │
        │  STEP 2 · POSITION         │  C12, C13, C15, V7.2, V8.3  │
        │  +3 +2 +1 0 −1 −2 −3       │                            │
        └────────────┬───────────────┘                            │
                     │                                            │
        ┌────────────▼───────────────┐                            │
        │  STEP 3 · EVENT            │  C16, C17, C18, V2.4, V7.3  │
        │  elephant │ tail │ colour  │                            │
        │  change │ red-bar takeout  │  V7.5                       │
        └────────────┬───────────────┘                            │
                     │                                            │
     ┌───────────────┼───────────────────────────────┐            │
     │               │                               │            │
┌────▼─────┐  ┌──────▼───────┐              ┌────────▼────────┐   │
│ GENERIC  │  │ SPECIALISED  │              │  DIRECTION      │   │
│ ENTRY    │  │ SETUPS       │              │  FILTER: with   │◄──┘
│ C16,C18  │  │ V3.4 sleepy  │              │  the 20's slope │
│          │  │ V4.2 surge   │              │  C7, V5.4       │
│          │  │ V7.4 opening │              └─────────────────┘
│          │  │ V7.8 hidden  │
│          │  │ V6.2 dual sp │
│          │  │ V6.5 scalp   │
│          │  │ V1.3 deep dr │
└────┬─────┘  └──────┬───────┘
     └────────┬──────┘
              │
   ┌──────────▼───────────┐
   │ VIRTUAL LIFECYCLE    │
   │  entry → add (C20)   │
   │  → stop rotation     │  C21, V2.5, V3.8, V7.7
   │  → exit context      │  C22, C23, C11
   └──────────────────────┘
```

**Hard dependency (C1, p.5): STATE → POSITION → EVENT. The order is stated as not optional.**
Every entry-bearing module in this project therefore evaluates state first, position second,
event third, and reports which step failed when a candidate does not qualify.

---

## 5. EVIDENCE LEDGER

Each record appears in **Table A** (identification and exact proposition) and **Table B**
(classification, dependencies, ambiguity). IDs are stable and are referenced by every other
artefact in this repository.

### Source classification key
- **DIRECT** — explicitly stated in the PDF with enough context to support the documented proposition.
- **IMPLIED** — reasonably suggested by the PDF but not stated as a complete rule.
- **SUBJECTIVE** — described visually, discretionarily or qualitatively rather than with an objective formula.
- **UNSUPPORTED** — absent, contradicted, insufficiently defined, or only referenced without mechanics.

### Implementation-impact key
- **EXACT** — deterministic translation possible from the source statement alone.
- **APPROX** — requires a documented, configurable numeric proxy the source does not supply.
- **DISPLAY** — carried as context/diagnostic only; never emits a signal.
- **OMIT** — not implemented; reason recorded.

---

### DOMAIN D1 — PROVENANCE, EDITORIAL FRAMING AND SCOPE

#### Table A
| SRC | Page(s) | Locator | Source type | Short evidence | Exact proposition | Direction | TF/Session scope |
|---|---|---|---|---|---|---|---|
| SRC-001 | 2 | "WHY IT IS BUILT THIS WAY" | Editorial note | Manual splits source into a synthesis layer and a per-video layer | The manual's Part One arrangement is editorial; Part Two preserves each video whole | n/a | n/a |
| SRC-002 | 2 | "THE RULE NUMBERS" | Editorial note | C1–C24 core, V4.3 = video 4 rule 3 | The rule identifiers are the manual's navigation scheme | n/a | n/a |
| SRC-003 | 2, 71 | "READING THE DIAGRAMS"; colophon | Editorial convention | "Both moving averages are drawn as a line sitting inside a soft band" | The band rendering is the manual's chosen visual convention, adopted because Velez describes MAs as zones | n/a | All |
| SRC-004 | 2 | "A CAUTION ON NUMBERS" | Warning (editorial) | "VELEZ'S STATISTICS ARE HIS OWN … none are published, peer-reviewed, or independently verifiable, and none come with a defined sample, date range, or instrument set" | Every percentage in the manual is a self-reported claim without stated sample, date range or instrument set | n/a | n/a |
| SRC-005 | 70 | §3.6 "WHAT THIS MANUAL ADDED" | Editorial note | "All 30 diagrams were drawn for this document. None are from the videos." | The diagrams are the compiler's illustrations, not Velez's chart annotations | n/a | n/a |
| SRC-006 | 70 | §3.6 "WHAT THIS MANUAL ADDED" | Editorial note | "Part one is a synthesis … the arrangement into one framework is not how any single video presents it" | The unified funnel presentation is editorial synthesis | n/a | n/a |
| SRC-007 | 70 | §3.6 coverage table | Editorial note | Per-video transcript coverage with named missing ranges | Videos 2, 3, 7 and 8 have declared unrecovered portions | n/a | n/a |
| SRC-008 | 70 | "HOW THE GAPS WERE HANDLED" | Editorial note | "No content was written to fill a gap" | The manual asserts it invented nothing to cover transcript gaps | n/a | n/a |
| SRC-009 | 68 | §3.5 preamble | Editorial critique | "This section is not Velez. It is an assessment of the material as a system" | §3.5 is explicitly the compiler's critique | n/a | n/a |
| SRC-010 | 36, 56, 68 | Commercial content boxes; GAP FIVE | Marketing | Funded-programme pitches at the end of videos 3, 7, 8; "$50,000 … $375,000 … $750,000"; "$8,000 lifetime … 60% vs 40% split" | Three of eight videos end in sales presentations containing no trading instruction | n/a | n/a |
| SRC-011 | 52 | "TWO ASIDES" | Anecdote | Remark on trader demographics, with the manual's own disclaimer | Recorded by the manual as "an anecdotal generalisation, not a finding, and nothing in the method depends on it" | n/a | n/a |
| SRC-012 | 4, 56 | "THE CLAIM UNDERNEATH ALL OF IT"; V8 "HIS REASONING" | Rationale | "a small rule set is a rule set you can actually follow under pressure"; three independent filters compound | The design intent is to stack three independent filters so their odds compound | n/a | All |

#### Table B
| SRC | Source class | Confidence | Dependencies | Impl. impact | Open ambiguity |
|---|---|---|---|---|---|
| SRC-001 | DIRECT | High | — | DISPLAY | None |
| SRC-002 | DIRECT | High | — | DISPLAY | IDs are editorial; must not be presented as "Velez's rule numbers" |
| SRC-003 | DIRECT | High | SRC-042 | APPROX | Band width never specified numerically |
| SRC-004 | DIRECT | High | D12 (all statistics) | DISPLAY | None — this is the governing constraint on all statistics |
| SRC-005 | DIRECT | High | — | DISPLAY | Diagram geometry is illustrative and cannot be used to derive thresholds |
| SRC-006 | DIRECT | High | SRC-020 | DISPLAY | None |
| SRC-007 | DIRECT | High | — | DISPLAY | Unknown content in named gaps |
| SRC-008 | DIRECT | High | SRC-007 | DISPLAY | None |
| SRC-009 | DIRECT | High | D14 | DISPLAY | None |
| SRC-010 | DIRECT | High | — | OMIT | None |
| SRC-011 | DIRECT | High | — | OMIT | Not implemented; not repeated in output |
| SRC-012 | DIRECT | High | SRC-020 | DISPLAY | None |

---

### DOMAIN D2 — THE CORE FUNNEL

#### Table A
| SRC | Page(s) | Locator | Source type | Short evidence | Exact proposition | Direction | TF/Session scope |
|---|---|---|---|---|---|---|---|
| SRC-020 | 5, 62 | §1.1; C1; index | Rule | "STATE FIRST, THEN POSITION INSIDE THAT STATE, THEN THE TRIGGERING BAR IN THAT POSITION"; "The order is not optional" | Evaluation order is state → position → event, and the manual states it is not optional | Mirrored | All markets, all timeframes |
| SRC-021 | 5, 62 | C2 | Rule / failure mode | "the trade was not bad, it was the wrong trade for that state" | A valid event occurring in the wrong position is the wrong trade, not a misread event | Mirrored | All |
| SRC-022 | 5, 62 | C3 | Rule | "the same three steps run on stocks, options, futures and crypto, on a 2-minute chart or a monthly one" | The three steps are timeframe-agnostic and market-agnostic; only holding time changes | Mirrored | All |
| SRC-023 | 5 | §1.1 step 1 summary | Definition | "Close together is narrow. Far apart is wide" | State = relationship between the 20 and the 200 | n/a | All |
| SRC-024 | 5 | §1.1 step 2 summary | Definition | "Immediately above it, moderately away, or far away" | Position = where price sits relative to the state | Mirrored | All |
| SRC-025 | 5 | §1.1 step 3 summary | Rule | "No event means no trade, however good the first two look" | An event is mandatory; state and position alone never constitute a signal | Mirrored | All |
| SRC-026 | 5, 55 | C1 analogy; V8.5 | Rationale | County → plot of land → house | The real-estate analogy expresses the same ordering constraint | n/a | All |
| SRC-027 | 55, 64 | V8.6 | Practice | "Grab the state. Locate the position. Identify the event." | The drill is to state all three aloud before acting | n/a | All |
| SRC-028 | 66 | §3.4 "BEFORE CLICKING BUY" | Checklist | 8 checklist items incl. state/position/event, tail validity, direction of the 20, stop placement, stop fit, leg number | The manual operationalises the framework as an 8-item pre-entry checklist | Mirrored | All |
| SRC-029 | 66 | §3.4 "THE FIVE REASONS TO DO NOTHING" | Rule | No narrow state; perfect location but no event; already wide and entering with trend; stop will not fit in the bottom third within max loss; reaching for a name not on the list | Five explicitly enumerated no-trade conditions | Mirrored | All |

#### Table B
| SRC | Source class | Confidence | Dependencies | Impl. impact | Open ambiguity |
|---|---|---|---|---|---|
| SRC-020 | DIRECT | High | SRC-050, SRC-070, SRC-090 | EXACT | None on ordering; the *thresholds* of each step are separately ambiguous |
| SRC-021 | DIRECT | High | SRC-020 | EXACT | None |
| SRC-022 | DIRECT | High | — | EXACT | Does not say thresholds are constant across timeframes |
| SRC-023 | DIRECT | High | SRC-030 | APPROX | "Close together" undefined — see SRC-051 |
| SRC-024 | DIRECT | High | SRC-023 | APPROX | Boundaries undefined — see SRC-071 |
| SRC-025 | DIRECT | High | SRC-090 | EXACT | None |
| SRC-026 | DIRECT | High | SRC-020 | DISPLAY | None |
| SRC-027 | DIRECT | High | SRC-020 | DISPLAY | None |
| SRC-028 | DIRECT | High | Most of D3–D8 | EXACT | Item "Is this leg one or leg two?" depends on SRC-201 |
| SRC-029 | DIRECT | High | SRC-050, SRC-090, SRC-197 | EXACT | "Reaching for a name not on my list" is a watchlist-discipline item, not chartable |

---

### DOMAIN D3 — THE MOVING AVERAGES

#### Table A
| SRC | Page(s) | Locator | Source type | Short evidence | Exact proposition | Direction | TF/Session scope |
|---|---|---|---|---|---|---|---|
| SRC-030 | 6, 62 | §1.2; C4 | Rule | "The 20 and the 200 stay on every chart, on every timeframe, permanently" | The 20 and 200 are permanent fixtures | n/a | All |
| SRC-031 | 6, 45 | §1.2; V6.1 | Rule | "Both are simple moving averages calculated on closing prices" | Both averages are **simple** MAs on **close** | n/a | All |
| SRC-032 | 6, 45 | §1.2; V6.1 | Claim | "tested exponential and weighted variants across 40 years and found no advantage worth the complexity" | Velez states simple beats exponential/weighted for his purpose | n/a | All |
| SRC-033 | 6, 29, 51 | C4; V2.2; V7.7 | Rule | "an 8-period average appears as a situational trailing tool"; "It comes out only when a stock accelerates faster than the 20 can track, and it is used for trailing rather than for entries" | The 8-period SMA is a situational **trailing-only** tool, never an entry tool | Mirrored | All |
| SRC-034 | 6, 45 | C4; V6.1 buddy system | Reference | "He also uses 13 and 8 as the short partner in some setups"; "20 and 200, or 13 and 200, or 8 and 200" | A 13-period short partner is referenced as existing in "some setups" | n/a | Unspecified |
| SRC-035 | 6, 7, 62 | C5; diagrams p.7 | Rule | "20 SMA — strongest when sloping, ideally near 45°; weakest when flat" | The 20's power comes from **slope** | Mirrored | All |
| SRC-036 | 6, 7, 37, 62 | C5; V4.2 | Rule | "200 SMA — strongest when flat; weakest when sloping. A sloping 200 still works, it just gets cut through more often" | The 200's power comes from **flatness**; a sloping 200 retains reduced power | Mirrored | All |
| SRC-037 | 6, 7 | C5 table | Definition | "Support when rising, resistance when declining" (20) | The 20 acts as support in an uptrend and resistance in a downtrend | Mirrored | All |
| SRC-038 | 6, 7 | C5 table | Definition | "Floor when approached from above, ceiling when approached from below" (200) | The 200 acts as floor/ceiling depending on approach side | Mirrored | All |
| SRC-039 | 6, 7 | C5 table | Definition | "Flat means resting, not broken: price crisscrosses it and ignores it" | A flat 20 is dormant, not invalidated | n/a | All |
| SRC-040 | 7, 45, 62 | C7; V6.1 | Rule | "If the 20 is declining, the trend is down no matter how many green bars print inside it. If the 20 is rising, the trend is up no matter how much red appears" | The 20's slope defines trend direction irrespective of individual bar colours | Mirrored | All |
| SRC-041 | 7, 45 | C7; V6.1 | Rationale | "moving averages were once called trend lines for exactly this reason" | Rationale for SRC-040 | n/a | All |
| SRC-042 | 7, 37, 62 | C6; V4.1 | Rule | "A small penetration through either average is a lean, not a break"; "if he built his own platform he would shade a coloured band on each side of the line" | Both averages are **zones, not lines**; minor penetrations are leans | Mirrored | All |
| SRC-043 | 7 | C6 practical version | Rule (subjective) | "judge breaks by eye and by follow-through, not by whether the close printed a fraction below the line" | Break determination is explicitly discretionary and follow-through-based | Mirrored | All |
| SRC-044 | 37, 40 | V4.2; "HIS REASONING" | Rationale | "A flat 200 is a genuine equilibrium. Supply and demand have been balanced there for a long time" | Rationale for the flat-200 power claim | n/a | All |

#### Table B
| SRC | Source class | Confidence | Dependencies | Impl. impact | Open ambiguity |
|---|---|---|---|---|---|
| SRC-030 | DIRECT | High | — | EXACT | None |
| SRC-031 | DIRECT | High | SRC-030 | EXACT | None — length, type and source series are all stated |
| SRC-032 | DIRECT | High | SRC-031 | DISPLAY | Unverifiable claim; does not affect implementation beyond confirming SMA |
| SRC-033 | DIRECT | High | SRC-146, SRC-155 | EXACT | "Accelerates faster than the 20 can track" is undefined — see SRC-147 |
| SRC-034 | **UNSUPPORTED** | High | — | OMIT | Which setups use 13 is never stated. **No 13-period rule may be created.** |
| SRC-035 | DIRECT | High | SRC-031 | APPROX | "Sloping" and "flat" have no numeric definition; "ideally near 45°" is scale-dependent and not computable from price data alone |
| SRC-036 | DIRECT | High | SRC-031 | APPROX | Same flatness ambiguity |
| SRC-037 | DIRECT | High | SRC-035 | DISPLAY | None |
| SRC-038 | DIRECT | High | SRC-036 | DISPLAY | None |
| SRC-039 | DIRECT | High | SRC-035 | DISPLAY | None |
| SRC-040 | DIRECT | High | SRC-035 | APPROX | Requires a slope threshold to separate rising/declining from flat |
| SRC-041 | DIRECT | High | SRC-040 | DISPLAY | None |
| SRC-042 | DIRECT | High | SRC-003 | APPROX | Band width never quantified anywhere in the manual |
| SRC-043 | **SUBJECTIVE** | High | SRC-042 | DISPLAY | Explicitly resists quantification |
| SRC-044 | DIRECT | High | SRC-036 | DISPLAY | None |

**Note on "ideally near 45°" (SRC-035).** A 45° angle on a price chart is a function of the
chart's pixel aspect ratio, price scaling and zoom level. It is not a property of the data. It
is therefore recorded as an unimplementable visual heuristic and is **not** used to derive any
slope threshold.

---

### DOMAIN D4 — MARKET STATE (STEP ONE)

#### Table A
| SRC | Page(s) | Locator | Source type | Short evidence | Exact proposition | Direction | TF/Session scope |
|---|---|---|---|---|---|---|---|
| SRC-050 | 8, 28, 53 | §1.3; V2.3; V8.2 | Definition | "A market has three states and moves between them permanently" | Three states exist: narrow, regular (middling), wide | n/a | All |
| SRC-051 | 8, 54, 62 | C8; "EYEBALL IT" | Rule (subjective) | "Are the 20 and the 200 relatively close together? … Velez is emphatic that this is an eyeball judgement and warns against measuring the gap with a tool"; "Close does not mean touching" | The narrow test is explicitly an eye judgement and the manual records an explicit instruction **not to measure it** | n/a | All |
| SRC-052 | 8, 28, 53 | §1.3; V2.3; V8.2 | Definition | "markets are trapped on rails: narrow to wide, wide back to narrow, with no third option" | State transitions are a two-way cycle only | n/a | All |
| SRC-053 | 8, 54 | §1.3; V8.2 "WHY IT WORKS" | Rationale | "A narrow state is the no-volatility extreme. Once volatility has been squeezed out … the only place left to go is back into volatility" | Volatility-compression-precedes-expansion is the stated mechanism | n/a | All |
| SRC-054 | 8, 54 | §1.3; V8.2 | Rule | "You do not need to predict the direction. You wait to see which side triggers first and take the emergence" | Direction is not forecast; the emergence direction is followed | Mirrored | All |
| SRC-055 | 8, 9, 53, 54 | C9; V8.2 grades table | Rule | Rank 1: "20 flat, 200 flat, close together, and the stock itself flat and sandwiched between them" | Grade 1 narrow requires **three** simultaneous flat items plus proximity | n/a | All |
| SRC-056 | 8, 9, 53, 54 | C9; V8.2 | Rule | Rank 2: "200 flat, 20 sloping, still close together" | Grade 2 narrow: 200 flat, 20 sloping, still close | n/a | All |
| SRC-057 | 8, 9, 53, 54 | C9; V8.2 | Rule | Rank 3: "Close together, neither one flat" | Grade 3 narrow: close, neither flat; "price gets whippy" | n/a | All |
| SRC-058 | 9, 53 | C9 detail; V8.2 | Claim | "high frequency, not a once-a-month event"; "appears every day"; "He refuses to teach anything low-frequency" | Velez claims grade-1 narrow states are common | n/a | All |
| SRC-059 | 9, 62 | C10 | Definition | "Regular is the middle. Price has emerged from narrow and is trending, but the averages are not extremely separated yet. This is where you trade colour changes off the 20" | Regular state: emerged and trending; the colour-change-off-the-20 regime | Mirrored | All |
| SRC-060 | 9, 28, 62 | C10; V2.3 | Definition | "Wide is the high-volatility extreme, with the averages meaningfully apart. The 20 MA trading game stops here" | Wide state: averages meaningfully apart; with-trend 20-MA entries stop | Mirrored | All |
| SRC-061 | 9, 54, 62 | C10; V8.2 | Rule | "Narrow. Buy or short the emergence out of it, in the direction of the emergence. Regular. Buy colour changes and tail bars at or near the 20, with the trend. Wide. Stop trading with the trend. Look for power in the reverse direction." | Mode by state: emergence / near-the-20 continuation / reversion | Mirrored | All |
| SRC-062 | 9, 52, 62 | C11; V7.9 | Rule | "If you entered from a narrow state and the state is now wide, that is the exit signal by itself, independent of any other rule" | Narrow→wide transition is a standalone exit signal | Mirrored | All |
| SRC-063 | 28, 62 | V2.3 "ONLY TWO SPOTS MATTER" | Rule | "Spot one … the emergence out of a narrow state … Spot two: the reversion from a wide state back toward narrow … there is no third spot" | Exactly two tradable spots in the state cycle | Mirrored | All |
| SRC-064 | 48, 62 | V7.2 | Rule | "State … Narrow or wide. The relationship between the 20 and the 200"; "Location … Above or below" | State and location are separate axes and must not be conflated | Mirrored | All |

#### Table B
| SRC | Source class | Confidence | Dependencies | Impl. impact | Open ambiguity |
|---|---|---|---|---|---|
| SRC-050 | DIRECT | High | SRC-030 | APPROX | Boundaries between the three undefined |
| SRC-051 | **SUBJECTIVE** | High | SRC-050 | APPROX | **The manual explicitly instructs against measurement.** Any numeric threshold is a departure from the source and must be labelled as such |
| SRC-052 | DIRECT | High | SRC-050 | EXACT | Does not forbid regular as an intermediate — "no third option" refers to direction of travel, and p.28/p.9 both name a middle state. Recorded as terminology tension, see §10.3 |
| SRC-053 | DIRECT | High | — | DISPLAY | None |
| SRC-054 | DIRECT | High | SRC-052 | EXACT | "Triggers first" is operationalised by the event layer |
| SRC-055 | DIRECT | High | SRC-035, SRC-036 | APPROX | Requires flatness thresholds for three separate series |
| SRC-056 | DIRECT | High | SRC-055 | APPROX | Same |
| SRC-057 | DIRECT | High | SRC-055 | APPROX | Same |
| SRC-058 | **UNSUPPORTED** as a measurable | High | SRC-055 | DISPLAY | Frequency claim with no sample; must not be turned into a calibration target |
| SRC-059 | DIRECT | High | SRC-050 | APPROX | "Not extremely separated yet" undefined |
| SRC-060 | DIRECT | High | SRC-050 | APPROX | "Meaningfully apart" undefined |
| SRC-061 | DIRECT | High | SRC-050 | EXACT | Mode selection is exact once state is known |
| SRC-062 | DIRECT | High | SRC-050 | EXACT | Exact once state is known |
| SRC-063 | DIRECT | High | SRC-061 | EXACT | Tension with SRC-059/061 which describe a third (regular) mode — see §10.3 |
| SRC-064 | DIRECT | High | SRC-050, SRC-070 | EXACT | None |

---

### DOMAIN D5 — POSITION (STEP TWO)

#### Table A
| SRC | Page(s) | Locator | Source type | Short evidence | Exact proposition | Direction | TF/Session scope |
|---|---|---|---|---|---|---|---|
| SRC-070 | 10, 54, 64 | §1.4; V8.3 | Definition | "Velez counts seven: three above, three below, and the state itself" | Seven positions: +3, +2, +1, 0, −1, −2, −3 | Mirrored | All |
| SRC-071 | 10 | Ladder diagram | Definition | +1 "immediately above"; +2 "not close, not far"; +3 "wide" | Position bands are ordered by distance from the state, measured from the state itself | Mirrored | All |
| SRC-072 | 10, 60, 62 | C12; glossary "Position one" | Rule | "The best buy zone in the system. Target 80%+ of all longs here"; "A zone, not a point" | Position one is a zone; the stated target is 80%+ of trades taken there | Mirrored | All |
| SRC-073 | 10, 54, 64 | C12; V8.3 | Claim / behavioural target | "If I could convince you that you need to be a position one expert, that 80 plus percent of all of your buys should be right above flat, tight, narrow states, you would be a trading star" | 80%+ is a behavioural target Velez sets, not an observed statistic | Mirrored | All |
| SRC-074 | 10, 62 | C13 | Rule | Position 1/−1 = "with the emergence"; Position 2 = "mostly with the trend … occasionally shortable if the counter-power is strong enough"; Position 3 = "against the move" | Directional bias by position band | Mirrored | All |
| SRC-075 | 10 | Ladder diagram, row 0 | Rule | "The state — Do not trade inside it. Wait for the emergence" | No entries inside the state band itself | Mirrored | All |
| SRC-076 | 10, 54 | Ladder; V8.3 | Rule | "The mirror is not optional. You do not have a favourite side. Power in plus one, you buy it. Power in negative one, you short it" | Long and short logic is fully symmetric | Mirrored | All |
| SRC-077 | 11, 55, 62 | C14; V8.4 | Rule | "NEVER PUT THE STOP INSIDE POSITION ONE … The stop belongs beyond the state itself, not merely beyond your entry bar" | Stops must sit outside the state band; a stop inside position one is named a "dumb stop" | Mirrored | All |
| SRC-078 | 11 | C14 exception | Exception | "The exception is the opening-bell method in video 7, where the whole design is a one-bar risk budget and the stop sits a penny below bar one on purpose" | The opening-bell method is an explicitly named exception to SRC-077 | Mirrored | Opening bell only |
| SRC-079 | 11, 56, 62 | C15; V8 warnings | Rule / warning | "Pluto land … the zone far past position one where traders chase green bars that have nothing to do with the state that produced them. A bar can look identical in Pluto land and in position one. The bar is not the signal. The bar plus the location is the signal" | Event validity is location-dependent; the far zone is named Pluto land | Mirrored | All |

#### Table B
| SRC | Source class | Confidence | Dependencies | Impl. impact | Open ambiguity |
|---|---|---|---|---|---|
| SRC-070 | DIRECT | High | SRC-050 | APPROX | Band boundaries never quantified |
| SRC-071 | DIRECT (structure) / SUBJECTIVE (boundaries) | High | SRC-070 | APPROX | "Immediately above", "not close not far", "wide" are all qualitative |
| SRC-072 | DIRECT | High | SRC-070 | EXACT | Zone-not-point is exact once boundaries exist |
| SRC-073 | DIRECT as a stated target | High | SRC-072 | DISPLAY | **Must not be presented as a measured statistic** — p.19 explicitly calls it "A behavioural target he sets, not an observed statistic" |
| SRC-074 | DIRECT | High | SRC-070 | EXACT | "Occasionally shortable if the counter-power is strong enough" is discretionary |
| SRC-075 | DIRECT | High | SRC-070 | EXACT | None |
| SRC-076 | DIRECT | High | SRC-074 | EXACT | None |
| SRC-077 | DIRECT | High | SRC-070 | EXACT | None |
| SRC-078 | DIRECT | High | SRC-077, SRC-210 | EXACT | None |
| SRC-079 | DIRECT | High | SRC-070 | APPROX | "Far past" boundary undefined; equals the +3/−3 band in the ladder |

---

### DOMAIN D6 — EVENTS (STEP THREE)

#### Table A
| SRC | Page(s) | Locator | Source type | Short evidence | Exact proposition | Direction | TF/Session scope |
|---|---|---|---|---|---|---|---|
| SRC-090 | 12, 29, 62 | §1.5; V2.4 | Definition | "the market repeats a limited alphabet of 13 bar types in no fixed order, and … he teaches professional traders 14 actionable events. He publicly teaches three" | Three events are taught publicly: elephant bar, tail bar, colour change | Mirrored | All |
| SRC-091 | 12, 20, 55 | §1.5; stats; V8.5 | Reference | "13 … Never enumerated in these eight videos. Only four are named"; "14 … The other eleven are not in this material" | The 13 bar types and 14 events are referenced but **never enumerated** | n/a | All |
| SRC-092 | 12, 59, 62 | C16; glossary | Definition | "A bar visibly larger and taller than the bars around it, in either colour" | Elephant bar = a bar visibly larger than its neighbours | Both | All |
| SRC-093 | 12, 49 | C16; V7.3 | Rationale | "Only an institution with the size to move price can print a bar that far out of line with its neighbours"; "Institutions … wish they could go in without leaving a footprint behind. But they can't. They're too big" | The institutional-footprint rationale for elephant bars | n/a | All |
| SRC-094 | 12, 19, 29 | C16; stats table | Claim | "follow-through at roughly 80% … That figure only applies when the bar occurs in the right position" | 80% follow-through is claimed **and explicitly conditioned on correct position** | Both | All |
| SRC-095 | 12, 55, 62 | C16; V8.5 | Rule | "Buy into the elephant bar before it finishes trading, or buy the next bar that clears the high of the elephant bar" — "he tells his traders to use both" | Two accepted elephant entry methods | Mirrored | All |
| SRC-096 | 12, 55, 60, 62 | C17; V8.5; glossary | Definition | "Most of the bar must be tail. The body is the small part. A bar that is mostly body with a wick hanging off it is not a tail bar" | Tail bar = tail dominates the bar; body is the minority | Both | All |
| SRC-097 | 12, 55, 60 | C17; V8.5; glossary "Snowman" | Definition / counterexample | "He calls that a snowman and says it does not count" | The snowman is the named counterfeit tail bar | Both | All |
| SRC-098 | 12, 29, 49 | C17; V2.4; V7.3 | Rule | "A bottoming tail bar signals likely upward continuation. A topping tail bar signals the opposite" | Directional meaning of the two tail variants | Mirrored | All |
| SRC-099 | 12 | C17 | Rule | "both are only meaningful in position one or off the 20 in a regular state" | Tail bars are location-gated | Mirrored | All |
| SRC-100 | 49 | V7.3 | Definition | "A bottoming tail means price was at the bottom of the tail and moved to the top of the body" | Mechanical description of the bottoming tail | Both | All |
| SRC-101 | 49 | V7.3 | Grouping | "four bars as money going into the market: the green elephant and the bottoming tail, plus their bearish mirrors as money coming out" | The four power bars grouped by money-in / money-out | Mirrored | All |
| SRC-102 | 13, 29, 42, 62 | C18; V2.4; V5.3 | Definition | "One colour takes out the high of an opposite-coloured bar next to it. Green trades above the high of a red bar, or red trades below the low of a green bar" | Colour change definition | Mirrored | All |
| SRC-103 | 13, 60 | C18; glossary "Bull 180" | Terminology | "He also calls the bullish version a bull 180" | Bull 180 = bullish colour change | Long | All |
| SRC-104 | 13, 29, 42, 62 | C18; V2.4; V5.3 | Rule | "It does not need to be back to back. Two or three reds can print first. What counts is the first green bar that takes out a red high" | The takeout need not be adjacent | Mirrored | All |
| SRC-105 | 13, 42, 62 | C18; V5.3 | Rule | "Ignore the tails. Use the body of the red bar when marking the level" | The reference level is the **body** extreme, not the wick | Mirrored | All |
| SRC-106 | 13, 62 | C18 | Rule | "The entry price is the takeout price, one tick above the marked high" | Entry = one tick beyond the marked body extreme | Mirrored | All |
| SRC-107 | 13, 29, 55, 62 | C18; V2.4; V8.5 | Rule | "A colour change is a two-bar elephant event in itself. If a single bar is both an elephant bar and a colour change at once, Velez rates it more powerful than either alone" | The combination effect | Mirrored | All |
| SRC-108 | 13 | C18 | Rule | "Where it works: strongest right off position one. Still strong off the 20 in a regular state. Not relevant once price is wide" | Colour-change validity by location/state | Mirrored | All |
| SRC-109 | 12 | Eight-shape diagram | Visual definition | Bull elephant, bear elephant, bottoming tail, topping tail, colour change bull, colour change bear, elephant+change, "NOT A TAIL BAR" | Eight canonical shapes; the eighth is a counterexample, not an event | Mirrored | All |

#### Table B
| SRC | Source class | Confidence | Dependencies | Impl. impact | Open ambiguity |
|---|---|---|---|---|---|
| SRC-090 | DIRECT | High | SRC-020 | EXACT | None |
| SRC-091 | **UNSUPPORTED** | High | SRC-090 | OMIT | **The missing 10/11 members must not be invented.** |
| SRC-092 | DIRECT concept / SUBJECTIVE threshold | High | — | APPROX | "Visibly larger"; neighbourhood size undefined; no ratio given |
| SRC-093 | DIRECT | High | SRC-092 | DISPLAY | None |
| SRC-094 | DIRECT claim | High | SRC-092, SRC-070 | DISPLAY | Self-reported; must never be rendered as a probability output |
| SRC-095 | DIRECT | High | SRC-092 | EXACT (method 2) / OMIT (method 1) | Method 1 ("before it finishes trading") is intrabar and cannot be confirmed at bar close |
| SRC-096 | DIRECT | High | — | APPROX (ratio) | "Most" implies >50% but the exact ratio is not given |
| SRC-097 | DIRECT | High | SRC-096 | EXACT | None — it is the logical negation of SRC-096 |
| SRC-098 | DIRECT | High | SRC-096 | EXACT | None |
| SRC-099 | DIRECT | High | SRC-070, SRC-059 | EXACT | None |
| SRC-100 | DIRECT | High | SRC-096 | DISPLAY | None |
| SRC-101 | DIRECT | High | SRC-092, SRC-096 | EXACT | None |
| SRC-102 | DIRECT | High | — | EXACT | None |
| SRC-103 | DIRECT | High | SRC-102 | EXACT | None |
| SRC-104 | DIRECT | High | SRC-102 | **APPROX** | **Which red** supplies the level when several print is not stated — see §10.2 |
| SRC-105 | DIRECT | High | SRC-102 | EXACT | None — body extreme is unambiguous |
| SRC-106 | DIRECT | High | SRC-105 | EXACT | "One tick" generalises to `syminfo.mintick` |
| SRC-107 | DIRECT | High | SRC-092, SRC-102 | EXACT | "More powerful" is ordinal only — no magnitude given |
| SRC-108 | DIRECT | High | SRC-050, SRC-070 | EXACT | None |
| SRC-109 | DIRECT | High | SRC-092–108 | EXACT | Diagram geometry is illustrative; no ratios derivable |

---

### DOMAIN D7 — ENTRIES, ADDS AND GAPS

#### Table A
| SRC | Page(s) | Locator | Source type | Short evidence | Exact proposition | Direction | TF/Session scope |
|---|---|---|---|---|---|---|---|
| SRC-120 | 13, 30, 62 | C19; V2.7 | Rule | "When a stock gaps out of a narrow state, mentally fill the gap in as if price had traded through it continuously. That turns the gap itself into the elephant bar one, so the first real bar of continued green becomes your bar two trigger" | Gap-fill technique: the gap substitutes for elephant bar one | Mirrored | Instruments that gap (session-based) |
| SRC-121 | 13, 52 | C19 caveat; V7 warnings | Exception | "if the gap is so large that you are already opening in a wide state, the setup is gone. A gap can carry price past position one before you ever get to trade it" | Oversized gap invalidates the setup | Mirrored | Session open |
| SRC-122 | 13, 65 | C19; §3.3 | Editorial note | "Velez teaches this in two separate videos, which is a signal he considers it load-bearing" | Recurrence is treated by the manual as an importance signal | n/a | n/a |
| SRC-123 | 13, 30, 62 | C20; V2.8 | Rule | "ADD ON THE FIRST COLOUR CHANGE AFTER ENTRY … described as mandatory rather than optional, in two separate videos" | Adding on the first post-entry colour change is mandatory | Mirrored | All |
| SRC-124 | 13, 30 | C20; V2.8 | Rule | "You enter with part of the intended size, then add when the first colour change confirms" | Initial entry is partial size | Mirrored | All |
| SRC-125 | 13 | C20 rationale | Rationale | "every large trader is an adder. An in-and-out trader with one entry per idea cannot build a large position on the trades that work" | Rationale for mandatory adds | n/a | All |
| SRC-126 | 30, 55 | V2.8; V8.5 | Rule | "during the trending leg you can keep taking additional plays at or near the 20. A tail bar sitting on the 20 counts. An elephant bar near the 20 counts. It does not have to be touching, just close" | Continued adds are permitted on subsequent events at/near the 20 | Mirrored | All |
| SRC-127 | 30, 34 | V2.8; V3.5 | Rule | "If the pullback happens close to the igniting move, take both and treat the second as an add to the same trade, labelled 1A and 1B. If they are far apart, treat them as trade one and trade two" | Proximity decides add-vs-new-trade | Mirrored | Swing (V3 context) |
| SRC-128 | 55 | V8.5 later examples | Example | WFC: "emerging elephant bar out of the narrow state, entry, mandatory add on the first colour change, continued adds on subsequent colour changes off the 20, and an explicit warning not to chase green bars once price is far from the state" | The full canonical sequence as one worked example | Long | 2-min (implied) |
| SRC-129 | 30, 55 | V2.8; V8.5 | Rule | "then exit as price stretches into a wide state where the game stops and a reversal becomes the next consideration"; "mapping the trade through position one to position two to position three, taking profits as price moves into three rather than continuing to hold" | Position progression 1→2→3 is the profit-taking map | Mirrored | All |
| SRC-130 | 34, 63 | V3.6 | Rule | "Enter the igniting bar during the latter part of the hour, roughly the last 15 to 20 minutes, rather than early" — because a move igniting early can fully reverse before the bar closes | Hourly-bar entry timing preference | Mirrored | **Hourly chart only** |
| SRC-131 | 34 | V3.6 | Rule | "If you miss the bar entirely, take the next bar that clears its high instead" | Fallback to the next-bar-clears-high method | Mirrored | Hourly |
| SRC-132 | 50, 64 | V7.5 | Rule | "Watch the very first red bar after entry. If that red bar does not produce a second consecutive red, and a green bar then trades one penny above the red bar's high, that is the add" | Little red bar takeout — fully mechanical add trigger | Mirrored | Opening bell (and generalised, see SRC-133) |
| SRC-133 | 50 | V7.5 "THIS GIVES YOU A THIRD PLAYABLE EVENT" | Rule | "The add rule works even when the original bar was not a clean elephant or tail. That means the little red bar takeout can be an *entry* in its own right. Three playable events total: elephants, tails, and little red bar takeouts, each valid only in the right location and the right state" | The red-bar takeout is promoted to a standalone entry event | Mirrored | All (location/state gated) |
| SRC-134 | 50 | V7.5 | Rule | "Split the second $25,000 rather than deploying it all at once. Roughly $12,000 to $13,000 at a time" | The add itself is scaled into roughly two tranches | Mirrored | Opening bell |

#### Table B
| SRC | Source class | Confidence | Dependencies | Impl. impact | Open ambiguity |
|---|---|---|---|---|---|
| SRC-120 | DIRECT | High | SRC-050, SRC-092 | EXACT | "First real bar of continued green" — continuation test not formally defined |
| SRC-121 | DIRECT | High | SRC-060 | EXACT | Depends on the wide-state threshold |
| SRC-122 | DIRECT (editorial) | High | — | DISPLAY | None |
| SRC-123 | DIRECT | High | SRC-102 | EXACT | None |
| SRC-124 | DIRECT | High | SRC-123 | DISPLAY | Size fractions are position sizing, not chart geometry |
| SRC-125 | DIRECT | High | SRC-123 | DISPLAY | None |
| SRC-126 | DIRECT | High | SRC-102, SRC-096, SRC-092 | APPROX | "Near"/"close" to the 20 undefined |
| SRC-127 | DIRECT | High | SRC-190 | **APPROX** | "Close"/"far apart" between ignition and pullback undefined |
| SRC-128 | DIRECT (example) | High | SRC-120–129 | DISPLAY | An example, not a distinct rule |
| SRC-129 | DIRECT | High | SRC-070 | EXACT | None |
| SRC-130 | DIRECT | High | — | **OMIT** | Intrabar timing preference; cannot be expressed by a bar-close indicator |
| SRC-131 | DIRECT | High | SRC-095 | EXACT | None |
| SRC-132 | DIRECT | High | — | EXACT | Fully mechanical; "one penny" → `syminfo.mintick` |
| SRC-133 | DIRECT | High | SRC-132, SRC-050, SRC-070 | EXACT | None |
| SRC-134 | DIRECT | High | SRC-132 | DISPLAY | Position sizing |

---

### DOMAIN D8 — STOPS AND TRADE MANAGEMENT

#### Table A
| SRC | Page(s) | Locator | Source type | Short evidence | Exact proposition | Direction | TF/Session scope |
|---|---|---|---|---|---|---|---|
| SRC-140 | 14, 62 | §1.6; C21 | Rule | "The original stop is a starting point, not a setting. As the trade develops you roll protection upward, always to whichever reference is currently highest" | The stop is never left static; always rolled to the most protective reference | Mirrored | All |
| SRC-141 | 14 | §1.6 intro | Rationale | "you can have the best entries in the world, but if you do not know how to handle the position once you are in it, all of that is for nothing" | Trade management is asserted as where the results come from | n/a | All |
| SRC-142 | 14, 29, 51, 59 | Five-methods table; V2.5; V7.7; glossary | Rule | "Big bar — Every time a large, solid bar prints, move the stop under it … healthy stocks do not take out their own fat bars" | Big-bar/fat-bar trailing method | Mirrored | All |
| SRC-143 | 14, 51, 59, 64 | Five-methods table; V7.7 | Rule | "When you get one counter-colour bar followed by two bars in your direction … move the stop just beyond that isolated counter-colour bar. **Long: red, then green-green. Short: green, then red-red**" | Colour-adjustment trailing method, with both directional forms stated | Mirrored | All (V7 origin) |
| SRC-144 | 14, 35, 60 | Five-methods table; V3.8; glossary "Pivot" | Rule | "When a new swing low forms and price moves away from it, put the stop under it. Hold there until the 20 climbs past that level, then go back to trailing the 20" | Pivot trailing method with an explicit hand-back condition | Mirrored | Swing (V3 origin), generalised in §1.6 |
| SRC-145 | 14, 29, 35 | Five-methods table; V2.5; V3.8 | Rule | "Trail under the 20. If price accelerates faster than the 20 can track, switch to the 8-period average and ride until price breaks the 8" | Moving-average trailing method with an 8-SMA escalation | Mirrored | All |
| SRC-146 | 14, 29, 59 | Five-methods table; V2.5; glossary | Rule | "Only once price has separated from the 20 and lost that support. Move the stop under every single bar until you get knocked out" | Bar-by-bar trailing method, gated on separation from the 20 | Mirrored | All |
| SRC-147 | 29, 62 | V2.5 | Rule | "The selection rule is distance from the 20. Close to it, use big bars. Away from it, use bar by bar. Accelerating hard, use the 8" | Method selection is a function of distance from the 20 | Mirrored | All |
| SRC-148 | 14, 15, 35 | "THE ROTATION RULE"; V3.8 | Rule | "You apply whichever one currently gives the highest protective level, and you re-check every time a new bar qualifies" | Rotation = take the most protective of the currently-qualifying references | Mirrored | All |
| SRC-149 | 14, 35, 36 | "WHY PIVOTS ALONE FAIL"; V3.8 | Rule / warning | "A pivot-only trailing stop can give back too much accumulated profit in a single sharp swing, because the stop sits far below price while the pivot waits to form" | Pivot-only trailing is explicitly rejected | Mirrored | All |
| SRC-150 | 14 | "THE ROTATION RULE" | Rule | "For swing trading Velez repeats it as a mantra: pivot, big bar, moving average. For the opening bell it is fat bar, colour adjust, fat bar, colour adjust" | Two named reference sets, selected by timeframe context | Mirrored | Swing / opening bell |
| SRC-151 | 38, 63 | V4.3 rule 4; stop-ladder diagram | Rule | "Ideally under the entire move, under the low of the origin, as long as that does not exceed your maximum loss per trade. If it does, move the stop up, but it must stay in the bottom third of the triggering bar. Higher than that and … it is not your trade" | Three-rung stop ladder with an explicit skip rung | Mirrored | Surge off the 200 |
| SRC-152 | 15 | Rotation diagram label | **Inconsistency** | Diagram labels an isolated group "GREEN, RED, RED" while illustrating a long-side (rising) sequence | The diagram label states the **short-side** colour-adjust pattern on a rising chart | — | — |
| SRC-153 | 33, 63 | V3.4 stop column | Rule | Igniting swing stop: "Under the entire sleepy base and both averages, or tighter, just under the igniting bar itself" | Two acceptable stop references for the igniting swing | Mirrored | Swing |
| SRC-154 | 33, 63 | V3.4 stop column | Rule | Pullback swing stop: "Under the pullback low, which he calls the V, or under the pivot" | Stop references for the pullback swing | Mirrored | Swing |
| SRC-155 | 49, 64 | V7.4 step 7; "A PROFESSIONAL LOSS IS ONE BAR" | Rule | "Stop goes one penny below the low of bar one"; "you will not allow it to move more than one bar against you, ever" | Opening-bell stop = one tick beyond bar one's extreme; the risk budget is one bar | Mirrored | Opening bell |
| SRC-156 | 51 | V7.6 | Rule | "raising the stop and taking profits happen at the same time. The stop is insurance in case something bad starts. The intended exit is on the way up with momentum, after which you cancel the stop" | Stop and profit management run concurrently; the stop is not the plan | Mirrored | All |
| SRC-157 | 52, 30 | V7 warnings; V2 warnings | Warning | "Leaving the original stop static instead of rotating it" | Static stops are a named failure mode in two lessons | Mirrored | All |
| SRC-158 | 66 | §3.4 "ONCE YOU ARE IN" | Checklist | First colour change → add; new fat bar → move stop under it; counter-colour then two in my direction → move stop past it; price separated from the 20 → switch to bar by bar or the 8 | The in-trade checklist restates the rotation as four triggers | Mirrored | All |
| SRC-159 | 35 | V3.8 | Terminology | "His phrasing for the combined method is that it is a very big money stop approach" | Velez's own characterisation of the rotating stop | n/a | Swing |

#### Table B
| SRC | Source class | Confidence | Dependencies | Impl. impact | Open ambiguity |
|---|---|---|---|---|---|
| SRC-140 | DIRECT | High | SRC-142–146 | EXACT | None |
| SRC-141 | DIRECT | High | — | DISPLAY | None |
| SRC-142 | DIRECT | High | SRC-092 | APPROX | "Large, solid bar" uses the same undefined size threshold as the elephant bar |
| SRC-143 | DIRECT | High | — | EXACT | Both directional forms are explicitly stated on p.51 and p.14 |
| SRC-144 | DIRECT | High | — | APPROX | Swing-low definition requires a pivot lookback the source never gives; "moves away from it" undefined |
| SRC-145 | DIRECT | High | SRC-033 | APPROX | "Accelerates faster than the 20 can track" undefined |
| SRC-146 | DIRECT | High | SRC-147 | APPROX | "Separated from the 20 and lost that support" undefined |
| SRC-147 | DIRECT | High | SRC-142, SRC-145, SRC-146 | APPROX | Distance bands undefined |
| SRC-148 | DIRECT | High | SRC-142–146 | EXACT | Tie-breaking between equally protective references not specified |
| SRC-149 | DIRECT | High | SRC-144 | EXACT | None |
| SRC-150 | DIRECT | High | SRC-148 | EXACT | Which set applies on a chart timeframe not covered by V3 or V7 is unstated |
| SRC-151 | DIRECT | High | SRC-190 | EXACT | "Maximum loss per trade" is a user input the source never quantifies |
| SRC-152 | **Contradiction (diagram vs text)** | High (that the inconsistency exists) | SRC-143 | — | Resolved in favour of the text, which states both forms twice (pp.14, 51). **Not silently smoothed** |
| SRC-153 | DIRECT | High | SRC-195 | EXACT | None |
| SRC-154 | DIRECT | High | SRC-144 | EXACT | "The V" = the pullback low |
| SRC-155 | DIRECT | High | SRC-078 | EXACT | "One penny" → `syminfo.mintick` |
| SRC-156 | DIRECT | High | SRC-170 | DISPLAY | None |
| SRC-157 | DIRECT | High | SRC-140 | DISPLAY | None |
| SRC-158 | DIRECT | High | SRC-140–148 | EXACT | None |
| SRC-159 | DIRECT | High | SRC-148 | DISPLAY | None |

---

### DOMAIN D9 — PROFIT MANAGEMENT AND EXITS

#### Table A
| SRC | Page(s) | Locator | Source type | Short evidence | Exact proposition | Direction | TF/Session scope |
|---|---|---|---|---|---|---|---|
| SRC-170 | 16, 29, 51, 62 | C22; V2.6; V7.6 | Rule | "Count pushes from entry. Come out on the third" | Three-push exit | Mirrored | All |
| SRC-171 | 16, 29, 51 | C22; V2.6; V7.6 | Rule | "You have the right to take profit on push two, but only if push two is unusually large and already looks like a third push. On an ordinary second push, wait" | Conditional right to exit on push two | Mirrored | All |
| SRC-172 | 16, 20, 51 | C22; stats; V7.6 | Rationale / claim | "the market tends to pause, rest or reverse after three to five pushes, and three happens more often than five, so three is where you plan to leave" | Rationale for choosing three | n/a | All |
| SRC-173 | 16, 29, 62 | C22; V2.6 | Rule | "Once price makes a fresh peak beyond the initial move (or a fresh trough when short), you have earned the right to take money off the table … this is a right and not an obligation" | New-high/new-low exit right | Mirrored | All |
| SRC-174 | 16, 29, 35, 62 | C22; V2.6; V3.8 | Rule | "Let the trailing stop get hit for a gain. This is the natural exit when a trade keeps running past your push count" | Stop-out profit take | Mirrored | All |
| SRC-175 | 16, 51, 62 | C23; V7.6 | Rule | "Once push two is in, put the sell order into the market ahead of price so the third push executes you automatically" | Order placement ahead of the market on push two | Mirrored | All |
| SRC-176 | 16, 51 | C23; V7.6 | Rule | "the ideal exit is on the way up with momentum, not on the way down into your stop. The stop is insurance, not a plan" | Preferred exit is momentum-side | Mirrored | All |
| SRC-177 | 16, 30, 62 | C24; V2.6 | Warning | "SNATCHING AT YOUR ROSES — Taking gains too fast and never letting a winner develop … getting fearful and ejecting from a good trade while it is still working" | Two named profit-taking failure modes | Mirrored | All |
| SRC-178 | 16, 30, 62 | C24; V2.6 | Claim / arithmetic | "if your average win is consistently smaller than your average loss, the win rate does not save you. A trader can be right most of the time and still go broke" | Expectancy argument against "you cannot go broke taking a profit" | n/a | All |
| SRC-179 | 16, 30 | C24 corollary; V2.6 | Rule | "leave at least part of the position on and let trade management take you out, rather than closing the whole thing at the first profit target" | Partial-position retention preference | Mirrored | All |
| SRC-180 | 9, 52, 62 | C11; V7.9 | Rule | "Get in near, get out away. Once a stock that started narrow has moved wide, that is the exit signal regardless of what else is happening" | State-based exit restated in V7 | Mirrored | All |

#### Table B
| SRC | Source class | Confidence | Dependencies | Impl. impact | Open ambiguity |
|---|---|---|---|---|---|
| SRC-170 | DIRECT | High | — | **APPROX** | **What constitutes a "push" is never mechanically defined anywhere in the manual** |
| SRC-171 | DIRECT | High | SRC-170 | APPROX | "Unusually large" undefined |
| SRC-172 | DIRECT claim | High | SRC-170 | DISPLAY | Self-reported tendency |
| SRC-173 | DIRECT | High | — | APPROX | "The initial move" boundary undefined |
| SRC-174 | DIRECT | High | SRC-140 | EXACT | None |
| SRC-175 | DIRECT | High | SRC-170 | **OMIT** | An order-placement instruction, not a chart computation |
| SRC-176 | DIRECT | High | SRC-175 | DISPLAY | None |
| SRC-177 | DIRECT | High | — | DISPLAY | Behavioural |
| SRC-178 | DIRECT | High | — | DISPLAY | Arithmetic argument; not an indicator output |
| SRC-179 | DIRECT | High | SRC-174 | DISPLAY | Position sizing |
| SRC-180 | DIRECT | High | SRC-062 | EXACT | None |

---

### DOMAIN D10 — SPECIALISED SETUPS

#### D10.1 · Surge off the 200 (Lesson 4)

#### Table A
| SRC | Page(s) | Locator | Source type | Short evidence | Exact proposition | Direction | TF/Session scope |
|---|---|---|---|---|---|---|---|
| SRC-190 | 37, 63 | V4.2 | Rule | "A strong surge originating from a flat 200 is … one of the most powerful moves the market can present. His instruction is to treat it as an automatic trade: no additional analysis, no hesitation, no deciding" | The surge off a flat 200 is ranked top of his trade list | Mirrored | 2-min in the lesson; framework is TF-agnostic (C3) |
| SRC-191 | 37, 63 | V4.3 rule 1 | Rule | "A strong surge, often a large-bodied elephant bar, originating at or very near a flat 200" | Component 1: surge at/near a flat 200 | Mirrored | — |
| SRC-192 | 37, 59, 63 | V4.3 rule 2; glossary | Rule | "Clear blue skies to the left. The critical qualifier. No significant price data immediately to the left of the surge … He acknowledges that going back far enough always finds something. He means the recent past" | Component 2: no nearby overhead/underfoot price to overcome | Mirrored | — |
| SRC-193 | 38, 63 | V4.3 rule 3 | Rule | "Buy into the bar before it finishes, roughly 1 minute 20 seconds to 1 minute 40 seconds into a 2-minute bar. Do not wait for the close" | Component 3: intrabar entry timing | Mirrored | 2-minute bars |
| SRC-194 | 38, 63 | V4.3 rule 5 | Rule | "The mandatory colour change is used as an additional trigger point in his worked examples" | Component 5: colour change as confirmation/second trigger | Mirrored | — |
| SRC-195 | 38, 63 | V4.3 rule 6 | Rule | "The move can start a bar or two away from the 200 and still qualify, as long as the origin is not far from the zone. Originating directly at the 200 is preferred and stronger. A surge too far removed does not count" | Component 6: origin tolerance of a bar or two | Mirrored | — |
| SRC-196 | 38, 63 | V4.4 | Rule | "A bottoming tail bar is also a surge. The bar opened red, sold off, and the move that erases the red is the surge you are looking for. Same phenomenon, different costume" | The surge may present as an elephant bar **or** a tail bar | Mirrored | — |
| SRC-197 | 38, 40, 66 | V4.3 rule 4; warnings; checklist | Rule | "Placing the stop above the bottom third of the trigger bar just to make the trade fit your maximum loss. Skip the trade instead" | The stop ladder has no fourth rung | Mirrored | — |
| SRC-198 | 39, 63 | V4.6 | Rule | "The merge. The 20 and the 200 converging tightly together"; "The purge. A move that clears out every nearby price to the left in one motion"; "The surge. The move off the 200 itself" | Merge / purge / surge are three separable conditions | Mirrored | — |
| SRC-199 | 39, 63 | V4.6 | Rule | "All three together is the strongest version of the setup, because it satisfies the moving-average launch condition and the nothing-to-overcome condition at the same time" | All three simultaneously = highest conviction | Mirrored | — |
| SRC-200 | 39, 40, 64 | V4.7 | Rule | "A normal two-lot trader takes three lots on this setup. A one-lot trader takes two" | This setup earns one size tier above normal | Mirrored | — |
| SRC-201 | 38, 39, 63 | V4.5 | Rule | "A brief pause acts as a reset, which creates a fresh tradable leg two off that point. The pause can be sideways or a shallow drift down at roughly 45 degrees" | Pauses reset the move and start a new leg | Mirrored | — |
| SRC-202 | 38, 40, 63 | V4.5 warning box | Rule | "You buy the pullback after leg one. Once you are already into leg two, the next pullback loses its status" | Pullback quality degrades after leg two | Mirrored | — |
| SRC-203 | 40 | Warnings | Warning | "Mistaking a move that has travelled too far from the 200 for a fresh surge. If it is too late, it is not your trade" | Late-surge warning | Mirrored | — |

#### D10.2 · Swing method (Lesson 3)

| SRC | Page(s) | Locator | Source type | Short evidence | Exact proposition | Direction | TF/Session scope |
|---|---|---|---|---|---|---|---|
| SRC-205 | 33, 63 | V3.4 | Rule | "Igniting swing — An explosive move out of a sleepy state. Can ignite off either the 20 or the 200" | Setup 1 of 2 | Mirrored | Hourly / 4-hour |
| SRC-206 | 33, 63 | V3.4 | Rule | "Pullback swing — A colour change at or near the 20. Green takes out red, or the mirror" | Setup 2 of 2 | Mirrored | Hourly / 4-hour |
| SRC-207 | 33, 34, 60 | V3.4 "THE SLEEPY STATE, STRICTLY DEFINED"; glossary | Definition | "All three items flat and sideways at the same time: the pair itself, the 20, and the 200. **Not two of three.** A bigger, more established block of flat data before the ignition is more reliable than a small one, and he prefers an elephant-sized bar rather than a regular bar to confirm you are genuinely out of it" | Sleepy state requires all three flat; longer base preferred; elephant-sized confirmation preferred | n/a | Swing |
| SRC-208 | 33, 63 | V3.4 | Rule | "The best combination: a pullback that occurs after an igniting move … Igniting first, pullback second, is the highest-quality pairing available in the method" | The highest-quality pairing is ignite→pullback | Mirrored | Swing |
| SRC-209 | 34, 35, 59, 63 | V3.7; glossary "The box" / "Three-finger spread" | Rule | "The box … the zone near the origin of the move where all three items are still relatively close together. Inside the box is the entry zone. Once price has clearly separated beyond it, that area becomes a profit-taking zone rather than a place to initiate"; three fingers = "too far gone to enter" | Entries live in the box; three-finger separation means late | Mirrored | Swing |

#### D10.3 · Opening-bell method (Lesson 7)

| SRC | Page(s) | Locator | Source type | Short evidence | Exact proposition | Direction | TF/Session scope |
|---|---|---|---|---|---|---|---|
| SRC-210 | 49, 64 | V7.4 steps 1–7 | Rule | (1) narrow watchlist to 3–4 names in a tight state; (2) at the open watch which open above a narrow state with a green elephant/bottoming tail, or below with a red elephant/topping tail; (3) if none qualify, do nothing; (4) let bar one complete, no action during bar one; (5) mark one penny above bar one's high; (6) enter half the account the instant price crosses that mark, during bar two, not waiting for the close — if bar two does not cross it, whichever later bar does gets the entry; (7) stop one penny below bar one's low | The complete seven-step opening-bell sequence | Mirrored | **First bars of the session, 2-minute chart** |
| SRC-211 | 48, 64 | V7.1 | Rule | Five tools: capital, a 2-minute chart, a 20 SMA, a 200 SMA, "a tight picture of power: a narrow state combined with an elephant or tail bar" | The tool list for the method | n/a | Opening bell |
| SRC-212 | 52, 64 | V7.9 | Rule | "The entire process, entry through exit, is meant to complete in roughly 18 to 20 minutes" | Bounded holding window | Mirrored | Opening bell |
| SRC-213 | 51, 60, 64 | V7.8; glossary "Hidden green play" | Rule | "the first 2-minute bar of the day opens below a narrow state and starts printing green, which is the wrong colour for the location … mark the low of that green as a reference point and watch the green fade … The moment that green is fully erased by red, whether inside the same bar or on a later bar, that erasure point becomes the entry" | Hidden green play; mirrors for a red bar above a narrow state | Mirrored | Opening bell (also seen in V8, SRC-214) |
| SRC-214 | 55 | V8.5 later examples | Example | "PLTR, the hidden colour change. A bar opens green in the wrong position … only enters once that green is fully erased by red. The same idea as the hidden green plays in lesson seven" | The hidden-colour-change idea recurs in lesson 8 | Mirrored | — |
| SRC-215 | 52 | V7.9 | Claim | "more money is generally made on the downside because items fall faster and harder than they rise … he will take a short over a long any day" | A stated short-side preference | Short | All |

#### D10.4 · Scalping and retracement (Lesson 6)

| SRC | Page(s) | Locator | Source type | Short evidence | Exact proposition | Direction | TF/Session scope |
|---|---|---|---|---|---|---|---|
| SRC-216 | 46, 64 | V6.3 odds table | Claim table | 100% back → 1 of 10; 75% → 2 of 10; 50% → 6 of 10; 25% → 9 of 10 | Retracement hit-rate claims after a strong one-directional move | Mirrored | 2-min / 5-min |
| SRC-217 | 46, 64 | V6.4 | Definition | "Scalping is a counter-trend approach that deliberately targets roughly 25% of a strong move" | Definition of scalping in this system | Mirrored | 2-min / 5-min |
| SRC-218 | 46, 47, 64 | V6.5 | Rule | "Failed to clear the 50% mark of the decline → The odds strongly favour a new low. Treat it as continuation, not reversal. Take a 25% scalp at most"; "Cleared 50% by a significant margin, **toward 65 or 70%** → the next pullback becomes a higher-conviction trade rather than a scalp" | The prior bounce's depth decides scalp vs trade | Mirrored | 2-min / 5-min |
| SRC-219 | 47 | Warnings | Warning | "THE SUCKER PLAY BOUNCE — a bounce that gets bought too high because someone expected it to run 50, 75 or 100% back" | Named failure mode of buying high in the retracement range | Mirrored | — |

#### D10.5 · Space and the monthly method (Lesson 5), dual space (Lesson 6)

| SRC | Page(s) | Locator | Source type | Short evidence | Exact proposition | Direction | TF/Session scope |
|---|---|---|---|---|---|---|---|
| SRC-220 | 41, 42, 60, 64 | V5.2; glossary "Space" | Rule | "Stocks can get away from the 20. They just can't stay away from the 20" | Space mean-reverts; large space is temporary | Mirrored | Monthly in the lesson; concept generalised in V6/V7 |
| SRC-221 | 41, 42 | V5.2 | Example | "at the time of recording, the space between Apple and its monthly 20 was larger than any space produced in the previous ten years. He treats that as actionable" | Space is judged **relative to that instrument's own history** | n/a | Monthly |
| SRC-222 | 42, 64 | V5.3 | Rule | "A green bar takes out the high of a red bar. That is the entry … The takeout can happen on the next bar, three bars later, or four bars later. It does not matter" | The long-term colour game is the same colour-change rule | Mirrored | Monthly |
| SRC-223 | 42, 64 | V5.3 "THE ONE CONDITION THAT MAKES IT VALID" | Rule | "The signal only counts near the 20. A little above, a little below, or right on it are all fine … A colour-game-shaped signal that occurs far from the 20 is explicitly not an entry" | Proximity to the 20 is the sole validity condition for the colour game | Mirrored | Monthly (generalised in C18/SRC-108) |
| SRC-224 | 43, 64 | V5.4 | Claim / rule | "85% of all losses violated one rule. They were trades taken against the direction of the 20-period moving average" | The 85% loss claim and the rule it supports | Mirrored | Any horizon |
| SRC-225 | 43 | V5.4 "THE SCREENING METHOD" | Rule | "Sort every name into three buckets. 1. A rising 20 MA list … upside only, near the 20. 2. A declining 20 MA list … downside only, near the 20. 3. Neither rising nor declining. **Discard these**" | Three-bucket screen by the 20's direction; flat names are discarded | Mirrored | Any |
| SRC-226 | 43, 64 | V5.5 | Rule | "Taking a signal that looks correct in shape but occurs too far from the moving average to carry statistical weight" | Loss reason two: entering far from the 20 | Mirrored | Any |
| SRC-227 | 43, 64 | V5.6 | Rule | "NEAR, ADD. AWAY, PARE." — add near the 20 on a valid colour-game signal; reduce when stretched into an unusually large space; redeploy near the 20 on the next fresh signal; repeat indefinitely | The position-sizing cycle | Mirrored | Monthly / position |
| SRC-228 | 45, 60, 64 | V6.2; glossary "Dual space reversal" | Rule | "price, the 20 and the 200 were all separated from each other. Two layers of space at once … dual separation sets up a reversal. The trade: short once green gets eliminated by red, stop above the recent green, target the gap closing back to the 20. When there is zero space between all three items, the move is done" | Dual space reversal: setup, trigger, stop and target all stated | Mirrored | 2-min in the example |
| SRC-229 | 45 | V6.2 | Scope note | "He is explicit that this is a different topic from the scalping lesson and only mentions it because it is visible on the chart he is using" | The manual records this as an aside, not a core lesson component | n/a | — |

#### D10.6 · Reversal structure (Lesson 1)

| SRC | Page(s) | Locator | Source type | Short evidence | Exact proposition | Direction | TF/Session scope |
|---|---|---|---|---|---|---|---|
| SRC-230 | 23, 63 | V1.1 | Rule | Six stages: uptrend → first warning drop (left shoulder) → rally to new high (head) → second deep drop → right shoulder (lower high) → neckline break | The topping sequence in six stages | Mirrored | Daily/weekly primary; "applies down to the 2-minute chart" |
| SRC-231 | 23 | V1.1 stage 2 | Rule | "The first decline noticeably larger or sharper than any pullback before it. This is the signal Velez most wants you to train on" | The first warning drop is defined relative to prior pullbacks | Mirrored | — |
| SRC-232 | 24, 63 | V1.3 | Rule | "Take the up-move and split it in half. If the pullback **severely breaks** that halfway point, it is a deep drop. Anything shallower is a normal pullback and the trend is intact" | The 50% halfway test — "the only quantified rule in this lesson" | Mirrored | — |
| SRC-233 | 25, 63 | V1.5 | Definition | "Market Law Four … a head and shoulders without the head. Price makes a high, pulls back, rallies again but fails to make a significant new high, then breaks down. A double top … with a lower top" | The headless variant | Mirrored | — |
| SRC-234 | 26, 63 | V1.6 | Rule | "Best entry, tops: off the right shoulder, the lower high"; "Best entry, bottoms: off the higher low … frequently coincides with a halt at the 20 MA"; "Second choice: off the failure of the new high itself. **Only take this if something violent or genuinely powerful kicks off that failure**" | Entry preference ordering within the pattern | Mirrored | — |
| SRC-235 | 26, 63 | V1.7 | Rule | "Igniting elephant. A large green bar early in a move … Exhaustion elephant. A visually identical bar appearing late in a move … Nothing about the candle itself tells you which one you are looking at. Only its location in the trend does" | Igniting vs exhaustion is decided by location only | Mirrored | All |
| SRC-236 | 24 | V1.4 stage 6 | Rule | "The neckline break. Draw a line through the two pullback lows. Price breaking below it puts the reversal in play" | Neckline construction and break | Mirrored | — |
| SRC-237 | 27 | Warnings | Warning | "Treating it as a mechanical checklist. Velez qualifies constantly: it does not always break, the shoulders do not have to be equal, usually but not always. **The pattern is probabilistic. A slanted right shoulder still counts**" | The manual explicitly warns against mechanising this pattern | Mirrored | — |
| SRC-238 | 27 | Warnings | Warning | "Expecting a quick reclaim of the neckline. Once broken it becomes resistance that is not easily taken back" | Post-break behaviour | Mirrored | — |
| SRC-239 | 25, 61 | Bottoms; glossary "20 MA halt" | Rule | "Price pausing or holding at the 20-period moving average, which often coincides with the right shoulder of an inverse head and shoulders" | The 20 MA halt | Long | — |

#### Table B — Domain D10 (all records)
| SRC | Source class | Confidence | Dependencies | Impl. impact | Open ambiguity |
|---|---|---|---|---|---|
| SRC-190 | DIRECT | High | SRC-036 | EXACT (as a composite gate) | Depends on flatness threshold |
| SRC-191 | DIRECT | High | SRC-092, SRC-036 | APPROX | "Strong", "very near" undefined |
| SRC-192 | DIRECT | High | — | **APPROX** | "Immediately to the left" / "the recent past" — **lookback length never stated** |
| SRC-193 | DIRECT | High | — | **OMIT** | Intrabar; a bar-close indicator cannot express "1:20–1:40 into the bar" |
| SRC-194 | DIRECT | High | SRC-102 | EXACT | None |
| SRC-195 | DIRECT | High | SRC-042 | APPROX | "A bar or two" is quantified; "not far from the zone" is not |
| SRC-196 | DIRECT | High | SRC-096 | EXACT | None |
| SRC-197 | DIRECT | High | SRC-151 | EXACT | Requires the user's own max-loss figure |
| SRC-198 | DIRECT | High | SRC-050, SRC-192 | APPROX | "Tightly", "nearby" undefined |
| SRC-199 | DIRECT | High | SRC-198 | EXACT | None |
| SRC-200 | DIRECT | High | — | DISPLAY | Position sizing, not chart geometry |
| SRC-201 | DIRECT | High | — | **APPROX** | Pause duration and "shallow ~45°" undefined; 45° is not a data property |
| SRC-202 | DIRECT | High | SRC-201 | EXACT | None once legs are counted |
| SRC-203 | DIRECT | High | SRC-195 | DISPLAY | None |
| SRC-205 | DIRECT | High | SRC-207 | EXACT | "Explosive" leans on SRC-092 |
| SRC-206 | DIRECT | High | SRC-102 | APPROX | "At or near the 20" undefined |
| SRC-207 | DIRECT | High | SRC-055 | APPROX | Same flatness ambiguity; "bigger, more established block" undefined |
| SRC-208 | DIRECT | High | SRC-205, SRC-206 | EXACT | None |
| SRC-209 | DIRECT | High | SRC-050 | APPROX | "Relatively close" / "clearly separated" undefined |
| SRC-210 | DIRECT | High | SRC-050, SRC-092, SRC-096 | EXACT | Fully mechanical apart from state/event thresholds |
| SRC-211 | DIRECT | High | SRC-210 | DISPLAY | None |
| SRC-212 | DIRECT | High | SRC-210 | DISPLAY | None |
| SRC-213 | DIRECT | High | SRC-050 | **APPROX** | "Fully erased … whether inside the same bar" is intrabar; only the bar-close form is observable |
| SRC-214 | DIRECT (example) | High | SRC-213 | DISPLAY | None |
| SRC-215 | DIRECT claim | High | — | DISPLAY | A preference; must not become an asymmetric gate |
| SRC-216 | DIRECT claim table | High | — | DISPLAY (levels EXACT) | The **levels** are computable; the **hit rates** are self-reported claims |
| SRC-217 | DIRECT | High | SRC-216 | EXACT | "A strong move" undefined |
| SRC-218 | DIRECT | High | SRC-232 | EXACT | "Significant margin" is quantified as "toward 65 or 70%" |
| SRC-219 | DIRECT | High | SRC-216 | EXACT | None |
| SRC-220 | DIRECT | High | — | EXACT | None |
| SRC-221 | DIRECT (example) | High | SRC-220 | APPROX | Establishes historical-relative measurement; lookback undefined |
| SRC-222 | DIRECT | High | SRC-102 | EXACT | None |
| SRC-223 | DIRECT | High | SRC-220 | **APPROX** | "Near" undefined — the manual explicitly declines to specify ("He does not care which") |
| SRC-224 | DIRECT rule / self-reported claim | High | SRC-040 | EXACT (rule) / DISPLAY (claim) | The 85% figure is unverifiable; the *rule* is implementable |
| SRC-225 | DIRECT | High | SRC-224 | APPROX | Rising/declining/flat requires a slope threshold |
| SRC-226 | DIRECT | High | SRC-223 | APPROX | Same "far" ambiguity |
| SRC-227 | DIRECT | High | SRC-220, SRC-222 | EXACT | "Unusually large space" needs SRC-221's ranking |
| SRC-228 | DIRECT | High | SRC-220 | APPROX | "Separated" undefined for both layers |
| SRC-229 | DIRECT | High | SRC-228 | DISPLAY | None |
| SRC-230 | DIRECT | High | — | **OMIT (full pattern)** | Stage tolerances, shoulder symmetry and timing never specified |
| SRC-231 | DIRECT | High | SRC-230 | APPROX | "Noticeably larger or sharper" undefined, but "larger than any pullback before it" is comparably objective |
| SRC-232 | DIRECT | High | — | APPROX | "**Severely** breaks" — the 50% level is exact, the severity margin is not |
| SRC-233 | DIRECT | High | SRC-230 | APPROX | "Fails to make a significant new high" undefined |
| SRC-234 | DIRECT | High | SRC-230 | OMIT | Depends on full pattern identification |
| SRC-235 | DIRECT | High | SRC-092, SRC-070 | APPROX | "Early"/"late" undefined; the manual's own illustration says "five legs into the move" |
| SRC-236 | DIRECT | High | SRC-230 | OMIT | Requires identified pullback lows |
| SRC-237 | DIRECT | High | SRC-230 | DISPLAY | **This warning is the primary reason the full pattern is not implemented** |
| SRC-238 | DIRECT | High | SRC-236 | DISPLAY | None |
| SRC-239 | DIRECT | High | SRC-035 | APPROX | "Pausing or holding" undefined |

---

### DOMAIN D11 — TIMEFRAME, SESSION AND UNIVERSE

#### Table A
| SRC | Page(s) | Locator | Source type | Short evidence | Exact proposition | Direction | TF/Session scope |
|---|---|---|---|---|---|---|---|
| SRC-240 | 21 | §1.9 table | Rule / mapping | Monthly→V5; 4-hour→V3; 1-hour→V3; Daily/weekly→V1; 5-minute→V2,V6; 2-minute→V2,V4,V6,V7 | Which lesson runs on which chart | n/a | Six named timeframes |
| SRC-241 | 21, 28, 63 | §1.9; V2.1 | Claim | "Eight to twelve bars is the average trade on every timeframe he teaches. Only the wall-clock translation changes" | 8–12 bar holding period is timeframe-invariant | n/a | All |
| SRC-242 | 21 | §1.9 "CHOOSING" | Rule | "how long do you want your money exposed to the possibility of something going wrong?" | Timeframe selection criterion is exposure, not profit | n/a | All |
| SRC-243 | 28, 63 | V2.1 | Claim | 2-minute = 16–24 minutes, ~80% of intraday work; 5-minute = 35 minutes and up, ~20% | The two intraday battle charts | n/a | Intraday |
| SRC-244 | 32, 63 | V3.1 | Definition | Day trading 8–20 min; swing 2–10 days; core weeks–months; investing years | Four styles by holding period | n/a | — |
| SRC-245 | 32, 63 | V3.1 | Claim | "roughly 80% of swing trades resolve in the 2 to 3 day sweet spot. The remaining 20% run 4 to 10 days" | Swing duration split | n/a | Swing |
| SRC-246 | 33, 63 | V3.3 | Rule | Six pairs: EUR/USD, USD/JPY, GBP/USD, AUD/USD, USD/CHF, USD/CAD; "take the NASDAQ 100 or the S&P 100 and narrow it to six to ten names that become your market" | Universe narrowing | n/a | Forex / equities |
| SRC-247 | 32, 63 | V3.2 | Rationale | Liquidity, no overnight gap risk, no licensing barrier, trend consistency, forced specialisation, relaxed pace | Six reasons for applying the method to currencies | n/a | Forex |
| SRC-248 | 66 | §3.4 "BEFORE THE OPEN" | Checklist | "Note the 20's direction on the higher timeframe" | A higher-timeframe 20-direction check is part of the pre-open routine | Mirrored | All |
| SRC-249 | 66 | §3.4 "BEFORE THE OPEN" | Checklist | "Check what price data sits to the left of the current level"; "Maximum loss per trade defined in dollars, before anything else"; "Size tier decided per name" | Three further pre-open items | n/a | All |

#### Table B
| SRC | Source class | Confidence | Dependencies | Impl. impact | Open ambiguity |
|---|---|---|---|---|---|
| SRC-240 | DIRECT | High | — | EXACT (as advisory) | The mapping does not say a lesson is *invalid* off its chart; C3 says the framework is TF-agnostic |
| SRC-241 | DIRECT claim | High | — | DISPLAY | Self-reported average |
| SRC-242 | DIRECT | High | — | DISPLAY | None |
| SRC-243 | DIRECT claim | High | SRC-240 | DISPLAY | Percentages are preferences, not results (p.19) |
| SRC-244 | DIRECT | High | — | DISPLAY | None |
| SRC-245 | DIRECT claim | High | SRC-244 | DISPLAY | Self-reported |
| SRC-246 | DIRECT | High | — | **OMIT** | Watchlist construction is outside a single-chart indicator's scope |
| SRC-247 | DIRECT | High | SRC-246 | DISPLAY | None |
| SRC-248 | DIRECT | High | SRC-040 | EXACT (optional module) | Which higher timeframe is not specified |
| SRC-249 | DIRECT | High | SRC-192, SRC-151 | EXACT / DISPLAY | Max loss in dollars is a user input |

---

### DOMAIN D12 — STATISTICS AND NUMERICAL CLAIMS

**Governing constraint (SRC-004, p.2 and SRC-009, p.68): every figure below is Velez's own,
unpublished, without stated sample, date range or instrument set. None may be converted into an
indicator promise, probability output, score, or default threshold.**

Classification of each number uses the taxonomy required by this project:
`MECH` = direct mechanical threshold · `TIME` = timing convention · `STAT` = statistic ·
`SELF` = self-reported claim · `EG` = illustrative example · `RISK` = risk-management convention ·
`NONPROG` = non-programmable description.

| SRC | Page | Figure | Claim | Attributed | Manual's own caveat | Number type | Impl. |
|---|---|---|---|---|---|---|---|
| SRC-250 | 19, 28, 48 | **87%** | A first bar opening above a narrow state is followed by upside that morning; mirrored below | V2, V7 | "Pre-screened watchlist, a genuine narrow state, and an elephant or tail bar at the open. **Not a claim about any random gap**" | SELF / STAT | DISPLAY only |
| SRC-251 | 19, 48 | **85%** | Correctly identifying the state accounts for 85% of the game | V7 | "A qualitative weighting, not a measured win rate. **Read it as emphasis**" | NONPROG | DISPLAY only |
| SRC-252 | 19, 43, 44 | **85%** | Of all realised losses across his trader base, 85% were trades against the direction of the 20 MA | V5 | "A stated internal 6-month study … No published sample size, instrument set or methodology. **The most consequential number … and the least verifiable**" | SELF / STAT | DISPLAY only; the *rule* it supports (SRC-224) is implemented |
| SRC-253 | 19, 12, 29 | **80%** | Elephant bar follow-through | V2, V7 | "The bar must be in the correct position. He is explicit that the figure does not survive the bar occurring anywhere" | SELF / STAT | DISPLAY only |
| SRC-254 | 19, 10, 54 | **80%+** | Target share of trades from position one | V8 | "**A behavioural target he sets, not an observed statistic**" | RISK / behavioural | DISPLAY only |
| SRC-255 | 19, 25 | **80%+** | H&S plus Market Law Four account for how markets top and bottom | V1 | "Asserted from experience. No study cited" | SELF | DISPLAY only |
| SRC-256 | 19, 49 | **8 of 10** | Opening-bell trades that run without intervention | V7 | "His personal record on this exact setup, self-reported" | SELF | DISPLAY only |
| SRC-257 | 19, 35 | **2–3 of 10** | Igniting/pullback swing trades that do not work | V3 | "Note this **contradicts the more optimistic figures elsewhere**, which is useful" | SELF | DISPLAY only |
| SRC-258 | 19, 46 | **9 / 6 / 2 / 1 of 10** | Bounce retraces 25% / 50% / 75% / 100% | V6 | "Requires a genuinely strong, one-directional prior move. Experience-based, not a published backtest" | SELF | Levels EXACT; hit rates DISPLAY only |
| SRC-259 | 19, 21, 28 | **8–12 bars** | Average holding period on any timeframe | V2 | "Timeframe sets the clock time" | TIME | DISPLAY only |
| SRC-260 | 19, 28 | **80 / 20** | Split of intraday work between 2-min and 5-min | V2 | "A preference, not a result" | NONPROG | DISPLAY only |
| SRC-261 | 19, 32 | **80 / 20** | 80% of swings resolve in 2–3 days, 20% run 4–10 | V3 | "The 20% bucket is where he says the outsized wins come from" | SELF | DISPLAY only |
| SRC-262 | 19 | **80 / 20** | Split of swing work between hourly and 4-hour | V3 | "Higher timeframe pushes trades toward the longer, larger bucket" | NONPROG | DISPLAY only |
| SRC-263 | 19, 12, 29 | **13** | Bar types the market repeats | V2, V7 | "**Never enumerated in these eight videos. Only four are named**" | NONPROG | OMIT |
| SRC-264 | 20, 12, 55 | **14** | Actionable events taught to professional traders | V8 | "Three are taught publicly. **The other eleven are not in this material**" | NONPROG | OMIT |
| SRC-265 | 20, 16, 51 | **3–5** | Pushes after which a move tends to pause, rest or reverse | V7 | "Three is chosen as the exit because it occurs more often than five" | SELF | Push count EXACT-ish; the tendency claim DISPLAY only |
| SRC-266 | 20, 33 | **23%** | EUR/USD share of global forex volume | V3 | "Broadly consistent with published BIS survey data" | STAT (externally corroborated) | OMIT (not chart logic) |
| SRC-267 | 20, 33 | **60%** | Share of global currency trading covered by his six pairs | V3 | "Plausible as an order-of-magnitude figure for the majors" | STAT | OMIT |
| SRC-268 | 41 | **~95%** | Two rules account for ~95% of avoidable losses | V5 | Manual notes he says "three main reasons" then names two totalling 95%; "The third is never separately enumerated" | SELF | DISPLAY only |
| SRC-269 | 46, 47 | **65–70%** | The retracement depth that converts a scalp into a trade | V6 | Stated inline as "a significant margin, toward 65 or 70%" | MECH | **EXACT** — the only "significant margin" the manual quantifies |
| SRC-270 | 24, 46 | **50%** | The halfway line — deep-drop test and scalp/trade decision | V1, V6 | "The same tool reappears in the scalping lesson" | MECH | **EXACT** level; "severely breaks" is the approximate part |
| SRC-271 | 46, 47 | **25%** | Default scalp target as a share of the preceding strong move | V6 | "Where the scalper lives" | MECH | **EXACT** level |
| SRC-272 | 17, 39, 40 | **1 / 2 / 3–4 lots** | Three size tiers: light / medium / heavy | C-sizing, V4.7 | "The tier is set by the quality of the setup, not by how you feel about it" | RISK | DISPLAY only |
| SRC-273 | 20 | *(meta)* | "READ THE CONTRADICTION, DO NOT SMOOTH IT OVER" — win rates cluster 70–90% across very different methods; "more likely to be a teaching device than a measurement. Use them to rank setups against each other. **Do not use them to size a position**" | Editorial | — | NONPROG | **Governing instruction for this project's treatment of all statistics** |

---

### DOMAIN D13 — WARNINGS AND NAMED FAILURE MODES

| SRC | Page(s) | Source | Warning | Impl. |
|---|---|---|---|---|
| SRC-280 | 5, 62 | C2 | A valid event in the wrong position is the wrong trade | EXACT — blocker |
| SRC-281 | 11, 55, 56 | C14, V8.4 | Placing a stop inside position one ("the dumb stop") | EXACT — blocker |
| SRC-282 | 11, 56 | C15, V8 | Chasing bars once price is clearly separated from the state ("Pluto land") | EXACT — blocker |
| SRC-283 | 12, 56 | C17, V8 | Calling a bar with a mere wick a tail bar ("snowman") | EXACT — diagnostic |
| SRC-284 | 56 | V8 | Taking a buy from position three; only a reversal or short bias belongs that far out; mirror for shorts | EXACT — blocker |
| SRC-285 | 27 | V1 | Treating the reversal pattern as a mechanical checklist | DISPLAY — and the reason SRC-230 is not implemented |
| SRC-286 | 27 | V1 | Chasing the new high (or the low in a bottom) — frequently a trap | DISPLAY |
| SRC-287 | 27 | V1 | Reading an exhaustion elephant as an igniting one | EXACT — the classifier addresses this directly |
| SRC-288 | 27 | V1 | Expecting a quick reclaim of the neckline | DISPLAY |
| SRC-289 | 30, 52 | V2, V7 | Acting on location alone without a qualifying event | EXACT — blocker |
| SRC-290 | 30, 52 | V2, V7 | Leaving a stop static instead of rotating it | EXACT — lifecycle behaviour |
| SRC-291 | 30 | V2 | Confusing many small profitable trades with actual profitability | DISPLAY |
| SRC-292 | 36 | V3 | Continuing to trade a three-finger wide state | EXACT — blocker |
| SRC-293 | 40 | V4 | Mistaking a move too far from the 200 for a fresh surge; placing the stop above the bottom third to make it fit; treating the leg-two pullback like leg one; fixating on elephant shape and missing tail-bar surges | EXACT — blockers/diagnostics |
| SRC-294 | 44 | V5 | Being cute (fading the 20's direction); taking a correctly shaped colour change too far from the 20; failing to pare at a space extreme and then failing to redeploy | EXACT — blockers |
| SRC-295 | 47 | V6 | Scalping a sub-50% bounce as though it were a full trade; confusing a trade with a scalp; holding a scalp hoping for a full reversal | EXACT — context classifier |
| SRC-296 | 52 | V7 | Overcomplicating with fundamental analysis; holding beyond the intended window / marrying a stock off the open | DISPLAY |
| SRC-297 | 36 | V3 | "Not every dollar has your name on it" — overtrading discipline | DISPLAY |

---

### DOMAIN D14 — THE MANUAL'S OWN STATEMENT OF GAPS (editorial, p.68–69)

| SRC | Page | Gap | Exact proposition | Consequence for this project |
|---|---|---|---|---|
| SRC-300 | 68 | **GAP ONE — No position sizing maths** | "There is no discussion of what percentage of capital a maximum loss should represent, how to size when the stop distance varies, how many positions to hold at once, or what happens to sizing after a losing streak" | **No risk-sizing mathematics is implemented.** Any sizing field is display-only and attributed |
| SRC-301 | 68 | **GAP TWO — Every statistic is self-reported** | "None of the percentages are published or independently verifiable" | Governs D12 |
| SRC-302 | 68 | **GAP THREE — Survivorship in the chart examples** | "Every worked example in all eight videos is a setup that worked. There is no walk-through of a valid setup that failed" | **No example may be used to calibrate or validate a threshold.** This is why the project ships no "tuned" defaults |
| SRC-303 | 68 | **GAP FOUR — The definitions are deliberately loose** | "Every core definition in the method is a judgement call, and Velez actively resists tightening them, telling you to eyeball it rather than measure. … A method built on judgement calls cannot be backtested … two traders applying the same lesson will take different trades. **Be aware you are buying a discretionary framework, not a system**" | **The single most important constraint on this project.** Every numeric threshold in the indicator is an addition, not a translation |
| SRC-304 | 68 | **GAP FIVE — The commercial context** | Three of eight videos end in sales presentations | No commercial content reproduced |
| SRC-305 | 69 | "WHAT HOLDS UP" | Five ideas the compiler judges independently supported: trade with the intermediate trend; enter near / exit away from a reference level; volatility compression precedes expansion; never leave a stop static and let winners run; narrow your universe | Editorial assessment — recorded, not implemented as rules |
| SRC-306 | 67 | "THE ONE NUMBER WORTH TRACKING YOURSELF" | "Tag every trade with two fields: with or against the 20, and which position" | Motivates the two dashboard fields that are always shown |

---

### DOMAIN D15 — GLOSSARY TERMS AS EVIDENCE

The glossary (pp.59–61) restates concepts already recorded above. Each glossary entry is treated
as **corroborating evidence** for its parent SRC record rather than as a new proposition, except
where the glossary supplies a definition not given in the body. Those exceptions are:

| SRC | Page | Term | New content supplied only by the glossary |
|---|---|---|---|
| SRC-320 | 60 | Pivot | "used as a trailing stop reference **until the 20 climbs past it**" — restates the hand-back condition compactly |
| SRC-321 | 60 | Sucker play bounce | Defines the failure mode as expecting "a 50, 75 or 100% retracement, which are the rare outcomes" |
| SRC-322 | 60 | Space | Explicitly binds "space" to **price ↔ the 20** (not price ↔ the 200) |
| SRC-323 | 60 | Three-finger spread | Explicitly enumerates the three items: "The instrument, the 20 and the 200 all separated from each other" |
| SRC-324 | 59 | Clear blue skies to the left | "No significant recent price data immediately to the left of a surge, **so nothing for price to overcome**" — states the purpose, which constrains any proxy |
| SRC-325 | 61 | 20 MA halt | The only definition of this term in the manual |
| SRC-326 | 59 | The box | "Beyond it is a profit-taking zone" — binds the box to exit context, not only entry |
| SRC-327 | 60 | Professional loss | "A loss limited to one bar of adverse movement" |
| SRC-328 | 59 | Colour adjustment stop | Confirms "just beyond that isolated counter-colour bar" |

---

## 6. RULE HIERARCHY

When two source statements bear on the same decision, this project resolves them in the following
order. **This hierarchy is a project decision, not a source statement** — the manual never
declares a precedence order.

| Tier | Content | Basis |
|---|---|---|
| **T0 — Ordering invariant** | State → Position → Event (C1) | The manual calls it "not optional" (p.5) |
| **T1 — Hard blockers** | No event = no trade (C1/§1.1); never stop inside position one (C14/V8.4); do not trade inside the state (p.10); no with-trend entries from position three (C13, V8 warnings); the five reasons to do nothing (p.66) | Stated as absolutes |
| **T2 — Directional filter** | Never against the 20's direction (C7, V5.4) | Stated as the single most consequential rule (p.43) |
| **T3 — Event definitions** | Elephant (C16), tail (C17), colour change (C18), red-bar takeout (V7.5) | Stated definitions |
| **T4 — Specialised setup rules** | V3.4, V4.3, V6.5, V7.4, V7.8, V6.2 | Lesson-scoped; each carries its own conditions |
| **T5 — Management rules** | C20 add, C21 rotation, C22 exits | Apply after entry |
| **T6 — Preferences and rankings** | Grades of narrow (C9), 1A/1B (V3.5), ignite-then-pullback (V3.4), size tiers (V4.7) | Ordinal, not binary |
| **T7 — Discretionary guidance** | Eyeball it (C8), zones not lines (C6), judge breaks by follow-through (C6) | Explicitly non-mechanical |

**Where the manual gives no precedence**, this project applies a conservative platform policy and
tags it `PLATFORM_SAFEGUARD`. Every such policy is enumerated in `04_INDICATOR_ARCHITECTURE.md §7`.

---

## 7. DIRECT RULES VERSUS DISCRETIONARY GUIDANCE

### 7.1 Rules stated mechanically enough to translate without inventing a number

| Rule | Source | Why it is mechanical |
|---|---|---|
| SMA 20 and SMA 200 on close | SRC-031 | Length, type and input series all stated |
| Colour change = one colour takes out the **body** extreme of an opposite-coloured bar, need not be adjacent, entry one tick beyond | SRC-102/104/105/106 | Every component specified |
| Tail bar = tail is the majority of the bar, body the minority; a mostly-body bar with a wick does not qualify | SRC-096/097 | A ratio comparison, with the counterexample given |
| Elephant + colour change on one bar outranks either alone | SRC-107 | Ordinal rule over two defined predicates |
| Little red bar takeout = first counter-colour bar after entry that fails to produce a second consecutive counter-colour bar, then is taken out by one tick | SRC-132 | Fully specified bar sequence |
| Opening bell: bar one completes untouched; mark = bar-one high + 1 tick; stop = bar-one low − 1 tick; entry on the first bar that crosses the mark | SRC-210 | Every price defined in advance (p.50) |
| Colour-adjust stop: one counter-colour bar followed by two bars in your direction → stop just beyond that isolated bar | SRC-143 | Both directional forms stated |
| Rotation: use whichever qualifying reference is currently most protective | SRC-148 | A max/min over defined candidates |
| Pivot hand-back: hold the pivot stop until the 20 climbs past it, then trail the 20 | SRC-144 | Explicit condition |
| Stop ladder: under the whole base → bottom third of the trigger bar → skip | SRC-151 | Three rungs, geometric |
| Deep-drop level = 50% of the preceding move | SRC-232, SRC-270 | Exact level |
| Retracement levels 25 / 50 / 75 / 100% of the prior move | SRC-216 | Exact levels |
| Scalp-vs-trade: prior bounce under 50% → continuation; cleared toward 65–70% → trade | SRC-218, SRC-269 | The only quantified "significant margin" in the manual |
| Narrow → wide transition is a standalone exit | SRC-062 | Exact once state is known |
| Never trade against the 20's direction; discard flat names | SRC-224/225 | Exact once slope sign is known |

### 7.2 Guidance the manual explicitly refuses to mechanise

| Guidance | Source | The manual's own words |
|---|---|---|
| How close is "close together" | SRC-051 | "an eyeball judgement and warns against measuring the gap with a tool"; "Close does not mean touching" |
| What counts as a break of an average | SRC-043 | "judge breaks by eye and by follow-through, not by whether the close printed a fraction below the line" |
| How large is "visibly larger" | SRC-092 | "visibly larger and taller than the bars around it" |
| How near is "near the 20" | SRC-223 | "A little above, a little below, or right on it are all fine. **He does not care which**" |
| How far is "not far from the zone" | SRC-195 | "a bar or two away … and still qualify" |
| How severe is "severely breaks" | SRC-232 | "severely breaks that halfway point" |
| How far back is "the recent past" | SRC-192 | "He acknowledges that going back far enough always finds something. He means the recent past" |
| Whether the reversal pattern is mechanical | SRC-237 | "it does not always break, the shoulders do not have to be equal, usually but not always" |
| Every core definition | SRC-303 | "Velez actively resists tightening them, telling you to eyeball it rather than measure" |

**Everything in 7.2 that the indicator nevertheless renders is a `PROGRAMMABLE_APPROXIMATION`.
Each one is disclosed in the input tooltip, in `02_CONCEPT_TO_CODE_MAP.md`, in Debug Mode and in
the README.**

---

## 8. EXAMPLES VERSUS UNIVERSAL RULES

The manual contains many worked examples. **None of them establishes a default, a threshold or a
rule.** They are recorded so that a reader can see they were considered and rejected as calibration
sources — reinforced by SRC-302 (every example shown is a setup that worked).

| Example | Page | What it demonstrates | What it does **not** establish |
|---|---|---|---|
| Bitcoin daily ~69,000 → ~16,000 inverse H&S | 26 | The bottoming pattern narrative | Any threshold for shoulder depth or timing |
| NVIDIA weekly | 26 | Igniting vs exhaustion elephant | Any bar-size ratio |
| Broad market indices as a possible forming left shoulder | 26 | Explicitly "framed as speculative with no guarantees" | Anything |
| NVIDIA and AMD 2-minute walk-throughs | 30 | State → position-one entry → add → exit into wide | Any state or position boundary |
| CVX 2-minute, four bars off the 200 | 38, 40 | Leg one / leg two and the "too far from the 200" caveat | The number of bars that makes a surge "too late" |
| UBER, two reset points in one move | 40 | Multiple resets can occur in one move | Reset duration |
| MSFT and BABA 2-minute | 46 | Sequential declines, halfway points, retracement levels | What makes a prior move "strong" |
| BABA morning dual separation | 45 | The dual space reversal | Separation thresholds |
| Apple monthly, "biggest space in 10 years" | 41–43 | Space is judged against the instrument's own history | The lookback length or the percentile that counts as "unusual" |
| WFC full formula | 55 | The canonical entry → add → continued adds → Pluto-land warning sequence | Any numeric parameter |
| PLTR hidden colour change | 55 | The hidden-green concept recurring in lesson 8 | Intrabar erasure mechanics observable at bar close |
| Hand-drawn topping/bottoming diagrams | 26 | Used "before any real chart" | Anything measurable |
| $50,000 / $25,000 / $12–13,000 tranches | 49, 50 | The opening-bell scaling shape | Any risk percentage — see SRC-300 |

---

## 9. SOURCE STATISTICS AND THEIR LIMITATIONS

See Domain D12 for the full ledger. The operative conclusions:

1. **Every percentage is self-reported** (SRC-004, SRC-301). None carries a sample size, date
   range, instrument set, or definition of the population.
2. **The manual itself flags the pattern as suspicious** (SRC-273, p.20): win rates cluster
   between 70% and 90% across methods that are mechanically unrelated, which the compiler argues
   is "more likely to be a teaching device than a measurement."
3. **The manual's own instruction is: use them to rank setups against each other; do not use them
   to size a position** (SRC-273). This project honours that literally — the statistics appear
   only as attributed reference text, never as an output number, score weight or threshold.
4. **The single most consequential figure (85% of losses, SRC-252) is also the least verifiable**
   and is described as such by the manual on both p.19 and p.44. The *rule* it supports — do not
   trade against the 20 — is implemented. The *number* is not repeated as evidence for it.
5. **Two figures are internally contradictory in tone** (SRC-257 vs SRC-250/253/256): 2–3 of 10
   swing setups fail outright, against 80–87% success elsewhere. The manual explicitly instructs
   the reader not to smooth this over. It is recorded here unresolved.
6. **Three numbers are genuine mechanical levels, not statistics** — 50% (SRC-270), 25%
   (SRC-271) and 65–70% (SRC-269). These are implemented as levels. Their associated hit rates
   are not.

---

## 10. MISSING, AMBIGUOUS AND NON-PROGRAMMABLE CONCEPTS

### 10.1 Referenced but never defined — no rule may be created

| Concept | Source | What is missing |
|---|---|---|
| The **13 bar types** | SRC-091, SRC-263 | "Never enumerated in these eight videos. Only four are named." |
| The **14 actionable events** | SRC-091, SRC-264 | "The other eleven are not in this material." |
| The **13-period moving average** as a short partner | SRC-034 | Which setups use it, and how, is never stated |
| The **third reason** traders lose (V5) | SRC-268 | "He opens the video saying there are three main reasons and then says two of them account for 95%. The third is never separately enumerated" |
| **Maximum loss per trade** | SRC-300 | The method requires one but never specifies how to derive it |
| **Position sizing mathematics** | SRC-300 | No percentage of capital, no stop-distance-based sizing, no concurrency limit, no post-drawdown rule |
| **Concurrency of trades** | SRC-300 | "how many positions to hold at once" is explicitly listed as absent |
| The content of the **transcript gaps** | SRC-007 | Videos 2, 3, 7, 8 have declared unrecovered ranges |

### 10.2 Ambiguities that affect code and have no source resolution

| # | Ambiguity | Source | Resolution policy |
|---|---|---|---|
| A1 | **Which opposite-colour bar supplies the colour-change level** when several print consecutively | SRC-104 | The manual says "the first green bar that takes out **a** red high". Default: the **most recent** opposite-colour bar's body extreme (the nearest, and therefore the first level a reversal can take out). An alternative (the extreme of the whole counter-colour run) is exposed as an input. Both are labelled approximations |
| A2 | **What constitutes a "push"** | SRC-170 | Never defined mechanically anywhere in the manual. Implemented as a confirmed swing-leg count with a configurable pivot length; tagged approximation |
| A3 | **Where "the initial move" ends** for the new-high/new-low right | SRC-173 | Defined as entry → first confirmed counter-swing; tagged approximation |
| A4 | **How close "close together"** must be for narrow | SRC-051 | Normalised gap threshold; explicitly a departure from "eyeball it" |
| A5 | **Position band boundaries** | SRC-071 | Multiples of the state band width; tagged approximation |
| A6 | **Elephant bar size** | SRC-092 | Range and body compared to a rolling neighbourhood; tagged approximation |
| A7 | **Flatness** for the 20, the 200 and price | SRC-055 | Normalised slope threshold; tagged approximation |
| A8 | **Clear-space lookback** | SRC-192 | Configurable bar count; the manual only says "the recent past" |
| A9 | **Reset/pause duration** | SRC-201 | Configurable bar count and containment test; tagged approximation |
| A10 | **"Accelerating faster than the 20 can track"** | SRC-145 | Comparative slope test; tagged approximation |
| A11 | **"Separated from the 20 and lost that support"** | SRC-146 | Space threshold; tagged approximation |
| A12 | **Which trailing reference set applies** on a timeframe covered by neither V3 nor V7 | SRC-150 | All qualifying references participate in the rotation; tagged platform safeguard |
| A13 | **Tie-breaking between equally protective stop references** | SRC-148 | Fixed display precedence; tagged platform safeguard |
| A14 | **"Unusually large" space** | SRC-221, SRC-227 | Historical percentile rank over a configurable lookback, following the "biggest in 10 years" framing; tagged approximation |
| A15 | **What makes a prior move "strong"** for the retracement table | SRC-217 | Confirmed swing leg exceeding a configurable ATR multiple; tagged approximation |
| A16 | **How far apart** ignition and pullback must be to become two trades rather than 1A/1B | SRC-127 | Configurable bar count; tagged approximation |
| A17 | **Which higher timeframe** for the pre-open 20-direction check | SRC-248 | User-selected; off by default |

### 10.3 Internal tensions recorded without invented precedence

| # | Tension | Where | Status |
|---|---|---|---|
| T1 | **"No third option"** (SRC-052, SRC-063: narrow ↔ wide only, "there is no third spot") versus the explicitly named **middling/regular state** with its own trading mode (SRC-059, SRC-061, V2.3 "Narrow, middling, wide") | pp.8–9, 28, 54 | Both are recorded. Reading adopted: three *states* exist; only two *spots* are named as tradable, with regular described as where colour changes off the 20 are traded. **No precedence invented** — the indicator reports all three states and marks regular-state signals distinctly |
| T2 | **Stop must be beyond the state** (SRC-077) versus "sometimes the stop goes under the bar and sometimes under the state" (p.55, V8.5) versus the opening-bell one-bar stop (SRC-078) | pp.11, 49, 55 | The manual itself reconciles it: "The rule is that it **must never sit inside the zone you entered from**" (p.55). Adopted as the invariant; both placements allowed outside it |
| T3 | **Win-rate figures** 80–87% versus 2–3 in 10 failures | pp.19–20, 35 | Recorded unresolved, per the manual's explicit instruction (SRC-273) |
| T4 | **Diagram label vs text** on the colour-adjust pattern | p.15 vs pp.14, 51 | SRC-152. Text wins (stated twice, both directions); the diagram inconsistency is disclosed, not smoothed |
| T5 | **Elephant entry method 1** ("buy into the bar before it finishes") versus a bar-close indicator's observability | pp.12, 38, 55 | Method 1 is intrabar and is not implemented as a confirmed signal; only method 2 (next bar clears the high) is |

### 10.4 Concepts that are real in the source but not programmable as signals

| Concept | Source | Why |
|---|---|---|
| "Ideally near 45°" slope | SRC-035 | A chart-rendering artefact, not a data property |
| Pause "at roughly 45 degrees" | SRC-201 | Same |
| Entry at 1:20–1:40 into a 2-minute bar | SRC-193 | Intrabar clock position; not available to bar-close logic and not stable intrabar |
| Entry in the last 15–20 minutes of an hourly bar | SRC-130 | Same |
| "Buy into the elephant bar before it finishes trading" | SRC-095 | Same |
| Intrabar green-erasure instant for the hidden green play | SRC-213 | Only the bar-close outcome is observable |
| Placing the exit order ahead of the market on push two | SRC-175 | An order-routing instruction |
| Watchlist construction / narrowing to six names | SRC-246 | Multi-instrument screening, outside a single-chart indicator |
| Psychology of each H&S leg | SRC-230/V1.2 | Interpretive narrative |
| All discipline maxims | SRC-297, SRC-177 | Behavioural |
| Position sizing in lots or dollars | SRC-272, SRC-300 | The manual supplies no maths (SRC-300) |

---

## 11. SOURCE TERMINOLOGY NORMALISATION RULES

These rules govern every user-facing string in the indicator.

1. **Prefer the source's own term and spelling.** The manual is British-spelling ("colour");
   source terms are reproduced as spelled: `COLOUR CHANGE`, not `COLOR CHANGE`.
2. **A UI shortening is not a source term.** Where a source term is too long for a chart label,
   the shortened form is listed in §3 as a **UI shortening** and the full source term is available
   in the dashboard/Debug output. Shortenings used: `CLEAR LEFT`, `NARROW G1`, `DUAL SPACE`,
   `IGNITE`, `RED BAR TAKEOUT`, `1-BAR RISK`, `PULLBACK`, `SUCKER-PLAY ZONE`, `SURGE OFF 200`,
   `THREE FINGERS`.
3. **Never substitute a convenient phrase for an unsupported concept.** Labels tested against the
   manual and rejected are listed in `02_CONCEPT_TO_CODE_MAP.md §6` (candidate callout audit).
4. **Implementation-layer vocabulary must be visibly marked as such.** The signal-quality tiers
   (`WATCH`, `DEVELOPING`, `SETUP`, `HIGH QUALITY`, `ELITE SETUP`) are **not** Velez terminology.
   They are always accompanied by the marker `⟨impl⟩` (ASCII: `[impl]`) and by a dashboard row
   naming them an implementation-defined confluence layer.
5. **Approximation marking.** Any value or label whose computation depends on a threshold the
   manual does not supply carries a tilde prefix `~` in Analysis/Full/Debug modes, and the
   dashboard shows an `APPROXIMATIONS ACTIVE` row.
6. **Rule identifiers are the manual's, not Velez's** (SRC-002). Wherever a `C##` or `V#.#` ID is
   shown, it is presented as a manual reference, not as an official rule number.
7. **No emoji carries meaning.** Emoji, where used, are decorative only; every glyph has an
   ASCII-safe text equivalent and an input to disable them entirely.
8. **No branding.** No logo, no proprietary artwork, no name styling implying authorship,
   endorsement or affiliation.

---

## 12. WHAT THIS INDICATOR CAN AND CANNOT CLAIM

### It can claim

- That every rendered behaviour traces to a numbered evidence record in this document with a page
  citation.
- That it distinguishes, in the interface itself, between a source-defined mechanic and a
  threshold this project added.
- That its actionable signals are evaluated on chart-bar close, and that the confirmation policy
  is documented and auditable.
- That it implements the manual's stated evaluation order (state → position → event) and reports
  which step blocked a candidate.
- That it does not invent members of the unenumerated event sets (SRC-091).
- That it reproduces the manual's statistics only as attributed reference text.

### It cannot claim — and does not

- **Not official, endorsed, affiliated or authorised.** It is an independent, unofficial,
  methodology-inspired study tool.
- **No accuracy, profitability, win rate or performance claim of any kind.** The manual's own
  figures are self-reported (SRC-004) and are not this project's outputs.
- **Not a strategy, backtest, broker simulation or execution engine.** There are no fills, no P&L,
  no position, no `strategy.*` calls. The trade lifecycle it draws is explicitly a *virtual
  methodology lifecycle*.
- **Not a faithful mechanisation of the method.** The manual states the definitions are
  deliberately loose and that the framework "cannot be backtested" (SRC-303). Any numeric
  threshold here is an addition by this project.
- **Not validated against the source's examples.** Every worked example in the source is a setup
  that worked (SRC-302), so the examples cannot validate anything.
- **Not a complete rendering of the methodology.** Ten of thirteen bar types and eleven of
  fourteen actionable events are absent from the source material itself (SRC-091).
- **Not blanket non-repainting.** The specific confirmation behaviour of each component is
  documented in `07_QA_REVIEW.md`; pivot-derived references are confirmed-later by construction
  and are labelled as such.
- **Not investment advice.**
