# Book Study Publishing Guide

Read this guide before adding new HTML study notes to the blog's Book Study sector.

The public section lives at:

- Blog index: `book-study/index.html`
- Public URL: `https://medicihouse07.github.io/book-study/`

The design is subject-first:

1. A subject block, such as `Linear Algebra`, is visible on the Book Study index.
2. The subject block shows coverage before expansion: covered pages, total pages, percentage, and a progress bar.
3. Each subject has its own folder and subject shelf page.
4. Individual HTML notes live inside the relevant subject folder.

## Source Of Truth

The blog repo is the source of truth for Book Study HTML pages.

Store new generated HTML notes directly in this repository, under a subject folder:

```text
MediciHouse07/MediciHouse07.github.io/book-study/<subject-slug>/<note-slug>.html
```

Examples:

```text
book-study/linear-algebra/index.html
book-study/linear-algebra/linear-algebra-and-learning-from-data.html
book-study/linear-algebra/2026-07-04-taylor-expansion-expansion-point.html
book-study/calculus/2026-07-05-example-note.html
book-study/differential-equations/2026-07-05-example-note.html
```

Do not create a second copy in `Learning_Records` unless the user explicitly asks for that. Avoid wrapper/CDN patterns for new notes; direct storage in this repo is cleaner and avoids redundancy.

Future pages should be standalone HTML files committed directly to the right subject folder.

## UI Rule (Hexo/NexT Alignment)

Book Study pages should visually align with the current Hexo/NexT blog UI.

Load the existing blog assets and shared Book Study stylesheet:

```html
<link rel="stylesheet" href="/css/main.css">
<link rel="stylesheet" href="/lib/font-awesome/css/all.min.css">
<link rel="stylesheet" href="/book-study/book-study.css">
```

Rules:

- Use the standard outer shell: `book-study-shell`.
- Do not use NexT `use-motion` on standalone Book Study pages.
- Always include visibility guard CSS to prevent hidden content.
- Keep note pages left-aligned on mobile; do not justify headings or paragraphs.

Required visibility guard:

```css
.book-study-shell .brand,
.book-study-shell .menu-item,
.book-study-shell .post-block,
.book-study-shell .post-header,
.book-study-shell .post-body,
.book-study-shell .site-title,
.book-study-shell .site-subtitle {
  opacity: 1 !important;
  top: auto !important;
  transform: none !important;
}
```

## Mathematical Formula Rule

Mathematical notes should render formulas as math, not as dark code blocks.

Preferred approach:

- Use native MathML for formulas.
- Wrap display formulas in `.math-display`.
- Wrap inline formulas in `.math-inline`.
- Keep long formulas inside their own scrollable math box.
- Do not put math formulas in `pre`, `code`, or old `.formula` blocks unless the content is actually source code.
- For partial derivative symbols, use `<mo>&#x2202;</mo>`. Do not use `&partial;`, because some browsers render it literally.
- If a future note needs LaTeX syntax rendered by MathJax or KaTeX, get explicit approval before adding any external JavaScript dependency.

Example display formula:

```html
<div class="math-display">
  <math display="block">
    <mi>f</mi><mo>(</mo><mi>x</mi><mo>)</mo>
    <mo>&approx;</mo>
    <mi>f</mi><mo>(</mo><mi>a</mi><mo>)</mo>
    <mo>+</mo>
    <msup><mi>f</mi><mo>&prime;</mo></msup><mo>(</mo><mi>a</mi><mo>)</mo>
    <mo>(</mo><mi>x</mi><mo>-</mo><mi>a</mi><mo>)</mo>
  </math>
</div>
```

Example partial derivative formula:

```html
<div class="math-display">
  <math display="block">
    <mi>J</mi><mo>=</mo>
    <mfrac>
      <mrow><mo>&#x2202;</mo><mo>(</mo><mi>x</mi><mo>,</mo><mi>y</mi><mo>)</mo></mrow>
      <mrow><mo>&#x2202;</mo><mo>(</mo><mi>u</mi><mo>,</mo><mi>v</mi><mo>)</mo></mrow>
    </mfrac>
  </math>
</div>
```

Example inline formula:

```html
<span class="math-inline"><math><mi>J</mi><mo>=</mo><mi>r</mi></math></span>
```

## Mobile Responsiveness Rule (Important)

All Book Study HTML pages must work on mobile phones.

Target viewport:

```text
360px wide minimum, tested in Chrome mobile view or a real phone browser
```

Acceptance standard:

- The page itself must not have horizontal scrolling.
- Text must remain readable without zooming.
- Cards, panels, formulas, diagrams, and tables must stay inside the phone viewport.
- Only code blocks, formulas, and tables may scroll horizontally, and only inside their own box.
- The browser should not show clipped text like a sentence or panel disappearing off the right side.
- Visual bars, axes, progress bars, and diagrams must be responsive SVG or percentage-based CSS, never fixed-width desktop elements.

Required mobile CSS baseline for every note page:

```css
html,
body {
  max-width: 100%;
  overflow-x: hidden;
}

*,
*::before,
*::after {
  box-sizing: border-box;
}

.book-study-shell,
.main,
.main-inner,
.content-wrap,
.content,
.post-block,
.post-body,
.page,
section,
article,
.card,
.panel,
.mini-panel,
.hero,
.study-frame,
.math-display {
  max-width: 100%;
}

.post-body,
.post-body h1,
.post-body h2,
.post-body h3,
.post-body p,
.post-body li,
.post-body td,
.post-body th {
  text-align: left;
}

.post-body,
.page,
section,
article,
.card,
.panel,
.mini-panel {
  overflow-wrap: break-word;
}

img,
svg,
canvas,
video,
iframe {
  max-width: 100%;
  height: auto;
}

.math-display {
  max-width: 100%;
  overflow-x: auto;
  overflow-y: hidden;
  -webkit-overflow-scrolling: touch;
}

pre,
code,
.code-block {
  max-width: 100%;
  overflow-x: auto;
  white-space: pre;
}

table {
  width: 100%;
  max-width: 100%;
  display: block;
  overflow-x: auto;
}

@media (max-width: 768px) {
  .main-inner,
  .content-wrap,
  .content,
  .post-block {
    width: 100% !important;
    max-width: 100% !important;
    margin-left: 0 !important;
    margin-right: 0 !important;
  }

  .post-block,
  .post-body,
  .page,
  section,
  article,
  .card,
  .panel,
  .mini-panel,
  .hero {
    padding-left: 16px !important;
    padding-right: 16px !important;
  }

  .hero-grid,
  .visual-grid,
  .split,
  .cards,
  .flow,
  .note-grid,
  .subject-header,
  .coverage {
    grid-template-columns: 1fr !important;
  }

  h1 {
    font-size: clamp(1.6rem, 8vw, 2.2rem) !important;
    line-height: 1.15;
  }

  h2 {
    font-size: 1.35rem !important;
    line-height: 1.2;
  }

  p,
  li,
  td,
  th {
    font-size: 1rem;
    line-height: 1.6;
  }
}
```

Rules for generated note HTML:

- Do not use fixed desktop widths like `width: 1140px` on main containers.
- Use `width: min(100%, 1140px)` or `max-width: 1140px; width: 100%;` instead.
- Do not use large fixed left/right padding on mobile.
- Do not place wide formulas directly in normal paragraphs.
- Put display formulas in `.math-display` using MathML.
- Do not use multi-column grid layouts without a mobile collapse rule.
- SVG diagrams must use `viewBox` and `max-width: 100%`.
- If an iframe is used for a legacy study artifact, the iframe must be `width: 100%`, must not set a fixed pixel width, and must be height-adjusted after content loads.

Mobile test checklist:

1. Open the page at 360px width.
2. Scroll vertically from top to bottom.
3. Confirm the body itself does not move left/right.
4. Confirm long formulas scroll only inside their formula box.
5. Confirm tables scroll only inside the table box.
6. Confirm every card/panel fits inside the viewport.
7. Confirm the same page still looks acceptable on desktop.

## Add A New Note To An Existing Subject

1. Create the standalone HTML note directly in this blog repo:

```text
book-study/<subject-slug>/<note-slug>.html
```

2. Update subject shelf and index page metadata.

3. Keep progress bar consistent across all pages.

4. Apply the required mobile CSS baseline from this guide.

5. Test the page at 360px width before commit.

## Add A New Subject

1. Create folder:

```text
book-study/<subject-slug>/
```

2. Create:

```text
book-study/<subject-slug>/index.html
```

3. Register subject in main index.

4. Apply the same mobile rules to the subject shelf page.

## Progress Calculation

```text
covered_pages / total_pages * 100
```

Round to one decimal place.

## Checklist Before Commit

- New note exists under correct subject folder.
- Shelf page exists for subject.
- Index updated.
- Progress bar updated consistently.
- Mobile layout tested at 360px width.
- No page-level horizontal scroll on mobile.
- Formulas use MathML in `.math-display` or `.math-inline`.
- Code blocks, formulas, and tables scroll only inside their own boxes.
- Cards, panels, diagrams, and iframes fit inside the phone viewport.
- No fixed desktop-only widths on main containers.
- No `use-motion` class in standalone pages.
- Visibility guard included.