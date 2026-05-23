# Writing a new post

Self-hosted posts on ivolinengong.com. Plain HTML, no build step.
Save your file → push to GitHub → it's live.

## The 3-step workflow

### 1. Create the post file

Copy `_template.html` to a new file. Use the `YYYY-MM-DD-slug` naming
convention so posts sort chronologically in the directory listing:

```bash
cp posts/_template.html posts/2025-08-15-thoughts-on-dp.html
```

Slug = a short lowercase URL-friendly version of the title
(use hyphens, no spaces, no punctuation).

### 2. Edit the post

Open your new file. Replace these placeholders:

| Where | Replace with |
|---|---|
| `<title>POST TITLE — Ivoline Ngong</title>` | The real title |
| `<meta name="description" content="...">` | One-sentence summary (for search engines + link previews) |
| `<h2 class="post-title">POST TITLE</h2>` | The post title shown on the page |
| `<p class="post-date">Month YYYY</p>` | e.g. `August 2025` |
| Body paragraphs | Your actual content |

Plain HTML works as-is — no Markdown, no preprocessor. The site's CSS
already styles every common element:

```html
<p>Paragraphs.</p>
<p>Inline <a href="https://example.com" target="_blank" rel="noopener noreferrer">external link</a>,
  <em>italics</em>, <strong>bold</strong>, inline <code>code</code>.</p>

<h2>Optional section heading</h2>

<ul>
  <li>List item</li>
  <li>Another list item</li>
</ul>

<blockquote>
  <p>Pull-quote.</p>
</blockquote>

<pre><code>// Code block — escape angle brackets as &lt; and &gt;
function example() { return "looks like code"; }
</code></pre>

<img src="../assets/your-image.png" alt="Description of the image">
```

### 3. Link it from the Writing index

Open `writing.html`. Find the year section for your post (or add a new
`<section class="writing-year-group">` if it's a new year — newest year
goes first). Newer entries go at the **top** of their year group:

```html
<section class="writing-year-group">
  <h3>2025</h3>

  <article class="writing-entry">
    <p class="writing-body">
      <a href="posts/2025-08-15-thoughts-on-dp.html">Thoughts on DP:</a>
      Short one-line description that shows on the Writing index.
    </p>
  </article>

  <!-- older 2025 posts below this one -->
</section>
```

The link to your post is **internal** (relative path, no leading `/`)
so it stays in the same tab — the visitor lands on the post and can
use the top nav or the "← Back to Writing" link to navigate.

That's it. Save, commit, push.

---

## Conventions & gotchas

### Path prefixes inside `posts/`

Files inside `posts/` are one directory deep, so every reference back
to root files needs `../`. The template already has this right:

| Where | Correct path |
|---|---|
| Stylesheet | `<link rel="stylesheet" href="../style.css">` |
| Site title (links Home) | `<a class="site-title" href="../index.html">` |
| Nav items | `href="../index.html"`, `href="../publications.html"`, etc. |
| Images stored in `/assets/` | `<img src="../assets/your-image.png">` |
| Skip link (same-page anchor) | `href="#main"` (no prefix — same page) |

**Active nav state.** The template sets `aria-current="page"` on the
Writing nav item — leave it there. When a visitor opens your post, the
"Writing" link in the top nav will highlight in teal so they know
where they are.

### Date format on the post

Use a human-readable date like `August 2025` (matching the wayback
style) rather than `2025-08-15`. The filename's ISO date is for
file-system sorting; the displayed date is for readers.

### External links open in a new tab

When you link out to a paper, a video, a tweet, an arXiv page, etc.,
add `target="_blank" rel="noopener noreferrer"` so the visitor doesn't
leave the site:

```html
<a href="https://arxiv.org/abs/1234.56789" target="_blank" rel="noopener noreferrer">
  the paper
</a>
```

Internal links (to other site pages, other posts, same-page anchors,
`mailto:` addresses) should NOT use `target="_blank"`.

### Images

Drop images into `/assets/` (or a sub-folder like `/assets/posts/`
if you want to keep them grouped). Reference them with `../assets/...`
from inside a post. Aim to keep images well under 500 KB each — large
images slow first paint.

If you need an image to display large (like the PATE comic), use the
`comic-post` class on the `<article>` instead of plain `post` — that
widens the page to 1000px and lets the image breathe. See
`posts/pate-comic.html` for a working example.

### Don't touch the shell

The `<header class="site-header">`, the nav, the `<a class="skip-link">`,
and the `<link rel="stylesheet" href="../style.css">` are part of the
shared site shell. Don't customise them per post — if every post uses
the same shell, the site stays coherent.

### What if I need a new CSS rule?

If a post needs something the existing CSS doesn't cover (a new layout,
a custom block, etc.), add the rule to the main `style.css` rather than
inline-styling the post. The same rule is then available to every
future post. Add a comment marking which post first needed the rule.

---

## Quick reference: copy-paste snippets

### A 3-paragraph standard post body

```html
<p>Opening paragraph. Hook the reader with the problem or the punchline.</p>

<p>Middle paragraph. Develop the idea. Use inline
<a href="https://example.com" target="_blank" rel="noopener noreferrer">links</a>
to evidence or references.</p>

<p>Closing paragraph. Where to go next; how to reach out
(<a href="mailto:ivolinengong@gmail.com">email</a>).</p>
```

### A new year section in `writing.html`

```html
<section class="writing-year-group">
  <h3>2026</h3>
  <article class="writing-entry">
    <p class="writing-body">
      <a href="posts/2026-XX-XX-slug.html">Post Title:</a>
      One-line description.
    </p>
  </article>
</section>
```

(Put the new year section **above** the existing `<section>` for the
previous year — Writing is reverse-chronological.)

---

## After saving

Commit and push to GitHub:

```bash
git add posts/2025-08-15-thoughts-on-dp.html writing.html
git commit -m "writing: add 'Thoughts on DP' post"
git push
```

GitHub Pages picks it up within a minute or two — refresh
ivolinengong.com to see it live.
