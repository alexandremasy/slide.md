# Contributing to slide.md

Thanks for your interest. slide.md is in early alpha — contributions are welcome, but expect the format to change.

## What we're looking for

- **Format feedback** — is FORMAT.md clear and unambiguous? Can you implement a `slide.md` file without questions?
- **New templates** — missing a standard layout? Open an issue with the slot anatomy and ASCII layout
- **Examples** — real-world `slide.md` implementations using different typefaces and color systems
- **Edge cases** — situations the format doesn't handle well

## What we're not looking for (yet)

- CLI tooling — planned, not open for contribution in 0.1
- Narrative/storytelling guidance — out of scope by design
- Figma plugins — later phase

## How to contribute

1. Open an issue first — describe the problem or proposal
2. Wait for a response before starting work
3. Submit a PR with a clear description of what changed and why

## Format principles (read before proposing changes)

- **Content-agnostic** — the system describes structure, not intent. Never prescribe what to put in a slot.
- **Structural naming** — template and token names describe layout, not narrative role
- **Author owns the story** — the system is infrastructure; tone, voice, and sequence are the author's domain
- **Relative units** — always `pt` or `%`, never `px`, `em`, or `rem`

## License

By contributing, you agree your contributions will be licensed under Apache 2.0.
