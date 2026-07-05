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

## UI Rule

Book Study pages should visually align with the current Hexo/NexT blog UI.

For new subject shelves and new note pages:

- Load the existing blog assets:

```html
<link rel="stylesheet" href="/css/main.css">
<link rel="stylesheet" href="/lib/font-awesome/css/all.min.css">
```

- Use the same outer shell pattern as the current Book Study pages.
- Do not add NexT's `use-motion` class to standalone Book Study pages. These pages do not run the full generated Hexo motion lifecycle, so `use-motion` can leave content hidden.
- Include the visibility guard below in the page-level `<style>` block.

```html
<style>
  .book-study-shell .post-body { font-size: 1rem; }
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
</style>

<body itemscope itemtype="http://schema.org/WebPage">
  <div class="container book-study-shell">
    <div class="headband"></div>
    <header class="header" itemscope itemtype="http://schema.org/WPHeader">
      <!-- Medici House brand and site nav -->
    </header>
    <main class="main">
      <div class="main-inner">
        <div class="content-wrap">
          <div class="content">
            <article class="post-block" itemscope itemtype="http://schema.org/Article">
              <header class="post-header">
                <h1 class="post-title" itemprop="name headline">Note Title</h1>
              </header>
              <div class="post-body" itemprop="articleBody">
                <!-- Study note content -->
              </div>
            </article>
          </div>
        </div>
      </div>
    </main>
    <footer class="footer"><!-- Hexo/NexT style footer --></footer>
  </div>
</body>
```

The study content can still have richer diagrams or dashboards, but the page frame, navigation, and footer should feel like the rest of the Hexo site.

## Add A New Note To An Existing Subject

1. Create the standalone HTML note directly in this blog repo:

```text
book-study/<subject-slug>/<note-slug>.html
```

2. In `book-study/index.html`, find the relevant subject block.
3. In `book-study/<subject-slug>/index.html`, update that subject shelf too.
4. Update metadata in both places as needed:

```html
<span>3 notes</span>
<span>50 pages covered</span>
<span>415 main pages total</span>
```

5. Update the progress bar width and visible text:

```html
<span class="progress-bar" style="width: 12%;"></span>
<strong>50 / 415 pages, 12.0%</strong>
```

If the CSS class sets a fixed width, use an inline `style="width: ...%;"` on that subject's `.progress-bar`.

6. Add a note card inside that subject's `.note-grid`:

```html
<article class="note-card">
  <div class="note-date">YYYY-MM-DD</div>
  <h3>Note Title</h3>
  <p>One short sentence describing what this note clarifies.</p>
  <a class="note-link" href="/book-study/<subject-slug>/<note-slug>.html">Open note</a>
</article>
```

7. Make sure the note page links back to the subject shelf, Book Study index, and home page:

```html
<nav class="book-study-actions" aria-label="Book study note navigation">
  <a href="/book-study/<subject-slug>/">Subject shelf</a>
  <a class="secondary" href="/book-study/">Book Study</a>
  <a class="secondary" href="/">Home</a>
</nav>
```

## Add A New Subject

1. Create a subject folder:

```text
book-study/<subject-slug>/
```

2. Create its subject shelf:

```text
book-study/<subject-slug>/index.html
```

3. Add a collapsed subject block inside `book-study/index.html`:

```html
<details class="subject-block">
  <summary>
    <div class="subject-header">
      <div>
        <h2>Subject Name</h2>
        <p>Short description of the book, course, or subject cluster.</p>
        <a class="subject-link" href="/book-study/<subject-slug>/">Open subject shelf</a>
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
      <a class="note-link" href="/book-study/<subject-slug>/<note-slug>.html">Open note</a>
    </article>
  </div>
</details>
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

Use lowercase, hyphen-separated slugs for subject folders and notes:

```text
book-study/<subject-slug>/<note-slug>.html
```

Prefer dated filenames for timestamped updates and stable filenames for dashboards.

Examples:

```text
book-study/linear-algebra/linear-algebra-and-learning-from-data.html
book-study/linear-algebra/2026-07-04-taylor-expansion-expansion-point.html
book-study/calculus/2026-07-05-example-note-title.html
```

## Checklist Before Commit

- The new standalone HTML note exists directly under `book-study/<subject-slug>/` in this blog repo.
- The subject has a shelf page at `book-study/<subject-slug>/index.html`.
- The note uses the Hexo/NexT-style outer shell and links back to `/book-study/<subject-slug>/`, `/book-study/`, and `/`.
- The note shell uses `class="container book-study-shell"`, not `class="container use-motion book-study-shell"`.
- The note includes the Book Study visibility guard CSS from this guide.
- The main `book-study/index.html` subject block has updated note count, page count, percent text, and progress width.
- The subject shelf page has updated note count, page count, percent text, and progress width.
- The note card appears under the correct subject.
- Existing blog navigation does not need page-by-page edits; `js/next-boot.js` already injects the `Book Study` menu item into generated Hexo pages.
- Keep `.nojekyll` in the blog repo so GitHub Pages serves static files directly.
