# slide.md — Format Specification

`slide.md` is a plain-text format for defining a slide design system. It gives authors, tools, and AI agents a shared, structured understanding of how a presentation is built — layouts, tokens, and components — without prescribing content or narrative.

**Version:** 0.1-alpha

---

## File Structure

A `slide.md` file has two layers:

```
---
[YAML front matter — machine-readable tokens]
---

[Markdown body — human-readable rationale, in prescribed section order]
```

The YAML layer is the source of truth for tooling. The Markdown layer explains the *why* behind decisions. Both are required.

---

## YAML Front Matter

### Schema

```yaml
---
version: "0.1"
name: <string>             # required
description: <string>      # optional

canvas:
  ratio: <string>          # required — e.g. "16:9", "4:3"
  width-ref: <Dimension>   # required — reference width in pt, e.g. "720pt"

colors:
  <token-name>: <Color>

typography:
  fonts:
    <font-name>:           # e.g. primary, secondary, mono
      family: <string>     # required — typeface name
      variable: <boolean>  # required — true if variable font
  paragraph:
    <scale-name>:
      font: <font-name>    # required — references a key under typography.fonts
      size: <Dimension>
      weight: <number>
      line-height: <number>
      paragraph-spacing: <Dimension>
  character:
    <style-name>: <TypographyCharacter>

spacing:
  base: <Dimension>        # base unit, all other spacing derives from this
  slide-margin: <Dimension>
  slot-gap: <Dimension>

shapes:
  <scale-name>: <Dimension>

components:
  <component-name>:
    <property>: <value | token reference>

templates:
  <template-name>:
    figma: <string>        # exact Figma component name
    slots:
      <slot-name>:
        required: <boolean>
        accepts: <SlotType>
        notes: <string>    # optional
    variants: [<string>]   # optional
---
```

### Token Types

| Type | Format | Example |
|---|---|---|
| Color | Hex sRGB with `#` prefix | `"#1A1C1E"` |
| Dimension | Number + unit (`pt` or `%`) | `"720pt"`, `"5%"` |
| Boolean | `true` / `false` | `true` |
| Reference | `{path.to.token}` | `{colors.accent}` |
| SlotType | `text`, `visual`, `list`, `any` | `"text"` |

### TypographyFont Object

```yaml
family: <string>           # typeface name — must be available in the authoring tool
variable: <boolean>        # true if variable font; determines inline character style support
```

### TypographyParagraph Object

```yaml
font: <font-name>          # key from typography.fonts — e.g. "primary", "secondary"
size: <Dimension>          # in pt
weight: <number>           # 100–900
line-height: <number>      # unitless ratio, e.g. 1.5
paragraph-spacing: <Dimension>  # space after paragraph break, in pt
```

### TypographyCharacter Object

```yaml
weight: <number>           # optional — overrides paragraph weight
style: <string>            # optional — "italic", "normal"
note: <string>             # optional — implementation guidance
```

> Character styles apply inline within a paragraph. Whether they work without breaking the token link depends on the `variable` flag of the font used by that paragraph style.

---

## Markdown Body

Sections must appear in this order. All sections are required unless marked optional.

### 1. Overview

Short description of the design system's visual identity. What kind of presentations this system is built for. Tone is descriptive, not prescriptive.

### 2. Canvas

Documents the aspect ratio choice and reference dimensions. Explains scaling behavior: all dimensions in this system are relative to `canvas.width-ref`.

### 3. Colors

Documents color roles and their intent. Each token name maps to a role, not a brand name. Explains contrast decisions.

### 4. Typography

Documents the typeface choice and why. Covers:
- **Paragraph styles** — block-level text treatment (display, heading, body, caption)
- **Character styles** — inline overrides (bold, italic). Notes variable font requirement.
- **Paragraph rhythm** — line-height vs paragraph-spacing distinction

### 5. Spacing

Documents the base unit and the spacing scale. Explains the relationship between slide-margin, slot-gap, and the base unit.

### 6. Shapes

Optional. Documents border-radius scale if the system uses rounded containers.

### 7. Components

Documents reusable sub-template elements: `list-item`, `stat-unit`, etc. Each component entry describes its slots, token wiring, and any layout constraints.

Format per component:

```markdown
#### component-name

Purpose sentence.

| Property | Token | Notes |
|---|---|---|
| text | {typography.paragraph.body} | |
| color | {colors.text.primary} | |

Layout (if applicable):
[ASCII representation]
```

### 8. Templates

The core of slide.md. Documents each named layout. Format per template:

```markdown
#### template-name

Purpose sentence — what kind of content this layout holds.

figma: `Component/Name`

**Slots**

| Slot | Required | Accepts | Notes |
|---|---|---|---|
| title | yes | text | {typography.paragraph.heading} |
| visual | yes | visual | dominant area |
| caption | no | text | {typography.paragraph.caption} |

**Layout**

\```
┌──────────────────────────────┐
│  [slot-name]                 │
│                              │
│  ░░░░░░ visual ░░░░░░░░░░░  │
└──────────────────────────────┘
\```

**Variants**

- `template-name--variant`: description of what changes
```

### 9. Do's and Don'ts

Optional. Specific usage rules that can't be inferred from the tokens alone.

---

## Conventions

### Units

- All dimensions in `pt` (points) — resolution-independent, native to presentation tools
- Spacing may also use `%` relative to `canvas.width-ref`
- Never use `px`, `em`, or `rem`

### Naming

- kebab-case throughout (tokens, templates, slots, variants)
- Token names describe **role**, not brand: `color/text/primary`, not `blue-500`
- Template names describe **structure**, not intent: `split-50`, not `hero-opening`
- Variants use `--` double-dash suffix: `hero--dark`, `split-50--reversed`

### Token References

Use dot notation: `{colors.accent}`, `{typography.paragraph.body}`.

References resolve at read time. Circular references are invalid.

### ASCII Layout

Use box-drawing characters for layout representations:
- `┌ ┐ └ ┘ │ ─ ┬ ┴ ├ ┤ ┼` for structure
- `░` for visual/image areas
- `[slot-name]` for text slots
- `[slot-name — optional]` for optional slots

### Character Styles & Variable Fonts

Character styles (bold, italic) apply to runs of characters within a paragraph, not to the full text block. They require a variable font to work without breaking token links in Figma.

If the font is **not variable**: inline bold/italic requires a manual weight/style override in the authoring tool. This is an accepted deviation from the token system — document it explicitly.

---

## Example

```yaml
---
version: "0.1"
name: "Acme Deck System"
description: "Presentation design system for Acme internal decks."

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
      family: "Playfair Display"
      variable: false
    secondary:
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
      font: secondary
      size: "16pt"
      weight: 400
      line-height: 1.5
      paragraph-spacing: "8pt"
    caption:
      font: secondary
      size: "11pt"
      weight: 400
      line-height: 1.4
      paragraph-spacing: "4pt"
  character:
    bold:
      weight: 700
      note: "inline only — requires variable font; static font needs a separate text frame"
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
  lg: "16pt"

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
  split-50:
    figma: "Template/Split-50"
    slots:
      left:
        required: true
        accepts: any
      right:
        required: true
        accepts: any
    variants:
      - split-50--reversed
---

## Overview

Clean, structured system for internal presentations. High whitespace, restrained palette, one accent color.

## Canvas

16:9 ratio. Reference width: 720pt (10 inches). All spacing values are derived from the 8pt base unit. The system scales proportionally — a slide displayed at any resolution maintains the same proportions.

## Colors

Neutral background with a single accent for emphasis. Secondary text is used for captions and supporting information only — never for primary content.

## Typography

Inter (variable font). Paragraph styles define block-level treatment. Inline bold and italic are supported via the variable font's weight and style axes.

Paragraph-spacing is distinct from line-height: line-height controls the rhythm within a paragraph, paragraph-spacing adds breath between paragraphs.

## Spacing

Base unit: 8pt. Slide margin: 36pt (4.5 × base). Slot gap: 24pt (3 × base). All spacing in the system is a multiple of the base unit.

## Components

#### list-item

A single bulleted item. Stacked vertically with auto-layout; item-gap controls spacing between items.

| Property | Value | Notes |
|---|---|---|
| marker | • | 6pt, {colors.text.secondary} |
| text | {typography.paragraph.body} | |
| gap | 6pt | between marker and text |
| item-gap | 4pt | between stacked items |

#### stat-unit

A single data point with value, label, and optional context.

| Property | Token | Notes |
|---|---|---|
| value | {typography.paragraph.display} | Large, prominent |
| label | {typography.paragraph.caption} | Below value |
| context | {typography.paragraph.caption} | {colors.text.secondary}, optional |

## Templates

#### split-50

Two equal columns. Each column accepts any content type.

figma: `Template/Split-50`

**Slots**

| Slot | Required | Accepts | Notes |
|---|---|---|---|
| left | yes | any | 50% of available width |
| right | yes | any | 50% of available width |

**Layout**

```
┌──────────────┬───────────────┐
│              │               │
│   [left]     │   [right]     │
│              │               │
└──────────────┴───────────────┘
```

**Variants**

- `split-50--reversed`: swaps visual weight between left and right

## Do's and Don'ts

- Do: use `caption` for source attribution, footnotes, and secondary data
- Don't: mix more than two paragraph styles in a single text slot
- Don't: use `display` outside of `hero` or `stat-unit` contexts
```
