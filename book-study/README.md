# Book Study Publishing Guide

Read this guide before adding new HTML study notes to the blog's Book Study sector.

The public section lives at:

- Blog index: `book-study/index.html`
- Public URL: `https://medicihouse07.github.io/book-study/`

The current design is subject-first:

1. A subject block, such as `Linear Algebra`, is visible on the Book Study index.
2. The subject block shows coverage before expansion: covered pages, total pages, percentage, and a progress bar.
3. Opening the subject reveals individual HTML note cards.
4. Each note card links to a blog-side viewer page under `book-study/`.

## Source Of Truth

Use `MediciHouse07/Learning_Records` as the source of truth for generated study HTML files when possible.

Recommended source path:

```text
MediciHouse07/Learning_Records/book-study/<note-slug>.html
```

The blog repo should usually store a lightweight viewer page that loads the source HTML from jsDelivr:

```text
https://cdn.jsdelivr.net/gh/MediciHouse07/Learning_Records@main/book-study/<note-slug>.html
```

This keeps the learning repo as the archive and lets the blog present the note inside the Book Study sector.

## Add A New Note To An Existing Subject

1. Create or confirm the source HTML in `MediciHouse07/Learning_Records/book-study/`.
2. Create a viewer page in this blog repo:

```text
book-study/<note-slug>.html
```

3. In `book-study/index.html`, find the relevant subject block.
4. Update its metadata:

```html
<span>3 notes</span>
<span>50 pages covered</span>
<span>415 main pages total</span>
```

5. Update its progress bar width and text:

```html
<span class="progress-bar" style="width: 12%;"></span>
<strong>50 / 415 pages, 12.0%</strong>
```

If the existing CSS class sets a fixed width, add an inline `style="width: ...%;"` on the subject's `.progress-bar` for that subject.

6. Add a new note card inside that subject's `.note-grid`:

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

## Viewer Page Template

Create one viewer page per source HTML file.

Replace:

- `<note-title>`
- `<note-slug>`
- `<source-github-url>`
- `<source-cdn-url>`

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title><note-title> | Book Study</title>
  <meta name="description" content="Book study record.">
  <link rel="icon" type="image/png" sizes="32x32" href="/images/florence.png">
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      background: #f7f8fa;
      color: #26323f;
      font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
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
    #status {
      width: min(760px, calc(100% - 32px));
      margin: 36px auto;
      padding: 18px;
      border: 1px solid #dce3eb;
      border-radius: 10px;
      background: #fff;
      color: #607286;
    }
    iframe {
      width: 100%;
      min-height: calc(100vh - 58px);
      border: 0;
      display: block;
      background: #fbfaf7;
    }
  </style>
</head>
<body>
  <header class="topbar">
    <strong>Book Study: <note-title></strong>
    <nav class="links" aria-label="Book study navigation">
      <a class="primary" href="/book-study/">Book Study</a>
      <a href="/">Home</a>
      <a href="<source-github-url>" rel="noopener" target="_blank">Source</a>
    </nav>
  </header>
  <div id="status">Loading study record from Learning_Records...</div>
  <iframe id="record" title="<note-title> study record" hidden></iframe>
  <script>
    const sourceUrl = '<source-cdn-url>';
    const iframe = document.getElementById('record');
    const statusBox = document.getElementById('status');

    fetch(sourceUrl)
      .then(response => {
        if (!response.ok) throw new Error('HTTP ' + response.status);
        return response.text();
      })
      .then(html => {
        iframe.srcdoc = html;
        iframe.hidden = false;
        statusBox.hidden = true;
      })
      .catch(error => {
        statusBox.innerHTML = 'Could not load the embedded study record. <a href="<source-github-url>" target="_blank" rel="noopener">Open the source file on GitHub</a>.';
        console.error(error);
      });
  </script>
</body>
</html>
```

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

Use lowercase, hyphen-separated slugs for new blog viewer pages:

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

- The source HTML exists in `MediciHouse07/Learning_Records/book-study/` or the note is intentionally stored directly in the blog repo.
- The blog viewer page exists under `book-study/`.
- The subject block in `book-study/index.html` has updated note count, page count, percent text, and progress width.
- The note card appears under the correct subject.
- Existing blog navigation does not need page-by-page edits; `js/next-boot.js` already injects the `Book Study` menu item.
- Keep `.nojekyll` in the blog repo so GitHub Pages serves static files directly.
