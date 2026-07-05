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

## UI Rule (Hexo/NexT alignment)

Book Study pages should visually align with the current Hexo/NexT blog UI.

- Load the existing blog assets:

```html
<link rel="stylesheet" href="/css/main.css">
<link rel="stylesheet" href="/lib/font-awesome/css/all.min.css">
```

- Use the standard outer shell (`book-study-shell`).
- Do NOT use NexT `use-motion` on standalone Book Study pages.
- Always include visibility guard CSS to prevent hidden content.

## Mobile Responsiveness Rule (IMPORTANT)

All Book Study HTML pages MUST be mobile-friendly.

Requirements:

- Layout must adapt to screens as small as 360px width
- No horizontal scrolling allowed
- All containers must use `max-width: 100%`
- Use responsive grid collapse (multi-column → single column on mobile)
- Code blocks must wrap or scroll horizontally:

```css
pre, code { overflow-x: auto; }
```

- Tables must not break layout:

```css
table { width: 100%; display: block; overflow-x: auto; }
```

- Images must be responsive:

```css
img { max-width: 100%; height: auto; }
```

- Iframes (used in study embeds) must be responsive and height-adjustable
- Reduce padding and font size on small screens using media queries:

```css
@media (max-width: 768px) {
  .page { padding: 16px; }
  h1 { font-size: 1.6rem; }
}
```

- Avoid fixed pixel widths for layout containers
- Prefer flex/grid layouts that collapse naturally

## Add A New Note To An Existing Subject

1. Create the standalone HTML note directly in this blog repo:

```text
book-study/<subject-slug>/<note-slug>.html
```

2. Update subject shelf and index page metadata.

3. Keep progress bar consistent across all pages.

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

## Progress Calculation

```text
covered_pages / total_pages * 100
```

Round to one decimal place.

## Checklist Before Commit

- New note exists under correct subject folder
- Shelf page exists for subject
- Index updated
- Progress bar updated consistently
- Mobile layout tested (360px width)
- No horizontal scroll
- No `use-motion` class in standalone pages
- Visibility guard included
