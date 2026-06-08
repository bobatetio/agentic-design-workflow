---
name: design-review
description: >
  Use this skill as the mandatory quality GATE before declaring any section/component
  "done" in the TokScript V3 build. Triggers: any time a section, screen, or component
  has been built or modified and is about to be reported complete, committed, or pushed.
  This skill runs the section against the CLAUDE.md constitution (sections 2-9) and the
  Definition of Done. A section that has not passed this gate is not done. Read and run
  this before every commit. If any check fails, fix it before proceeding — do not report
  done, do not push to the Bob branch.
---

# Skill: Design Review (the GATE)

## Purpose
Turn the CLAUDE.md constitution from a document into an enforced standard. Every section
must pass this review before it is considered done. The gate is per-section and
non-negotiable (CLAUDE.md section 7). "Looks right" is not the bar — "matches the
system" is.

## How to run
Go through every check below in order against the section just built. For each, mark
PASS or FAIL. On any FAIL: stop, fix it, re-run from the top. Only when every check
is PASS may the section be reported done and included in a push to the Bob branch.

Report the result as a short checklist (see Output format). Never skip a check, never
mark a check PASS without actually verifying it.

---

## 1. Tokens & values (CLAUDE.md sections 2-3)
- [ ] No raw hex anywhere in JSX/CSS — every colour is a token from section 2-3.
- [ ] No off-scale spacing — every padding/gap/margin is on the `4/8/12/16/24/32/48/64/96` scale.
- [ ] Radius values are from the `--r-*` set only.
- [ ] Type uses the Inter scale roles; body is never below 16px.
- [ ] Surfaces respect the elevation ladder
      (`#0A0A0A < #0D0D0D < #1A1A1A < #1C1C1C < #2D2D2D`); no raised element is darker
      than the section it sits on.
- [ ] Adjacent sections do not share a background (alternate `--bg` / `--bg-alt`).

## 2. Color & platform rules (CLAUDE.md section 2)
- [ ] Teal `#00B8B2` is the only accent; no secondary accent or gradient introduced.
- [ ] Platform colours used only to denote a platform — never as general UI accent.
- [ ] Where platforms appear together, teal/primary takes precedence; YouTube red and
      Instagram orange appear only on their own platform elements.

## 3. Primary action (CLAUDE.md section 5)
- [ ] Exactly ONE primary action on the section — no competing primaries.
- [ ] Correct primary style for context: white app-primary in app/auth surfaces;
      teal brand-primary on marketing surfaces. Not mixed.

## 4. Interactive states (CLAUDE.md section 5)
- [ ] Hover present and **darkens** (never lightens).
- [ ] **Focus-visible** present on every focusable control: 2px teal ring, 2px offset.
      Focus outline is never removed. (Hard fail if missing.)
- [ ] Active (scale 0.99), Disabled (40% opacity + not-allowed + aria-disabled),
      Loading (spinner + aria-busy, size stable) — all present where applicable.
- [ ] Transitions are 150ms ease and do not shift layout.

## 5. Responsiveness (CLAUDE.md section 4)
- [ ] Verified at **360px** — nothing breaks; this is the floor.
- [ ] Verified at **768px** (tablet) — reflow rules applied (e.g. multi-column ->
      2x2; feature rows stacked; nav collapsed below `lg`).
- [ ] Verified at **1440px** (desktop reference).
- [ ] No fixed pixel container widths; media is fluid with preserved aspect ratio.
- [ ] Touch targets >=44x44px on touch breakpoints.

## 6. Product domain rules (CLAUDE.md section 6)
- [ ] Platform nouns correct: TikTok grouping = **Collection**, YouTube grouping =
      **Playlist**. Never "Playlist" for TikTok.
- [ ] If this is an extension surface: TikTok only — no Instagram/YouTube affordances.
- [ ] Export formats shown are **TXT / XML / JSON** — never PDF.
- [ ] Bulk limit references say "up to 50 videos" where relevant.
- [ ] No conflation of the MCP product with this design-to-code build.

## 7. Component contract & accessibility (CLAUDE.md section 5)
- [ ] One component per file; `PascalCase`; explicit `interface` props; no `any`.
- [ ] Semantic element first; `aria-*` only where semantics are insufficient.
- [ ] Fully keyboard-operable; logical tab order.

## 8. Commit readiness (CLAUDE.md sections 8-9)
- [ ] No secrets / `.env` / keys in the diff.
- [ ] Conventional Commit message, one logical change, clean history.
- [ ] Commit author identity is the work account.
- [ ] Ready to push to the **Bob branch** only — no PR, no protected branch, `main`
      never touched.

---

## Output format
Report the gate result like this (only after genuinely checking each):
