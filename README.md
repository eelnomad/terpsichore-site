# terpsichore-site

The promo and compliance site for **Calliope**, the rhythm trainer for dancers
(app source: [`eelnomad/calliope`](https://github.com/eelnomad/calliope)).

Three pages, two of which a store listing requires:

| Page | Purpose |
| --- | --- |
| `index.html` | What the app is, how it works, screenshots, coming-soon CTA |
| `privacy.html` | The privacy policy — **required** by both App Store and Google Play |
| `support.html` | Contact, FAQ, troubleshooting — **required** as the listing's support URL |

Plus `404.html` for unknown paths.

## Running it

Static HTML and CSS. No build step, no dependencies, no `package.json`.

```bash
python3 -m http.server 8000
```

Then open <http://localhost:8000/>. Edit a file, reload the browser — that's the whole loop.

Note that `python3 -m http.server` will not serve `404.html` for unknown paths (that's
GitHub Pages behaviour); open `404.html` directly to check it.

## Layout

```
index.html  privacy.html  support.html  404.html
.nojekyll                 # stop Pages running Jekyll over these files
robots.txt  sitemap.xml
assets/
  css/tokens.css          # brand tokens, ported from the app's design system
  css/fonts.css           # @font-face for the three bundled families
  css/site.css            # layout + components
  fonts/                  # Instrument Serif, Space Grotesk, Space Mono (copied from the app)
  terpsichore-mark.svg  favicon.png  og-image.png  icon-512.webp  icon-192.webp
  screenshots/            # drop screenshots here — see its README
```

### Design tokens

`assets/css/tokens.css` is a port of the app's own design system —
`src/design-system/tokens/*.css` and `theme.ts` in the calliope repo. If the app's palette
changes, re-port rather than hand-editing, so the site and the app stay in step. The site is
**dark-only**, matching the app's `userInterfaceStyle: "dark"`.

### Screenshots

None are committed yet. See `assets/screenshots/README.md` for the filenames the home page
already expects and the size/format conventions. Empty slots render as blank phone frames rather
than broken images, so the site is safe to ship before they exist.

## Deploying to GitHub Pages

The directory is not yet a git repository. To publish:

```bash
git init -b main
git add .
git commit -m "Calliope promo site"
gh repo create terpsichore-site --public --source=. --push
```

Then in the repo: **Settings → Pages → Build and deployment → Deploy from a branch**, branch
`main`, folder `/ (root)`. The site appears at
<https://eelnomad.github.io/terpsichore-site/> within a minute or two.

`.nojekyll` matters — without it Pages runs the files through Jekyll, which ignores paths
beginning with an underscore and can mangle assets.

### If you use a custom domain

Three things change:

1. Add a `CNAME` file at the repo root containing only the domain. **Don't add one with a
   placeholder value** — a `CNAME` pointing at a domain you don't control breaks the deploy.
2. Update the absolute URLs — `<link rel="canonical">`, `og:url`, `og:image`, `twitter:image` —
   in `index.html`, `privacy.html`, and `support.html`, plus `robots.txt` and `sitemap.xml`.
3. Strip the `/terpsichore-site/` prefix from the paths in `404.html`. That page uses root-absolute
   paths deliberately (Pages serves it for unknown URLs at any depth, where relative paths would
   resolve against the missing path); every other page uses relative paths and needs no change.

## Editing the copy

Everything on the pages is drawn from the app itself, not invented — the three modes, the
on-device analysis, the requirements, the privacy claims. Before changing a factual claim,
check it against the app source. The privacy policy text is a verbatim port of
`docs/privacy.html` in the calliope repo; keep the two in sync, and update the "Last updated"
date only when the wording actually changes.
