# Sharing the C-HAWQ Water Quality Explorer

The explorer is a self-contained, browser-only app. There is no server-side code and no database — the page reads static files that sit next to it. Wherever those files travel together, the full features and data travel with them.

## What makes up the app

Ship these together:

- `wq_explorer.html` — the page itself
- `wq_summary.json` — the small overview the page loads first
- `data/` — the per-parameter CSVs the page loads on demand (~407 MB total, ~54 files)

Do **not** include `master_wq.csv` (~1.1 GB). The page never loads it; it exists only as a merged export for ArcGIS, R, or Excel. Leaving it out keeps the upload small and avoids host file-size limits.

So the shareable package is the `wq_explorer` folder with `master_wq.csv` removed.

## Option A — Put it on a static web host (best for a review link)

Because nothing runs on a server, the app drops onto any static host and everyone gets a URL.

1. Create the deployable folder: copy the `wq_explorer` folder and delete `master_wq.csv` from the copy.
2. Pick a host and upload that folder:
   - **Netlify** (netlify.com) or **Cloudflare Pages** (pages.cloudflare.com) — drag-and-drop or connect a folder; both give an instant URL.
   - **GitHub Pages** — commit the folder to a repo and enable Pages. Every file is under the 100 MB per-file limit, so this works.
3. Share the URL with the team. They click it and use the full explorer, data included.

Note on free tiers: 407 MB is fine for most, but a few free plans cap total size or monthly bandwidth. If you hit a wall, that's the cause.

## Option B — Zip and send (quick technical review)

Good for sharing with someone comfortable running a command.

1. Zip the deployable folder (the copy with `master_wq.csv` removed) and send it (Google Drive works).
2. The recipient unzips, opens a terminal in that folder, and runs:
   ```
   python -m http.server 8000
   ```
3. They open `http://localhost:8000/wq_explorer.html`.

Important: double-clicking `wq_explorer.html` will **not** work. Browsers block a local file from reading its neighboring files, so the page must be served (the command above does that).

## Using GitHub (recommended for the team)

GitHub Pages hosts the static files for free and gives the team a link. The data is public (U.S. Water Quality Portal), so a public repository is fine and is the simplest path to a live link.

1. Make a deploy copy of this folder. The included `.gitignore` already keeps `master_wq.csv` out of the commit, so you don't have to delete it manually.
2. On GitHub, create a new repository (for example `wq-explorer`). Choose **Public** if you want the free live link.
3. In a terminal, from the deploy folder:
   ```
   git init
   git add .
   git commit -m "C-HAWQ Water Quality Explorer"
   git branch -M main
   git remote add origin https://github.com/YOUR-USERNAME/wq-explorer.git
   git push -u origin main
   ```
4. In the repo: **Settings → Pages → Build and deployment → Source: Deploy from a branch**, branch `main`, folder `/ (root)`, then Save.
5. After about a minute the site is live at:
   `https://YOUR-USERNAME.github.io/wq-explorer/wq_explorer.html`

Tip: rename `wq_explorer.html` to `index.html` before pushing, and the link becomes the cleaner `https://YOUR-USERNAME.github.io/wq-explorer/`.

Notes and limits:

- The largest data file is ~43 MB, under GitHub's 100 MB per-file limit, so plain `git` works — no Git LFS needed.
- Never commit `master_wq.csv` (~1.1 GB). A push that includes it will be rejected. The `.gitignore` prevents this.
- The repo is ~407 MB, which is within GitHub's limits; cloning just takes a moment.
- The Pages site is served over HTTPS, so all data files, the map tiles, and the charting library load correctly.
- **Private repo:** GitHub Pages from a private repository requires a paid plan. On a free account, either keep the repo public, or keep it private and have the team clone it and run `python -m http.server` locally (see Option B).

## Putting it on chawq.org

The data still carries with it, but how cleanly depends on the platform.

- Most site builders (Squarespace, Wix, WordPress) don't cleanly host a multi-file app with a 400 MB folder of CSVs and relative paths.
- The reliable approach: deploy the explorer with Option A, then **embed it on chawq.org with an iframe**:
  ```html
  <iframe src="https://YOUR-DEPLOY-URL/wq_explorer.html"
          style="width:100%;height:900px;border:0"></iframe>
  ```
- If the website host lets you upload a folder to a subdirectory (for example `/explorer/`), you can place the files there directly and link to `/explorer/wq_explorer.html`.

Tell whoever maintains the site which platform it's on, and the embed or upload step can be tailored to it.

## What viewers need

- **Internet.** The map tiles, the charting library (Chart.js), and the place search all load from the web.
- The place search uses OpenStreetMap's free Nominatim geocoder, which has rate limits. That's fine for team review. For a high-traffic public page, switch to a dedicated geocoding service.
- The data is public (U.S. Water Quality Portal), so there's no privacy concern in sharing it.

## Updating the data later

Re-run `build_wq_explorer.py` to regenerate `wq_summary.json` and the `data/` folder, then re-upload (or re-zip). The page picks up the new data on the next load.
