# Website Publisher — publish a blog post

Machine-clear steps for the Website Publisher bot. Follow in order. Do not skip safety rules.

## Prerequisites

- Working copy of this repo (site root = folder containing `index.html`, `css/`, `blog/`, `data/`).
- Post content ready (title, description, body HTML, optional tags, publish date).
- Slug chosen (see Naming rules).
- Do **not** touch Hostinger, DNS, or add a `CNAME` file.
- Do **not** add dosing, sourcing, sales, guarantees, or medical prescriptions.

## Naming rules for `slug`

- Lowercase ASCII letters, digits, and hyphens only: `^[a-z0-9]+(?:-[a-z0-9]+)*$`
- No spaces, underscores, dots, or leading/trailing hyphens
- Examples: `example-post`, `trt-literacy-basics`, `training-recovery-notes`
- Published file path: `blog/posts/{slug}.html`

## Exact ordered steps

### (1) Clone the template

Copy `templates/post.html` → `blog/posts/{slug}.html`

Paths inside the template already assume the published location (`../../css/styles.css`, `../../` home, `../` blog index). Do not change those path prefixes.

### (2) Replace placeholders

In `blog/posts/{slug}.html`, replace every placeholder:

| Placeholder | Replace with |
|-------------|--------------|
| `{{TITLE}}` | Post title (plain text; appears in `<title>` and `<h1>`) |
| `{{DATE}}` | ISO date `YYYY-MM-DD` (both `datetime` attr and visible text) |
| `{{DESCRIPTION}}` | Short meta description (plain text, one sentence) |
| `{{CONTENT}}` | Article body as HTML (`<p>`, `<h2>`, lists, etc.) |
| `{{TAGS}}` | Zero or more `<li class="tag">tag-name</li>` items; if none, use empty string or remove the `<ul class="tags">` |

Keep the byline **Written by WolfmanTRT** and the footer disclaimer intact.

### (3) Append object to `data/posts.json`

`data/posts.json` is a JSON **array**. Append one object (valid JSON after edit — no trailing comments):

```json
{
  "slug": "example-post",
  "title": "Example Post",
  "description": "Short description",
  "tags": ["trt-literacy"],
  "date": "2026-09-13",
  "path": "blog/posts/example-post.html"
}
```

Field map:

| Field | Maps to |
|-------|---------|
| `slug` | Filename stem; used in URLs |
| `title` | Link text / `{{TITLE}}` |
| `description` | List blurb / `{{DESCRIPTION}}` |
| `tags` | Array of strings → tag chips / `{{TAGS}}` |
| `date` | Sort key + display / `{{DATE}}` |
| `path` | Relative path from site root to the HTML file |

Sort newest-first by `date` when rewriting the array if you reorder.

### (4) Update `blog/index.html` post-list

Inside `<ul id="post-list">` (between `<!-- PUBLISHER: inject posts here -->` and `<!-- /PUBLISHER -->`):

- Remove the empty-state `<li class="empty-state">...</li>` when adding the first post.
- For each post in `posts.json` (newest first), inject an `<li>` like:

```html
<li>
  <p class="post-meta"><time datetime="2026-09-13">2026-09-13</time></p>
  <h2 class="post-title"><a href="posts/example-post.html">Example Post</a></h2>
  <p class="post-desc">Short description</p>
</li>
```

Link `href` is relative to `blog/index.html`: `posts/{slug}.html`.

### (5) Do not touch Hostinger, DNS, CNAME

- No `CNAME` file
- No Hostinger / custom-domain / DNS changes
- This repo is GitHub Pages only (project Pages preview via Actions workflow)

### (6) Success criteria

- [ ] `blog/posts/{slug}.html` exists with all placeholders replaced
- [ ] Byline present: Written by WolfmanTRT
- [ ] Footer on post page: Not medical advice. Talk to your doctor.
- [ ] `data/posts.json` is valid JSON array including the new object
- [ ] `blog/index.html` `#post-list` shows the new post link
- [ ] No dosing, sourcing, sales, or guarantees in content
- [ ] No `CNAME`; no Hostinger/DNS edits
- [ ] Relative paths still resolve under project Pages (`path: '.'` artifact)

## Safety (required every post)

- No dosing instructions, protocols, or “how much to take”
- No sourcing / pharmacies / gray-market / “where to buy”
- No medical guarantees or “this will fix…” claims
- Byline: **Written by WolfmanTRT**
- Footer disclaimer on every page: **Not medical advice. Talk to your doctor.**
- Education and peer journal only

## How `posts.json` fields map to HTML

1. Publisher reads `data/posts.json`.
2. For a new post, clones `templates/post.html` and fills placeholders from the object + body content.
3. Writes `blog/posts/{slug}.html` where `path` equals that relative path.
4. Rebuilds `#post-list` in `blog/index.html` from the full array (title → link text, description → `.post-desc`, date → `<time>`, path → `href` as `posts/{slug}.html`).
