# bettertranslations.co

Single-page landing site for BetterTranslations.co, served from GitHub Pages.

## Contents

- `index.html` - the entire site. One static file, no JavaScript, no build step, no external requests.
- `og.png` - the 2400x1260 social preview image referenced by the `og:image` tags.
- `robots.txt`, `sitemap.xml`, `CNAME` - hosting metadata. `CNAME` is managed by GitHub Pages; do not edit it by hand.
- `source/BetterTranslations - Landing Page.html` - the original Claude artifact export the design came from. Kept for provenance only; it is not served.
- `source/og-image.html` - the source used to render `og.png`. See below.

## Regenerating og.png

`og.png` is a screenshot of `source/og-image.html`, which is a standalone 1200x630 page with Archivo inlined. Edit the HTML, then re-render at 2x:

```
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" \
  --headless --disable-gpu --force-device-scale-factor=2 \
  --window-size=1200,630 --hide-scrollbars \
  --screenshot=og.png "file://$PWD/source/og-image.html"
```

If you change the image's dimensions, update the `og:image:width` and `og:image:height` tags in `index.html` to match, and re-scrape the URL in the LinkedIn Post Inspector and Facebook Sharing Debugger - both cache the old image indefinitely otherwise.

## How index.html was made

The original export was a bundled single-page app: an empty `<body>` plus a base64 manifest that a runtime script unpacked into React at load time. That renders fine in a browser but is invisible to search engines and link-preview crawlers, which see only the placeholder title. `index.html` is that same design flattened by hand into plain HTML and CSS.

What changed in the process:

- Real `<title>`, meta description, canonical URL, Open Graph and Twitter card tags, and an inline SVG favicon.
- The Archivo variable font (latin and latin-ext subsets) is inlined as `data:` URIs, so the page makes zero network requests after the initial HTML. Cyrillic is not included - the original export did not ship a Cyrillic subset, and the page copy is English only.
- Unused parts of the design system CSS (cards, tables, dialogs, forms, nav, tags, segmented controls) were dropped. Only the tokens and rules the page actually uses remain.
- The headline's three lines are `<span class="line">` elements rather than `<br>` tags. Below 900px they stay inline so the headline wraps naturally; at 900px and up they go block-level to restore the intended three-line composition.
- The lede's `white-space: nowrap` was removed - it forced horizontal overflow on every phone.
- A small `ProfessionalService` JSON-LD block was added for search engines. It is inert data, not executable script.

## Editing

Edit `index.html` directly. The font `data:` URIs are the only large blobs in the file; everything else is readable markup near the bottom.

To preview locally:

```
python3 -m http.server 8000
```

Then open http://localhost:8000/ - opening the file over `file://` also works.

## Deployment

Pushing to `main` publishes via GitHub Pages.
