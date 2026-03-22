# Frontend Slides

An AI skill for creating stunning, animation-rich HTML presentations — supports building from scratch or converting PowerPoint files into web slides.

## Overview

**Frontend Slides** helps non-designers create high-quality web presentations without knowing CSS or JavaScript. It uses a "show, don't tell" approach: AI generates visual style previews so you can pick what you like, rather than describing preferences in words.

### Key Features

- **Zero dependencies** — Single HTML file with all CSS/JS inline. No npm, build tools, or frameworks needed.
- **Visual style discovery** — Can't articulate your design taste? No problem. Pick directly from generated visual previews.
- **PPT conversion** — Convert existing PowerPoint files to web slides, preserving all images and content.
- **Anti-AI-template** — Curated unique styles that avoid generic AI aesthetics (no more white backgrounds with purple gradients).
- **Viewport fit (mandatory)** — Every slide must render completely within 100vh, with no internal scrollbars.
- **Browser inline editing** (optional) — Edit text directly in the browser after generation, auto-save to localStorage, export supported.
- **Interactive presentation mode (Mode D)** — Single-page interactive demos with tag clouds, clickable cards, and expandable panels.
- **Effect library reuse** — AI proactively checks the existing effect library (`EFFECT_LIBRARY.md`) to reuse effects before writing new code.

---

## Workflow

### Phase 0: Detect Mode

| Mode | Trigger |
|------|---------|
| **Mode A** New presentation | Create from scratch |
| **Mode B** PPT conversion | Convert a .pptx file |
| **Mode C** Enhancement | Optimize an existing HTML presentation |
| **Mode D** Interactive demo | Single-page multi-panel click interaction |

---

### Phase 1: Content Collection (Mode A)

Ask 4 questions in one go:
1. **Purpose** — Pitch deck / Tutorial / Conference talk / Internal report
2. **Length** — 5-10 / 10-20 / 20+ slides
3. **Content status** — Complete content / Rough notes / Topics only
4. **Inline editing** — Whether browser-based text editing is needed

If the user provides images, AI reviews each one and incorporates usable images into the slide structure design (image-text co-design, not "structure first, fill images later").

---

### Phase 2: Style Discovery

1. User selects a preferred mood (Impressed / Excited / Calm / Inspired)
2. AI generates 3 independent style previews (style-a/b/c.html), auto-opened in browser
3. User picks a style or mixes elements from previews
4. **File naming** — Ask the user for a presentation name (e.g., `my-pitch`), auto-used for all subsequent filenames

---

### Phase 3: Generate Presentation

AI reads the following support files before generating:
- `viewport-base.css` — Mandatory responsive CSS (fully embedded)
- `html-template.md` — HTML architecture and JS features
- `animation-patterns.md` — Animation reference
- `EFFECT_LIBRARY.md` — Existing effect library (reuse first)

Output is a single self-contained HTML file with all CSS/JS inline, using Fontshare/Google Fonts, with detailed comments.

**Slide Numbering Rules:**
- Each slide gets an auto-assigned structured number (`01`, `02`, ...), HTML comment format: `<!-- Slide 01: Title -->`
- Multi-file mode filename format: `[name]-01-[keyword].html`
- Keyword derived from title, max 3 words, lowercase hyphen-separated (e.g., `my-pitch-03-solution.html`)
- TOC page: `[name]-toc.html`

**Layout Density Principle:**
- **Fill the page. Eliminate large visual whitespace** — Target ≥ 85% visual coverage
- Content elements must not be isolated; use multi-column grids, card grids, or layered compositions to fill space
- Keep margins minimal (sides `clamp(1.5rem, 3vw, 3rem)`, top/bottom `clamp(1rem, 2vh, 2rem)`)
- Use `flex: 1` to stretch content areas and fill remaining viewport height
- Use grids, dot patterns, gradients, or other decorative backgrounds to eliminate visual emptiness
- Keep element spacing tight (`gap: 0.5rem–1rem`), avoid loose layouts
- Anti-patterns: single centered text block, large whitespace areas, fixed-width containers that don't fill available space

---

### Phase 4: PPT Conversion

1. Run `python scripts/extract-pptx.py <input.pptx> <output_dir>` to extract content
2. Show extracted slide titles, content, and image counts for confirmation
3. Enter Phase 2 for style selection
4. Generate HTML presentation preserving all text, images, and speaker notes

---

### Phase 5: Delivery

- Delete temporary preview files (`.claude-design/slide-previews/`)
- Open final file in browser
- Inform: file location, navigation methods (arrow keys / space / swipe / dots), CSS variable customization

---

### Phase E: Effect Capture

User or AI proactively saves novel effects to `EFFECT_LIBRARY.md` for reuse in future presentations.

---

## Built-in Styles (12)

### Dark Themes
- **Bold Signal** — High impact, vivid cards on dark background
- **Electric Studio** — Clean professional, columnar layout
- **Creative Voltage** — Energetic, retro-modern, electric blue + neon
- **Dark Botanical** — Elegant refined, warm accents

### Light Themes
- **Notebook Tabs** — Editorial feel, paper texture with colorful tabs
- **Pastel Geometry** — Friendly approachable, vertical color blocks
- **Split Pastel** — Lively modern, dual-color vertical split
- **Vintage Editorial** — Character-driven, geometric shape accents

### Specialty Themes
- **Neon Cyber** — Futuristic, particle background, neon glow
- **Terminal Green** — Developer aesthetic, hacker vibes
- **Swiss Modern** — Minimalist, Bauhaus-inspired
- **Paper & Ink** — Literary feel, drop caps, blockquotes

---

## Vercel Static Deployment

```json
{
  "outputDirectory": "tera-intro",
  "routes": [
    { "src": "/", "dest": "/slide-toc.html" }
  ]
}
```

- Root route rewrite is required when no `index.html` exists, otherwise Vercel returns 404
- Redeploy: `npx vercel --prod --yes`
- Single file size limit: 100MB (watch out for `.mov` and other large files)

---

## Support Files

| File | Purpose | Read When |
|------|---------|-----------|
| `SKILL.md` | Core workflow and rules | Skill invocation (always) |
| `STYLE_PRESETS.md` | 12 visual presets | Phase 2 (style selection) |
| `viewport-base.css` | Mandatory responsive CSS | Phase 3 (generation) |
| `html-template.md` | HTML structure and JS features | Phase 3 (generation) |
| `animation-patterns.md` | CSS/JS animation reference | Phase 3 (generation) |
| `EFFECT_LIBRARY.md` | Reusable effect library | Phase 3 (generation), Phase E (capture) |
| `scripts/extract-pptx.py` | PPT content extraction script | Phase 4 (conversion) |

---

## Design Principles

1. **You don't need to be a designer to make something beautiful** — You just need to react to what you see.
2. **Dependencies are debt** — A single HTML file still works a decade later. A React project from 2019? Good luck.
3. **Generic is mediocre** — Every presentation should feel tailor-made for its scenario, not templated.
4. **Comments are kindness** — Code should explain itself to future you (or anyone who opens it).

---

## Requirements

- CodeBuddy / Claude Code CLI
- PPT conversion requires: Python + `python-pptx` library (`pip install python-pptx`)

## License

MIT — Free to use, modify, and share.
