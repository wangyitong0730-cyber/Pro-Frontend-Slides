# Pro Frontend Slides

![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-Skill-7c3aed)
![HTML Slides](https://img.shields.io/badge/output-HTML%20slides-2563eb)
![PPTX Conversion](https://img.shields.io/badge/support-PPTX%20conversion-0f766e)
![MIT License](https://img.shields.io/badge/license-MIT-16a34a)

A Claude Code skill for creating high-impact HTML slide decks with polished motion, fragments, themes, canvas visuals, and presentation-first storytelling.

Pro Frontend Slides is built for moments when a normal webpage is not enough and a static slide deck is too flat. It helps Claude generate keynote-like HTML presentations that are easier to present, easier to theme, and easier to evolve.

## Why This Skill

Most AI-generated slides fail in the same ways: too much text, weak pacing, generic styling, and layouts that break at presentation size.

This skill is designed to avoid that by enforcing:
- presentation-first composition
- viewport-safe slides
- stronger motion and reveal pacing
- reusable storytelling patterns
- optional pro features for live presenting

## What It Can Do

| Capability | What It Enables |
|---|---|
| **New presentations** | Create a slide deck from a topic, notes, or full content |
| **PPTX conversion** | Convert PowerPoint content into HTML slides |
| **Deck enhancement** | Upgrade an existing HTML presentation |
| **Fragment reveals** | Reveal content step by step with `Space` |
| **Theme switching** | Switch among live themes while presenting |
| **Cinematic transitions** | Use Fade / Slide / Zoom / Flip transitions |
| **Drawing mode** | Annotate slides live with a canvas overlay |
| **Data storytelling** | Animate counters and charts for metric-heavy slides |
| **Export-ready output** | Generate self-contained HTML suitable for sharing and printing |

## Who This Is For

This skill is a good fit if you want Claude to help with:
- pitch decks
- technical talks
- product demos
- launch presentations
- educational explainers
- data storytelling slides
- keynote-style HTML decks

## What Makes It Different

The upstream `frontend-design` workflow is great for beautiful interfaces and pages.

**Pro Frontend Slides** is optimized specifically for presentation work:
- stronger pacing
- staged reveals
- presentation-oriented composition
- live theme switching
- better support for demos, talks, and storytelling decks
- stricter viewport rules to avoid broken slides

## Install

1. Put this repository in your Claude Code skills directory.
2. Make sure Claude Code can access the skill files.
3. Invoke the skill when you want HTML presentations instead of generic webpages.

If you are packaging or sharing it with others, keep the supporting files next to `SKILL.md` so the skill can read its references.

## Quick Start

Use prompts like these:

```text
Create a 10-slide product launch presentation with bold motion and fragment reveals.
```

```text
Convert this PPTX into HTML slides with a dark cinematic theme.
```

```text
Improve this existing slide deck and make it feel more keynote-like.
```

## Try These First

If you want the fastest way to feel what this skill is good at, start with one of these:

- **Launch deck** — create a bold product presentation with fragments and transitions
- **PPTX upgrade** — convert an existing PowerPoint into a cleaner HTML deck
- **Deck polish** — take an existing HTML deck and improve pacing, hierarchy, and visuals

Suggested first prompts:

```text
Create a product launch deck with high-energy visuals and cinematic transitions.
```

```text
Convert this PPTX into keynote-like HTML slides.
```

```text
Improve this existing deck and reduce text density while making it more presentation-ready.
```

## Example Prompts

More ready-to-copy prompts and showcase material are available here:
- [`examples/prompts.md`](examples/prompts.md)
- [`examples/before-after.md`](examples/before-after.md)
- [`examples/showcase.md`](examples/showcase.md)

## How It Works

At a high level, the skill will:
1. detect whether the task is new creation, PPT conversion, or deck enhancement
2. gather content and presentation goals
3. choose a style path or show style options
4. read the necessary reference files
5. generate a self-contained HTML presentation
6. optionally support export, deployment, or enhancement workflows

## Repository Structure

| File | Purpose |
|---|---|
| `SKILL.md` | Main skill definition and execution rules |
| `STYLE_PRESETS.md` | Visual style presets and aesthetic directions |
| `animation-patterns.md` | Motion and transition reference |
| `html-template.md` | Base HTML presentation architecture |
| `pro-frontend-skill.md` | Advanced presentation systems and pro features |
| `viewport-base.css` | Mandatory viewport-safe slide foundation |
| `scripts/` | Helper tooling for PPT extraction, export, and deployment |
| `docs/` | Installation and usage guidance |
| `examples/` | Example prompts and usage patterns |

## Requirements and Dependency Boundaries

### Core output
The generated presentation output is designed to be:
- a single self-contained HTML file
- inline CSS/JS
- no build step required to view it in the browser

### Optional tooling
Some helper workflows may require extra tools:
- **PPT extraction** may require Python packages such as `python-pptx`
- **Image processing** may require Pillow
- **PDF export** may require Playwright or other browser tooling
- **Deployment** may require Vercel CLI or other hosting tools

The important distinction is:
- the **generated slides** are lightweight and build-free
- some **helper scripts** are optional and may have dependencies

## Documentation

- [`docs/INSTALL.md`](docs/INSTALL.md) — install notes and setup expectations
- [`docs/USAGE.md`](docs/USAGE.md) — common workflows and when to use each mode
- [`examples/prompts.md`](examples/prompts.md) — example prompts for common scenarios
- [`examples/before-after.md`](examples/before-after.md) — transformation-oriented examples
- [`examples/showcase.md`](examples/showcase.md) — scenario-based showcase of expected outcomes
- [`pro-frontend-skill.md`](pro-frontend-skill.md) — advanced features such as fragments, transitions, themes, controls, and drawing

## GitHub About Suggestion

**Description**

> A Claude Code skill for generating high-impact HTML slide decks with animations, themes, fragments, and presentation-first storytelling.

**Suggested topics**

`claude-code`, `skill`, `slides`, `presentation`, `html-slides`, `animation`, `storytelling`, `pptx`, `frontend`

## FAQ

### Does this generate websites or slide decks?
Primarily slide decks. The output is presentation-first HTML, not a generic landing page.

### Do I need npm to use the generated deck?
No build step is required for the generated HTML itself.

### Can it convert PPTX files?
Yes. The skill supports PPT conversion workflows, with optional helper scripts.

### Can it upgrade an existing deck?
Yes. One of the core modes is enhancing an existing HTML presentation.

## License

MIT
