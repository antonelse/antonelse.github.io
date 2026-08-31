# antonelse portfolio — deploy instructions

Single-file site: everything is in `index.html` (fonts, favicon, avatar images
embedded as base64). No build step, no dependencies. `attribution.html` and
`privacy.html` are separate standalone pages that share the same embedded
fonts/theme.

## Deploy on GitHub Pages

1. Create a **public** repo named `antonelse.github.io` (gives you the root
   domain `https://antonelse.github.io/`; any other name works, just lives at
   `.../<repo-name>/` instead).
2. Push these files to the repo root: `index.html`, `og-image.png`,
   `robots.txt`, `sitemap.xml`, `attribution.html`, `privacy.html`.
3. **Settings → Pages → Source**: Deploy from a branch, `main`, `/ (root)`.
4. Live in a minute or two, at the URL GitHub shows you.

### Custom domain (optional)

1. Add a `CNAME` file in the repo root containing just `yourdomain.com`.
2. DNS at your registrar: apex domain → four `A` records to
   `185.199.108.153` / `.109.153` / `.110.153` / `.111.153`; `www` → `CNAME`
   record to `antonelse.github.io`.
3. **Settings → Pages**: enter the custom domain, enable **Enforce HTTPS**
   once available.
4. If you switch domains, update `robots.txt`, `sitemap.xml`, and the
   `og:url` / `canonical` / `og:image` tags in `index.html`.

## Editing content

Everything (WORK, EDUCATION, PROJECTS, PUBLICATIONS, AWARDS, INFLUENCES)
lives in the `SITE_DATA` object near the top of the `<script>` block —
search for `SITE_DATA`. Each section is a plain array of objects; the page
builds tables/cards from them automatically. **No HTML to write or copy.**

- Add/edit/remove an entry → add/edit/remove its object in the array.
- Reorder → reorder the objects (STEP numbers 00, 01, 02… are automatic).
  `WORK` and every `PROJECTS.*` array are ordered newest-first — keep new
  entries in that order (ties broken by whichever ended later). The `code`
  field (e.g. `SMPL_01`, `AUD_01`, `VID_01`) runs the other way: number 01
  is the *oldest* item, counting up to the newest — so adding a new entry
  at the top means bumping every existing code in that array by one and
  giving the new entry the highest number. Cosmetic, not derived automatically.

| Section | Fields |
|---|---|
| `WORK`, `EDUCATION`, `AWARDS` | `role, org, period, desc, note` |
| `PUBLICATIONS` | `role, url, org, period, desc, note` — `role` links when `url` is set |
| `PROJECTS.*`, `INFLUENCES.*` | `code, meta, title, desc, tags, linkText, linkUrl, note` |

- `tags`: optional, omit (or `[]`) for no chips.
- `note`: optional, the hover synth pitch (any number, or omit for silent).
- `desc`: escape `"` as `\"` (it's a JS string, not raw HTML).
- `AWARDS.period`: year only (no month) for consistency with other sections.

**CONTACTS is the one exception** — it's not in `SITE_DATA`, just plain
`.contact-line` markup near the end of the `<body>`. To add/remove a
contact, copy/edit a line there directly, and also update `sameAs` in the
`ld+json` structured-data block near the top (same links, helps SEO).

## Open loose ends

- **PROJECTS → SOFTWARE → Analog Shutterino**, and **VIDEO → METRO / Ozne
  Production**: `linkUrl: "#"`, no public link yet — update in `SITE_DATA`.
- **INFLUENCES → READING**: not yet in `SITE_DATA` (nothing to show). Once
  populated: add `READING: [...]` under `SITE_DATA.INFLUENCES` (same shape
  as `WATCHING`), give its `.samples` div `id="infReadingSamples"`, and call
  `renderSampleCards('infReadingSamples', SITE_DATA.INFLUENCES.READING)`.

## Page behavior, good to know

- **Navmenu**: all sections start collapsed; the active link tracks a fixed
  reference line near the top of the viewport as you scroll (not a plain
  visibility check, so it stays correct even when sections have very
  different heights). On wide screens (≥1320px) it becomes a sidebar fixed
  to the left of the content, with EDUCATION/PROJECTS/PUBLICATIONS/AWARDS/
  INFLUENCES/CONTACTS shown as the shortened EDU/PROJ/PUBS/AWRD/INFL/CNTCT
  to fit — section headers stay full-length everywhere.
- **⌃⌄ ALL** expands every section on the first click, collapses on the
  second (click a section header to toggle just that one).
- **Lissajous** (`LJ`): drag the mouse over the canvas to change the a:b
  ratio; phase drifts slowly on its own for extra shape variety. Link to a
  short explainer sits under the canvas.
- **Custom cursor**: on desktop (mouse + hover), the native pointer is
  hidden everywhere and replaced with a small 8-bit triangle that follows
  the mouse, spins slowly in fake-3D and bobs; it renders under the CRT
  overlay so it's affected by the scanlines/vignette like the rest of the
  page. Off on touch devices, where the native pointer is untouched.
- After ~60s idle, an ASCII tunnel screensaver kicks in; any input dismisses it.
- Top bar **MUTE/SOUND** and **REC ■** aren't what they look like: sound is
  off by default and just adds synth blips on hover, REC is purely decorative
  (no actual recording).
- Footer **"SAVE & EXIT"** is an easter egg (ASCII logo + typed message).
  Next to it, **Attribution** and **Privacy** link to their standalone pages.
- Title ("ANTONIO GIGANTI") is `contenteditable` and self-heals: edit or
  clear it, wait 1.8s after you stop, it retypes the real name (rebuilds
  stripped `<span>`s if needed). Native caret is hidden here (iOS renders it
  oversized with the custom pixel font) — the blinking block next to the
  text is the real cursor indicator.

## Files

| File | Purpose |
|---|---|
| `index.html` | Entire site — HTML, CSS, JS, fonts, favicon, avatar images |
| `attribution.html` | Credits page — fonts owed (license requirement) + inspiration links |
| `privacy.html` | Privacy policy — plain description of what the site does/doesn't collect |
| `og-image.png` | Social preview image (LinkedIn, Twitter, Slack, etc.) |
| `robots.txt` | Search engine indexing permissions |
| `sitemap.xml` | Search engine discovery — update URL if using a custom domain |

## Credits / licenses

- **Elektron Pixel Font** (display font, embedded in every HTML file) — © 2008
  savingaurora, [CC BY-SA 3.0](http://creativecommons.org/licenses/by-sa/3.0/),
  via [FontStruct](http://fontstruct.com/fontstructions/show/70152/elektron-pixel-font).
  Unofficial fan recreation, not affiliated with Elektron (the company).
  Attribution required by the license — kept both here and on the live
  `attribution.html` page; keep this note if you redistribute the site.
- **JetBrains Mono** (body font) — [SIL OFL 1.1](https://scripts.sil.org/OFL),
  no attribution required. Self-hosted as base64-embedded woff2 in every HTML
  file (not loaded from Google Fonts) so no visitor data is sent to Google on
  page load.
- No repo-wide LICENSE file: this is a personal portfolio with personal bio,
  photos and CV content, not open-source software — all rights reserved.
