# mattcope.land

A personal website: essays and notes, written in Markdown, rendered by Hugo, hosted on Cloudflare Pages. No database, no CMS, no JavaScript required to read.

## Local preview

Install [Hugo](https://gohugo.io/installation/) (extended, current stable &mdash; v0.165.0+):

```bash
brew install hugo   # macOS
```

Run the dev server:

```bash
hugo server -D
```

Open `http://localhost:1313/`.

## Add a note

Notes are short posts without a title. They appear in the stream with their date and body.

```bash
hugo new content notes/2026-08-31-my-thought.md
```

Edit the file &mdash; the date is set automatically. Write Markdown below the front matter. No title field.

## Add an essay

Essays have a title, an optional summary, and a stable URL.

```bash
hugo new content essays/my-essay.md
```

Edit the file: set the `title`, optionally add a `summary`, then write Markdown below.

## Build

```bash
hugo --minify
```

Output goes to `public/`. The site is static HTML + CSS + an RSS feed at `/index.xml`.

## Deploy (Cloudflare Pages)

1. Push this repo to GitHub.
2. In Cloudflare Pages, create a project connected to the GitHub repo.
3. Build settings:
   - **Build command:** `hugo --minify`
   - **Build output directory:** `public`
   - **Hugo version:** set `HUGO_VERSION` env to `0.165.0` (or current stable)
4. Push to the default branch to deploy. Cloudflare builds and serves automatically.
5. Add a custom domain in the Pages project settings when ready.

## Feed

RSS is at `/index.xml` &mdash; `https://mattcope.land/index.xml` once the domain is attached.

## What's intentionally missing

- No comments, no analytics, no tracking
- No CMS admin UI or database
- No client-side JavaScript
- No federation (ActivityPub, Mastodon, Bluesky)
- No image pipeline &mdash; images are hotlinked via Markdown URLs
- No newsletter, no search, no dark-mode toggle (dark mode follows system preference via CSS)

## Repository layout

```
content/          Markdown files (the actual site)
  _index.md       Homepage
  about.md        About page
  contact.md      Contact page
  notes/          Short posts (no title)
  essays/         Longer posts (with title)
layouts/          HTML templates (in-repo, no external theme)
static/css/       Stylesheet (no build step)
archetypes/       Templates for `hugo new`
hugo.toml         Site configuration
```

Markdown files are the site. Hugo is a camera. The host is a shelf.
