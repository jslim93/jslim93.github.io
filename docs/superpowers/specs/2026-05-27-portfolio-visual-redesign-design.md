# Portfolio Visual Redesign — Design

**Date:** 2026-05-27
**Status:** Approved (pending spec review)
**Owner:** Jung-Sub Lim

## Goal

Make `jslim93.github.io` read as **distinctive, professional, and easy to scan at a glance**, eliminating the "generic al-folio template" impression. The content (publications, CV, talks) is already strong; this redesign changes only the **visual identity and homepage information hierarchy**, not the underlying content pipeline.

## Audience

Faculty search committees, collaborating researchers, and fellowship reviewers. These are sophisticated readers who value restraint, clarity, and a clear research identity over flashiness. Design decisions favor "credible + memorable" over "busy."

## Problem (from reviewer feedback)

- Looks like a default al-folio site (the **#1 complaint** — generic purple theme, default layout).
- Hard to take in at a glance; the homepage opens with prose and a News list rather than impact.
- Beautiful cloud-science visuals are buried; nothing anchors the eye.

## Scope

**Strategic de-templatization on al-folio.** Keep al-folio's structure and content machinery (BibTeX bibliography, `cv.yml`, projects collection, news). Change only: color palette, typography, and the homepage hero/structure.

### In scope
1. **Site-wide blue palette** replacing the default purple (and current cyan dark accent), applied across home, Publications, CV, Projects, nav, links, accents.
2. **Homepage hero redesign** — minimal split hero with a research headline, small profile photo, and a faint cloud background.
3. **Homepage simplification** — remove Selected Publications, News, and any research-card strip from the homepage; those live on their nav pages.
4. **Refined serif headline font** (Newsreader) for the hero headline and section headers.

### Out of scope (YAGNI)
- Authoring/expanding research/project page content (pages exist; not part of this work).
- Blog, new template, or platform migration.
- Changes to publication/CV **data** (already completed in prior commit `481af3d`).

## Design System

### Palette — "Airy Sky" (B3)
| Token | Light | Dark |
|---|---|---|
| Background | `#ffffff` | `#1c1c1d` (al-folio default) |
| Hero tint / section bg | `#f4f9fd` | subtle dark blue-grey |
| Ink / headline | `#0f1f3d` (deep navy) | `#e8e8e8` |
| Body text | `#46566b` / muted `#7c8aa0` | existing light grey |
| **Accent (theme color)** | `#2b8fd6` (sky blue) | `#4aa8e0` (brighter sky for dark bg) |
| Accent hover | `#1f6fb0` | `#2b8fd6` |

Implementation: add `$sky-blue-color: #2b8fd6` (and a darker hover variant) to `_sass/_variables.scss`; point `--global-theme-color` and `--global-hover-color` to it in `_sass/_themes.scss` for both light and dark themes. Update `$code-bg-color-light` if it still reads purple.

### Typography
- **Headline serif:** Newsreader (Google Font), weights ~500–700. Used for the hero headline and section headers (`h1`/`h2` on content pages). Added to the existing Google Fonts URL in `_config.yml` (line ~438), alongside the current Roboto / Roboto Slab.
- **Body / nav / labels:** keep al-folio's Roboto (sans). No change.

## Homepage Redesign

The homepage becomes essentially **one screen**: hero + a 3-sentence About + social footer.

### Hero (split layout)
```
[ nav: "Jung-Sub Lim"  ........  About · Research · Publications · CV ]

  JUNG-SUB LIM · ATMOSPHERIC PHYSICIST, CIRES / NOAA   (eyebrow, uppercase sky-blue)
  From cloud droplets                                  (big Newsreader serif headline)
  to Arctic climate
  ▔▔▔                                                  (44px sky-blue rule)
  Cloud microphysics, turbulent mixing, and what       (tagline, slate)
  tiny droplets tell us about a warming Arctic.
  [ Publications ]  CV · Scholar · ORCID · Email        (primary button + text links)
                                          [ photo ]     (small profile photo, right)

  ── faint blurred cloud image fills the hero background, with a white wash
     overlay so all text/photo stay crisp ──
```

- **Photo:** small (≈96×112 on desktop), right-aligned, rounded. Reuses existing `assets/img/prof_pic.jpg`.
- **Cloud background:** a licensed (CC0/Unsplash/Pexels) cloud image at low opacity behind the hero, with a `rgba(255,255,255,.32)` wash + light blue gradient to guarantee text contrast. 2–3 candidate images to be selected during implementation and committed to `assets/img/`.
- **Name in DOM:** the name appears in an `<h1>` (visually the eyebrow line) so SEO/accessibility keep a clear page identity; the research headline is the dominant visual element.

### Below the hero
- **Short About:** ~3 sentences (current second paragraph, condensed) ending with a `Research →` link.
- **Social footer:** existing social icons (email, Scholar, ORCID, GitHub, ResearchGate) + copyright.

### Removed from homepage
- Selected Publications block (`selected_papers: false`) — Publications page covers this.
- News list (`announcements.enabled: false`) — News page covers this.
- Any research-card strip (not added).

## Files Affected
1. `_sass/_variables.scss` — add sky-blue tokens.
2. `_sass/_themes.scss` — repoint theme/hover color (light + dark); fix code-bg if purple.
3. `_config.yml` — add Newsreader to the Google Fonts URL.
4. `_sass/_base.scss` (or new `_sass/_hero.scss`) — hero styles: serif headline, split layout, faint cloud bg, responsive stacking; serif for section headers.
5. `_layouts/about.liquid` — restructure hero (eyebrow, serif headline, tagline, links, small photo, cloud-bg wrapper).
6. `_pages/about.md` — front matter toggles (`selected_papers: false`, `announcements.enabled: false`, keep `social: true`, keep profile image); condense About body; add `Research →` link.
7. `assets/img/` — add chosen licensed cloud background image.

## Verification
- `bundle exec jekyll build` succeeds with no new errors (SCSS deprecation warnings pre-exist).
- **Light and dark mode** both render the blue accent (no leftover purple/cyan); toggle and confirm.
- **Responsive:** hero stacks cleanly on mobile (<576px) — text above, photo below; tagline and links wrap correctly.
- **Contrast:** hero text and photo remain clearly legible over the cloud background.
- **Site-wide:** Publications, CV, Projects, and nav links all show blue (spot-check each page).
- Name present in the rendered DOM for SEO.

## Open Items (locked to recommendations; change at spec review if desired)
- **Scope = site-wide** recolor (recommended for a cohesive de-templatized look).
- **Headline font = Newsreader** (more refined than system Georgia).
- **Cloud background image** = to be chosen from 2–3 licensed candidates during implementation.
