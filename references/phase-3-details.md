# Phase 3 · Generation Details

> 提取自 SKILL.md · Background Image Integration + Bilingual Support + Impeccable Enhancements。主流程在 SKILL.md Phase 3。

### Background Image Integration (when Phase 2.8 was not skipped)

If the user opted for AI background images or provided their own, integrate them during HTML generation:

1. **Add `.slide-bg` CSS** from Phase 2.8.3 to the `<style>` block
2. **For each slide with a background image**, add the background div as the first child:
   ```html
   <div class="slide" id="slide-N">
       <div class="slide-bg" style="background-image:url(bg/NN-slug.jpg); opacity: 0.10;"></div>
       <div class="slide-content">
           <!-- slide content here -->
       </div>
   </div>
   ```
3. **Set opacity per slide type** according to the table in Phase 2.8.3
4. **Add dark overlay** if needed (for slides with more text):
   ```html
   <div class="slide-bg" style="background-image:url(bg/NN-slug.jpg); opacity: 0.15;"></div>
   <div class="slide-bg-overlay" style="background:rgba(0,0,0,0.65);position:absolute;inset:0;z-index:0;"></div>
   ```
5. **Add `text-shadow`** to headings if text contrast is borderline
6. **If base64 mode**: replace `url(bg/...)` with `url(data:image/jpeg;base64,...)`

**Output structure when using background images:**
```
presentation-name/
├── index.html          ← main presentation
├── bg/                 ← AI-generated or user-provided backgrounds
│   ├── 01-cover.jpg
│   ├── 02-topic.jpg
│   └── ...
└── img/                ← any other images (optional)
```

### Bilingual Support

When language is Chinese or Bilingual:
- Include Chinese web fonts: `Noto Sans SC` for body, `Noto Serif SC` or `LXGW WenKai` for display
- Font stack example: `'LXGW WenKai', 'Noto Serif SC', serif`
- Ensure `lang="zh"` or `lang="zh-en"` on `<html>`
- Text line-height for Chinese: at least 1.8

### Impeccable Generation Enhancements (when IMPECCABLE_CONTEXT = true)

When generating with design context active, apply these additional principles from the impeccable ecosystem:

**For ALL generations (whether using preset or custom style):**
1. Consult {{command_prefix}}frontend-design aesthetics guidelines during generation — specifically the DO/DON'T lists for typography, color, layout, motion, and visual details
2. Apply the AI Slop Test mentally as you generate: avoid the fingerprints of AI work (cyan-on-dark, purple-to-blue gradients, glassmorphism, gradient text on metrics, identical card grids, neon accents on dark backgrounds)
3. If using a preset, apply the elevations confirmed in Step 2.6
4. Reference `.impeccable.md` design principles for content hierarchy decisions — how to split content, which layout types to choose, where to place emphasis

**For custom style generations (from Step 2.5):**
1. The `:root` CSS variables come from Step 2.5.2 synthesis — do NOT fall back to a preset
2. Animation patterns: use [ANIMATION_PATTERNS.md](ANIMATION_PATTERNS.md) as REFERENCE but adapt timing/easing/choreography to match the design context mood
3. Background treatments: create custom backgrounds informed by design context aesthetic direction (not limited to preset patterns)
4. Decorative elements: design unique signature elements that reflect brand personality — these should be the "one thing someone remembers" about this presentation
5. Apply the {{command_prefix}}frontend-design principle: "Interpret creatively and make unexpected choices that feel genuinely designed for the context. No design should be the same."

**What stays IDENTICAL regardless of impeccable context:**
- viewport-base.css inclusion (non-negotiable)
- html-template.md architecture (non-negotiable)
- Content density limits (non-negotiable)
- Navigation, progress bar, page counter
- Font loading via Google Fonts / Fontshare
- Single self-contained HTML file output
- All accessibility requirements (prefers-reduced-motion, keyboard nav, etc.)

---

