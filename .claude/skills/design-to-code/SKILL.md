---
name: design-to-code
description: >
  Use this skill whenever the task is to translate a TokScript V3 design (a Figma frame
  pulled via Figma MCP, a screenshot, or a described layout) into production front-end
  code. Triggers: "build this section", "turn this Figma into code", "implement this
  screen", any task that starts from a visual and ends in a component or page section.
  This skill defines HOW the translation is done so output always conforms to the
  CLAUDE.md constitution. It is the front of the per-section loop; design-review is the
  gate at the end. Read this and the relevant CLAUDE.md sections before writing any
  design-derived code.
---

# Skill: Design-to-Code Translation

## Purpose
Convert design intent into production code **deterministically**, so the same Figma
section yields the same code structure every time. This skill is the operational layer
beneath the Figma-to-developer handoff that CLAUDE.md replaces. Consistency is the product.

## Required reading order (before writing code)
1. `CLAUDE.md` section 2 (color), section 3 (spacing/radius/type/surfaces).
2. `CLAUDE.md` section 4 (responsiveness) and section 5 (component standards).
3. `CLAUDE.md` section 6 (product domain rules) — always, for any TokScript UI.
4. This skill.

## Mode check first
Confirm you are in **Mode B — Production** (CLAUDE.md section 7): one section at a time.
Whole-page generation belongs to Mode A (exploration, in Figma) and is not production
code. If asked to generate an entire page at once in production, stop and build the
first section only.

## Step-by-step procedure

### Step 1 — Pull and decompose the design
Pull the target section from Figma via **Figma MCP**. Then, before any code, state:
- The **layout primitive** (stack, grid, split, centered card, etc.)
- Every **distinct component** in the section, top to bottom
- The **one primary action** (there must be exactly one — CLAUDE.md section 5)
- The **token mapping**: which section 2-3 token maps to each colour/space/radius/type
- Which **surface** this section sits on, and therefore which alternate background and
  which card/input surfaces are valid above it (elevation ladder, section 3)

If a colour in the design is not a token, do NOT invent one — snap to the nearest
sanctioned token and flag the mismatch in the report. Never hardcode a hex in JSX.

### Step 2 — Build the skeleton
Responsive container + layout first, **mobile-first, verified at 360px**. No content
yet — just structure with correct section-3 spacing and the right surface tokens.
Apply the section-4 reflow plan now (how this section collapses at `md` and base).

### Step 3 — Fill components
Build each component using the **component-builder** Skill. Wire the **primary action
last** so it is never accidentally subordinate to a secondary control. Use the correct
primary style for the surface (white app-primary for app/auth; teal brand-primary for
marketing — section 5).

### Step 4 — States
Add every interactive state: hover-darken, focus-visible teal ring, active, disabled,
loading where async (CLAUDE.md section 5). A section with only default states is
incomplete by definition.

### Step 5 — Gate
Run the **design-review** Skill against the section. Do not report done until every
check is PASS.

## Mapping rules (design -> code)

| In the design you see... | Produce... |
|--------------------------|------------|
| Near-black page canvas | `--bg` #0D0D0D (or `--bg-alt` #0A0A0A if it alternates) — never raw black |
| A raised card/tile | `--surface` #1C1C1C + `--border` #2E2E30 + `--r-lg`/`--r-xl` |
| An input / search bar | `--surface-input` #2D2D2D + `--border-strong` #3C3C3C |
| A bright teal button | brand-primary `--accent` #00B8B2; only ONE primary per section |
| A white solid button (app/auth) | app-primary #FFFFFF, dark text |
| A quieter outline button | secondary/outline (transparent + `--border-strong`) |
| A fully-rounded tag/label | Pill/Badge, `--r-full`, Caption type |
| A TikTok video grouping | label **Collection** (section 6) — never "Playlist" |
| A YouTube video grouping | label **Playlist** (correct for YouTube only) |
| Export format options | TXT / XML / JSON only — never PDF |
| Fixed-looking widths | fluid responsive layout; widths are constraints, not literals |
| Multiple platform chips together | teal/primary leads; red/orange only on their own elements |

## Output contract (report every time)
- Components built
- Token mapping applied (list colour/space/radius/type tokens used)
- Surface + elevation decision (which background, which card/input surfaces)
- Responsive plan applied (what reflows at `md` and base)
- Domain rules honored (e.g. "TikTok grouping labeled Collection"; "exports TXT/XML/JSON")
- Any deviation from the design + justification
- The Conventional Commit message used

## Anti-patterns (automatic fail at the gate)
- Hardcoded hex in JSX
- Arbitrary spacing outside the section-3 scale
- More than one primary action on a section
- Missing focus-visible state
- Introducing a secondary accent or gradient (there is none — section 2)
- A raised element darker than its section (inverted elevation)
- "Playlist" on a TikTok surface, or "PDF" in exports
- Generating a whole page at once in production (that is Mode A only)
