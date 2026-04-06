---
name: frontend-slides
description: 从零创建或从 PPT 转换炫酷的 HTML 演示文稿，支持丰富动画效果。
---

# Frontend Slides

Create zero-dependency, animation-rich HTML presentations that run entirely in the browser.

## Core Principles

1. **Zero Dependencies** — Single HTML files with inline CSS/JS. No npm, no build tools.
2. **Show, Don't Tell** — Generate visual previews, not abstract choices. People discover what they want by seeing it.
3. **Distinctive Design** — No generic "AI slop." Every presentation must feel custom-crafted.
4. **Viewport Fitting (NON-NEGOTIABLE)** — Every slide MUST fit exactly within 100vh. No scrolling within slides, ever. Content overflows? Split into multiple slides.

## Design Aesthetics

You tend to converge toward generic, "on distribution" outputs. In frontend design, this creates what users call the "AI slop" aesthetic. Avoid this: make creative, distinctive frontends that surprise and delight.

Focus on:
- Typography: Choose fonts that are beautiful, unique, and interesting. Avoid generic fonts like Arial and Inter; opt instead for distinctive choices that elevate the frontend's aesthetics.
- Color & Theme: Commit to a cohesive aesthetic. Use CSS variables for consistency. Dominant colors with sharp accents outperform timid, evenly-distributed palettes. Draw from IDE themes and cultural aesthetics for inspiration.
- Motion: Use animations for effects and micro-interactions. Prioritize CSS-only solutions for HTML. Use Motion library for React when available. Focus on high-impact moments: one well-orchestrated page load with staggered reveals (animation-delay) creates more delight than scattered micro-interactions.
- Backgrounds: Create atmosphere and depth rather than defaulting to solid colors. Layer CSS gradients, use geometric patterns, or add contextual effects that match the overall aesthetic.

Avoid generic AI-generated aesthetics:
- Overused font families (Inter, Roboto, Arial, system fonts)
- Cliched color schemes (particularly purple gradients on white backgrounds)
- Predictable layouts and component patterns
- Cookie-cutter design that lacks context-specific character

Interpret creatively and make unexpected choices that feel genuinely designed for the context. Vary between light and dark themes, different fonts, different aesthetics. You still tend to converge on common choices (Space Grotesk, for example) across generations. Avoid this: it is critical that you think outside the box!

## Viewport Fitting Rules

These invariants apply to EVERY slide in EVERY presentation:

- Every `.slide` must have `height: 100vh; height: 100dvh; overflow: hidden;`
- ALL font sizes and spacing must use `clamp(min, preferred, max)` — never fixed px/rem
- Content containers need `max-height` constraints
- Images: `max-height: min(50vh, 400px)`
- Breakpoints required for heights: 700px, 600px, 500px
- Include `prefers-reduced-motion` support
- Never negate CSS functions directly (`-clamp()`, `-min()`, `-max()` are silently ignored) — use `calc(-1 * clamp(...))` instead

**When generating, read `viewport-base.css` and include its full contents in every presentation.**

### Content Density Limits Per Slide

| Slide Type | Maximum Content |
|------------|-----------------|
| Title slide | 1 heading + 1 subtitle + optional tagline |
| Content slide | 1 heading + 4-6 bullet points OR 1 heading + 2 paragraphs |
| Feature grid | 1 heading + 6 cards maximum (2x3 or 3x2) |
| Code slide | 1 heading + 8-10 lines of code |
| Quote slide | 1 quote (max 3 lines) + attribution |
| Image slide | 1 heading + 1 image (max 60vh height) |

**Content exceeds limits? Split into multiple slides. Never cram, never scroll.**

---

## Phase 0: Detect Mode

Determine what the user wants:

- **Mode A: New Presentation** — Create from scratch. Go to Phase 1.
- **Mode B: PPT Conversion** — Convert a .pptx file. Go to Phase 4.
- **Mode C: Enhancement** — Improve an existing HTML presentation. Read it, understand it, enhance. **Follow Mode C modification rules below.**

### Mode C: Modification Rules

When enhancing existing presentations, viewport fitting is the biggest risk:

1. **Before adding content:** Count existing elements, check against density limits
2. **Adding images:** Must have `max-height: min(50vh, 400px)`. If slide already has max content, split into two slides
3. **Adding text:** Max 4-6 bullets per slide. Exceeds limits? Split into continuation slides
4. **After ANY modification, verify:** `.slide` has `overflow: hidden`, new elements use `clamp()`, images have viewport-relative max-height, content fits at 1280x720
5. **Proactively reorganize:** If modifications will cause overflow, automatically split content and inform the user. Don't wait to be asked

**When adding images to existing slides:** Move image to new slide or reduce other content first. Never add images without checking if existing content already fills the viewport.

---

## Phase 1: Content Discovery (New Presentations)

**Ask ALL questions in a single AskUserQuestion call** so the user fills everything out at once:

**Question 1 — Purpose** (header: "Purpose"):
What is this presentation for? Options: Pitch deck / Teaching-Tutorial / Conference talk / Internal presentation

**Question 2 — Length** (header: "Length"):
Approximately how many slides? Options: Short 5-10 / Medium 10-20 / Long 20+

**Question 3 — Content** (header: "Content"):
Do you have content ready? Options: All content ready / Rough notes / Topic only

**Question 4 — Pro Features** (header: "Features", multiSelect: true):
Which enhanced features do you want? Options:
- "Fragment animations (Recommended)" — Step-by-step content reveal with Space key, controls pacing
- "Settings panel" — Gear icon with scene presets (Present/Edit/Minimal), feature toggles, page management (reorder/edit/delete slides), theme & transition selectors
- "Drawing mode" — Pen annotation overlay (D key), 5 colors, 3 brush sizes
- "Data chart animations" — Counter animations for numbers, bar chart growth effects

**Remember the user's feature choices — they determine what code is included in Phase 3.**

If user has content, ask them to share it.

### Step 1.2: Image Evaluation (if images provided)

If user selected "No images" → skip to Phase 2.

If user provides an image folder:
1. **Scan** — List all image files (.png, .jpg, .svg, .webp, etc.)
2. **View each image** — Use the Read tool (Claude is multimodal)
3. **Evaluate** — For each: what it shows, USABLE or NOT USABLE (with reason), what concept it represents, dominant colors
4. **Co-design the outline** — Curated images inform slide structure alongside text. This is NOT "plan slides then add images" — design around both from the start (e.g., 3 screenshots → 3 feature slides, 1 logo → title/closing slide)
5. **Confirm via AskUserQuestion** (header: "Outline"): "Does this slide outline and image selection look right?" Options: Looks good / Adjust images / Adjust outline

**Logo in previews:** If a usable logo was identified, embed it (base64) into each style preview in Phase 2 — the user sees their brand styled three different ways.

---

## Phase 2: Style Discovery

**This is the "show, don't tell" phase.** Most people can't articulate design preferences in words.

### Step 2.0: Style Path

Ask how they want to choose (header: "Style"):
- "Show me options" (recommended) — Generate 3 previews based on mood
- "I know what I want" — Pick from preset list directly

**If direct selection:** Show preset picker and skip to Phase 3. Available presets are defined in [STYLE_PRESETS.md](STYLE_PRESETS.md).

### Step 2.1: Mood Selection (Guided Discovery)

Ask (header: "Vibe", multiSelect: true, max 2):
What feeling should the audience have? Options:
- Impressed/Confident — Professional, trustworthy
- Excited/Energized — Innovative, bold
- Calm/Focused — Clear, thoughtful
- Inspired/Moved — Emotional, memorable

### Step 2.2: Generate 3 Style Previews

Based on mood, generate 3 distinct single-slide HTML previews showing typography, colors, animation, and overall aesthetic. Read [STYLE_PRESETS.md](STYLE_PRESETS.md) for available presets and their specifications.

| Mood | Suggested Presets |
|------|-------------------|
| Impressed/Confident | Bold Signal, Electric Studio, Dark Botanical |
| Excited/Energized | Creative Voltage, Neon Cyber, Split Pastel |
| Calm/Focused | Notebook Tabs, Paper & Ink, Swiss Modern |
| Inspired/Moved | Dark Botanical, Vintage Editorial, Pastel Geometry |

Save previews to `.claude-design/slide-previews/` (style-a.html, style-b.html, style-c.html). Each should be self-contained, ~50-100 lines, showing one animated title slide.

Open each preview automatically for the user.

### Step 2.3: User Picks

Ask (header: "Style"):
Which style preview do you prefer? Options: Style A: [Name] / Style B: [Name] / Style C: [Name] / Mix elements

If "Mix elements", ask for specifics.

---

## Phase 3: Generate Presentation

Generate the full presentation using content from Phase 1 (text, or text + curated images) and style from Phase 2.

If images were provided, the slide outline already incorporates them from Step 1.2. If not, CSS-generated visuals (gradients, shapes, patterns) provide visual interest — this is a fully supported first-class path.

**Before generating, read these supporting files:**
- [html-template.md](html-template.md) — HTML architecture, JS features, inline editing
- [pro-fronted-skill.md](pro-fronted-skill.md) — Fragments, transitions, themes, settings panel, data animations, drawing, PDF (read if user selected any Pro Features)
- [viewport-base.css](viewport-base.css) — Mandatory CSS (include in full)
- [animation-patterns.md](animation-patterns.md) — Animation reference for the chosen feeling

**Key requirements:**
- Single self-contained HTML file, all CSS/JS inline
- Include the FULL contents of viewport-base.css in the `<style>` block
- Use fonts from Fontshare or Google Fonts — never system fonts
- Add detailed comments explaining each section
- Every section needs a clear `/* === SECTION NAME === */` comment block
- **Page transitions**: Use absolute-positioned slides with JS-controlled transitions (fade/slide/zoom/flip), NOT scroll-snap. See html-template.md "Page Transition System" section.
- **If user selected Pro Features in Phase 1**, include the corresponding code from html-template.md:
  - Fragment animations → Fragment System section
  - Settings panel → Settings Panel section (includes scene presets, feature toggles, page management with drag reorder, appearance tab with theme/transition selectors)
  - Drawing mode → Drawing Canvas section
  - Data chart animations → Data Animation section
- **Always include**: page number display, progress bar, keyboard help panel (press ?), fullscreen (F key), blackout (B key), theme switching (T key + 4 themes: dark/light/warm/ocean), print/PDF support with `print-color-adjust: exact`

---

## Phase 4: PPT Conversion

When converting PowerPoint files:

1. **Extract content** — Run `python scripts/extract-pptx.py <input.pptx> <output_dir>` (install python-pptx if needed: `pip install python-pptx`)
2. **Confirm with user** — Present extracted slide titles, content summaries, and image counts
3. **Style selection** — Proceed to Phase 2 for style discovery
4. **Generate HTML** — Convert to chosen style, preserving all text, images (from assets/), slide order, and speaker notes (as HTML comments)

---

## Visual Impact Rules (Learned from Real Projects)

These rules are **mandatory** — every presentation must follow them. They come from real user feedback on what makes slides "抓人眼球" (eye-catching) vs "没人看" (ignored).

### Rule 1: No Pure-Text Slides

Every slide MUST have at least one visual element. Text alone = death in social media feeds.

| Visual Element | When to Use |
|----------------|-------------|
| Data card with large number + counter animation | Any slide with a key metric (%, ×, ¥, count) |
| Person avatar (real photo) | Cover slide, source attribution, quote attribution |
| SVG concept diagram | Any slide explaining an abstract concept (vs, flow, evolution) |
| Terminal/code mockup (CSS-drawn) | Developer tool demos, CLI references |
| Timeline / flow diagram | Sequential processes, historical analogies, cause→effect |
| Icon + label grid | Lists of features, tips, or action items |

### Rule 2: Data = Big Numbers + Animation

Key numbers must be:
- **Large**: `font-size: clamp(2.5rem, 5.5vw, 4.5rem)` minimum for stat numbers
- **Colored**: Use accent colors with `text-shadow` glow
- **Animated**: Counter animation (JS `requestAnimationFrame`, ease-out cubic, ~1.2s duration)
- **Card-wrapped**: `stat-card` with subtle border, glow pulse on entrance

Never bury numbers in body text paragraphs.

### Rule 3: Diagrams Must Be Large

SVG/diagram minimum sizes:
- In two-column layouts: `max-height: min(40vh, 320px)` (NOT 28vh/220px — too small)
- Full-width diagrams: `max-height: min(50vh, 400px)`
- Avatars: minimum `clamp(56px, 7vw, 84px)` — faces need to be recognizable
- Terminal mockups: `min-height: 160px`, generous padding

If a diagram is too small to read at arm's length on a laptop, it's too small.

### Rule 4: Timelines — Max 4 Nodes Per Row

Timeline/flow components with more than 4 nodes MUST split into multiple rows:
- Use a `flow-rows` container with `flex-direction: column`
- Each row gets its own `flow-row` with `flex-wrap: wrap`
- Add `flow-row-label` headers to distinguish rows (e.g., "Facebook 案例" / "Claude Code 案例")
- NEVER use `flex-wrap: nowrap` on timelines — they will overflow on narrow screens
- NEVER use negative margins to overlap nodes — text will collide

### Rule 5: Cover Slide Must Have Three Elements

The cover slide determines whether someone stops scrolling:
1. **Real human faces** — Avatar photos of speaker/author (builds trust and credibility)
2. **One punchy headline** — Short, provocative question or statement with gradient text
3. **Ambient motion** — Floating particles, gradient pulse, or subtle background animation

### Rule 6: Quote Blocks Need Decorative Marks

Plain `border-left` quote blocks look generic. Add:
```css
.quote-block::before {
    content: '\201C'; /* decorative " mark */
    position: absolute;
    font-size: clamp(2.5rem, 5vw, 4rem);
    color: var(--accent);
    opacity: 0.12;
}
```
This creates a magazine-style watermark effect that elevates every quote.

### Rule 7: Abstract Concepts → SVG Diagrams

Before writing text to explain a concept, ask: "Can this be drawn?"

| Concept Type | Diagram Style |
|-------------|---------------|
| A vs B comparison | Split layout: red ✗ top, green ✓ bottom |
| Two forces converging | Two curved lines crossing, intersection highlighted |
| Growth / exponential | Curve with "now" and "future" markers |
| Evolution / transformation | Icons → arrow → highlighted target |
| Sequential process | Flow nodes with arrows, max 4 per row |

Draw with inline SVG. No external images needed.

### Rule 8: Terminal Mockups Should Type

Static terminal text feels dead. Add typing animation:
- Question types at ~80ms per character
- Answer types at ~60ms per character
- Blinking cursor shows at the end
- Trigger only when slide enters viewport (IntersectionObserver)

### Rule 9: Always Include Inline Editing by Default

Unless explicitly told not to, every presentation should include:
- Edit toggle button (fixed, top-left, press E to toggle)
- contenteditable on all text elements
- Auto-save to localStorage on every edit
- This dramatically reduces the cost of post-generation text tweaks

### Rule 10: Ending Slide Needs a CTA

Don't end with just a quote. Add a clear next action:
- "Go listen to this podcast →" button linking to source
- "Follow me on [platform]" for social content
- The CTA should be a styled button (`cta-btn`), not a plain link

---

## Phase 5: Delivery

1. **Clean up** — Delete `.claude-design/slide-previews/` if it exists
2. **Open** — Use `open [filename].html` to launch in browser
3. **Summarize** — Tell the user:
   - File location, style name, slide count
   - Navigation: Arrow keys / Space (next fragment or slide) / swipe
   - Key shortcuts: T (theme) · O (overview) · D (draw) · B (blackout) · F (fullscreen) · P (PDF) · E (edit) · ? (help)
   - Settings panel: click gear icon (top-left) for scene presets, feature toggles, page management
   - How to customize: `:root` CSS variables for colors, font link for typography
   - Print/PDF: press P, backgrounds will print correctly

---

## Phase 6: Share & Deploy (Optional)

After delivery, ask the user: "要分享这个演示文稿吗？可以一键部署到公网链接（手机电脑都能看），或导出为 PDF。"

Options:
- **Deploy to URL** — 部署到 Vercel，生成公网链接
- **Export to PDF** — 导出静态 PDF
- **Both**
- **No thanks**

### 6A: Deploy to Vercel

Run `bash scripts/deploy.sh <path-to-presentation>`. The script:
- Supports single HTML file or folder
- Auto-detects and bundles local images/assets referenced in HTML
- Handles Vercel CLI install and login guidance
- Generates a permanent public URL

**Gotchas:**
- Local images via CSS `background-image` may not be auto-detected — prefer folder deployments when assets are involved
- Redeploying updates the same URL, no need to share a new link

### 6B: Export to PDF

Run `bash scripts/export-pdf.sh <path-to-html> [output.pdf]`. The script:
- Uses Playwright to screenshot each slide at 1920x1080
- Combines into a single PDF
- `--compact` flag renders at 1280x720 for smaller file size

**Gotchas:**
- First run is slow (~30-60s) as it installs Playwright + Chromium
- Animations are not preserved — PDF is a static snapshot
- Slides must use `class="slide"` for detection

---

## Supporting Files

| File | Purpose | When to Read |
|------|---------|-------------|
| [STYLE_PRESETS.md](STYLE_PRESETS.md) | 12 curated visual presets with colors, fonts, and signature elements | Phase 2 (style selection) |
| [viewport-base.css](viewport-base.css) | Mandatory responsive CSS — copy into every presentation | Phase 3 (generation) |
| [html-template.md](html-template.md) | HTML structure, JS controller, inline editing, image pipeline | Phase 3 (generation) |
| [pro-fronted-skill.md](pro-fronted-skill.md) | Fragments, page transitions, themes, settings panel, data animations, drawing, PDF export | Phase 3 (if user selected Pro Features) |
| [animation-patterns.md](animation-patterns.md) | CSS/JS animation snippets, effect-to-feeling guide | Phase 3 (generation) |
| [scripts/extract-pptx.py](scripts/extract-pptx.py) | Python script for PPT content extraction | Phase 4 (conversion) |
| [scripts/deploy.sh](scripts/deploy.sh) | Deploy slides to Vercel for instant sharing | Phase 6 (sharing) |
| [scripts/export-pdf.sh](scripts/export-pdf.sh) | Export slides to PDF via Playwright screenshots | Phase 6 (sharing) |
