## CotizadorFloreria — Guidance for AI coding agents

This is a small static Progressive Web App (PWA) used as a quotation generator for "Arte y Distinción con Flores". The goal of these instructions is to help AI agents make safe, high-value edits quickly and consistently.

- Project type: static HTML/CSS/JS, no build step. Files served from repository root.
- Key files:
  - `index.html` — Single-file app (UI + logic). Contains the `CotizadorApp` class, inline app state, and event wiring (e.g. `onchange="app.actualizarCliente(...)"`).
  - `sw.js` — Service Worker. Implements a simple cache strategy: network-first for navigations, cache-first for assets. Cache name derives from `APP_VERSION`.
  - `manifest.webmanifest` — PWA manifest (relative paths, maskable icons).
  - `icons/` — App icons used by the manifest and SW cache.

Primary constraints and patterns to preserve
- The app is purely static and expects to be deployed as-is (GitHub Pages recommended). Avoid introducing a Node build unless adding a clear justification and manifest changes.
- Keep paths relative (e.g. `./manifest.webmanifest`); this repository explicitly uses relative URLs so the site can be hosted under a subpath.
- The UI and logic are colocated in `index.html` and rely on a global `app` instance. When refactoring, preserve the `window.app` global or update inline event attributes accordingly.
- Local persistence uses `localStorage` under the key `cotizacion_draft`. Reads are JSON-parsed with a try/catch already present — keep that defensive behavior.

Important behaviors & examples
- PDF export: `CotizadorApp.generarYCompartirPDF()` uses `html2canvas` and `jsPDF`. Changes that affect the preview DOM (id `cotizacion-preview`) can break PDF rendering — test exports after DOM/stylesheet edits.
- Install prompt: the code uses a deferred `beforeinstallprompt` flow and a visible `#install-prompt` UI. If you change PWA install UX, keep the `deferredPrompt` pattern.
- Service worker registration is in `index.html` (`navigator.serviceWorker.register('./sw.js')`). If you rename/move the SW, update this registration and the `CORE_ASSETS` list inside `sw.js`.

Developer workflows
- No build system: to preview locally, serve the folder over a local static server (GitHub Pages or any static file server). Example (not required to commit): `npx http-server .` or use VS Code Live Server.
- PWA validation: use Chrome/Edge DevTools → Lighthouse → PWA to test installability and offline behavior.
- When editing offline caching behavior, update `APP_VERSION` in `index.html` (used by `sw.js`) or update `CACHE_NAME` directly — otherwise clients may keep old caches.

Testing and verification checklist for changes
1. Open the site via a static server or GitHub Pages and confirm the app loads.
2. Register/refresh the Service Worker and check the Application → Cache Storage for `cotizador-cache-v*`.
3. Fill the form, refresh, and confirm draft persisted in `localStorage` key `cotizacion_draft`.
4. Generate PDF (button `📄 Generar PDF y Compartir`) and verify a downloadable PDF is produced with correct layout.
5. Toggle offline (DevTools) and verify the app still loads and the connection indicator shows offline.

When updating code, prefer minimal, local changes. If you split `index.html` into modules, update inline handlers (e.g. `onchange="app..."`) or replace them with delegated event listeners and keep the `window.app` reference for compatibility.

If anything in these instructions is unclear or missing (for example: planned CI, tests, or external integrations), tell me what to add and I will update this file.
