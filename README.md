# PSI Funnel — GAS Marketing

Single-page static funnel for **PSI (Pre-Sales Intelligence)**. Paid media traffic lands here
and is routed into WhatsApp conversations that qualify leads before they reach sales.

**Stack:** hand-written HTML/CSS/JS in one file. No framework, no build step, no dependencies.
**Host:** Vercel (static), deployed from the `main` branch of this repo.

---

## Repository layout

```
psi-funnel/
├── index.html            The entire site — markup, CSS, and JS in one file
├── vercel.json           Host config: security headers, cache policy, clean URLs
├── robots.txt            Crawler rules + sitemap pointer
├── sitemap.xml           Search engine index hints
├── assets/
│   ├── brand/            Logos and favicons for GAS itself
│   │   └── gas-mark.png  Used as a CSS background in 4 places
│   ├── logos/            Empty by design — the marquee uses text wordmarks
│   └── og/               Social share / Open Graph images
├── docs/
│   └── DEPLOY.md         Go-live runbook and post-launch checks
└── .gitignore
```

### Why this shape

- **`index.html` at the repo root.** Vercel serves a root-level `index.html` with zero
  configuration. No output directory to set, no framework preset to pick, nothing to
  misconfigure in the dashboard.
- **All static files under `assets/`, grouped by purpose.** `brand/` is ours, `logos/` is
  clients', `og/` is for social crawlers. A file's folder tells you who owns it and what
  breaks if you delete it.
- **Config lives in the repo, not the dashboard.** `vercel.json` is version-controlled, so
  headers and caching are reviewable and survive someone else touching project settings.
- **Lowercase, hyphenated filenames everywhere.** Vercel's CDN is case-sensitive; local
  machines often are not. `Old-Mutual.PNG` works on a laptop and 404s in production.

---

## Editing the site

Everything is in [index.html](index.html). Notable anchors:

| What | Where |
|---|---|
| Brand colours and type scale | `:root` CSS variables, near the top |
| WhatsApp number | `var WA_NUMBER` in the script block near the bottom |
| Client logo marquee | the two `.logo-item` rows (duplicated for a seamless scroll) |
| SVG icon sprite | `<symbol id="i-*">` definitions, referenced by `<use href="#i-*">` |

### Preview locally

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

Open it through a server, not by double-clicking the file — `file://` breaks relative
asset paths and some browser APIs.

---

## Deploying

`main` is the production branch. Push to it and Vercel rebuilds automatically.

```bash
git add -A
git commit -m "Describe the change"
git push origin main
```

Any other branch gets its own preview URL — use one for anything you would not want
landing in front of paid traffic.

Full first-time setup and launch checks: [docs/DEPLOY.md](docs/DEPLOY.md).
