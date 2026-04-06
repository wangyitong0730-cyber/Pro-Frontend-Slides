---
name: frontend-slides
description: 创建高冲击力 HTML 演示文稿的 Claude Code skill，支持动画、主题、分步揭示与 PPT 转换。
---

# Pro Frontend Slides

Create presentation-first HTML slide decks that feel more like keynote-grade experiences than generic web pages.

## Use This Skill When

Use this skill when the user wants any of the following:
- create a presentation from scratch
- turn notes or rough content into slides
- convert a `.pptx` into HTML slides
- improve an existing HTML presentation
- add stronger motion, themes, reveal pacing, or presentation controls

This skill is optimized for:
- pitch decks
- technical talks
- demos
- launch presentations
- teaching materials
- data storytelling

## Non-Negotiables

These rules always apply unless the user explicitly asks for something narrower:

1. **Presentation-first output**
   - Generate slides, not generic landing pages.
   - Structure content as a real deck with clear pacing.

2. **Viewport safety**
   - Every slide must fit within the viewport.
   - No scrolling inside slides.
   - If content is too dense, split it into more slides.

3. **Self-contained output**
   - Generate a single HTML file with inline CSS and JS unless the user asks otherwise.

4. **Strong visual hierarchy**
   - Avoid text-heavy slides.
   - Prefer visual structure, large metrics, diagrams, cards, and staged reveals.

5. **Distinctive design**
   - Avoid generic AI-looking slides.
   - Use deliberate typography, color, spacing, and motion choices.

6. **Reference-driven generation**
   - Read supporting files only when needed.
   - Reuse the repository’s existing patterns instead of inventing a separate system.

## Output Contract

The final output should usually be:
- a self-contained HTML presentation
- presentation-ready at typical display sizes
- aligned to the chosen style or preset
- consistent with selected pro features
- printable/exportable when that workflow is requested

## Workflow

### 1. Detect the Mode

Classify the request into one of these modes:

- **Mode A — New Presentation**
  Create a new deck from a topic, notes, or complete content.

- **Mode B — PPT Conversion**
  Convert an existing PowerPoint into HTML slides.

- **Mode C — Enhancement**
  Improve an existing HTML presentation without breaking viewport fit or pacing.

### 2. Gather Inputs

Collect the minimum information needed to proceed.

For new decks, gather:
- presentation purpose
- approximate length
- content readiness
- desired pro features

If images are provided:
- inspect them first
- judge whether they are usable
- let them influence the slide outline

### 3. Choose the Style Path

Use a show-don’t-tell approach when possible.

Two valid paths:
- generate style previews and let the user choose
- let the user pick directly from style presets

The chosen style should shape:
- typography
- palette
- layout rhythm
- motion language
- visual density

### 4. Read Supporting References

Read only the files needed for the requested workflow.

| File | Read it when |
|---|---|
| `viewport-base.css` | Always, before generating slides |
| `html-template.md` | Always, for presentation structure and assembly rules |
| `STYLE_PRESETS.md` | When choosing or applying a visual preset |
| `animation-patterns.md` | When designing motion, transitions, or reveal patterns |
| `pro-frontend-skill.md` | When the user wants advanced presentation systems |

### 5. Generate the Presentation

When generating the final HTML:
- include the full contents of `viewport-base.css`
- use viewport-safe sizing with `clamp()` and height-aware layout decisions
- keep content density realistic for presentation use
- use Google Fonts or Fontshare fonts instead of generic system defaults
- include presentation controls appropriate to the selected feature set

### 6. Apply Mode-Specific Rules

#### Mode A — New Presentation
- turn the topic or notes into a slide outline
- choose a visual system that matches the intended audience feeling
- prioritize pacing and storytelling over dumping information

#### Mode B — PPT Conversion
- preserve slide order and important content
- extract and reuse images when available
- convert the deck into a cleaner HTML presentation system
- keep the result faithful, but not visually trapped by the original PPT styling

#### Mode C — Enhancement
- read the current presentation first
- preserve what already works
- check slide density before adding anything
- split content instead of forcing overflow
- verify all added text, images, and layouts still fit presentation constraints

## Pro Features

Offer or include these when they improve the user’s workflow:
- fragment reveals
- settings panel
- drawing mode
- data animations
- live theme switching
- page transitions
- presentation controls such as progress, page number, fullscreen, help, and blackout

Do not include every advanced feature by default if the user wants a lighter deck. Match the implementation to the request.

## Visual Heuristics

Keep these rules in mind during generation:
- no pure-text slides when a visual structure would communicate better
- key metrics should be large, prominent, and easy to scan
- diagrams must be large enough to read at presentation distance
- timelines and flows should split before they become cramped
- cover slides should feel intentional, not template-like
- motion should reinforce pacing, not distract from content

## Optional Tooling

The generated HTML should stay lightweight and build-free.

However, some workflows may rely on optional tooling:
- PPT extraction may require Python packages
- image workflows may require Pillow
- PDF export may require browser automation tools
- deployment may require hosting CLIs such as Vercel

Treat these as optional helper workflows, not core output requirements.

## Reference Notes

- `html-template.md` is the base architecture guide.
- `pro-frontend-skill.md` is the advanced feature reference.
- `STYLE_PRESETS.md` is the preset catalog.
- `animation-patterns.md` is the motion cookbook.

If any guidance conflicts, prefer:
1. user request
2. viewport safety
3. presentation clarity
4. repository reference files

## Important Consistency Rule

Inline editing is **opt-in only**.
Only include inline editing UI and logic if the user explicitly wants editing capabilities for the generated deck.
