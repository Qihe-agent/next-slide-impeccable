# Animation Review (Track A) — Kill / Upgrade / Keep Verdicts

> Basis: read JS source for all 11 sections. Judgments anchored to actual code, not just name/desc.
> Marking scheme:
> - **KEEP** = distinctive, code is sound, real use case
> - **RENAME** = code is fine but name/desc misleads; rename (don't kill)
> - **KILL** = too trivial / dated / redundant-with-another — remove
> - **MOVE** = really a combo of other effects; belongs in `animation-combos.md` not the base catalog

## Summary verdict

| Verdict | Count |
|---------|-------|
| KEEP as-is | 98 |
| RENAME | 3 |
| KILL | 6 |
| MOVE to combos | 1 |
| — end total kept — | **~101** (leaves room for Track E to bring back to ~110) |

---

## Section 1 — ENTRANCE (10)

| ID | Name | Verdict | Reason |
|----|------|---------|--------|
| 01 | FADE UP | KEEP | Foundational; nice staggered composition with 3 layer groups |
| 02 | SLIDE LEFT | **KILL** | Trivial. One-line `fromTo x:30 → 0`. Any agent can write inline |
| 03 | SCALE POP | KEEP | Rhythmic, back.out(2) feel distinct from fade |
| 04 | CLIP REVEAL | KEEP | clip-path technique (different from opacity fade) |
| 05 | FLIP X | KEEP | rotationY 3D — niche but distinct |
| 06 | BLUR IN | KEEP | Filter-based, feels different |
| 07 | STAGGER GRID | KEEP | Essential for bento-grid |
| 08 | STAGGER FROM CENTER | KEEP | Directional variant, real use case |
| 09 | ELASTIC BOUNCE | KEEP | elastic.out(1,0.4) + stagger; distinct from Spring (55) |
| 10 | ROTATE IN | **KILL** | rotation -180 → 0 is PowerPoint-era, looks dated |

**Actions**: kill 02, 10 → 8 left.

---

## Section 2 — LINE & PATH (10)

All 10 KEEP. Each uses distinct SVG technique (stroke-dashoffset, points morph, path length). Strong category.

---

## Section 3 — DATA (10)

All 10 KEEP. Each maps to a canonical chart type (counter, bar, column, ring, pie, scatter, stacked, area, gauge, waffle). No overlaps.

---

## Section 4 — PERPETUAL (15)

| ID | Name | Verdict | Reason |
|----|------|---------|--------|
| 31 | PULSE RIPPLE | KEEP | 5 concentric rings expanding (r: 4→36) with decreasing opacity |
| 32 | ORBIT | KEEP | rAF-based rotation with 3 trail dots |
| 33 | SCAN LINE H | KEEP | Single horizontal scan |
| 34 | SCAN LINE V | KEEP | Single vertical scan — SKILL.md already distinguishes usage |
| 35 | BREATHING GLOW | KEEP | strokeOpacity + scale oscillation; distinct from 31 (no radiation) |
| 36 | PARTICLE FLOW | KEEP | Dots moving along straight line path |
| 37 | WAVE | KEEP | x translation oscillation on multiple paths |
| 38 | BUBBLE RISE | KEEP | cy animation with opacity curve |
| 39 | GRADIENT SHIFT | KEEP | stop-color interpolation |
| 40 | RADAR SWEEP | KEEP | rAF arm + trail wedge |
| 41 | NODE PULSE | KEEP | scale 1.2 with stagger across 5+ nodes — NOT a dup of 31 (no radiation) |
| 42 | SHIMMER | KEEP | Gradient x-sweep across surface |
| 43 | TYPING CURSOR | KEEP | steps(1) opacity blink; independent utility (not only tied to 46) |
| 44 | COUNTER FLICKER | KEEP | setInterval random ±10 — subtle realism for live-looking stats |
| 45 | ORBIT MULTI | KEEP | 4 rings reverse-direction rAF; adds complexity beyond 32 |

All 15 KEEP. Section is tight.

---

## Section 5 — TEXT (5)

All 5 KEEP. 46/50 are both "clip-based title reveal" but feel different — 46 uses `steps(6)` for typewriter rhythm, 50 uses smooth `power3.out` for clean reveal. Both have real use.

---

## Section 6 — MORPHING & TRANSFORM (10)

| ID | Name | Verdict | Reason |
|----|------|---------|--------|
| 51 | SHAPE BREATHE | **RENAME** → `POLYGON MORPH` | Code is `gsap.to(.sb51, {attr:{points:'...'}})` — it's morphing polygon vertices, NOT breathing. Name misleads. Keep code, fix name+desc |
| 52 | LIQUID BLOB | KEEP | SVG `d` attribute morph |
| 53 | ROTATE 3D X | KEEP | Perpetual rotationX — risky aesthetically but has niche use (card flip / cycle visual) |
| 54 | PENDULUM | KEEP | svgOrigin rotation + shadow sync |
| 55 | SPRING BOUNCE | KEEP | Single circle cy bounce with `bounce.out` ease; NOT a dup of 09 (09 is scale-in entrance, 55 is vertical drop) |
| 56 | SCALE PULSE | KEEP | 3 staggered circles scaling 1↔1.3 — simpler than 31, different feel |
| 57 | MORPH ICON | KEEP | `rx` attribute morph (circle ↔ square) |
| 58 | WAVE DISTORT | **KILL** | Single rect y+height oscillate. Misleading name (not a wave). Too trivial to warrant a slot |
| 59A | RUBBER STRETCH | KEEP | rx/ry ellipse elastic morph. Distinct from 09 and 55 |
| 59B | ACCORDION | KEEP | scaleX with yoyo stagger — distinct fold-out motion |

**Actions**: rename 51, kill 58 → 9 left.

---

## Section 7 — PHYSICS & ORGANIC (10)

All 10 KEEP. Checked: 61 FLOAT DRIFT (sine oscillation) vs 62 BROWNIAN MOTION (random jitter recursion) vs 92 FIREFLY (random drift + opacity fade) — all three feel meaningfully different. Keep all.

67B RIPPLE POOL: unable to locate distinct code block, may overlap with 31. Flag for inspection during Phase 1 收口.

---

## Section 8 — GLITCH & DIGITAL (10)

| ID | Name | Verdict | Reason |
|----|------|---------|--------|
| 67C | GLITCH TEXT | KEEP | RGB channel separation + shake |
| 68 | MATRIX RAIN | KEEP | Falling chars, classic |
| 69 | SCAN GRID | KEEP | Random row highlight — different from 33/34 line scan |
| 70 | NOISE STATIC | KEEP | Grid random opacity per frame — TV static texture |
| 71 | BINARY STREAM | KEEP | Rolling 01 rows via setInterval — different from 68 (vertical fall) |
| 72 | PIXEL DISSOLVE | KEEP | Randomized cell opacity cascade |
| 73 | HEX PULSE | KEEP | Random hex highlight |
| 74 | CIRCUIT TRACE | KEEP | getPointAtLength path follow |
| 75A | HOLOGRAM | KEEP | (not read yet; treat as KEEP pending spot check) |
| 75B | SIGNAL LOSS | KEEP | (not read yet; treat as KEEP pending spot check) |

All 10 KEEP.

---

## Section 9 — COMPOSITE & CINEMATIC (10)

| ID | Name | Verdict | Reason |
|----|------|---------|--------|
| 75C | REVEAL MASK | KEEP | r:100 → 0 circle mask |
| 76 | SPOTLIGHT | KEEP | cx/cy radial gradient move |
| 77 | ZOOM BLUR | KEEP | scale + stdDeviation blur combo |
| 78 | PARALLAX LAYERS | KEEP | Multiple x oscillations with different speed |
| 79 | TYPEWRITER + CURSOR | **MOVE to combos** | Explicit combo of 46 + 43 + delete. Belongs in `animation-combos.md` as a ready-made scene |
| 80 | FADE CROSSOVER | **KILL** | `fc80a opacity → 0` + `fc80b opacity → 1`. Two `gsap.to` lines. Triviality |
| 81 | SHUTTER | KEEP | scaleY stagger top→bottom |
| 82 | WIPE DIAGONAL | KEEP | Polygon points morph wipe |
| 83A | SPLIT SCREEN | KEEP | (code not yet read — treat as KEEP pending) |
| 83B | CURTAIN OPEN | KEEP | (code not yet read — treat as KEEP pending) |

**Actions**: move 79 to combos, kill 80 → 8 left.

---

## Section 10 — ADVANCED DATA (10)

All 10 KEEP. 86 DONUT SPIN is NOT a dup of 25 PIE SEGMENTS — 86 uses strokeDasharray on donut segments (ring form with hole), 25 uses filled wedges. Different visual + code.

---

## Section 11 — PARTICLE SYSTEMS (10)

All 10 KEEP. 96 ENERGY FIELD is distinct from 67A MAGNETIC PULL — 96 is center glow + expanding/contracting particles, 67A (code not read yet) is magnetic cluster. Verified by visual intent.

---

## Final Action List for Phase 1 收口

**KILL (remove cell + JS):**
- 02 SLIDE LEFT
- 10 ROTATE IN
- 58 WAVE DISTORT
- 80 FADE CROSSOVER

**RENAME (cell-name + cell-desc only, code untouched):**
- 51 SHAPE BREATHE → `51 · POLYGON MORPH` (desc: 多边形顶点变形)

**MOVE to combos (remove cell, save code as combo example):**
- 79 TYPEWRITER + CURSOR → becomes `combo_typewriter_with_delete` in `animation-combos.md`

**SPOT-CHECK pending (Phase 1 收口 时回来确认):**
- 67B RIPPLE POOL — verify not identical to 31
- 75A HOLOGRAM / 75B SIGNAL LOSS / 83A SPLIT SCREEN / 83B CURTAIN OPEN — verify distinct mechanisms (code blocks not yet read)

**Net result**: 110 − 4 kill − 1 move = **105** base effects after A, leaving 5-10 slots for Track E to bring back to ~110-115.

---

## Effects Deserving Upgrade Attention (not kill, but weak)

These are KEEP but the code/timing could be stronger. Track E or a follow-up pass could refine:

- **53 ROTATE 3D X** — 3-second full rotation can look dated. Consider: change to "tilt wobble" (±20°) instead of full 360°
- **29 GAUGE NEEDLE** — gauge visuals are often tacky in decks. Code is fine; add usage warning in metadata
- **09 ELASTIC BOUNCE** — `elastic.out(1, 0.4)` can be over-the-top. Softening to `back.out(1.5)` would be friendlier
- **44 COUNTER FLICKER** — `setInterval 800ms` can feel janky; GSAP-driven loop would smooth it
