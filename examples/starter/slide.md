---
version: "0.1"
name: "Starter"
description: "Minimal slide.md example. A starting point for your own system."

canvas:
  ratio: "16:9"
  width-ref: "720pt"

colors:
  bg: "#FFFFFF"
  surface: "#F7F7F7"
  text:
    primary: "#111111"
    secondary: "#6B6B6B"
  accent: "#0052CC"
  border: "#E0E0E0"

typography:
  fonts:
    primary:
      family: "Inter"
      variable: true
  paragraph:
    display:
      font: primary
      size: "44pt"
      weight: 700
      line-height: 1.1
      paragraph-spacing: "0pt"
    heading:
      font: primary
      size: "28pt"
      weight: 600
      line-height: 1.2
      paragraph-spacing: "12pt"
    body:
      font: primary
      size: "16pt"
      weight: 400
      line-height: 1.5
      paragraph-spacing: "8pt"
    caption:
      font: primary
      size: "11pt"
      weight: 400
      line-height: 1.4
      paragraph-spacing: "4pt"
  character:
    bold:
      weight: 700
      note: "inline only — requires variable font"
    italic:
      style: "italic"
      note: "inline only — requires variable font"

spacing:
  base: "8pt"
  slide-margin: "36pt"
  slot-gap: "24pt"

shapes:
  sm: "4pt"
  md: "8pt"

components:
  list-item:
    marker: "•"
    text: "{typography.paragraph.body}"
    gap: "6pt"
    item-gap: "4pt"
  stat-unit:
    value: "{typography.paragraph.display}"
    label: "{typography.paragraph.caption}"
    context: "{typography.paragraph.caption}"
    context-color: "{colors.text.secondary}"

templates:
  hero:
    figma: "Template/Hero"
    slots:
      title: { required: true, accepts: text }
      subtitle: { required: false, accepts: text }
      visual: { required: true, accepts: visual }
    variants:
      - hero--dark
      - hero--centered
  split-50:
    figma: "Template/Split-50"
    slots:
      left: { required: true, accepts: any }
      right: { required: true, accepts: any }
    variants:
      - split-50--reversed
  blank:
    figma: "Template/Blank"
    slots: {}
---

## Overview

Minimal, neutral starting point. One accent color, high contrast, generous whitespace. Works for any domain.

## Canvas

16:9. Reference width 720pt (10 inches at 72pt/in). All spacing is a multiple of the 8pt base unit. The system scales proportionally at any resolution.

## Colors

Neutral palette. `text.primary` for all primary content; `text.secondary` for captions, labels, and supporting information only. `accent` for emphasis — use sparingly.

## Typography

Inter variable font. Paragraph styles are block-level; character styles (`bold`, `italic`) apply to inline runs and require the variable font's `wght` and `ital` axes.

`paragraph-spacing` adds space after a paragraph break. `line-height` controls rhythm within a paragraph. Both are required for predictable layout.

## Spacing

Base unit: 8pt. Slide margin: 36pt (4.5×). Slot gap: 24pt (3×). All spacing derives from the base unit.

## Components

#### list-item

Single bulleted item. Stack vertically using auto-layout; `item-gap` controls space between items.

| Property | Value |
|---|---|
| marker | • at `{colors.text.secondary}` |
| text | `{typography.paragraph.body}` |
| gap (marker→text) | 6pt |
| item-gap | 4pt |

#### stat-unit

Single data point with value, label, and optional context line.

| Property | Token |
|---|---|
| value | `{typography.paragraph.display}` |
| label | `{typography.paragraph.caption}` |
| context | `{typography.paragraph.caption}` at `{colors.text.secondary}` |

## Templates

#### hero

Dominant visual with title overlay or adjacent text.

**Slots**

| Slot | Required | Accepts |
|---|---|---|
| title | yes | text — `{typography.paragraph.display}` |
| subtitle | no | text — `{typography.paragraph.body}` |
| visual | yes | visual |

**Layout**

```
┌──────────────────────────────┐
│  [title]                     │
│  [subtitle — optional]       │
│                              │
│  ░░░░░░░░ visual ░░░░░░░░░  │
└──────────────────────────────┘
```

**Variants**
- `hero--dark`: inverted — light text on dark background
- `hero--centered`: title and subtitle centered over visual

---

#### split-50

Two equal columns, each accepts any content type.

**Slots**

| Slot | Required | Accepts |
|---|---|---|
| left | yes | any |
| right | yes | any |

**Layout**

```
┌──────────────┬───────────────┐
│              │               │
│   [left]     │   [right]     │
│              │               │
└──────────────┴───────────────┘
```

**Variants**
- `split-50--reversed`: swaps visual weight

---

#### blank

Empty canvas. Tokens apply (background, margins) — no predefined slots.

```
┌──────────────────────────────┐
│                              │
│                              │
│                              │
└──────────────────────────────┘
```

## Do's and Don'ts

- Do: use `caption` for source attribution and footnotes
- Do: use `blank` when the layout doesn't fit any template — better than forcing the wrong template
- Don't: use `display` outside of `hero` or `stat-unit`
- Don't: mix more than two paragraph styles in a single slot
