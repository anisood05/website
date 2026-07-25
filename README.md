# Anirudh Sood — Personal Site

Static, dependency-free HTML/CSS/JS site: landing page, CV and portfolio deck. No build step — every page opens directly in a browser or serves as-is from any static host. Single visual direction: **Blueprint, Signal Red & Ink** (cream `#f4f1ea`, ink `#161616`, red `#e2001a`).

## Structure

```
index.html                 Landing page (entry point)
cv/
  cv.html                   CV
  cv-system.css
  tweaks-panel.jsx
portfolio/
  portfolio.html            Portfolio deck (entry point)
  portfolio.css
  deck-stage.js
  image-slot.js
work/
  client-xr/                Embedded Client Portal demo
    admin.html
    client-view.html
writing/
  posts.js                  Data file for the Writing section — edit this to publish new posts
  editor.html                Local tool to add/edit/reorder posts and regenerate posts.js
assets/
  portrait-signal-red.png
README.md
.gitignore
```

Each top-level feature (cv, portfolio, work, writing) is self-contained in its own folder with its entry HTML and assets alongside it — no shared asset folders split across the repo root.

## Deploying to Netlify via Git

1. Push this repo to GitHub (see below).
2. In Netlify: **Add new site → Import an existing project → connect to Git provider** → select this repo.
3. Build settings: leave **Build command** blank, set **Publish directory** to `/` (root) — static site, nothing to build.
4. Deploy. Every push to the connected branch auto-redeploys.

## Pushing to GitHub (first time)

```bash
git init
git add .
git commit -m "Initial site — Signal Red & Ink"
git branch -M main
git remote add origin https://github.com/<your-username>/<your-repo>.git
git push -u origin main
```

## Updating the Writing section

Open `writing/editor.html` in a browser, add/edit/reorder your LinkedIn posts or articles, click "Generate posts.js", and save the downloaded file over `writing/posts.js`. Commit and push — only the 3 most recent entries show on the landing page.

## Working with Claude Code

Plain HTML/CSS/JS — no build tooling. Notes:
- `index.html` is fully self-contained (styles inline) — no external CSS dependency.
- Relative links (`cv/cv.html`, `portfolio/portfolio.html`, `work/client-xr/...`) assume the folder structure above stays intact.
- Tweaks panel scripts (`tweaks-panel.jsx`) load via Babel in-browser — no npm install needed.
