# Book Study Publishing Guide

Read this guide before adding new HTML study notes to the blog's Book Study sector.

The public section lives at:

- Blog index: `book-study/index.html`
- Public URL: `https://medicihouse07.github.io/book-study/`

The design is subject-first:

1. A subject block, such as `Linear Algebra`, is visible on the Book Study index.
2. The subject block shows coverage before expansion: covered pages, total pages, percentage, and a progress bar.
3. Opening the subject reveals individual HTML note cards.
4. Each note card links to a standalone HTML note under `book-study/`.

## Source Of Truth

From now on, the blog repo is the source of truth for Book Study HTML pages.

Store new generated HTML notes directly in this repository:

```text
MediciHouse07/MediciHouse07.github.io/book-study/<note-slug>.html
```

Do not create a second copy in `Learning_Records` unless the user explicitly asks for that. Avoid the older wrapper/CDN pattern for new notes; it was useful for the first import, but direct storage in this repo is cleaner and avoids redundancy.

Existing older pages may still be wrappers that load from `Learning_Records`. Future pages should be standalone HTML files committed directly to `book-study/`.

## Add A New Note To An Existing Subject

1. Create the standalone HTML note directly in this blog repo:

```text
book-study/<note-slug>.html
```

2. In `book-study/index.html`, find the relevant subject block.
3. Update the subject metadata:

```html
<span>3 notes</span>
<span>50 pages covered</span>
<span>415 main pages total</span>
```

4. Update the progress bar width and visible text:

```html
<span class="progress-bar" style="width: 12%;"></span>
<strong>50 / 415 pages, 12.0%</strong>
```

If the CSS class sets a fixed width, use an inline `style="width: ...%;"` on that subject's `.progress-bar`.

5. Add a note card inside that subject's `.note-grid`:

```html
<article class="note-card">
  <div class="note-date">YYYY-MM-DD</div>
  <h3>Note Title</h3>
  <p>One short sentence describing what this note clarifies.</p>
  <a class="note-link" href="/book-study/<note-slug>.html">Open note</a>
</article>
```

## Add A New Subject

Add a new collapsed subject block inside:

```html
<section class="subject-list" aria-label="Book study subjects">
```

Use this pattern:

```html
<details class="subject-block">
  <summary>
    <div class="subject-header">
      <div class="subject-title">
        <h2>Subject Name</h2>
        <p>Short description of the book, course, or subject cluster.</p>
      </div>
      <span class="subject-toggle">Tap to</span>
    </div>

    <div class="book-meta" aria-label="Subject metadata">
      <span>1 note</span>
      <span>1 book tracked</span>
      <span>50 pages covered</span>
      <span>400 pages total</span>
    </div>

    <div class="coverage" aria-label="Subject book coverage">
      <div class="progress-track" aria-hidden="true"><span class="progress-bar" style="width: 12.5%;"></span></div>
      <strong>50 / 400 pages, 12.5%</strong>
      <p class="coverage-caption">State the counting rule, for example main chapter pages or full physical pages.</p>
    </div>
  </summary>

  <div class="note-grid" aria-label="Subject notes">
    <article class="note-card">
      <div class="note-date">YYYY-MM-DD</div>
      <h3>Note Title</h3>
      <p>One short sentence describing the note.</p>
      <a class="note-link" href="/book-study/<note-slug>.html">Open note</a>
    </article>
  </div>
</details>
```

## Standalone Note Requirements

Each new note should be a complete HTML document. It should not depend on another repo for its core content.

Recommended minimum structure:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Note Title | Book Study</title>
  <meta name="description" content="Book study record.">
  <link rel="icon" type="image/png" sizes="32x32" href="/images/florence.png">
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      background: #fbfaf7;
      color: #26323f;
      font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      line-height: 1.55;
    }
    .topbar {
      position: sticky;
      top: 0;
      z-index: 10;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 16px;
      padding: 12px 18px;
      border-bottom: 1px solid #dce3eb;
      background: rgba(255, 255, 255, 0.94);
      backdrop-filter: blur(10px);
    }
    .topbar strong { font-size: 0.95rem; }
    .links { display: flex; gap: 10px; flex-wrap: wrap; }
    .links a {
      color: #26323f;
      text-decoration: none;
      border: 1px solid #dce3eb;
      border-radius: 999px;
      padding: 7px 11px;
      font-size: 0.86rem;
      background: #fff;
    }
    .links a.primary { color: #fff; background: #2563eb; border-color: #2563eb; }
    .page {
      width: min(1120px, calc(100% - 32px));
      margin: 0 auto;
      padding: 28px 0 56px;
    }
  </style>
</head>
<body>
  <header class="topbar">
    <strong>Book Study: Note Title</strong>
    <nav class="links" aria-label="Book study navigation">
      <a class="primary" href="/book-study/">Book Study</a>
      <a href="/">Home</a>
    </nav>
  </header>

  <main class="page">
    <!-- Put the generated study note content here. -->
  </main>
</body>
</html>
```

A generated note can use its own richer CSS and layout, but it should keep a visible path back to `/book-study/` and `/`.

## Progress Calculation

Use:

```text
covered_pages / total_pages * 100
```

Round to one decimal place in the visible text. Use the same value for the progress-bar width.

Example:

```text
50 / 400 = 12.5%
```

```html
<span class="progress-bar" style="width: 12.5%;"></span>
<strong>50 / 400 pages, 12.5%</strong>
```

## Naming Rules

Use lowercase, hyphen-separated slugs for new HTML notes:

```text
book-study/2026-07-05-example-note-title.html
```

Prefer dated filenames for timestamped updates and stable filenames for dashboards.

Examples:

```text
book-study/linear-algebra-and-learning-from-data.html
book-study/2026-07-04-taylor-expansion-expansion-point.html
```

## Checklist Before Commit

- The new standalone HTML note exists directly under `book-study/` in this blog repo.
- The note is a complete HTML document and does not require a duplicate source file in another repo.
- The note has navigation back to `/book-study/` and `/`.
- The subject block in `book-study/index.html` has updated note count, page count, percent text, and progress width.
- The note card appears under the correct subject.
- Existing blog navigation does not need page-by-page edits; `js/next-boot.js` already injects the `Book Study` menu item.
- Keep `.nojekyll` in the blog repo so GitHub Pages serves static files directly.
