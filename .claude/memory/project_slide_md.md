---
name: slide.md project
description: New open-source product — slide design system in .md format, modeled on design.md + Untitled UI
type: project
---

Open-source slide design system. Content-agnostic: describes structure and anatomy only — no tone & voice, no narrative intent.

**Core philosophy:** the system defines structure (slots, constraints, ASCII layout, Figma mapping), not intent. Author brings the meaning. Same model as design.md (Google Labs).

**Naming convention:** structural, not semantic — `split-50`, `full-bleed`, `list-3`, not "hero-opener" or "proof-point".

**Responsibilities:**
- Alex: visual identity, Figma component library (the "sexy" layer — Untitled UI quality standard)
- Atelier: format spec (FORMAT.md), naming convention (NAMING.md), GitHub repo, coordination
- Forge: catalogue documentation (11 tickets, Backlog, blocked until format spec + Figma done)

**Linear project:** [slide.md](https://linear.app/alexandremasy/project/slidemd-4fffaa845525) | ID: `2e6f4c6f-642f-48cb-99d7-d1fece8dbda1`

**Ticket map:**
- ALE-365: Format spec (Atelier)
- ALE-366: Token system (Alex)
- ALE-367: Naming convention (Atelier)
- ALE-368: GitHub repo (Atelier)
- ALE-369: Figma component library (Alex) — blocked by ALE-366+367
- ALE-370–380: Catalogue templates (Forge) — blocked by ALE-365+369

**Website:** deferred — launch only when the system carries daily value.

**Why:** Alex spends significant time on slide layout and reuses the same structures. A shared vocabulary (template names) eliminates the PPT→image→Figma translation layer in our collaboration.

**How to apply:** when working on slide decks with Alex, reference template names from this system once FORMAT.md + NAMING.md are established.
