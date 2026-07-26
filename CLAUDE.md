# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Anirudh Sood's personal site: a landing page, CV, gallery, and a client-facing demo, plus a "Writing" section. Static, dependency-free HTML/CSS/JS — **no build step, no package.json, no bundler**. Every page opens directly in a browser or is served as-is from any static host (deployed via Netlify with an empty build command and `/` as the publish directory).

## Commands

There is no build, lint, or test tooling in this repo. "Running" the site means opening the relevant HTML file directly in a browser (e.g. `open index.html` or a local static file server). Verify changes by loading the page in a browser — there is no automated test suite to run instead.

## Structure

```
index.html                 Landing page (entry point) — fully self-contained, styles inline
cv/
  cv.html                  CV (uses tweaks-panel.jsx for the in-browser edit-mode panel)
  cv-system.css
  tweaks-panel.jsx
gallery/
  gallery.html             View-only gallery page (entry point)
  editor.html              Local tool: drop images, edit captions, reorder — generates pieces.js
  pieces.js                Data file consumed by gallery.html
work/
  client-xr/               Standalone embedded Client Portal demo (admin.html, client-view.html)
writing/
  posts.js                 Data file for the Writing section on the landing page
  editor.html              Local tool to add/edit/reorder posts — generates posts.js
assets/
  portrait-signal-red.png
```

Each top-level feature (`cv/`, `gallery/`, `work/`, `writing/`) is self-contained in its own folder with its entry HTML and assets alongside it — there is no shared asset folder split across the repo root. Relative links between pages (e.g. `cv/cv.html`, `gallery/gallery.html`) assume this folder structure stays intact.

Note: `index.html` currently links to `cv/cv.html` and embeds `writing/posts.js` directly; `gallery/` is not yet linked from the landing page nav.

## The "editor generates a data.js" pattern

`gallery/` and `writing/` both follow the same convention: a view-only page (`gallery.html`) renders from a data file (`pieces.js` / `posts.js`) via a `window.SOME_DATA = [...]` global loaded with a plain `<script src>` tag. A companion `editor.html` is a local-only tool — open it in a browser, edit entries (including dragging/dropping images as base64 data URIs for the gallery), click "Generate", and it downloads a new version of the data file to save over the original. Content updates always go through the editor and a regenerated data file, not hand-written data-file edits (the gallery editor's generated file explicitly says not to hand-edit the `image` field).

When editing content in these sections, prefer updating via the editor's generation logic (i.e. matching the exact output format in `editor.html`'s generate handler) if hand-editing the data file directly.

## Design system

Single visual direction across `index.html`, `gallery/`: **Blueprint, Signal Red & Ink** — cream `#f4f1ea`, ink `#161616`, signal red `#e2001a`, fonts JetBrains Mono + Space Grotesk. `cv/` and `work/client-xr/` use a separate navy/copper/linen palette (Cormorant Garamond + DM Sans + IBM Plex Mono) and support light/dark theming via `@media (prefers-color-scheme: dark)` plus `:root[data-theme="dark"|"light"]` overrides driven by a theme toggle. Keep new pages within whichever palette their section already uses rather than mixing them.

## tweaks-panel.jsx

`cv/tweaks-panel.jsx` is a reusable, self-contained edit-mode panel (exports `useTweaks`, `TweaksPanel`, `TweakSection`, `TweakRow`, `TweakSlider`, `TweakToggle`, `TweakRadio`, `TweakSelect`, `TweakText`, `TweakNumber`, `TweakColor`, `TweakButton` onto `window`). It's loaded via Babel standalone directly in the browser (React/ReactDOM/Babel from unpkg, no npm install) — see the `<script type="text/babel" src="tweaks-panel.jsx">` tag in `cv/cv.html`. It owns a host protocol for an external edit-mode UI (listens for `__activate_edit_mode`/`__deactivate_edit_mode`, posts `__edit_mode_available`/`__edit_mode_set_keys`/`__edit_mode_dismissed`). Full usage docs are in the file's header comment — read that before modifying it, since other prototypes may come to depend on the same protocol.

## Updating the Writing section

Open `writing/editor.html` in a browser, add/edit/reorder posts, click "Generate posts.js", save the downloaded file over `writing/posts.js`. Only the 3 most recent entries show on the landing page (`index.html` does `.slice(0, 3)` on `window.WRITING_POSTS`).
