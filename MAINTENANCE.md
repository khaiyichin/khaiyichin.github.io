# Adding content to this site

Practical instructions for the two things most likely to be added: a new work
experience page, and a new project entry. Both are more involved than they look,
because the site is hand-written static HTML on top of an old template. The
[Traps](#traps) section at the end explains the parts that will bite you; it is
worth skimming before you start.

There is **no build step**. `assets/css/main.css` is the source — edit it directly.
Open any `.html` file straight from disk to preview.

---

## 1. Adding a new work experience page

### Files you will touch

| What | Where |
|---|---|
| The new page | `work-experiences/<slug>.html` |
| Images | `work-experiences/images/` |
| Nav menu | **all 15 HTML files** — see below |
| Hub page: sidebar list + teaser | `work-experiences/work-experiences.html` |
| "Last updated" date | `assets/js/main.js` |

### Steps

**1. Create the page.** Copy an existing one — `work-experiences/nestlab.html` is a
good model — and rename it. The page skeleton is:

```html
<article class="box page-content">
  <header>
    <h1>NEST Lab</h1>              <!-- employer / lab name -->
    <p>Graduate Research Assistant</p>   <!-- role -->
  </header>

  <section class="introduction">
    <span class="image left square" title="..."><img src="images/xxx1.jpg" alt="..."></span>
    <p>One-paragraph summary.</p>
  </section>

  <section class="work-experience section">
    <h3 class="individual">Background</h3>
    <p>...</p>
  </section>

  <section class="work-experience section">
    <h3 class="individual">My Work and Experience</h3>
    <span class="image left square" title="..."><img src="images/xxx2.jpg" alt="..."></span>
    ... (see "Exactly four images" below)
    <p>...</p>
  </section>

  <section>
    <h3 class="individual">External Links and Materials</h3>
    <h4 class="initial h4">Links</h4>
    <li><a href="...">...</a></li>
  </section>
</article>
```

Use these classes as-is — they are what the responsive rules key off:

- `section.introduction` — the lead section. **Do not give it a fixed height.**
- `section.work-experience.section` — each body section.
- `h3.individual` — section headings.
- `h4.initial.h4` — sub-headings.
- `span.image.left.square` / `span.image.right.square` — photos (275px on desktop,
  full width on phones).

**2. Update the nav in all 15 HTML files.** The nav block is copy-pasted into every
page, so a new entry has to be added 15 times, inside the "Work & Leadership
Experiences" list. The `href` differs by the file's location:

| File location | href to use |
|---|---|
| `index.html` | `work-experiences/<slug>.html` |
| `work-experiences/*.html` | `<slug>.html` |
| `interests/interests.html` | `../work-experiences/<slug>.html` |

Also set `class="current"` on the correct top-level `<li>` in your new page (for a
work experience page, that's the "Work & Leadership Experiences" item). Each page
has exactly one `class="current"`.

**3. Add it to the hub page** (`work-experiences/work-experiences.html`) in two
places:

- The **sidebar list** — a `<li>` containing
  `<article class="box post-summary"><h3><a href="…">Employer</a></h3><h5><a href="…">Role</a></h5></article>`.
  This list is hidden on phones.
- The **teaser** — an `<article class="box page-content">` with a `<header>`
  (`<h2>` employer, `<h4>` location, `<p>` role), then a
  `<section class="work-experience section">` with the logo and a summary ending in
  `<a href="<slug>.html">Read more</a>`.

**4. Add images** to `work-experiences/images/`. Roughly square photos work best —
they are placed in fixed-size square boxes.

**5. Update the date.** `assets/js/main.js`, line 8:

```js
var update_date = "July 2026";
```

This feeds the "Last Updated" line in every footer.

### Exactly four images

If a `.work-experience` section contains **exactly four** `span.image.left.square`
elements, a special rule kicks in that shrinks them to 23% width so all four fit on
one row on narrow screens, centres the row, and makes the following paragraph start
below them.

**Any other number (1, 2, 3, 5, …) does not get this** and stays at the standard
275px. With five images on a narrow screen they will wrap untidily. If you want a
row of thumbnails, use four. Otherwise one or two reads fine.

---

## 2. Adding a new project entry

All in `work-experiences/projects-resume.html`, inside the existing
`<article class="box page-content">`, after the `<h1>Projects</h1>` header.

Add a `<section class="project-entry">` — newest first, since entries are ordered
by recency:

```html
<section class="project-entry">
  <h4 class="initial h4">2026 - Project Name</h4>
  <span class="image right square project" title="Alt text"><img
      src="images/projects-xxx.png" alt="Alt text"> </span>
  <a href="https://github.com/khaiyichin/repo" target="_blank" aria-label="GitHub"
     style="display: inline-flex; align-items: center; text-decoration: none; color: black; font-family: 'Ubuntu', 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica, Arial, sans-serif; font-size: 16px; font-weight: 600; padding: .5em 0 .5em 0">
    <img src="https://github.com/fluidicon.png" alt="GitHub" width="30" height="30"
         style="margin-right: 8px;">
    Project Name
  </a>
  <p>Description.</p>
</section>
```

Notes:

- **`class="project-entry"`** is required. It supplies the minimum height that
  reserves room for the floated thumbnail on wide screens, and it makes the section
  contain that float on narrower ones. Without it the thumbnail overhangs into the
  next entry and pushes the following entry's thumbnail out of the right margin.
- **`class="image right square project"`** — the `project` keyword makes the
  thumbnail 200px rather than the standard 275px. Do not put the size in a `style`
  attribute; an inline style beats the phone rules and the image will stay 200px and
  floated on a phone instead of going full width.
- The GitHub link's long inline `style` is copy-pasted per entry. It is duplication,
  but keep it consistent with the existing entries unless you extract it into a class
  in `main.css` for all of them at once.
- Put the image `<span>` **before** the `<a>` and `<p>`, as above.
- Update `update_date` in `assets/js/main.js`.

---

## Checking your work

There is no test suite. Open the page and check it at three widths, which is where
the layout rules change:

| Width | What to look for |
|---|---|
| **~375px** (phone) | Images full width, nothing overlapping, title larger than the section headings below it |
| **~960px** (half-screen desktop) | This is the one people forget. Half of a 1920px monitor. Check the four-image row fits on one line, headings sit on their grey rule, and the last entry does not run into the footer |
| **~1400px** (desktop) | Normal layout |

In a browser's device toolbar, phone sizes need **device emulation on** — just
narrowing a desktop window is not the same, because the site rewrites its own
viewport at tablet widths and a narrowed desktop window ignores that.

Also scroll to the bottom: a common failure is the last entry colliding with the
footer.

---

## Traps

Things in this codebase that are surprising, and why they are still here. Each is
commented at its location in `main.css`.

**The nav is duplicated 15 times.** There is no include mechanism. Adding or renaming
a page means editing every file. The copies have not drifted so far — they differ
only in href prefixes and which item is `current` — but nothing prevents it. Fixing
this needs either a Jekyll build (GitHub Pages runs it, but then pages cannot be
previewed straight from disk) or a generator script (which would put a source and a
generated copy in the repo — the same trap that made the old Sass tree dangerous).

**Spacing values compensate for the template, not for taste.** Several margins are
tuned to cancel behaviour in the underlying template rather than to produce the gap
they name. `.box.page-content header` uses `calc(3.125em + 25px)` because 25px of it
is swallowed by margin collapsing. `h2.major` carries a 2em top margin because the
grid lets content spill about 25px past its row. If a gap looks wrong, read the
comment before changing the number.

**`article > :last-child` gets `margin-bottom: -25px`.** This is part of the
template's grid arithmetic. It assumes the last child carries roughly a gutter's
worth of bottom margin, which is true for paragraphs and false for headings —
a section ending in a heading gets pulled up about 18px too far. **Prefer ending a
section with a paragraph.**

**Fixed pixel sizes are desktop-only.** The custom rules near the top of `main.css`
are in pixels and assume a desktop-width column; the `max-width: 736px` block near
the bottom relaxes them for phones. If you add a rule with a fixed px width or
height there, add the phone counterpart too, or it will overflow the ~335px column.

**Inline `style` attributes beat the responsive rules.** They cannot be overridden by
the phone media query. Use a class instead.

**Tablets deliberately render a scaled-down desktop layout.** `assets/js/main.js`
tells the page to lay out at 1080px on tablets, and the 737–1200px CSS assumes that.
The duplicate `<meta name="viewport">` tags this produces are intentional — do not
"clean them up".

**`assets/css/main.css` is the only stylesheet source.** The template's Sass tree was
removed because it had gone stale and compiling it would have silently reverted years
of edits. Do not reintroduce a second source without a build step that actually runs.
