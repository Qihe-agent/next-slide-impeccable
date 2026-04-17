# Design Elevation Protocol

> Templates define structure. Design context defines soul. The intersection creates presentations that are both technically reliable AND emotionally resonant.

This document bridges next-slide's preset system with the impeccable design philosophy. It guides how to elevate presets and create custom styles from design context.

---

## 1. Elevation Philosophy

A preset is a **complete visual system** — fonts, colors, spacing, animations — designed to work together. It guarantees consistency and quality.

A `.impeccable.md` design context describes **intent** — who the audience is, what feeling to evoke, what the brand stands for.

**Elevation** is the act of infusing a preset's structure with design context's intent. The result should feel like it was hand-crafted for this specific audience and brand, while maintaining the preset's technical reliability.

### The Three Levels

| Level | Creative Freedom | Safety | When to Use |
|-------|-----------------|--------|-------------|
| **Level 0: Pure Preset** | Low — follows preset exactly | Maximum — battle-tested | No `.impeccable.md`, user wants reliability |
| **Level 1: Elevated Preset** | Medium — preset + targeted refinements | High — structure preserved | `.impeccable.md` exists, user chose a preset |
| **Level 2: Custom from Context** | High — full creative synthesis | Moderate — HTML architecture preserved | `.impeccable.md` exists, user wants uniqueness |

**Critical rule**: Every level still outputs a single self-contained HTML file using viewport-base.css and html-template.md architecture. The HTML/JS skeleton is invariant. Only the CSS design layer changes.

---

## 2. Preset-to-Context Mapping

When elevating a preset (Level 1), use this mapping to determine WHAT to change based on the design context.

### Dark Themes (Keynote Noir, Bold Signal, Neon Cyber, Terminal Green, Midnight Corporate, Cinema Scope, Dark Botanical, Dark Premium, Dark Cinema, Futuristic Blue, Starfield)

**Typography upgrade paths:**
- If brand personality is "bold/confident/powerful" → Keep heavy display fonts, consider: Clash Display, Archivo Black, Bebas Neue
- If brand personality is "elegant/refined/luxury" → Switch to: Playfair Display, Cormorant, DM Serif Display
- If brand personality is "modern/clean/tech" → Consider: Space Grotesk, Outfit, Sora, Instrument Sans
- If brand personality is "creative/playful" → Consider: Bricolage Grotesque, Unbounded, Righteous

**Color harmonization:**
- Dark themes have limited neutral palette — tinting works subtly on surface colors (`--surface`, `--bg-secondary`)
- Accent color is the primary elevation target — swap preset accent for brand-aligned accent
- Keep contrast ratios: text-on-dark must remain WCAG AA compliant (4.5:1 minimum)
- If brand uses warm colors on a cool-dark preset, add warm tints to `--border-subtle` and `--surface`

**Animation matching:**
- Executive/investor audience → Slow, dramatic (0.8-1.5s), minimal motion, fade-only transitions
- Technical audience → Crisp, fast (0.2-0.4s), code-like reveals
- Creative audience → More experimental: clip-path wipes, text scramble effects

### Light Themes (Swiss Modern, Paper & Ink, Notebook Tabs, Pastel Geometry, Morning Brief, Campus White, Soft Landing, Watercolor Wash, Korean Soft, Claymorphism 3D, Wabi-Sabi Zen)

**Typography upgrade paths:**
- If brand personality is "warm/friendly/approachable" → Consider: Source Serif 4, Literata, Lora
- If brand personality is "professional/clean" → Consider: General Sans, Switzer, Geist
- If brand personality is "editorial/sophisticated" → Consider: Fraunces, Newsreader, Libre Baskerville
- If brand personality includes Chinese → Pair with: LXGW WenKai (warm), Noto Serif SC (editorial), Noto Sans SC (clean)

**Color harmonization:**
- Light themes have more room for tinting — adjust `--bg-primary`, `--bg-secondary` with brand warmth/coolness
- Keep pure whites rare (use off-white tinted toward brand hue, per frontend-design principles)
- If brand is warm: shift grays toward warm gray (amber/rose tint)
- If brand is cool: shift grays toward cool gray (blue/slate tint)

**Animation matching:**
- Teaching/educational audience → Gentle, encouraging (0.4-0.8s), bounce-in for emphasis
- Professional/business audience → Crisp, minimal (0.2-0.4s), subtle fade-up
- Calm/wellness audience → Very slow (1-2s), breathing animations, gentle fades

### Editorial (Editorial Serif, Fashion Editorial, Newsprint Broadsheet, Vintage Editorial)

**Typography is paramount in editorial styles.** Elevation here focuses almost entirely on font choice:
- Match the brand's tone of voice to the display font
- If brand has a specific typeface, use it as the display font and adjust body to complement
- Drop caps and pull quotes should reflect brand personality

**Color harmonization:**
- Editorial styles use restrained palettes — only adjust accent color to brand
- Ink tones: if brand is warm, shift black toward warm charcoal; if cool, toward navy

### Bold & Creative (Electric Studio, Creative Voltage, Split Pastel, Pop Art, Bold Typography, Neon Brutalism, Memphis Pop)

**These presets have the MOST room for elevation** — they expect expressiveness:
- Replace accent colors freely with brand colors (these styles embrace bold palettes)
- Typography: pick the most expressive font that matches brand personality
- Background treatments: adapt geometric patterns to reflect brand aesthetic
- Signature elements: this is where custom brand-specific details belong (custom shapes, branded motifs)

### Retro & Vintage (Grainy Retro, Art Deco Gatsby, Risograph Overprint, Vintage Poster, Retro Arcade)

**Elevation must preserve the retro feel:**
- Typography: stay within the era's aesthetic but pick specific variants that match brand
- Colors: vintage presets use desaturated palettes — adjust hues but keep saturation low
- Textures: grain, paper, halftone effects should match brand warmth/coolness
- Avoid: modern clean typography on retro styles (breaks the illusion)

### Artistic (Surrealism Gallery, Scrapbook Portfolio, Blue Collage, Pink Handwritten, Art Nouveau Botanical, Soft Dreamy, Terracotta Earth)

**These styles are personal and expressive:**
- Typography: handwritten or display fonts should feel authentic to brand personality
- Colors: organic, imperfect palettes — adjust undertones to match brand
- Layout: asymmetric, unexpected — elevation can push this further with brand-specific spatial rhythm

### Cultural & Special (东方墨韵, 和風, Gradient Dreams, Blueprint, Bauhaus Primary, Swiss Grid, Aurora Mesh, Chinese Ink Wash)

**Cultural sensitivity is critical:**
- Chinese/Japanese styles: elevation focuses on CJK font selection matching brand voice
- Design system styles (Bauhaus, Swiss Grid): elevation through color and content choices, NOT structural changes
- Gradient/Mesh styles: shift gradient colors toward brand palette while maintaining flow

---

## 3. Custom Style Architecture (Level 2)

When creating a style entirely from design context (Step 2.5), the custom style MUST follow this CSS variable structure. This ensures compatibility with html-template.md and all slide type templates.

### Required CSS Variables

```css
:root {
    /* === COLOR PALETTE (REQUIRED) === */
    --bg-primary: /* Main background */;
    --bg-secondary: /* Secondary/alternating background */;
    --bg-tertiary: /* Tertiary/card background */;
    --text-primary: /* Main text color */;
    --text-secondary: /* Secondary/supporting text */;
    --text-muted: /* Muted/caption text */;
    --accent: /* Primary accent color */;
    --accent-secondary: /* Secondary accent (optional) */;
    --accent-glow: /* Glow/hover version of accent (optional) */;
    --border-subtle: /* Subtle borders/dividers */;
    --surface: /* Card/surface background overlay */;

    /* === TYPOGRAPHY (REQUIRED) === */
    --font-display: /* Display/heading font stack */;
    --font-body: /* Body text font stack */;
    --font-mono: /* Monospace font stack (for code slides) */;

    /* === SIZING — ALL must use clamp() === */
    --title-size: clamp(2rem, 6vw, 5rem);
    --h2-size: clamp(1.25rem, 3.5vw, 2.5rem);
    --h3-size: clamp(1rem, 2.5vw, 1.75rem);
    --subtitle-size: clamp(0.875rem, 2vw, 1.25rem);
    --body-size: clamp(0.75rem, 1.5vw, 1.125rem);
    --small-size: clamp(0.65rem, 1vw, 0.875rem);
    --stat-size: clamp(2rem, 8vw, 6rem);

    /* === SPACING — ALL must use clamp() === */
    --slide-padding: clamp(1.5rem, 4vw, 4rem);
    --content-gap: clamp(1rem, 2vw, 2rem);
    --element-gap: clamp(0.25rem, 1vw, 1rem);

    /* === ANIMATION === */
    --ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1);
    --ease-out-back: cubic-bezier(0.34, 1.56, 0.64, 1);
    --ease-spring: cubic-bezier(0.175, 0.885, 0.32, 1.275);
    --duration-fast: 0.3s;
    --duration-normal: 0.6s;
    --duration-slow: 1s;
    --duration-dramatic: 1.5s;
}
```

### Deriving Values from Design Context

| `.impeccable.md` Field | CSS Variables Affected | How to Derive |
|------------------------|----------------------|---------------|
| **Audience** (Users section) | `--duration-*`, spacing density | Executive → slower, more spacious; Technical → faster, denser; Student → moderate, generous |
| **Brand Personality** (3 words) | Overall mood: colors, font weight, animation tempo | "Bold confident modern" → high contrast, heavy weights, medium-fast | "Warm approachable playful" → soft colors, rounded type, bouncy timing |
| **Aesthetic Direction** | Colors, fonts, backgrounds, signature elements | Direct mapping from stated references; anti-references define what to avoid |
| **Design Principles** | Layout choices, hierarchy, decorative decisions | Each principle guides specific choices during generation |

### Signature Element Design

Every custom style should have ONE memorable signature element that reflects brand personality. Examples:

- **Geometric brand** → Repeating CSS pattern in slide backgrounds using brand shapes
- **Organic/natural brand** → Soft gradient blobs or wave patterns
- **Technical brand** → Subtle grid overlay or monospace accent details
- **Luxurious brand** → Thin gold/silver accent lines, generous whitespace, elegant transitions
- **Playful brand** → Unexpected color pops, slightly rotated elements, bouncy reveals

**Anti-patterns to avoid** (from frontend-design AI Slop Test):
- Glassmorphism as decoration
- Gradient text on headings/metrics
- Neon accents on dark backgrounds
- Purple-to-blue gradients
- Rounded rectangles with thick colored border on one side
- Generic drop shadows on cards

---

## 4. Elevation Checklist

After generating a presentation with IMPECCABLE_CONTEXT active, verify that the design context meaningfully influenced the output. Check each item:

### For Level 1 (Elevated Preset):
- [ ] Typography is NOT the preset's default font (or default was already ideal for the brand)
- [ ] At least one color reflects brand identity or aesthetic direction
- [ ] Animation tempo matches audience context (not just preset default)
- [ ] Neutral colors are tinted toward brand hue (even subtly)
- [ ] The presentation feels different from a "bare" version of the same preset

### For Level 2 (Custom from Context):
- [ ] Color palette is derived from design context, NOT from any existing preset
- [ ] Font pairing reflects brand personality (not overused defaults like Inter/Roboto)
- [ ] Signature element is present and reflects brand personality
- [ ] Animation choreography matches stated mood/audience
- [ ] Background treatment is custom (not copied from a preset)
- [ ] The presentation would NOT be produced by next-slide alone (without impeccable context)
- [ ] The AI Slop Test passes — this does NOT look like generic AI output
- [ ] Design principles from `.impeccable.md` are reflected in at least 2 layout decisions

### For Both Levels:
- [ ] viewport-base.css is included in full (non-negotiable)
- [ ] All font sizes use clamp() (non-negotiable)
- [ ] Content density limits are respected (non-negotiable)
- [ ] Accessibility: prefers-reduced-motion is supported (non-negotiable)
- [ ] CJK fonts included if Chinese text present (non-negotiable)
- [ ] Single self-contained HTML file (non-negotiable)

---

## 5. Troubleshooting

### "The elevated preset looks basically the same as the default"
The elevation was too conservative. Push harder:
- Change the display font (this has the biggest visual impact)
- Shift the accent color to brand color
- Add a signature background element
- Adjust animation timing dramatically

### "The custom style feels incoherent"
The design context may lack specificity. Solutions:
- Re-read `.impeccable.md` — focus on the 3-word personality and derive a single dominant mood
- Pick ONE preset as structural inspiration and deviate only in color/font/signature
- Reduce the number of unique elements — cohesion comes from repetition

### "The presentation triggers the AI Slop Test"
Common fixes:
- Replace gradient backgrounds with solid colors + subtle texture
- Remove glassmorphism effects entirely
- Switch from purple/cyan/neon accents to muted, brand-aligned colors
- Use intentional decorative elements instead of generic effects
- Ensure typography has personality (not default sans-serif)

### "The user keeps going back to presets"
This is fine. The escape hatch exists for a reason. Custom styles require:
- A well-defined `.impeccable.md` (the lightweight 3-question version may not be enough)
- Trust in the AI's creative choices
- Willingness to iterate

If users consistently prefer presets, recommend they run `{{command_prefix}}teach-impeccable` for a richer design context that produces better custom results.
