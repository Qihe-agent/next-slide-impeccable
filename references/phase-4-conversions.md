# Phase 4: Content Conversion

> 提取自 SKILL.md · 用于 Mode B (PPT conversion) 和 Mode E (Markdown conversion)。

## Phase 4A: PPT Conversion

When converting PowerPoint files:

1. **Extract content** — Run `python scripts/extract-pptx.py <input.pptx> <output_dir>` (install python-pptx if needed: `pip install python-pptx`)
2. **Confirm with user** — Present extracted slide titles, content summaries, and image counts
3. **Style selection** — Proceed to Phase 2 for style discovery
4. **Generate HTML** — Convert to chosen style, preserving all text, images, slide order, and speaker notes

---

## Phase 4B: Markdown Conversion

When converting Markdown content to slides (triggered by Mode E detection):

### Step 4B.1: Parse Markdown Structure

Read the markdown file/content and extract slide structure using these rules:

**Slide boundary detection (in priority order):**
1. `---` (horizontal rule) = explicit slide separator — highest priority
2. `## Heading` (h2) = new slide starts at each h2 — used when no `---` separators exist
3. `# Heading` (h1) = title slide
4. If neither `---` nor `##` patterns are found, split by `### Heading` (h3) blocks

**Content mapping within each slide:**
- `# Heading` -> Title slide (heading + first paragraph as subtitle)
- `## Heading` -> Slide title
- Bullet lists (`- ` or `* `) -> Slide bullet content
- Numbered lists (`1. `) -> Ordered content or step-by-step slides
- `> Blockquote` -> Quote slide
- Code blocks (`` ``` ``) -> Code slide
- `![alt](url)` -> Image slide or image within content slide
- Bold text (`**text**`) -> Emphasized/highlighted content
- Tables -> Data/comparison slide

### Step 4B.2: Auto-Detect Slide Types

Infer the best slide type from content patterns:

| Content Pattern | Slide Type |
|----------------|------------|
| Only a heading + short line | Title slide |
| "vs" or two parallel sections | Comparison slide |
| 3-4 standalone numbers/percentages | Stats slide |
| `> blockquote` | Quote slide |
| Bullet list with 4-6 items | Content slide |
| Bullet list with items that have sub-descriptions | Feature grid |
| Sequential/numbered items with dates or steps | Timeline slide |
| Code block | Code slide |
| Image reference | Image slide |
| Everything else | Content slide (default) |

### Step 4B.3: Confirm Structure

Present the parsed slide outline to the user:
- Slide count, titles, and detected types
- Flag any slides that may exceed content density limits (see Phase 0 table)
- Suggest splits for overloaded slides

### Step 4B.4: Style Selection

If the user hasn't specified a style:
- Proceed to Phase 2 (Style Discovery) for full style selection
- **When running in Claude Code CLI**, use `AskUserQuestion` to ask style preference

If the user specified a style (e.g. "use Swiss Modern" or "dark theme"), skip to Phase 3.

### Step 4B.5: Generate HTML

- Preserve ALL text content from the original markdown — do not summarize or omit
- Convert markdown formatting to HTML: `**bold**` -> `<strong>`, `*italic*` -> `<em>`, etc.
- Apply the chosen style's CSS variables, fonts, colors, and animations
- Respect content density limits — auto-split slides that exceed them
- Include speaker notes as HTML comments if the markdown contains `<!-- notes: ... -->` patterns
- Follow all Phase 3 requirements (viewport-base.css, fonts, navigation, etc.)
