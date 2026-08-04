# Lynton Sutton — Portfolio

A static portfolio site. No build step, no dependencies, no JavaScript.

Design: light "Swiss engineering document" — flat off-white paper, hard rules, monospace annotations, one heavyweight display type moment. The page is still until touched; movement is hover response, plus a swipe when you open a work.

## Structure

```
index.html                 the one-page portfolio
style.css                  every style, shared by all pages
media/
  iter-4d-planning.mp4           self-hosted clip for work 02 (~27MB)
work/
  brigantium-alfred.html         01
  iter-4d-planning.html          02
  drone-worksite-capture.html    03
  vr-planning-bentley.html       04
  tokamak-welding-logistics.html 05
  robotic-systems.html           06
```

Clicking a row in SELECTED WORKS navigates to that work's page. Each work page is deliberately bare: a back control, the work itself, and the footer — nothing else. Real URLs, so they're linkable and bookmarkable.

## Run locally

Open `index.html` in any browser. That's it. (The page transition needs a real HTTP origin — over `file://` navigation still works, it just doesn't animate. Any static server will do: `npx serve`.)

## Deploy

**GitHub Pages:** Settings → Pages → Build and deployment → Deploy from a branch → `main` / `/ (root)`. The site publishes at `https://dizzymammoth91.github.io/portfolio/`.

**Netlify (drag-and-drop):** drop the whole folder onto https://app.netlify.com/drop — it needs the folder now, not just `index.html`, since the pages and stylesheet are separate files.

## Page transition

The index swipes left to a work page; the work page swipes back right. This uses cross-document [view transitions](https://developer.mozilla.org/en-US/docs/Web/API/View_Transition_API) — CSS only, no script.

Direction is fixed per page rather than computed: a work page is only ever entered going *forward*, and the index only ever going *back*. So each page declares its own direction with a class on `<html>` — `nav-fwd` on work pages, `nav-back` on the index — and no JavaScript is needed to work out which way to slide. Browser back/forward gets the correct direction for free.

Supported in Chromium and Safari. Firefox doesn't implement cross-document view transitions yet and simply navigates instantly, which is a clean fallback. `prefers-reduced-motion` disables the slide.

## Embeds

Everything embedded sits in the same `.fig-frame` plate — hard 1.5px rule, 16:9, square.

**Nira 3D viewer** (work 03) — the Tokamak Pit photogrammetry scan, live and orbitable in the page. `brigantium.nira.app` sends no `X-Frame-Options` and no CSP `frame-ancestors`, so framing is permitted, and the asset loads without authentication. `allow="fullscreen; xr-spatial-tracking"` lets the viewer go fullscreen. A `.fig-link` sits below as a deliberate escape hatch — if the viewer can't start (no WebGL, third-party frames blocked by the visitor), there's still a way through to it.

Note that the embed depends on that asset staying publicly shared in Nira. If its sharing is revoked the frame goes blank, and only the fallback link will hint at what's missing.

### Video

Two kinds, both in the same plate:

- **YouTube** (work 01) — embedded from `youtube-nocookie.com`, YouTube's privacy-enhanced domain, so no tracking cookie is set unless a visitor presses play. `loading="lazy"`, so nothing is fetched until the frame scrolls into view.
- **Self-hosted** (work 02) — a plain `<video>` pointing at `media/`. **`preload="metadata"` is important**: without it browsers pull the entire file on page load, and this one is ~27MB. It also carries a download-link fallback for browsers that can't decode it.

Self-hosted clips are `object-fit: contain`, so a 4D sequence is letterboxed rather than cropped.

Two things to watch if you add more:
- GitHub rejects any file over 100MB, and git history keeps large files forever even after deletion. Anything much past ~30MB is better on YouTube/Vimeo than in the repo.
- `.avi` doesn't play in any browser. Transcode to MP4 (H.264) or WebM first — `ffmpeg -i in.avi -c:v libx264 -crf 23 -movflags +faststart out.mp4`. `+faststart` matters: it moves the index to the front of the file so playback can begin before the whole thing downloads.

Neither clip has a `poster` image. Adding one means the frame shows something before playback instead of going to metadata-load first: `ffmpeg -i clip.mp4 -ss 3 -vframes 1 poster.jpg`, then `<video poster="../media/poster.jpg">`.

## Design tokens

Defined as CSS custom properties in `:root` in `style.css`:

| Token | Value | Use |
|---|---|---|
| `--page` | `#f5f4f1` | paper background |
| `--ink` | `#17181a` | display type, heavy rules, inverted row plate |
| `--body` | `#3c3e42` | paragraph text |
| `--mono-2` | `#5a5c60` | header / footer mono |
| `--mono-3` | `#8e8f92` | faint mono labels |
| `--hair` | `#c9c8c3` | hairline rules |
| `--accent` | `oklch(0.55 0.17 250)` | contact underline, focus rings, send button hover |
| `--gutter` | `48px` | page gutter (24px below 760px) |

Rules: heavy `1.5px solid --ink` (header, table head, intro rule, footer, figure frame); hairline `1px solid --hair` (row and section dividers). No radii, no shadows.

Type: **Archivo Black** (hero only), **Archivo** 400–800 (body, titles), **Geist Mono** 400–600 (all labels, always letter-spaced .06–.14em). Loaded from Google Fonts.

## Contact section

Deviates from the design handoff, deliberately. The handoff specified a large `mailto:` link; this build uses a Formspree-backed contact form instead so no email address is exposed. **There is no email address anywhere in this repo** — don't reintroduce one with a `mailto:`. The 40px display-type moment the mailto held is carried by the "Get in touch." lede. A LinkedIn link sits alongside `LONDON, UK`.

Fields are rule-only, square and transparent, with the underline going to `--ink` on focus. The submit button is an ink plate that goes to `--accent` on hover, echoing the works-row inversion. A `_gotcha` honeypot sits off-screen rather than `display: none` so bots still fill it.

## To finish

**1. Connect the contact form.** This is the only step that blocks launch — until it's done the form silently fails.

1. Sign up at [formspree.io](https://formspree.io) and create a new form.
2. It gives you an endpoint like `https://formspree.io/f/abcdwxyz` — copy the ID off the end.
3. In `index.html`, replace `YOUR_FORM_ID` in the form's `action` attribute.

Free tier is ~50 submissions/month.

**2. Confirm the work years.** Rows 03–06 are all dated `2015–23`, the ITER window, because per-project years weren't recorded anywhere. Narrow them if you want the YEAR column to carry more signal.

**3. Settle the product naming.** Row 01 and its page say *Brigantium AI — "Alfred"*, carried over from the previous site. The embedded video is titled *4DX — 3 min demo*, and the repos are `4DX` / `4dx-releases`. If 4DX is the public product name, the row title and page should say so.

**4. Fill out the work pages.** Each currently carries one paragraph. They have room for images, figures, and longer write-ups — copy the `<figure class="figure">` block from `work/brigantium-alfred.html` as a starting point.

**5. Footer year.** All pages read `© 2026`, matching the design handoff and the current year. Change it in all seven files if you want a different one.

## Responsive

Validated at desktop widths. Below 760px the gutter narrows to 24px, the intro and contact grids collapse to one column, and each works row folds to two lines — `NO. + WORK` above `DOMAIN + YEAR` — with the column header row hidden. Below 420px the hero type scales down further to stay inside the page.
