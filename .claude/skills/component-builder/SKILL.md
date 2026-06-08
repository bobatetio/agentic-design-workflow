---
name: component-builder
description: >
  Use this skill whenever you create or modify a single UI component (button, card,
  input, pill/badge, platform chip, nav item, pricing tier, grid cell, etc.) in the
  TokScript V3 build. Triggers: "build a component", "make a button/card/input", or any
  sub-task spawned by design-to-code that produces one reusable unit. This skill enforces
  the component contract in CLAUDE.md section 5 so every component is typed, accessible,
  fully-stated, responsive, and token-driven. Read before writing any component.
---

# Skill: Component Builder

## Purpose
Guarantee every component meets one consistent bar: typed, accessible, fully-stated,
responsive, token-driven. No "good enough" components. This is the granular layer
called by design-to-code Step 3.

## Required reading
- `CLAUDE.md` section 2-3 (tokens) and section 5 (component standards) — always.
- `CLAUDE.md` section 6 (domain rules) — if the component shows product data.

## The component contract (all mandatory — CLAUDE.md section 5)
1. **One component per file**, `PascalCase` filename matching the export.
2. **Typed props** via an explicit `interface ComponentNameProps`. No inline types,
   no `any` without a justifying comment.
3. **All applicable states**: `default · hover · focus-visible · active · disabled`,
   plus `loading` if the component performs async work.
4. **Keyboard operable**: semantic element first (`button`, `a`, `input`...). `aria-*`
   only when semantics are insufficient.
5. **Token-driven**: colour/spacing/radius/type from CLAUDE.md section 2-3. Zero raw hex.
6. **Responsive**: no fixed container widths; works from 360px up; touch targets >=44px.
7. **Default-safe props**: optional props have sensible defaults; no required prop that
   crashes an empty render.

## Universal interaction rules (CLAUDE.md section 5)
- **Hover darkens** (~8% black step-down). Never lightens.
- **Focus-visible**: 2px teal `#00B8B2` ring at 2px offset. Never removed. (Hard fail if missing.)
- **Active**: scale 0.99, no colour change beyond the hover darken.
- **Disabled**: 40% opacity + `not-allowed` cursor + `aria-disabled`.
- **Loading**: spinner + `aria-busy`, action suppressed, size stable.
- **Transition**: 150ms ease on colour/opacity/transform; no layout shift.

## Procedure
1. Name the component; define its `Props` interface first.
2. Write the default (resting) markup with semantic elements.
3. Layer states in order; never skip `focus-visible`.
4. Apply tokens — map each visual property to a CLAUDE.md section 2-3 token.
5. Verify at 360px before finishing.
6. Hand back to `design-review` for the gate.

## Component-specific specs (from CLAUDE.md section 5)

### Button
Variants and when to use them:
- **Brand primary** — teal `#00B8B2`, near-black text — marketing CTAs.
- **App primary** — white `#FFFFFF`, near-black text — in-app / auth.
- **Secondary / outline** — transparent fill, 1px `--border-strong` #3C3C3C, white text
  — lower-priority actions (Log In, Get Monthly, Get Lifetime).
- **Ghost** — transparent, white text, no border — tertiary / nav actions.
Radius `--r-md` (12px). Icon+label gap 8px. Only ONE primary per screen.

### Card / Panel
Fill `--surface` #1C1C1C, 1px `--border` #2E2E30, radius `--r-lg`/`--r-xl`, padding
24-32. Interactive cards darken on hover. Never darker than the section it sits on.

### Pricing tier card
As Card. Recommended tier: teal border/accent + teal "Recommended" pill + brand-primary
CTA. Other tiers: secondary/outline CTA. Only the recommended tier gets teal.

### Input field
Fill `--surface-input` #2D2D2D, 1px `--border-strong` #3C3C3C, radius `--r-sm`/`--r-md`.
Leading icon + placeholder in `--text-muted` #B6B6B6. Focus-visible teal ring.
Error state: `--youtube` #DA2626 border + message.

### Pill / Badge
Fully rounded `--r-full`, Caption type (12/16, 500). Tags: "AI Powered", "New",
"Recommended", platform-support chips.

### Platform chip
Denotes a platform only: TikTok -> teal #00B8B2, YouTube -> red #DA2626,
Instagram -> orange #F77738. Shown together, teal/primary leads. Never a general accent.

### Reference pattern — primary button spec (not literal code)
