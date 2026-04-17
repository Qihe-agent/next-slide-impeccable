# Phase 2.5 · Custom Style from Design Context

> 提取自 SKILL.md · 用户选 'Design from my context' 时走这里，从 .impeccable.md 合成全新视觉系统（不从 53 预设里挑）。

### Step 2.5: Custom Style from Design Context

**Only triggered when user picks "Design from my context" in Step 2.0.**

This is where presentations go BEYOND presets. Instead of choosing from 53 predefined styles, the AI creates a custom visual system derived from the project's design context.

**Step 2.5.1: Invoke Design Principles**

Read the following (in this order):
1. `.impeccable.md` — the project's design context (audience, personality, aesthetic direction, principles)
2. Invoke {{command_prefix}}frontend-design principles mentally — apply its typography, color-and-contrast, motion-design, and spatial-design references
3. [STYLE_PRESETS.md](STYLE_PRESETS.md) — use as REFERENCE for what a complete style definition looks like (CSS variable structure, signature elements, font pairing patterns). Do NOT pick a preset — use the structure only.

**Step 2.5.2: Synthesize Custom Style**

From the design context, derive a complete visual system:

| Design Context Field | Maps To |
|---------------------|---------|
| Brand personality | Overall mood, animation tempo, spacing density |
| Aesthetic direction | Color palette, typography pairing, background treatment |
| Design principles | Layout decisions, content hierarchy, decorative element choices |
| Anti-references | What to explicitly AVOID |

Generate a complete `:root` CSS variable block following the exact structure from html-template.md, but with values derived from design context instead of a preset. Include:
- **Color palette** — using OKLCH if the context calls for sophistication; tint neutrals toward brand hue (per frontend-design color reference)
- **Font pairing** — distinctive choices that match brand personality (consult {{command_prefix}}frontend-design typography reference for alternatives to overused fonts)
- **Spacing rhythm** — density that matches audience context (executive = generous, technical = denser)
- **Animation timing and easing** — exponential easing (ease-out-quart/quint/expo), tempo matching audience mood
- **Signature elements** — unique visual details that reflect brand personality (NOT generic glassmorphism, NOT gradient text, NOT neon accents)

**Step 2.5.3: Generate 2 Variations**

Generate 2 single-slide HTML previews showing the custom style:
- **Variation A**: Closer interpretation of design context (faithful to stated direction)
- **Variation B**: More experimental interpretation (creative push beyond stated direction)

Both use the user's actual title. Save to `.claude-design/slide-previews/custom-a.html`, `custom-b.html`.

Open both in the browser:
```
open .claude-design/slide-previews/custom-a.html
open .claude-design/slide-previews/custom-b.html
```

**Step 2.5.4: User Picks or Refines**

**When running in Claude Code CLI**, use `AskUserQuestion` tool with options:

Options: Custom A (faithful) / Custom B (experimental) / Mix elements / Show me presets instead

The "Show me presets instead" escape hatch ensures the user ALWAYS has the safety net of the 53 curated presets. If chosen, go back to Step 2.1.

