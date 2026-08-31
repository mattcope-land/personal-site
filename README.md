# mattcope.land

A personal website: essays and notes, written in Markdown, rendered by Hugo, hosted on Cloudflare Workers. No database, no CMS, no JavaScript required to read.

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

## Deploy (Cloudflare Workers)

This repo deploys as a **Worker with static assets** via Workers Builds — not
Cloudflare Pages. `wrangler.jsonc` holds the deploy config so it lives in version
control rather than only in the dashboard.

Build settings (Workers project -> Settings -> Build):

- **Build command:** `hugo --minify`
- **Deploy command:** `npx wrangler deploy`
- **Build variable:** `HUGO_VERSION = 0.165.0`

`HUGO_VERSION` is not optional. Cloudflare's build image ships an older Hugo
(extended_0.147.7 as of this writing), and Hugo renamed config keys in 0.158
(`languageCode` -> `locale`). Set it as a **build** variable, not a runtime variable.

### Production vs preview

Workers Builds decides this by branch (Settings -> Build -> Branch control):

- The **production branch** runs the *Deploy command* (`npx wrangler deploy`) and
  updates the live site.
- Every **other branch** runs the *Non-production branch deploy command*
  (`npx wrangler versions upload`), which creates a preview version and does
  **not** promote it to production.

If pushes are landing as previews instead of going live, the production branch
is set to something other than `main`. Fix it in Branch control.

### Custom domain

`mattcope.land` must be an active zone on Cloudflare (nameservers pointed at
Cloudflare) before it can be attached to the Worker. Attaching a custom domain
does not work while DNS is hosted elsewhere.

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
wrangler.jsonc    Cloudflare Workers deploy config
LICENSE           MIT for the code; writing is all rights reserved
```

Markdown files are the site. Hugo is a camera. The host is a shelf.
