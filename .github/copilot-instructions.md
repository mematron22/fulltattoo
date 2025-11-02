# Copilot instructions for this repository

This is a small static HTML/CSS/JS website template (The Void Tattoo). The repository contains two similar copies and Pinegrow metadata/backups. Use the notes below to be immediately productive when editing, testing, or improving the site.

- Project type: Static site (no build step). Primary entry: `index.html` at repository root and a secondary copy at `void-tattoo-html-template/index.html`.
- Main assets:
  - HTML: `index.html` (root) and `void-tattoo-html-template/index.html` (local copy).
  - CSS: `css/vendor.css`, `css/style.css`, `css/bootstrap.min.css` (in sub-copy).
  - JS: `js/jquery-1.11.0.min.js`, `js/bootstrap.min.js`, `js/plugins.js`, `js/script.js`.
  - Images: `images/`, `images/gallery/`, `images/resource/`.
  - Pinegrow metadata/backups: `_pgbackup/`, `_pginfo/` — these are editor artifacts and can usually be ignored unless you need a previous Pinegrow state.

- Big picture & runtime ordering facts (important):
  - This is a DOM-first template that depends on a strict include order: jQuery -> iconify (CDN) -> bootstrap.min.js -> plugins.js -> script.js. Changing order will break initialization (e.g. Chocolat, Swiper, easyPieChart used by `script.js`).
  - `js/plugins.js` bundles third-party libraries (Swiper, Chocolat, easy-pie-chart). Avoid editing the bundle unless you know what you're doing; prefer updating initialisation in `js/script.js`.
  - Fonts and icons are loaded from CDNs (Google Fonts, Iconify). Offline usage requires replacing those tags.

- Conventions and patterns specific to this repo:
  - Pinegrow usage: `_pgbackup/` contains many snapshot files (`index_*.html`) and `pinegrow_*.json`. If you see odd structure, check `_pgbackup` for recent edits.
  - Visual assets: `images/resource/` holds small UI images; `images/gallery/` holds gallery media. Gallery markup uses `popup-gallery` + `image-link` classes and relies on Chocolat (plugins.js).
  - Two copies: edits to the root `index.html` are the canonical working files here; the `void-tattoo-html-template/` folder appears to be a packaged copy (keep both in sync if you intend to ship the packaged template).

- Editing and debugging tips (concrete):
  - Local test: open `index.html` in a browser (no build). Use DevTools to check console errors (missing assets, wrong paths). Example issue to watch for: root `index.html` references `../bootstrap_theme/bootstrap.css` — verify that path or change to the `css/bootstrap.min.css` used in the packaged copy.
  - When changing JS, edit `js/script.js` for site-specific behaviour (menu toggle, lightbox init, Swiper init, modal video). Keep plugin initialisation there.
  - If CSS changes don't appear, confirm you are editing the same copy the page is loading (root vs packaged copy). Clear cache / disable cache in DevTools to verify.
  - When adding images, preserve the relative paths used by the gallery (e.g. `images/gallery/<file>`). Use the same `image-link` anchor structure to maintain lightbox behaviour.

- Deployment / build workflow:
  - There is no build system. Deploy by copying the files to any static host (GitHub Pages, Netlify, IIS, S3, etc.). Ensure CDN resources (fonts, Iconify) are reachable, or vendor them into `css/` and `js/` if offline use is required.

- Integration points / external dependencies:
  - External CDNs: Google Fonts, Iconify, YouTube embed for the modal video, Gumroad link in header.
  - Third-party libs: jQuery 1.11, Bootstrap (v4-ish), Swiper (bundled), Chocolat (bundled), easy-pie-chart (bundled).

- Quick examples (where to change behaviour):
  - Menu toggle and mobile nav: `js/script.js` -> function `MobileMenu()`.
  - Lightbox init: `js/script.js` -> `initChocolat()` calls `Chocolat(document.querySelectorAll('.image-link'))`.
  - Slider config: `js/script.js` - `new Swiper(".mySwiper", { ... })` (pagination and nav elements are in the DOM).

- What NOT to change lightly:
  - Do not reorder script includes in `index.html` unless you update dependent initialisation.
  - Avoid hand-editing `js/plugins.js` unless you are replacing or upgrading bundled libraries; prefer replacing the whole bundle with an official release and adjusting `script.js` initialisation.

If anything here is unclear or you want a different level of detail (for example: converting to a build system, consolidating the two copies, or replacing the CDN libraries with local packages), tell me which direction you prefer and I'll update this file accordingly.
