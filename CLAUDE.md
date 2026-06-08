# CLAUDE.md — TokScript V3 Design-to-Code Constitution

> This file governs how AI agents (Claude Code) translate design intent into
> production front-end code for **TokScript V3**. It is the design-to-code layer.
> For all Git, branch, PR, CI, and promotion rules, this file defers to the dev
> team's workflow file at `.claude/git-workflow.md` — that file is the source of
> truth for *where and how* code ships; this file is the source of truth for
> *how a design becomes correct code*. On any conflict over Git matters, the
> git-workflow file wins.

---

## 1. Operating Model & Project Identity

### What this project is
TokScript V3. The build has two distinct surfaces, each a primary tool with a
different face:

1. **The dashboard** — the main application.
2. **The landing pages** — the public-facing marketing surface.

The dashboard was built first (started in Figma Make, then moved into Claude). Both
surfaces are **actively in progress** — the landing pages were prioritised for an
immediate market push following a design-system change, and the dashboard continues
in parallel.

### Where work comes from
Direction originates with **Michael** — founder, project manager, and product owner.
He provides the task and inspiration, sometimes as mockups generated in Claude,
sometimes as recordings of him working through the concept with Claude. The agent
treats Michael's brief as the product requirement and does not invent product
direction.

### The agent's job
Turn design intent into production front-end code that conforms to this constitution,
then hand off via the Bob branch (section 8). The agent executes the design faithfully
and flags ambiguity back to the operator rather than guessing.

### The loop (current workflow — applies to both surfaces)
1. Research / gather inspiration; receive Michael's brief and materials.
2. Design the surface in **Figma**.
3. Connect **Figma MCP -> Claude Code** (Claude Code runs inside **VS Code**, the IDE).
4. Build the front-end in Claude Code, reading the design directly via Figma MCP.
5. Use **standard Claude** separately for copy work.
6. Ship completed front-end to the **dev team**, who own the back end and integration.

### Roles
- **Operator (you):** designer / design-engineer running this workflow.
- **Michael:** founder / PM / product owner — source of direction.
- **Dev team:** owns the back end, final integration, and `.claude/git-workflow.md`.

---

## 2. Design System — Identity & Color

System name: **TokScript V3 Design System** — a dark, high-contrast system on a
near-black canvas with a single teal accent and per-platform colour coding.

### Typeface
**Inter** for all text.

### Core color tokens (the only sanctioned values)
| Token | Value | Use |
|-------|-------|-----|
| `--text` | `#FFFFFF` | Primary text |
| `--text-muted` | `#B6B6B6` | Secondary text, captions, sublabels, placeholders |
| `--accent` | `#00B8B2` | Primary accent (teal) — primary actions, active states, focus ring |

> There is **no secondary accent and no gradient.** The single teal accent carries
> all emphasis.

### Platform colors
Each supported platform has one identity colour, used only to denote that platform
(badges, filters, platform chips) — never as a general UI accent.

| Platform | Token | Value |
|----------|-------|-------|
| TikTok | `--tiktok` | `#00B8B2` (teal — shared with the primary accent) |
| YouTube | `--youtube` | `#DA2626` (red) |
| Instagram | `--instagram` | `#F77738` (orange) |

> Precedence: when multiple platforms appear in one view, the teal primary accent
> takes precedence for shared/primary UI; YouTube red and Instagram orange appear
> only on their own platform-specific elements. TokScript is TikTok-first.

---

## 3. Spacing, Radius, Type & Surfaces

### Spacing scale (8px base + half-steps)
All padding, gaps, and margins snap to this scale — no eyeballed/off-scale values.

`4 / 8 / 12 / 16 / 24 / 32 / 48 / 64 / 96` (px)

- Base unit **8**; use 4 and 12 only for tight/internal spacing.
- Within-card padding: 24-32. Grid gaps: 16-24. Between major sections: 64-96.

### Corner radius
| Token | Value | Use |
|-------|-------|-----|
| `--r-sm` | `8px` | Inputs, small buttons |
| `--r-md` | `12px` | Standard buttons, input bars |
| `--r-lg` | `16px` | Cards, panels |
| `--r-xl` | `24px` | Large feature cards, pricing tiers |
| `--r-full` | `9999px` | Pills, badges, platform chips, avatars |

### Type scale — Inter
| Role | Size / Line | Weight |
|------|-------------|--------|
| Display | 48 / 56 | 700 |
| H1 | 36 / 44 | 700 |
| H2 | 24 / 32 | 600 |
| H3 | 20 / 28 | 600 |
| Body | 16 / 24 | 400 |
| Small | 14 / 20 | 400 |
| Caption | 12 / 16 | 500 |

Body never goes below 16px.

### Surface tokens — layered dark system
The page alternates section backgrounds (no two adjacent sections share one) and
gives nav and footer their own treatment.

| Token | Value | Use |
|-------|-------|-----|
| `--bg` | `#0D0D0D` | Base canvas / section A / nav bar |
| `--bg-alt` | `#0A0A0A` | Section B (alternates with A) |
| `--surface` | `#1C1C1C` | Cards, panels, raised tiles |
| `--surface-input` | `#2D2D2D` | Input fields |
| `--border` | `#2E2E30` | Card hairline borders |
| `--border-strong` | `#3C3C3C` | Input borders |
| `--footer` | `#1A1A1A` | Footer |

> **Elevation ladder (never invert):**
> `--bg-alt` #0A0A0A < `--bg` #0D0D0D < `--footer` #1A1A1A < `--surface` #1C1C1C <
> `--surface-input` #2D2D2D. A raised element must never be darker than the section
> it sits on.
>
> **Section rule:** alternate `--bg` / `--bg-alt` down the page so adjacent sections
> never share a background. Nav uses `--bg`; footer uses `--footer`.

---

## 4. Responsiveness & Breakpoints

Mobile-first. Build the smallest layout first, then enhance upward. Use the standard
Tailwind breakpoint set — no custom widths.

### Breakpoints
| Name | Min width | Target |
|------|-----------|--------|
| Base (mobile) | 0-639px | Phones. Design floor is **360px** — must not break below this. |
| `sm` | 640px | Large phones / small tablets portrait |
| `md` | 768px | Tablets (portrait) |
| `lg` | 1024px | Tablets landscape / small laptops |
| `xl` | 1280px | Desktop |
| `2xl` | 1536px | Large desktop |

The reference design is 1440px (desktop) — treat it as the `xl` layout and scale
down. Never design only for desktop and shrink by eye.

### Standard reflow rules
- **Container:** fluid width with a max-width cap; gutter padding steps up with the
  breakpoint (16 mobile -> 24 tablet -> 32+ desktop, from section 3).
- **Multi-column card rows (e.g. 4-up pricing tiers):** 4 across at `lg`+ -> **2x2 at
  `md`** -> **single stacked column on mobile**.
- **Feature rows (image + text):** side-by-side at `lg`+ -> **stacked (text above
  image) at `md` and below**.
- **Navigation:** full horizontal nav at `lg`+ -> **collapses to hamburger/drawer
  below `lg`**. Primary CTA stays visible; secondary links move into the drawer.
- **Type:** display/H1 step down one level on mobile (e.g. 48 -> 32) so headlines
  don't overflow. Body stays >=16px.
- **Touch targets:** >=44x44px on touch breakpoints.
- **Media:** fluid, never fixed pixel widths; preserve aspect ratio.

### Verification
Every section is checked at **360px, 768px, and 1440px** before it passes. Desktop-
only is not done.

---

## 5. Component Standards

### Universal interaction rules
- **Hover:** every interactive element **darkens** (~8% black overlay / step down in
  lightness). Never lighten on hover.
- **Focus-visible (keyboard):** 2px ring in `--accent` #00B8B2 at 2px offset on every
  focusable control. Never remove focus outlines — mandatory.
- **Active:** subtle press, scale 0.99 (no colour change beyond the hover darken).
- **Disabled:** 40% opacity + `not-allowed` cursor + `aria-disabled`.
- **Loading:** spinner + `aria-busy`, action suppressed, size stable.
- **Transition:** 150ms ease on colour/opacity/transform; no layout-shifting motion.

### Component contract (all components)
- One component per file, `PascalCase`, explicit `interface` props, no `any`.
- All applicable states defined. Missing `focus-visible` = incomplete.
- Tokens only (sections 2-3). Zero raw hex in JSX. Spacing snaps to the section 3 scale.
- Semantic element first; `aria-*` only where semantics are insufficient.
- Mobile-first; works at 360px minimum.

### Buttons
| Variant | Resting | Hover | Use |
|---------|---------|-------|-----|
| Brand primary | teal `#00B8B2`, near-black text | darken teal | Marketing CTAs |
| App primary | white `#FFFFFF`, near-black text | darken to light grey | In-app / auth |
| Secondary / outline | transparent fill, 1px `--border-strong`, white text | darken bg to `--surface` | Lower-priority actions (Log In, Get Monthly, Get Lifetime) |
| Ghost | transparent, white text, no border | darken bg | Tertiary / nav actions |

Radius `--r-md`. **Exactly one primary per screen.** App/auth -> white app-primary;
marketing -> teal brand-primary. Never mix both as primary on one screen. Icon+label
gap 8px.

### Card / Panel
- Fill `--surface` #1C1C1C, 1px `--border` #2E2E30, radius `--r-lg`/`--r-xl`,
  padding 24-32. Interactive cards darken on hover. Never darker than their section.

### Pricing tier card
- As Card. The **recommended tier** gets a teal border/accent + teal "Recommended"
  pill, and its CTA is brand-primary; other tiers use secondary/outline CTAs. Only the
  recommended tier gets the teal treatment.

### Input field
- Fill `--surface-input` #2D2D2D, 1px `--border-strong` #3C3C3C, radius `--r-sm`/`--r-md`.
- Leading icon + placeholder in `--text-muted` #B6B6B6.
- Focus-visible: teal ring. Error: `--youtube` #DA2626 border + message.

### Pill / Badge
- Fully rounded `--r-full`, Caption type. Tags like "AI Powered", "New",
  "Recommended", platform-support chips.

### Platform chip
- Denotes a platform only: TikTok -> teal, YouTube -> red, Instagram -> orange. When
  shown together, teal/primary leads (section 2). Never a general UI accent.

### Nav bar
- Background `--bg` #0D0D0D, flush with hero, hairline bottom `--border`. Logo left;
  links center/left; language + Log In (ghost/secondary) + Get Started (brand primary)
  right. Collapses to drawer below `lg` (section 4).

### Footer
- Background `--footer` #1A1A1A, muted text, clear column grouping, section 3 spacing.

---

## 6. Product Domain Rules (TokScript V3)

Product facts, not style. Violating them produces factually wrong UI.

### Supported platforms
- **Web dashboard:** TikTok, Instagram Reels, YouTube Shorts (all three).
- **Chrome extension:** **TikTok only.** Never surface Instagram/YouTube in extension UI.

### Platform terminology (correct noun per platform)
- **TikTok groupings -> "Collections."** Never "Playlist" for TikTok.
- **YouTube groupings -> "Playlists."** "Playlist" is correct for YouTube only.
- Do not use one term generically. "Collection & Playlist Imports" is correct because
  it names both.

### Limits
- Bulk actions support **up to 50 videos** at once (links or a TikTok Collection).

### Export formats
- **TXT, XML, JSON.** **No PDF.** `[current — may expand]`
- (Note for Michael: the live landing History card still says "TXT, XML, PDF" — should
  be TXT/XML/JSON.)

### TokScript MCP — separate product, same package
- The **MCP product** (TokScript inside Claude & ChatGPT) is a **distinct product**
  from this design-to-code workflow, though shipped within the same overall package.
- Never conflate the two in code, docs, or copy. The MCP has its own surface
  (tokscript.com/mcp); this workflow builds TokScript's own front-end.

---

## 7. The Agent Loop (how a build session runs)

The workflow has two modes. Know which one you are in.

### Mode A — Exploration (upstream, in Figma; whole-page allowed)
Before the design is finalised, a full-page pass is intentional: it reveals the wider
picture — what sections exist, the page's shape and rhythm — and fuels brainstorming.
This happens in Figma, not production. Output is a refined Figma design and an agreed
section list, not production code.

### Mode B — Production (in Claude Code; one section at a time)
Once sections are known and the Figma is refined, build **one section at a time**, in
order (e.g. on a landing page: nav -> hero -> section 1 -> ...; on a dashboard screen:
shell -> nav/sidebar -> primary panel -> ...). Never generate the whole page at once in
production. Each section completes its own loop before the next begins.

### The per-section loop (Mode B)
1. **Read first.** This file (sections 2-6) + any relevant Skill. Pull the live design
   for the target section via **Figma MCP**.
2. **Restate.** Name the section, its components, the one primary action, and the
   exact tokens/rules that apply — before writing code.
3. **Skeleton.** Responsive container + layout, mobile-first, verified at 360px;
   spacing on the section 3 scale.
4. **Fill components.** Use the `component-builder` Skill. Wire the primary action last.
5. **States.** Every interactive state (hover-darken, focus-visible teal ring, active,
   disabled, loading). Default-only = incomplete.
6. **Self-review (GATE).** Run the `design-review` Skill. Do not move on while any
   check fails. The gate is per-section and non-negotiable.
7. **Report.** Built / tokens applied / domain rules honored / deviations + justification.
8. **Continue.** Next section, repeat 1-7. Sections are built and reviewed
   individually but accumulate locally — not pushed one at a time.

### Shipping rhythm (batched)
- Build and self-review several sections locally, then **push them together** to the
  Bob branch (section 8). Commits still follow Conventional Commits, one logical change
  each (e.g. `feat(landing): add hero section`, `feat(dashboard): add collections grid`),
  so history stays clean.
- Keep a batch coherent (e.g. "landing above-the-fold" or "dashboard library view")
  rather than mixing unrelated work.

### Tooling
Figma -> **Figma MCP** -> **Claude Code** (in **VS Code**). **Standard Claude** is used
separately for copy, not production code.

---

## 8. Git Handoff — defers to the dev team's workflow

All Git/branch/PR/CI/promotion rules live in **`.claude/git-workflow.md`** (source of
truth for *where and how* code ships). This constitution governs *how a design becomes
correct code*, not Git policy.

### The handoff boundary (real practice)
- Work happens on a **personal standing branch ("Bob")**; self-reviewed sections
  accumulate there (section 7 batched rhythm).
- **The operator's responsibility ends at pushing to the Bob branch.** That push *is*
  the handoff. The operator does not open PRs, merge into `development`, or touch
  `staging`/`main`.
- Everything downstream — review, PR into `development`, promotion through `staging`,
  release to `main` — is **owned by the dev team**.

### Why this matters
The agent must never carry code past the Bob branch. Pushing to Bob completes the
design-to-code job. Attempting to PR, merge, or promote crosses into the dev team's
territory and risks protected branches.

### Non-negotiable safety rules (apply on the Bob branch)
- Never commit secrets / `.env` / keys. Check the diff before staging.
- Conventional Commits, one logical change each, clean history.
- Verify commit author identity is the **work account** before the first commit.
- Never force-push in a way that disrupts shared history.
- `development`, `staging`, `main` are PROTECTED and off-limits to this workflow.
  **`main` is human-only, always.**

---

## 9. Definition of Done

A section is done only when ALL of the following are true. "Looks right" is not done;
"matches the system" is done.

- [ ] Tokens from sections 2-3 only. Zero raw hex / off-scale spacing.
- [ ] Correct primary style for context; exactly one primary per section.
- [ ] All interactive states: hover-darken, focus-visible (teal ring), active,
      disabled, loading where async.
- [ ] Responsive: verified at 360px, 768px, 1440px; reflow rules (section 4) applied.
- [ ] Domain rules honored (section 6): platform nouns (TikTok->Collections,
      YouTube->Playlists), extension TikTok-only, formats TXT/XML/JSON (no PDF),
      MCP not conflated.
- [ ] Accessibility: semantic-first, keyboard-operable, focus never removed,
      touch targets >=44px.
- [ ] Passed the `design-review` Skill gate.
- [ ] Conventional Commits, clean history, work-account author.
- [ ] Pushed to the Bob branch (handoff complete — nothing past Bob, section 8).
