# Portfolio Visual Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** De-templatize the al-folio site with a site-wide blue "Airy Sky" palette, a minimal split hero (research headline + small photo + faint cloud background), and Newsreader serif headlines — without touching the content pipeline.

**Architecture:** All changes are in al-folio's SCSS theme variables, the `_config.yml` font URL, the homepage layout (`_layouts/about.liquid`), and homepage front matter/body (`_pages/about.md`). Theme color is a CSS custom property (`--global-theme-color`) consumed everywhere, so a single variable change recolors the entire site. The hero degrades gracefully: a CSS gradient is the background fallback, and the cloud image is an optional enhancement layered on top.

**Tech Stack:** Jekyll, al-folio theme, Dart Sass, Liquid, Google Fonts. Local server: `export PATH="/opt/homebrew/opt/ruby/bin:$PATH" && bundle exec jekyll serve --livereload --port 4000`.

**Verification note:** This is a CSS/Liquid visual change — there are no unit tests. Each task's "test" is: (a) `jekyll build` succeeds with no NEW errors (pre-existing SCSS deprecation warnings about `tabler-icons` are expected), and (b) a `curl`/`grep` assertion on the rendered output. A final task covers manual light/dark/responsive checks.

---

### Task 1: Add blue palette tokens

**Files:**

- Modify: `_sass/_variables.scss` (color definitions block, around lines 9–22)

- [ ] **Step 1: Add sky-blue tokens after the purple definitions**

Find this block (around line 21–22):

```scss
$purple-color: #b509ac !default;
$light-purple-color: color.adjust($purple-color, $lightness: 25%);
```

Add directly below it:

```scss
$sky-blue-color: #2b8fd6 !default;
$sky-blue-dark-color: #1f6fb0 !default;
$sky-blue-bright-color: #4aa8e0 !default;
$navy-ink-color: #0f1f3d !default;
```

- [ ] **Step 2: Build and verify no new errors**

Run: `export PATH="/opt/homebrew/opt/ruby/bin:$PATH" && bundle exec jekyll build 2>&1 | grep -iE "error|liquid exception" | grep -vi deprecation`
Expected: no output (empty).

- [ ] **Step 3: Commit**

```bash
git add _sass/_variables.scss
git commit -m "Add sky-blue palette tokens"
```

---

### Task 2: Repoint theme color to blue (light + dark)

**Files:**

- Modify: `_sass/_themes.scss:13-14` (light theme), and the dark theme block (around lines 85–86)

- [ ] **Step 1: Recolor the light theme**

Replace (lines 13–14):

```scss
--global-theme-color: #{$purple-color};
--global-hover-color: #{$purple-color};
```

with:

```scss
--global-theme-color: #{$sky-blue-color};
--global-hover-color: #{$sky-blue-dark-color};
```

- [ ] **Step 2: Recolor the dark theme**

Find the dark theme block (around lines 85–86):

```scss
--global-theme-color: #{$cyan-color};
--global-hover-color: #{$cyan-color};
```

Replace with:

```scss
--global-theme-color: #{$sky-blue-bright-color};
--global-hover-color: #{$sky-blue-bright-color};
```

- [ ] **Step 3: Build, then verify the compiled CSS uses the blue and no longer the purple theme color**

Run:

```bash
export PATH="/opt/homebrew/opt/ruby/bin:$PATH" && bundle exec jekyll build 2>&1 | grep -iE "error|liquid exception" | grep -vi deprecation
grep -o "global-theme-color: #2b8fd6" _site/assets/css/main.css | head -1
```

Expected: first command empty; second prints `global-theme-color: #2b8fd6`.

- [ ] **Step 4: Commit**

```bash
git add _sass/_themes.scss
git commit -m "Switch site theme color from purple to sky blue"
```

---

### Task 3: Load the Newsreader serif font

**Files:**

- Modify: `_config.yml:438`

- [ ] **Step 1: Add Newsreader to the Google Fonts URL**

Replace line 438:

```yaml
fonts: "https://fonts.googleapis.com/css?family=Roboto:300,400,500,700|Roboto+Slab:100,300,400,500,700|Material+Icons&display=swap"
```

with:

```yaml
fonts: "https://fonts.googleapis.com/css?family=Roboto:300,400,500,700|Roboto+Slab:100,300,400,500,700|Newsreader:400,500,600,700|Material+Icons&display=swap"
```

- [ ] **Step 2: Build and verify the font URL is in the rendered <head>**

Run (config changes need a fresh build, not incremental regen):

```bash
export PATH="/opt/homebrew/opt/ruby/bin:$PATH" && bundle exec jekyll build 2>&1 | grep -iE "error|liquid exception" | grep -vi deprecation
grep -c "Newsreader" _site/index.html
```

Expected: first empty; second prints a number ≥ 1.

- [ ] **Step 3: Commit**

```bash
git add _config.yml
git commit -m "Load Newsreader serif font for headlines"
```

---

### Task 4: Add hero + serif-headline styles

**Files:**

- Modify: `_sass/_base.scss` (append at end of file)

- [ ] **Step 1: Append the hero styles**

Add at the END of `_sass/_base.scss`:

```scss
// Homepage hero
.home-hero {
  position: relative;
  overflow: hidden;
  padding: 2.5rem 0 1.5rem;
  margin-bottom: 1.25rem;

  .hero-bg {
    position: absolute;
    inset: 0;
    z-index: 0;
    background: linear-gradient(180deg, #eaf3fb 0%, #f4f9fd 55%, var(--global-bg-color) 100%);

    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      opacity: 0.18;
      filter: blur(1px);
    }

    &::after {
      content: "";
      position: absolute;
      inset: 0;
      background: rgba(255, 255, 255, 0.32);
    }
  }

  .hero-inner {
    position: relative;
    z-index: 1;
    display: flex;
    gap: 1.8rem;
    align-items: center;
  }

  .hero-text {
    flex: 1.45;
  }

  .hero-eyebrow {
    font-size: 0.72rem;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--global-theme-color);
    font-weight: 600;
    margin: 0 0 0.5rem;
  }

  .hero-headline {
    font-family: "Newsreader", Georgia, serif;
    font-weight: 700;
    font-size: 2.2rem;
    line-height: 1.12;
    color: #0f1f3d;
    margin: 0;
  }

  .hero-rule {
    width: 44px;
    height: 3px;
    background: var(--global-theme-color);
    margin: 0.9rem 0;
  }

  .hero-tagline {
    font-size: 1rem;
    color: #46566b;
    line-height: 1.55;
    max-width: 38rem;
    margin: 0;
  }

  .hero-links {
    margin-top: 1.1rem;
    font-size: 0.9rem;

    .btn-primary-link {
      background: var(--global-theme-color);
      color: #fff;
      padding: 0.45rem 1rem;
      border-radius: 4px;
      font-weight: 600;
      text-decoration: none;
      margin-right: 0.6rem;

      &:hover {
        background: var(--global-hover-color);
        text-decoration: none;
      }
    }

    a:not(.btn-primary-link) {
      color: var(--global-text-color-light);
    }
  }

  .hero-photo {
    flex: 0.55;
    text-align: center;

    img {
      width: 150px;
      max-width: 100%;
      border-radius: 8px;
      box-shadow: 0 6px 18px rgba(43, 143, 214, 0.22);
    }
  }
}

@media (max-width: 576px) {
  .home-hero .hero-inner {
    flex-direction: column-reverse;
    align-items: flex-start;
  }
  .home-hero .hero-photo {
    align-self: center;
    margin-bottom: 1rem;
  }
  .home-hero .hero-headline {
    font-size: 1.8rem;
  }
}

html[data-theme="dark"] .home-hero {
  .hero-bg {
    background: linear-gradient(180deg, #16242f 0%, #1a2630 55%, var(--global-bg-color) 100%);

    &::after {
      background: rgba(28, 28, 29, 0.35);
    }
  }
  .hero-headline {
    color: var(--global-text-color);
  }
  .hero-tagline {
    color: var(--global-text-color-light);
  }
}

// Serif headlines elsewhere for consistency
.publications h2.pub-section,
.publications h2.bibliography {
  font-family: "Newsreader", Georgia, serif;
}
```

- [ ] **Step 2: Build and verify the hero CSS compiled**

Run:

```bash
export PATH="/opt/homebrew/opt/ruby/bin:$PATH" && bundle exec jekyll build 2>&1 | grep -iE "error|liquid exception" | grep -vi deprecation
grep -c "home-hero" _site/assets/css/main.css
```

Expected: first empty; second ≥ 1.

- [ ] **Step 3: Commit**

```bash
git add _sass/_base.scss
git commit -m "Add homepage hero styles and serif headlines"
```

---

### Task 5: Restructure the homepage layout into the hero

**Files:**

- Modify: `_layouts/about.liquid` (replace the `<header class="post-header">` block and the in-article profile float)

- [ ] **Step 1: Replace the header + profile block with the hero**

Replace this (lines ~4–37, from `<div class="post">` through the end of the `{% if page.profile %}…{% endif %}` profile block, i.e. everything up to and including the `</div>` that closes the profile float, stopping BEFORE `<div class="clearfix">`):

```liquid
<div class="post">
  <header class="post-header">
    <h1 class="post-title">
      <span class="font-weight-bold">{{ site.first_name }}</span> {{ site.middle_name }}
      {{ site.last_name }}
    </h1>
    <p class="desc">{{ page.subtitle }}</p>
  </header>

  <article>
    {% if page.profile %}
      <div class="profile float-{% if page.profile.align == 'left' %}left{% else %}right{% endif %}">
        {% if page.profile.image %}
          {% assign profile_image_path = page.profile.image | prepend: 'assets/img/' %}
          {% if page.profile.image_circular %}
            {% assign profile_image_class = 'img-fluid z-depth-1 rounded-circle' %}
          {% else %}
            {% assign profile_image_class = 'img-fluid z-depth-1
      rounded' %}
          {% endif %}
          {% capture sizes %}(min-width: {{ site.max_width }}) {{ site.max_width | minus: 30 | times: 0.3}}px, (min-width: 576px)
      30vw, 95vw"{% endcapture %}
          {%
            include figure.liquid loading="eager" path=profile_image_path class=profile_image_class sizes=sizes alt=page.profile.image
            cache_bust=true
          %}
        {% endif %}
        {% if page.profile.more_info %}
          <div class="more-info">{{ page.profile.more_info }}</div>
        {% endif %}
      </div>
    {% endif %}

    <div class="clearfix">{{ content }}</div>
```

with:

```liquid
<div class="post">
  <section class="home-hero">
    <div class="hero-bg">
      {% if page.hero_image %}
        <img src="{{ page.hero_image | prepend: 'assets/img/' | relative_url }}" alt="">
      {% endif %}
    </div>
    <div class="hero-inner">
      <div class="hero-text">
        <h1 class="hero-eyebrow">{{ page.hero_eyebrow }}</h1>
        <div class="hero-headline">{{ page.hero_headline }}</div>
        <div class="hero-rule"></div>
        <p class="hero-tagline">{{ page.hero_tagline }}</p>
        <div class="hero-links">
          <a class="btn-primary-link" href="{{ '/publications/' | relative_url }}">Publications</a>
          <a href="{{ '/cv/' | relative_url }}">CV</a> ·
          <a href="https://scholar.google.com/citations?user={{ site.data.socials.scholar_userid }}" target="_blank" rel="noopener">Scholar</a> ·
          <a href="https://orcid.org/{{ site.data.socials.orcid_id }}" target="_blank" rel="noopener">ORCID</a> ·
          <a href="mailto:{{ site.data.socials.email }}">Email</a>
        </div>
      </div>
      {% if page.profile and page.profile.image %}
        <div class="hero-photo">
          {% assign profile_image_path = page.profile.image | prepend: 'assets/img/' %}
          {% include figure.liquid loading="eager" path=profile_image_path class="img-fluid" sizes="150px" alt=page.profile.image cache_bust=true %}
        </div>
      {% endif %}
    </div>
  </section>

  <article>
    <div class="clearfix">{{ content }}</div>
```

Note: leave the rest of the file (News, Latest posts, Selected papers, Social blocks, and closing tags) unchanged. Those blocks are guarded by `{% if page.X %}` and will be disabled via front matter in Task 6.

- [ ] **Step 2: Build and verify the hero renders on the homepage**

Run:

```bash
export PATH="/opt/homebrew/opt/ruby/bin:$PATH" && bundle exec jekyll build 2>&1 | grep -iE "error|liquid exception" | grep -vi deprecation
grep -c "home-hero" _site/index.html
```

Expected: first empty; second ≥ 1.

- [ ] **Step 3: Commit**

```bash
git add _layouts/about.liquid
git commit -m "Restructure homepage into minimal split hero"
```

---

### Task 6: Homepage front matter + condensed About body

**Files:**

- Modify: `_pages/about.md`

- [ ] **Step 1: Update front matter and body**

Replace the entire current front matter + body. The new file content is:

```markdown
---
layout: about
title: About
permalink: /
nav: false
nav_order: 1
subtitle: Postdoctoral Associate at <a href='https://cires.colorado.edu/'>CIRES</a>, University of Colorado Boulder / <a href='https://csl.noaa.gov/'>NOAA Chemical Sciences Laboratory</a>

hero_eyebrow: Jung-Sub Lim · Atmospheric Physicist, CIRES / NOAA
hero_headline: From cloud droplets to Arctic climate
hero_tagline: Cloud microphysics, turbulent mixing, and what tiny droplets tell us about a warming Arctic.
hero_image: # set to a filename in assets/img/ once chosen (e.g. clouds_hero.jpg); leave blank to use the gradient

profile:
  align: right
  image: prof_pic.jpg
  image_circular: false
  more_info: >
    <p>Contact me at</p>
    <p><a href="mailto:jung-sub.lim@noaa.gov">jung-sub.lim@noaa.gov</a></p>

selected_papers: false
social: true

announcements:
  enabled: false

latest_posts:
  enabled: false
---

I am a postdoctoral associate at [CIRES](https://cires.colorado.edu/) / NOAA Chemical Sciences Laboratory, working with [Dr. Graham Feingold](https://csl.noaa.gov/staff/graham.feingold/) on Arctic mixed-phase cloud stability using Lagrangian cloud modeling and observations from field campaigns like MOSAiC. My research connects microscale cloud physics — entrainment, mixing, and droplet growth — to cloud–climate interactions. [Explore my research →](/projects/)
```

- [ ] **Step 2: Build and verify homepage simplifications**

Run:

```bash
export PATH="/opt/homebrew/opt/ruby/bin:$PATH" && bundle exec jekyll build 2>&1 | grep -iE "error|liquid exception" | grep -vi deprecation
echo "hero headline present:"; grep -c "From cloud droplets to Arctic climate" _site/index.html
echo "selected pubs removed (expect 0):"; grep -c "Selected Publications" _site/index.html
echo "news removed (expect 0):"; grep -c "bibliography\|announcement" _site/index.html
```

Expected: first empty; headline count ≥ 1; "Selected Publications" = 0; news/announcement = 0.

- [ ] **Step 3: Commit**

```bash
git add _pages/about.md
git commit -m "Simplify homepage: hero fields, drop selected-pubs and news"
```

---

### Task 7: (Optional enhancement) Add cloud hero background image

**Files:**

- Create: `assets/img/clouds_hero.jpg` (a CC0/licensed cloud image)
- Modify: `_pages/about.md` (set `hero_image: clouds_hero.jpg`)

This task is optional — the hero already works with the gradient fallback. Do this only after the user picks an image.

- [ ] **Step 1: Obtain a licensed image**

Choose a CC0/Unsplash/Pexels cloud or atmospheric image (wide, light-toned so the white wash keeps text readable). Present 2–3 candidates to the user; on approval, save the chosen one to `assets/img/clouds_hero.jpg`. Record the source/license in the commit message.

- [ ] **Step 2: Point the hero at the image**

In `_pages/about.md` front matter, set:

```yaml
hero_image: clouds_hero.jpg
```

- [ ] **Step 3: Build and verify the image is referenced in the hero**

Run:

```bash
export PATH="/opt/homebrew/opt/ruby/bin:$PATH" && bundle exec jekyll build 2>&1 | grep -iE "error|liquid exception" | grep -vi deprecation
grep -o "clouds_hero" _site/index.html | head -1
```

Expected: first empty; second prints `clouds_hero`.

- [ ] **Step 4: Commit**

```bash
git add assets/img/clouds_hero.jpg _pages/about.md
git commit -m "Add cloud hero background image (source: <url>, license: <license>)"
```

---

### Task 8: Manual verification (light, dark, responsive, site-wide)

**Files:** none (verification only)

- [ ] **Step 1: Serve locally**

Run: `export PATH="/opt/homebrew/opt/ruby/bin:$PATH" && bundle exec jekyll serve --livereload --port 4000` (background it).

- [ ] **Step 2: Verify each item in a browser at http://localhost:4000**

Confirm and check off:

- Homepage hero: eyebrow (blue), serif headline, rule, tagline, Publications button + links, small photo on the right, faint cloud/gradient background; text fully legible.
- Toggle **dark mode**: accent is blue (not purple/cyan), hero background and headline adapt, text legible.
- **Responsive** (narrow the window < 576px): hero stacks (photo above text), nothing overflows, links wrap.
- **Site-wide blue:** open `/publications/`, `/cv/`, `/projects/` — section headers, links, and accents are blue, no purple remains.
- Homepage shows **no** Selected Publications block and **no** News list.

- [ ] **Step 3: If all pass, the redesign is complete.** If the user wants it live, push to `origin main` (GitHub Pages auto-deploys). Do not push without explicit confirmation.

---

## Self-Review

**Spec coverage:**

- Site-wide blue palette → Tasks 1, 2 ✓
- Newsreader serif headlines → Tasks 3, 4 ✓
- Minimal split hero (headline + small photo + faint cloud bg) → Tasks 4, 5, 7 ✓
- Homepage simplification (drop selected-pubs/news) → Task 6 ✓
- Dark mode + responsive → Tasks 4, 8 ✓
- Verification (build, light/dark, responsive, site-wide) → Tasks 2–8 ✓

**Deviation from spec (intentional, low-risk):** The spec's palette table lists navy ink / slate body text site-wide. To protect long-form readability and accessibility on content pages (Publications, CV), `--global-text-color` is left at al-folio's default; navy (`#0f1f3d`) and slate (`#46566b`) are applied specifically within the hero. The blue accent is the site-wide change. If the user wants navy body text everywhere, that is a one-line follow-up in `_themes.scss`.

**Placeholder scan:** `hero_image` is intentionally blank with an inline comment and an optional Task 7 — the gradient is the working default, so this is not a gap. No other placeholders.

**Type/name consistency:** Front-matter keys (`hero_eyebrow`, `hero_headline`, `hero_tagline`, `hero_image`) match exactly between `about.md` (Task 6) and `about.liquid` (Task 5). CSS class names (`home-hero`, `hero-bg`, `hero-inner`, `hero-text`, `hero-eyebrow`, `hero-headline`, `hero-rule`, `hero-tagline`, `hero-links`, `btn-primary-link`, `hero-photo`) match between `_base.scss` (Task 4) and `about.liquid` (Task 5).
