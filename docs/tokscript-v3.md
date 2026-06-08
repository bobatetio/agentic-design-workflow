# TokScript V3 — Commercial Application

This document records the live product the agentic design-to-code workflow was built
for and applied to. The methodology in this repository is not theoretical — it is the
process used to design and build TokScript V3 in production.

> Items marked `[CONFIRM]` are pending the operator's verification before this doc is
> treated as final. Everything else is observed from the live product surfaces.

---

## What TokScript is

TokScript is an AI-powered social-commerce tool for creators, marketers, and brands. It
transcribes and analyses short-form video at scale, and turns that into research,
downloads, and AI-ready content.

- **Tagline (live):** "TikTok Transcript Generator — turn speech into text for any
  TikTok, Reels, and Shorts video."
- **Supported platforms:** TikTok, Instagram Reels, YouTube Shorts.
- **Core capability:** bulk transcription and download of up to 50 videos at once, plus
  entire TikTok Collections.
- **Export formats:** TXT, XML, JSON.

### Traction (as shown on the live product)
The live sign-up surface displays: **41K+ users · 2.6M+ transcripts · 4.2★ rating**.
`[CONFIRM exact current figures with Michael before final submission]`

---

## The product surfaces

TokScript V3 spans four distinct surfaces, all designed through this workflow:

1. **Web dashboard** — the main application. Transcript library, Collections/Playlists,
   bulk import, history and bookmarking, HD downloads. Supports all three platforms.
2. **Landing pages** — the public marketing site. Hero, feature sections, social proof,
   pricing. The reference for the design system shown in `CLAUDE.md`.
3. **Chrome extension** — TikTok-only by design. Surfaces transcription directly on the
   platform.
4. **MCP product** — TokScript running *inside* Claude and ChatGPT, so users can pull
   transcripts, download videos, and analyse creator libraries without leaving the
   conversation. Marketing surface at `tokscript.com/mcp`. (A separate product from this
   design-to-code workflow — they share a package, not a purpose.)

---

## Pricing (live)

| Tier | Price | Position |
|------|-------|----------|
| Free | $0 | Basic transcripts, entry tier |
| Monthly | $10/mo | Flexible billing |
| Annual | $39/yr | Recommended tier (teal-highlighted) |
| Lifetime | $199 one-time | Best value |

Every paid tier includes Claude and ChatGPT integration, the Chrome extension, and full
transcript downloads. `[CONFIRM pricing is current]`

---

## The role of this workflow

The design system in `CLAUDE.md` — the `#0D0D0D` canvas, the teal `#00B8B2` accent, the
Inter type scale, the layered surface model, the per-platform colours — is the **live
TokScript V3 system**, sampled directly from the production surfaces. The tokens are not
illustrative; they are what ships.

The workflow was applied as follows:

- **The dashboard** was started in Figma Make, moved into Claude, and built up through
  the agent loop. Actively in progress.
- **The landing pages** were prioritised for an immediate market push after a
  design-system change, and built section-by-section through the same loop. Actively in
  progress.

In both cases the agent read `CLAUDE.md` and the Skills, built one section at a time,
passed each through the `design-review` gate, and handed off at the Bob branch — exactly
as the methodology describes.

---

## What this demonstrates

- A **repeatable, governed process** applied to a real commercial product with paying
  users, not a demo.
- **Design-engineering range**: a single operator owning the system from Figma design
  through production front-end code, across four product surfaces.
- **AI-native practice**: Figma MCP, Claude Code, a machine-readable design system, and
  a Skills-based quality gate — composed into one working pipeline.

---

## Verification

- Live product: `[CONFIRM primary URL — tokscript.com]`
- MCP product page: `tokscript.com/mcp`
- Design system, Skills, and workflow: this repository.
- Role: product designer & design engineer, TokScript (Tok Tools). `[CONFIRM title as
  you want it stated]`

*Built and maintained by [@bobatetio](https://github.com/bobatetio).*
