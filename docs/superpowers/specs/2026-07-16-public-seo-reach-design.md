# Tayyibat Guide — Public / Reach / SEO Enhancement

**Date:** 2026-07-16
**Deploy target:** `https://khaledalarda.github.io/alarda/` (GitHub Pages, no custom domain)
**Site:** Single-file Arabic (RTL) static nutrition guide, `index.html`

## Goal

Transform the currently hyper-personalized guide (calibrated for one person: 86kg / 174cm) into a
public, general Arabic nutrition resource that (a) ranks in Arabic Google search and (b) previews
beautifully when the link is shared on WhatsApp / Twitter / Facebook.

## Confirmed decisions

- **Audience:** public & general (not a per-visitor calculator — no interactive calorie tool).
- **Reach channels:** Google search *and* WhatsApp/social sharing (do both).
- **Images:** self-host a curated, verified, compressed set in the repo (drop the fragile Unsplash
  dependency). Emoji fallback stays in place throughout.
- **File structure:** keep single self-contained `index.html` (fastest load = best Core Web Vitals =
  best SEO). Clean up internally; do not split into external CSS/JS.
- **Content framing:** keep the existing rules/content as-is. Only generalize the personalized
  address ("you", 86kg/174cm). No new origin claims, no new health claims. Keep the medical
  disclaimer. Structured data / FAQ reflect only content already present on the page.

## Architecture

Stays one `index.html`. New sibling files added to the repo root:

- `og-image.png` — 1200×630 social preview (raster PNG; WhatsApp/Facebook do not render SVG).
- `favicon.svg` + `favicon.ico` (or PNG) — 🌿 leaf theme.
- `robots.txt` — allow all, point to sitemap.
- `sitemap.xml` — single URL entry for the site root.
- `images/` — one curated WebP per food (~70 items), optimized and dimension-known.

## Workstreams

### 1. Codebase enhancement
- Generalize copy: subtitle `مخصص لك — ٨٦ كجم · ١٧٤ سم · نشاط منخفض` → general framing; the
  coach-tab direct address (`كلام صريح معك`, `٨٦ كجم + حياة ساكنة`) → general phrasing. Meal-plan
  calorie math stays, presented as a general reference.
- Semantic HTML: wrap in `header`/`main`/`footer`; convert `.tabs` into a real `nav` with
  `role="tablist"` / `role="tab"` / `aria-selected` / `aria-controls`; convert styled-`div` section
  titles (`.sec-title`, `.cat-lbl`) into real `h2`/`h3` so heading hierarchy is `h1 → h2 → h3`.
- Accessibility: visible `:focus-visible` states on all buttons; `lang="en"` spans on Latin
  food-name text; meaningful `alt` on images; `aria-label` on the tab nav.
- Data tidy: fix the `قهوة` item mapped to `nk:'ارز'` (currently harmless at g:0); audit that every
  food referenced in `allowedCats`/`forbiddenCats`/meals has an `IMGS` entry.

### 2. Reach (social sharing)
- `<head>` Open Graph: `og:title`, `og:description`, `og:image` (absolute URL), `og:image:width/height`,
  `og:url`, `og:type=website`, `og:locale=ar_AR`, `og:site_name`.
- Twitter Card: `summary_large_image` + title/description/image.
- Designed 1200×630 `og-image.png` in the green/leaf theme with the Arabic title.
- Favicon links.

### 3. SEO
- Optimized `<title>` and `<meta name="description">` with Arabic keywords
  (نظام الطيبات، دليل غذائي، دايت، سعرات، بروتين، وجبات).
- `<link rel="canonical">` to the deploy URL.
- JSON-LD: `WebSite`, `HowTo` (six golden rules + meal structure), `FAQPage` (drawn only from
  existing on-page content). No fabricated claims.
- `robots.txt` + `sitemap.xml`.
- Performance (ranking factor): `loading="lazy"` + explicit `width`/`height` on every food image to
  eliminate layout shift.

### 4. Images (long pole, second pass)
- Curate one correct image per food (~70). Compress to WebP, reasonable dimensions (~220px card /
  60px inline source, 2× for retina).
- Verification loop: read each downloaded image visually to confirm it depicts the correct food;
  re-source mismatches. Emoji fallback remains via existing `onerror` path.

## Sequencing

Ship Workstreams 1–3 first (the full SEO/reach payoff, fast, low-risk), then do the ~70-image
curation as a focused second pass. Site is launch-ready after phase 1; emoji fallback means nothing
looks broken during image curation.

## Out of scope

- Interactive per-visitor calorie calculator.
- Custom domain / analytics.
- Splitting into multiple source files.
- Any new health/origin claims or author attribution.

## Success criteria

- Sharing the URL in WhatsApp shows a titled card with the preview image.
- Page passes a rich-results structured-data check with no errors.
- Lighthouse SEO ~100; no layout-shift warnings from images.
- All personalized ("you"/specific bodyweight) copy is gone; page reads for a general audience.
- No broken food images (curated set + emoji fallback).
