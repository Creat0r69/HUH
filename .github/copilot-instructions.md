# Copilot instructions — pomkori.fun

Purpose: Quickly orient an AI coding agent to the repository so it can be immediately productive.

Summary
- This is a static site exported from Webflow (no build tool, no package.json). The app is a single-page site with assets organized under `css/`, `js/`, `images/` and vendor/ CDN folders (e.g., `cdn.prod.website-files.com/`). The main entry is `index.html`.

What to look for first
- Entry: `index.html` — primary markup, inlined scripts, and CSS references.
- Styles: `css/webflow.css`, `css/pomkori.webflow.css` (do not rename; Webflow ties classes to these files).
- Scripts: `js/webflow.js` (Webflow runtime), inline `<script>` blocks inside `index.html` for site-specific behavior (e.g., the menu overlay logic).
- Assets: `images/` and `cdn.prod.website-files.com/` (hosted asset copies). Keep path references intact.

Important patterns & project-specific details
- Webflow export: many behaviors are implemented as inline scripts in `index.html`. When changing behavior, prefer editing the specific inline script block that defines the UI (example: the menu overlay open/close logic starting in `index.html`).
- Avoid renaming core Webflow files (`webflow.js`, `webflow.css`, `pomkori.webflow.css`) because class names and runtime expect those filenames.
- External integrations are referenced directly in HTML: Google Analytics (`gtag.js`), Google Fonts via `WebFont.load`, Finsweet Attributes (via `cdn.jsdelivr.net`), GSAP, and other CDNs. Verify network paths when testing locally.
- Notice attributes and selectors used by external libs: examples in `index.html` include `data-gsap`, `fs-copyclip-element`, and `fs-copyclip-element="copy-this"`. These conventions are used by GSAP and Finsweet — preserve attribute names.
- Some script src paths in the export are root-relative or contain backslash characters; be careful when serving the site from a different base path.

Dev / Debug workflows
- No build step. To preview locally run a simple HTTP server from the project root:

  - PowerShell (has Python): `python -m http.server 8000`
  - Or if Node is available: `npx serve .`

- Open `http://localhost:8000` and check browser console/network for missing files or CORS errors.
- When debugging JS issues, check `index.html` inline scripts first (they contain UI logic). Use the browser DevTools to locate failing script lines — many are not in separate files.

Editing guidance
- Preferred: edit in Webflow designer and re-export to keep class names and structure consistent.
- Manual edits: make minimal, focused changes. Keep Webflow runtime files and CSS names intact. If you add new JS modules, place them under `js/` and update `index.html` script tags.
- When changing markup, search for duplicated class names in `css/` files to avoid unintentionally breaking styles.

Examples of common tasks (concrete guidance)
- Change menu animation timing: update the inline script in `index.html` where `setTimeout` values control `menu.classList` and `texts.forEach(...)` timings.
- Add a small widget: create `js/widget.js`, include it near the end of `index.html` before `</body>`, and register any DOM-ready behavior inside `DOMContentLoaded`.

Files to check when troubleshooting
- `index.html` — first stop for markup, inline scripts, and external CDN tags.
- `css/` — `webflow.css` and `pomkori.webflow.css` for styling rules and class names.
- `js/webflow.js` — Webflow runtime; avoid wholesale edits unless necessary.
- `cdn.prod.website-files.com/` — external-hosted exported assets (images, SVGs).

If you are unsure
- Ask for clarification and point to the exact file and snippet you plan to change (e.g., "I will modify the inline menu script in `index.html` around the `menu.classList.add('animate')` call").

Notes
- This repo appears to be a plain Webflow export — there is no CI, tests, or build scripts to infer. Deploy updates as static files to the target host or re-export from Webflow when making structural changes.

---
If anything above is unclear or you'd like me to include additional examples (e.g., a short checklist for safe manual edits), tell me what to add and I'll update this file.
