# mattcope.land

My personal site: essays and notes.

Everything here is a Markdown file. Hugo renders those files into static HTML,
and Cloudflare serves the result. There is no database, no CMS, and no
client-side JavaScript — the pages are plain HTML and CSS.

Two kinds of writing:

- **Notes** — short, untitled. A thought, a link, an observation. They show up
  in the stream with just a date.
- **Essays** — longer, titled, with a stable URL and an optional summary.

Both appear together in reverse-chronological order on the homepage, and in the
RSS feed at [`/index.xml`](https://mattcope.land/index.xml).

## Layout

```
content/          the site itself — Markdown
  notes/          short posts (no title)
  essays/         longer posts (with title)
layouts/          HTML templates, in-repo (no external theme)
static/css/       site.css (layout) plus one file per theme in themes/
static/fonts/     self-hosted webfonts used by themes
archetypes/       front-matter templates for new posts
hugo.toml         Hugo config
wrangler.jsonc    Cloudflare deploy config
```

Note URLs come from the filename (`content/notes/2026-08-29-a-thought.md` →
`/notes/2026-08-29-a-thought/`), so any number of notes can share a date.
Essays drop the date: `content/essays/some-essay.md` → `/essays/some-essay/`.

## Themes

The look is split in two. `static/css/site.css` holds layout and structure only,
and reads its colors and fonts from CSS variables. Each file in
`static/css/themes/` is a complete look: it sets those variables (in light and
dark versions, following the OS setting) and can add rules of its own.

The active theme is one line in `hugo.toml`:

```toml
[params]
  theme = 'tokyo-night'
```

Themes are kept after they're retired, so going back to one is a matter of
changing that name. To make a new one, copy an existing theme file, give it a
new name, and change the values. A theme has to set every variable that
`classic.css` sets.

| Theme | Look |
| --- | --- |
| `classic` | The original: serif body, off-white / near-black, quiet blue links |
| `tokyo-night` | Tokyo Night palette, JetBrains Mono, neon cyan and magenta |

## Hosting

Deploys on push to `main`: Cloudflare builds with `hugo --minify` and publishes
`public/` as a Worker serving static assets. Posts dated in the future are
excluded at build time, and nothing rebuilds on a timer.

One thing not to lose: the build needs `HUGO_VERSION` set as a **build variable**
in Cloudflare. Its build image ships an older Hugo, and Hugo renamed a config key
in 0.158 (`languageCode` → `locale`), so an unpinned build can drift away from
what runs locally.

## License

MIT for the site code. The writing under `content/` is all rights reserved.
The JetBrains Mono fonts in `static/fonts/jetbrains-mono/` are under the SIL Open
Font License (`OFL.txt` in that folder).

---

Markdown files are the site. Hugo is a camera. The host is a shelf.
