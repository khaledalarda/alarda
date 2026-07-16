# Public / Reach / SEO Enhancement — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Turn the personalized single-file Tayyibat guide into a public, SEO-optimized, socially-shareable Arabic nutrition resource.

**Architecture:** Keep one self-contained `index.html`. Add sibling static assets (`og-image.png`, favicon, `robots.txt`, `sitemap.xml`, `images/`). Enhance `<head>` for SEO/social, make the body semantic/accessible, generalize copy, and move food images from external Unsplash to a curated self-hosted set with emoji fallback.

**Tech Stack:** Static HTML/CSS/vanilla JS, GitHub Pages, JSON-LD structured data.

## Global Constraints

- Deploy URL is exactly `https://khaledalarda.github.io/alarda/` — use this for all canonical / og:url / sitemap / absolute asset URLs.
- Single self-contained `index.html` — no external CSS/JS files.
- RTL Arabic (`<html lang="ar" dir="rtl">`) stays.
- No new health claims, no origin/author attribution. Keep the existing medical disclaimer verbatim.
- Structured data and FAQ content must reflect only content already on the page.
- Emoji fallback via the existing `onerror` path stays functional throughout.
- Verification is browser/validator-based (no test framework). "Run" steps mean open the file/URL and confirm the stated result.

---

### Task 1: SEO + social `<head>`

**Files:**
- Modify: `index.html:3-6` (the `<head>` open through `<title>`)

**Interfaces:**
- Produces: canonical URL `https://khaledalarda.github.io/alarda/`; references `og-image.png`, `favicon.svg`, `favicon.ico` created in Task 4.

- [ ] **Step 1: Replace the head metadata block**

Replace lines 4-6 (`<meta charset>` through `<title>`) with:

```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>دليل نظام الطيبات الشامل — الأكل بالصور والسعرات والوجبات</title>
<meta name="description" content="دليل نظام الطيبات الغذائي بالكامل: المسموحات والممنوعات بالصور، وجبات مقسّمة بالسعرات والبروتين والدهون، خطط للحياة العادية والجيم، ونصائح للنوم والصحة الهرمونية.">
<link rel="canonical" href="https://khaledalarda.github.io/alarda/">
<meta name="robots" content="index, follow">
<meta name="theme-color" content="#15803d">

<meta property="og:type" content="website">
<meta property="og:site_name" content="دليل نظام الطيبات">
<meta property="og:locale" content="ar_AR">
<meta property="og:url" content="https://khaledalarda.github.io/alarda/">
<meta property="og:title" content="دليل نظام الطيبات الشامل — الأكل بالصور والسعرات">
<meta property="og:description" content="المسموحات والممنوعات بالصور، وجبات مقسّمة بالسعرات والبروتين، وخطط للحياة العادية والجيم — دليل نظام الطيبات كامل.">
<meta property="og:image" content="https://khaledalarda.github.io/alarda/og-image.png">
<meta property="og:image:width" content="1200">
<meta property="og:image:height" content="630">
<meta property="og:image:alt" content="دليل نظام الطيبات الشامل">

<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="دليل نظام الطيبات الشامل — الأكل بالصور والسعرات">
<meta name="twitter:description" content="المسموحات والممنوعات بالصور، وجبات مقسّمة بالسعرات والبروتين، وخطط للحياة العادية والجيم.">
<meta name="twitter:image" content="https://khaledalarda.github.io/alarda/og-image.png">

<link rel="icon" href="favicon.ico" sizes="any">
<link rel="icon" href="favicon.svg" type="image/svg+xml">
<link rel="apple-touch-icon" href="favicon.svg">
```

- [ ] **Step 2: Add JSON-LD structured data before `</head>`**

Immediately before `</head>` (currently line 83) insert:

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@graph": [
    {
      "@type": "WebSite",
      "@id": "https://khaledalarda.github.io/alarda/#website",
      "url": "https://khaledalarda.github.io/alarda/",
      "name": "دليل نظام الطيبات",
      "inLanguage": "ar"
    },
    {
      "@type": "HowTo",
      "name": "القواعد الست الذهبية لنظام الطيبات",
      "inLanguage": "ar",
      "step": [
        {"@type":"HowToStep","name":"كُل عند الجوع الحقيقي","text":"مش لأنك ملول — جوع حقيقي بس"},
        {"@type":"HowToStep","name":"قف عند ٨٠٪ شبع","text":"قبل الشبع الكامل — الجهاز الهضمي يشتغل بكفاءة"},
        {"@type":"HowToStep","name":"٢-٣ عناصر في الوجبة","text":"ما تخلط كثير — خفف الحمل على جهازك"},
        {"@type":"HowToStep","name":"اشرب عند العطش","text":"جسمك يعرف — ما تحتاج قاعدة ٨ أكواب"},
        {"@type":"HowToStep","name":"الصيام أساسي","text":"الإثنين والخميس + أيام البيض ١٣،١٤،١٥"},
        {"@type":"HowToStep","name":"البروتين يوم آه ويوم لا","text":"يوم لحم/سمك، يوم راحة"}
      ]
    },
    {
      "@type": "FAQPage",
      "inLanguage": "ar",
      "mainEntity": [
        {"@type":"Question","name":"ما هي أساسيات نظام الطيبات؟","acceptedAnswer":{"@type":"Answer","text":"الأساسيات الخمسة اليومية: الأرز، البطاطس، التمر، الزبدة، والعسل — مع لحوم يوم آه ويوم لا، وأجبان معتقة، وفواكه ومكسرات."}},
        {"@type":"Question","name":"ما الممنوعات في نظام الطيبات؟","acceptedAnswer":{"@type":"Answer","text":"الدقيق الأبيض والخبز الأبيض والمكرونة، الدواجن والبيض، الحليب ومشتقاته، الخضروات الورقية النيئة، البقوليات، والمشروبات الغازية."}},
        {"@type":"Question","name":"هل الصيام جزء من النظام؟","acceptedAnswer":{"@type":"Answer","text":"نعم، الصيام أساسي — يومي الإثنين والخميس بالإضافة إلى أيام البيض ١٣ و١٤ و١٥."}}
      ]
    }
  ]
}
</script>
```

- [ ] **Step 3: Verify**

Open `index.html` in a browser, View Source, confirm: one `<title>`, one canonical, OG image URL is absolute. Paste the page source into https://validator.schema.org/ — expect WebSite, HowTo, FAQPage detected with zero errors.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: SEO head, OpenGraph/Twitter tags, JSON-LD structured data"
```

---

### Task 2: robots.txt + sitemap.xml

**Files:**
- Create: `robots.txt`
- Create: `sitemap.xml`

- [ ] **Step 1: Create `robots.txt`**

```
User-agent: *
Allow: /

Sitemap: https://khaledalarda.github.io/alarda/sitemap.xml
```

- [ ] **Step 2: Create `sitemap.xml`**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://khaledalarda.github.io/alarda/</loc>
    <changefreq>monthly</changefreq>
    <priority>1.0</priority>
  </url>
</urlset>
```

- [ ] **Step 3: Verify**

Confirm both files are valid: `sitemap.xml` opens without XML parse errors in a browser; `robots.txt` Sitemap line points at the deploy URL.

- [ ] **Step 4: Commit**

```bash
git add robots.txt sitemap.xml
git commit -m "feat: add robots.txt and sitemap.xml"
```

---

### Task 3: Semantic structure, accessibility, and generalized copy

**Files:**
- Modify: `index.html` — CSS block (add focus styles), body markup (lines 84-117), subtitle (line 87), coach-tab copy (line 112).

**Interfaces:**
- Consumes: nothing.
- Produces: heading hierarchy `h1 → h2 → h3`; `nav[role=tablist]` with `role=tab` buttons. The four tab buttons keep their `onclick="goTab(n)"` handlers (JS unchanged).

- [ ] **Step 1: Add focus + visually-hidden styles to the CSS block**

Before the closing `</style>` (line 82) add:

```css
.tab:focus-visible,.wbtn:focus-visible,.dbtn:focus-visible,.altbtn:focus-visible{outline:2px solid #1d4ed8;outline-offset:2px}
h2.sec-title,h3.cat-lbl{font-weight:700}
.vh{position:absolute;width:1px;height:1px;padding:0;margin:-1px;overflow:hidden;clip:rect(0,0,0,0);border:0}
```

- [ ] **Step 2: Generalize the subtitle**

Replace line 87:

```html
<p class="sub">دليل غذائي كامل بالصور — لكل وجبة سعراتها الحرارية والبروتين والدهون، وخطط للحياة العادية والجيم.</p>
```

- [ ] **Step 3: Wrap the page in semantic landmarks**

Change line 85 `<div class="wrap">` to `<main class="wrap">` and its matching close `</div>` at line 117 to `</main>`. Wrap the `<h1>` + `<p class="sub">` (lines 86-87) in a `<header>` … `</header>`.

- [ ] **Step 4: Make the tabs a real tablist**

Replace the `<div class="tabs">` block (lines 88-93) with:

```html
<nav class="tabs" role="tablist" aria-label="أقسام الدليل">
  <button class="tab on" role="tab" aria-selected="true" onclick="goTab(0)">📸 الأكل بالصور</button>
  <button class="tab" role="tab" aria-selected="false" onclick="goTab(1)">🏠 حياة عادية + سعرات</button>
  <button class="tab" role="tab" aria-selected="false" onclick="goTab(2)">🏋️ جيم + سعرات</button>
  <button class="tab" role="tab" aria-selected="false" onclick="goTab(3)">🧠 مدرب الحياة</button>
</nav>
```

- [ ] **Step 5: Promote section titles to real headings**

Change the four `<p class="sec-title">` occurrences (lines 95, 104, 106, and the inline one at 110-area) to `<h2 class="sec-title">…</h2>`. Change the `renderAllowed`/`renderForbidden` category label output in JS (lines 544 and 553) from `<div class="cat-lbl">` to `<h3 class="cat-lbl">` (and matching closing tag).

- [ ] **Step 6: Generalize the coach-tab direct address**

Replace the `.cwd` text on line 112:

```html
<div class="cwd">الأكل ٣٠٪ فقط من المعادلة. الباقي: نوم + شمس + حركة. النتائج الحقيقية تأتي من نمط حياة متكامل، لا من الطعام وحده.</div>
```

- [ ] **Step 7: Verify**

Open `index.html`. Tab through with the keyboard — every button shows a visible focus ring. Confirm exactly one `<h1>`, that section titles are `<h2>`, category labels are `<h3>`. Confirm no personalized text ("٨٦ كجم", "مخصص لك", "معك") remains via `grep -n "٨٦ كجم\|مخصص لك" index.html` → no output. Tabs still switch correctly.

- [ ] **Step 8: Commit**

```bash
git add index.html
git commit -m "feat: semantic landmarks, heading hierarchy, a11y focus states, generalized copy"
```

---

### Task 4: og-image.png + favicon assets

**Files:**
- Create: `og-image.png` (1200×630)
- Create: `favicon.svg`
- Create: `favicon.ico`

- [ ] **Step 1: Create `favicon.svg`**

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 64 64"><rect width="64" height="64" rx="14" fill="#15803d"/><text x="32" y="44" font-size="40" text-anchor="middle">🌿</text></svg>
```

- [ ] **Step 2: Generate `og-image.png` and `favicon.ico`**

Build a 1200×630 PNG in the green/leaf theme showing the Arabic title `دليل نظام الطيبات الشامل` and subtitle `الأكل بالصور · السعرات · الوجبات · الجيم`. Approach: write a standalone HTML file to the scratchpad, render it with the Playwright browser at 1200×630, screenshot to `og-image.png`. Generate `favicon.ico` (32×32) from the same leaf/green mark. Verify each rendered file visually with the Read tool.

- [ ] **Step 3: Verify**

Read `og-image.png` — confirm it is 1200×630, green-themed, Arabic text legible and not clipped. Read `favicon.svg`/`favicon.ico` render as a leaf mark.

- [ ] **Step 4: Commit**

```bash
git add og-image.png favicon.svg favicon.ico
git commit -m "feat: add social preview image and favicons"
```

---

### Task 5: Switch food images to self-hosted paths + performance attrs

**Files:**
- Modify: `index.html` — `fc()` (lines 529-540) and `ingHtml()` (lines 509-516); add a name→slug map.

**Interfaces:**
- Consumes: `IMGS` map (unchanged keys).
- Produces: image `src` of the form `images/<slug>.webp`; every `<img>` gets `loading="lazy"`, `decoding="async"`, and explicit `width`/`height`. Emoji fallback stays via existing `onerror`. Task 6 populates `images/` using the same slugs.

- [ ] **Step 1: Add a slug helper and per-food slug field**

In the JS, add to each `IMGS` entry (or a parallel map) a stable ASCII `slug`. Add helper:

```js
function imgSrc(name,large){const f=IMGS[name];if(!f||!f.slug)return '';return 'images/'+f.slug+(large?'.webp':'-sm.webp');}
```

- [ ] **Step 2: Rewrite `fc()` image markup to use local source with dimensions**

Replace the `imgHtml` line inside `fc()`:

```js
const src=imgSrc(name,true);
const imgHtml=src?`<img src="${src}" loading="lazy" decoding="async" width="110" height="88" onerror="this.style.display='none';this.nextSibling.style.display='flex'" alt="${name}"><div class="em" style="display:none">${f.e}</div>`:`<div class="em">${f.e}</div>`;
```

- [ ] **Step 3: Rewrite `ingHtml()` image markup**

Replace the `img` const inside `ingHtml()`:

```js
const src=imgSrc(i.n,false);
const img=src?`<img class="ii" src="${src}" loading="lazy" decoding="async" width="22" height="22" onerror="this.style.display='none';this.nextSibling.style.display='inline-block'" alt="${i.n}"><span class="ie" style="display:none">${f.e||'🍽️'}</span>`:`<span class="ie">🍽️</span>`;
```

- [ ] **Step 4: Remove the dead Unsplash constants**

Delete the `U`, `Q`, `SQ` constants (line 119) and the `p:` photo-id fields once no longer referenced (keep `e:` emoji and `a:` alt text). Confirm via `grep -n "images.unsplash" index.html` → no output.

- [ ] **Step 5: Verify**

Open `index.html` with no `images/` folder yet — every food card falls back to its emoji cleanly (no broken-image icons). No console errors referencing unsplash.

- [ ] **Step 6: Commit**

```bash
git add index.html
git commit -m "feat: self-hosted image paths with lazy-loading, dimensions, emoji fallback"
```

---

### Task 6: Curate and self-host the food images

**Files:**
- Create: `images/<slug>.webp` and `images/<slug>-sm.webp` for each food (~70).

**Interfaces:**
- Consumes: the slug list defined in Task 5.

- [ ] **Step 1: Enumerate the required slugs**

List every `IMGS` slug. This is the target set of images to source.

- [ ] **Step 2: Source one candidate photo per food**

For each food, download a candidate image (Unsplash/Openverse/Wikimedia — freely usable). Save originals to the scratchpad.

- [ ] **Step 3: Visually verify each candidate**

Read each downloaded image with the Read tool; confirm it depicts the correct food. Re-source any mismatch before proceeding. Log any food that could not be sourced (it will use emoji fallback).

- [ ] **Step 4: Optimize to WebP at two sizes**

Convert each verified image to `images/<slug>.webp` (~220px, card) and `images/<slug>-sm.webp` (~60px, inline), compressed. Use `cwebp`/`sips`/ImageMagick as available.

- [ ] **Step 5: Verify in browser**

Open `index.html`; confirm the "الأكل بالصور" grid shows real photos for every sourced food, emoji for any that fell back, and no broken images. Check a meal plan tab renders inline ingredient thumbnails.

- [ ] **Step 6: Commit**

```bash
git add images/
git commit -m "feat: curated self-hosted food image set"
```

---

## Self-Review Notes

- **Spec coverage:** WS1 → Task 3; WS2 → Tasks 1,4; WS3 → Tasks 1,2 + perf in Task 5; WS4 → Tasks 5,6. All covered.
- **Sequencing:** Tasks 1-4 are phase 1 (launch-ready SEO/reach); Tasks 5-6 are phase 2 (images). Phase 1 leaves the site on Unsplash images until Task 5 switches them — acceptable, emoji fallback is the safety net either way.
- **Slug consistency:** `imgSrc(name,large)` in Task 5 is the single source of image paths; Task 6 writes files at exactly those paths.
