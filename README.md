# Lynton Sutton — Portfolio

A single-file personal portfolio site. All markup, styles, and layout live in `index.html` (no build step, no dependencies).

## Run locally

Open `index.html` in any browser. That's it.

## Deploy

**GitHub Pages:** Settings → Pages → Build and deployment → Deploy from a branch → `main` / `/ (root)`. The site publishes at `https://dizzymammoth91.github.io/portfolio/`.

**Netlify (drag-and-drop):** drop `index.html` onto https://app.netlify.com/drop for an instant URL.

## To finish

The work cards use placeholder tiles marked `[ add image/video ]`. Swap each one by replacing the `<div class="media">…</div>` block with either:

- an image: `<img class="media" src="your-image.jpg" alt="…">`
- an embedded video: `<iframe class="media" src="https://www.youtube.com/embed/VIDEO_ID" allowfullscreen></iframe>`

To make a whole card clickable (e.g. link the Brigantium AI card to a demo video), change its opening `<div class="card">` to `<a class="card" href="VIDEO_URL" target="_blank" rel="noopener">` and the matching closing `</div>` to `</a>`.

Contact and social links live in the hero `.chips` and the footer.
