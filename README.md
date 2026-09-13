# WolfmanTRT — GitHub Pages (static)

Clean static site for **WolfmanTRT** peer education: TRT literacy, training & recovery, founder journal.

- Wordmark: WolfmanTRT only (no mascot)
- Brand: charcoal `#1A1A1A`, off-white `#F5F5F5`, steel blue `#4A90E2`, amber `#F5A623`
- Minimal / no JS; Inter via Google Fonts (system stack fallback)
- Relative paths so **private project Pages** preview works without a custom domain

**Out of scope:** CNAME, DNS, Hostinger, custom domain. Do not add a `CNAME` file.

## Local preview

From this folder:

```bash
python -m http.server 8080
```

Open `http://localhost:8080/` (or the port you chose).

## GitHub Pages

1. Push this folder as a GitHub repo (or use this tree as the repo root).
2. Enable Pages via **GitHub Actions** (recommended): workflow at `.github/workflows/pages.yml` uploads `path: '.'`.
3. Or publish from the `main`/`master` branch root in repo Settings → Pages.

No custom domain / CNAME is configured or required for project Pages preview.

## Publishing posts

See **[scripts/PUBLISH.md](scripts/PUBLISH.md)** for the Website Publisher bot: clone template, replace placeholders, update `data/posts.json` and `blog/index.html`.

Example `posts.json` object (do not leave comments in the JSON file itself):

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

`data/posts.json` starts as `[]`.

## Structure

```
index.html
404.html
README.md
css/styles.css
blog/index.html
blog/posts/
start-here/index.html
data/posts.json
templates/post.html
scripts/PUBLISH.md
.github/workflows/pages.yml
```
