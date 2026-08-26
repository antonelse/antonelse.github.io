# antonelse portfolio — deploy instructions

Everything needed to run the site is in `index.html` (fonts, favicon and the avatar
images are embedded as base64 — no build step, no dependencies).

## 1. Deploy on GitHub Pages

1. Create a new **public** repository named `antonelse.github.io` (this exact name
   gives you a root domain like `https://antonelse.github.io/`; any other repo name
   works too, but the site will then live at `https://antonelse.github.io/<repo-name>/`).
2. Put these files in the repo root:
   - `index.html`
   - `og-image.png`
   - `robots.txt`
   - `sitemap.xml`
   - `imgs/` (folder — currently empty, see §3 below)
3. Push to the `main` branch.
4. In the repo, go to **Settings → Pages**.
5. Under **Source**, select **Deploy from a branch**, branch `main`, folder `/ (root)`.
6. Save. The site will be live in a minute or two at the URL GitHub shows you.

## 2. Custom domain (optional)

If you want `yourdomain.com` instead of `antonelse.github.io`:

1. Add a file named `CNAME` (no extension) in the repo root, containing just your domain:
   ```
   yourdomain.com
   ```
2. At your domain registrar, add DNS records pointing to GitHub Pages:
   - Apex domain (`yourdomain.com`): four `A` records pointing to
     `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - `www` subdomain: a `CNAME` record pointing to `antonelse.github.io`
3. Back in **Settings → Pages**, enter your custom domain and enable **Enforce HTTPS**
   once available (can take a few hours after DNS propagates).
4. If you switch domains, update the URLs in `robots.txt`, `sitemap.xml`, and the
   `og:url` / `canonical` / `og:image` meta tags inside `index.html`.

## 3. Editing the content (WORK, EDUCATION, PROJECTS, PUBLICATIONS, AWARDS, INFLUENCES)

All of this content lives in one place: the `SITE_DATA` object near the top of
the `<script>` block in `index.html` (search for `SITE_DATA`). Each section is
a plain JS array of objects — the page builds the tables and project cards
from these arrays automatically. **You never need to write or copy HTML** to
add, edit, or remove an entry.

- **Add an entry**: copy an existing object in the relevant array, paste it as
  a new item, and change the values.
- **Edit an entry**: change the fields on the object directly.
- **Remove an entry**: delete its object from the array.
- **Reorder entries**: just reorder the objects — they render top to bottom
  in array order, and the STEP numbers (00, 01, 02…) are generated automatically.

Field reference:

| Section | Fields |
|---|---|
| `WORK`, `EDUCATION`, `AWARDS` | `role, org, period, desc, note` |
| `PUBLICATIONS` | `role, url, org, period, desc, note` — `role` becomes a clickable link when `url` is set |
| `PROJECTS.*`, `INFLUENCES.*` | `code, meta, title, desc, tags, linkText, linkUrl, note` |

- `tags` is optional — omit it (or use `[]`) for a card with no tag chips.
- `note` is optional and purely sonic: it's the synth pitch played when you
  hover that row/card (only audible if SOUND is on). Any number works, or
  omit it entirely for a silent row.
- `desc` accepts a `\"` inside the string as `\"like this\"` (escaped with a
  backslash) since it's inside a JS string, not raw HTML.

## 4. What's still a placeholder — the site is live, these are the only loose ends

Everything else (WORK, EDUCATION, PROJECTS, PUBLICATIONS, AWARDS, contacts) is
real, populated data. What's intentionally left for you to add:

- **`/PROJECTS` → IMAGES**: still a static "— COMING SOON —" placeholder (this
  subsection isn't part of `SITE_DATA` — it's plain HTML). To show a photo,
  replace that line with an `.img-cell` block — there's a commented example
  right below it in `index.html` showing the exact markup to copy.
- **`/PROJECTS` → DIY → Analog Shutterino**: the "build log / github" link is still `#`
  (no public repo linked yet) — update `linkUrl` for that entry in `SITE_DATA.PROJECTS.DIY`.
- **`imgs/`**: currently empty — drop any image files here that you reference
  from PROJECTS → IMAGES above (relative paths like `imgs/photo.jpg`).
- **`/INFLUENCES` → READING**: still empty; unlike WATCHING/LISTENING/WISHING it
  hasn't been moved into `SITE_DATA` yet since there's nothing to show — once you
  have entries, add a `READING: [...]` array under `SITE_DATA.INFLUENCES` (same
  shape as `WATCHING`), give its `.samples` div an `id="infReadingSamples"`, and
  call `renderSampleCards('infReadingSamples', SITE_DATA.INFLUENCES.READING)`
  next to the other `renderSampleCards(...)` calls.

Keep an eye on all of the above as things change (new job, new papers, new
projects) — update the corresponding `SITE_DATA` array whenever they do.

## 5. Good to know about the page itself

- Sections **PUBLICATIONS** and **AWARDS** start collapsed — click the header to expand.
- Hovering the avatar switches it from a low-res B&W pixelated version to the full
  color photo.
- After ~60s of inactivity an animated ASCII tunnel screensaver kicks in; any input
  (click/key/scroll/touch) dismisses it.
- Top bar controls: **PLAY** runs a demo playhead over the WORK rows; **MUTE**
  toggles sound (off by default) — hovering rows/cards plays short synthesized
  FM/IDM-style blips; the small bars next to MUTE are a real VU meter reading the
  actual audio level (not decorative); **BPM −/+** changes playhead speed;
  **LIGHT/DARK** switches the color theme; **CRT** toggles the scanline/vignette
  effect. All are keyboard-accessible (Tab + Enter/Space).
- "SAVE & EXIT" in the footer is an easter egg (ASCII logo + typed message), not a
  real navigation action.
- The footer's "last modified" date is automatic (`document.lastModified`) — no
  need to update it by hand when you edit the file.

## 6. Files reference

| File | Purpose |
|---|---|
| `index.html` | The entire site — HTML, CSS, JS, fonts, favicon and avatar images, all in one file |
| `og-image.png` | Social preview image shown when the link is shared (LinkedIn, Twitter, Slack, etc.) |
| `robots.txt` | Tells search engines they're allowed to index the site |
| `sitemap.xml` | Helps search engines find the page — update the URL if you use a custom domain |
| `imgs/` | Drop images here to reference from PROJECTS → IMAGES |
