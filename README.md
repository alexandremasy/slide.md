# slide.md

A plain-text format for slide design systems.

`slide.md` gives authors, tools, and AI agents a shared, structured understanding of how a presentation is built — layouts, tokens, and components — without prescribing content or narrative.

Inspired by [design.md](https://github.com/google-labs-code/design.md). Same philosophy, different canvas.

---

## What it is

A `slide.md` file combines two layers:

- **YAML front matter** — machine-readable tokens: colors, typography, spacing, components, and template definitions
- **Markdown body** — human-readable rationale explaining the *why* behind each decision

```yaml
---
version: "0.1"
name: "Acme Deck System"

canvas:
  ratio: "16:9"
  width-ref: "720pt"

colors:
  accent: "#0052CC"

typography:
  font-family: "Inter"
  variable: true
  paragraph:
    heading:
      size: "28pt"
      weight: 600
      line-height: 1.2
      paragraph-spacing: "12pt"

templates:
  split-50:
    figma: "Template/Split-50"
    slots:
      left: { required: true, accepts: any }
      right: { required: true, accepts: any }
---

## Overview

Clean system for internal decks. One accent, high whitespace.

## Templates

#### split-50

Two equal columns. Each accepts any content type.

    ┌──────────────┬───────────────┐
    │   [left]     │   [right]     │
    └──────────────┴───────────────┘
```

---

## Why

Slides are built from the same structural patterns repeated across decks. A shared vocabulary — `split-50`, `hero`, `stats-3` — eliminates ambiguity between authors, designers, and AI agents.

`slide.md` is the vocabulary layer. Content and narrative belong to the author.

---

## Format specification

→ [FORMAT.md](./FORMAT.md)

---

## Template catalog

→ [templates/](./templates/)

Standard templates: `hero` · `title-content` · `split-50` · `split-70-30` · `full-bleed` · `quote` · `stats-1..4` · `list-3..5` · `comparison` · `divider` · `blank`

---

## Examples

→ [examples/](./examples/)

---

## Status

**0.1-alpha** — format under active development. Breaking changes expected.

---

## Contributing

→ [CONTRIBUTING.md](./CONTRIBUTING.md)

## License

Apache 2.0 — see [LICENSE](./LICENSE)
