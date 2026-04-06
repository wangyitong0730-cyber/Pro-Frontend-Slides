# Install

This repository is designed to be used as a Claude Code skill.

## What You Need

At minimum, you need:
- Claude Code
- access to this repository and its supporting files

## Recommended Setup

Keep these files together:
- `SKILL.md`
- `STYLE_PRESETS.md`
- `animation-patterns.md`
- `html-template.md`
- `pro-frontend-skill.md`
- `viewport-base.css`
- `scripts/`

The skill relies on these references while generating presentations.

## Installation Notes

1. Place the repository where your Claude Code skill workflow can access it.
2. Make sure `SKILL.md` remains next to its supporting reference files.
3. Invoke the skill for presentation tasks rather than generic page design tasks.

## Optional Tooling

The generated HTML slides themselves are build-free.

However, some helper workflows may require extra tools:
- **PPT conversion**: `python-pptx`
- **Image processing**: Pillow
- **PDF export**: Playwright or browser automation tooling
- **Deployment**: Vercel CLI or another hosting workflow

## Related Reading

- [`USAGE.md`](USAGE.md) — common workflows after setup
- [`../examples/prompts.md`](../examples/prompts.md) — first prompts to try

## Verify Setup

A good first test is to ask Claude for one of these:

```text
Create a 6-slide technical talk in HTML with fragment reveals.
```

```text
Convert this PPTX into an HTML deck with a clean dark theme.
```

If Claude can read the reference files and produce a structured HTML slide deck, the setup is working.
