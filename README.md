# C-HAWQ Water Quality Explorer

An interactive map of water quality monitoring stations across Florida. Search and filter stations, select water quality parameters, view measurements over time, and export data — all in the browser.

Built and maintained by **C-HAWQ — the Coastal Habitat and Water Quality Initiative**.

## Live site

With GitHub Pages enabled (Settings → Pages → Deploy from a branch → `main` / root), the explorer is live at:

`https://<account>.github.io/wq-explorer/`

## What's in this repo

This is a static, browser-only app — no server, no database. The page reads static files that sit next to it, so they must travel together:

- **index.html** — the page itself
- **wq_summary.json** — the small overview the page loads first
- **data/** — the per-parameter CSVs the page loads on demand (~54 files)
- **White.png** — the C-HAWQ logo
- **.gitignore** — excludes `master_wq.csv`, the large merged export the page never loads

## Using it

Open the live link in any modern browser. An internet connection is required — the map tiles, charting library, and place search all load from the web.

To run locally, serve the folder over HTTP (`python -m http.server`) and open `index.html`. Double-clicking the file won't work, because browsers block a local page from reading its neighboring data files.

## Data

Water quality measurements come from the **U.S. Water Quality Portal** (National Water Quality Monitoring Council — USGS and EPA): https://www.waterqualitydata.us/ · DOI https://doi.org/10.5066/P9QRKUVJ. This source data is public. C-HAWQ's contribution is the compilation, processing, and presentation.

## License

Code © 2026 C-HAWQ. All rights reserved. The C-HAWQ-processed dashboard data is released under Creative Commons Attribution–NonCommercial–NoDerivatives 4.0 (CC BY-NC-ND 4.0). See [LICENSE](LICENSE) for details and for the terms covering the underlying public data.

## Contact

Nathan — nathan@chawq.org
