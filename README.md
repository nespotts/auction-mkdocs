# Whalen Sawmill Auction — Resale Analysis (MkDocs site)

A single-page [MkDocs](https://www.mkdocs.org/) site (using the [Material](https://squidfunk.github.io/mkdocs-material/) theme) presenting Facebook Marketplace resale values, sales comps, new-cost references, and estimated auction hammer prices for the **Whalen Realty Sawmill Equipment Auction** (formerly Glen Garbers Sawmill) — Wauseon, OH, June 13, 2026.

The content lives in the **`docs/`** pages (start at [`docs/index.md`](docs/index.md)). Everything else here is the site scaffolding.

## Project structure

```
AuctionInfo/
├─ docs/
│  ├─ index.md                  ← overview, terms & master summary table (links to each lot)
│  ├─ details-1-sawmill.md      ← Lots 1–4  (core sawmill & blade maintenance)
│  ├─ details-2-woodshop.md     ← Lots 5–9, 15, 18–24, 26  (woodworking shop machines)
│  ├─ details-3-specialty.md    ← Lots 11–14, 16–17, 27  (specialty wood-products machines)
│  ├─ details-4-handling.md     ← Lots 10, 25, 28–34  (material handling, tanks, chainsaw)
│  ├─ details-5-tractors.md     ← Lots 35–38  (tractors, loader & chipper)
│  ├─ details-6-trailers.md     ← Lots 39–44  (containers, trailers & forestry)
│  ├─ ring2.md                  ← Ring 2 pole-barn items
│  └─ bidding.md                ← bidding cheat-sheet + methodology & caveats
├─ mkdocs.yml                   ← site config, Material theme, page nav
├─ requirements.txt             ← Python dependencies (pinned)
├─ .github/workflows/deploy.yml ← auto-deploy to GitHub Pages on push to master
├─ .gitignore                   ← ignores .venv/ and site/
├─ .venv/                       ← local virtual environment (git-ignored)
└─ README.md                    ← you are here
```

Each lot in the master summary table on `index.md` links to its detail section; every detail section links back to the summary and out to the live Proxibid lot page.

## Prerequisites

- **Python 3.9+** (built and tested with 3.14). Check with `python --version`.
- **Git**, and a **GitHub account** for deployment.

## Local preview / editing

The virtual environment (`.venv/`) is already created. From the project folder:

**Windows (PowerShell):**
```powershell
.\.venv\Scripts\Activate.ps1          # activate the venv
pip install -r requirements.txt       # (first time only) install deps
mkdocs serve                          # live preview at http://127.0.0.1:8000
```

**macOS / Linux (bash):**
```bash
source .venv/bin/activate
pip install -r requirements.txt
mkdocs serve
```

> If activation is blocked on Windows, either run the binaries directly without
> activating (e.g. `.\.venv\Scripts\mkdocs.exe serve`) or allow scripts for your
> user once: `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`.

`mkdocs serve` auto-reloads as you edit `docs/index.md`. Edit the Markdown, save, and the browser refreshes.

To produce the static site in `site/` without serving:
```powershell
mkdocs build --strict
```
(`--strict` turns warnings into errors — the same check the deploy workflow runs.)

## Deploy to GitHub Pages

### One-time setup

The project is already on GitHub at **https://github.com/nespotts/auction-mkdocs** (branch `master`), and `mkdocs.yml` already has the matching `site_url` (`https://nespotts.github.io/auction-mkdocs/`) and `repo_url`. There's nothing to scaffold — just enable Pages (Option A below).

> If you fork or rename the repo, update `site_url` and `repo_url` in `mkdocs.yml` to match.

### Option A — Automatic deploy via GitHub Actions (recommended)

The included workflow [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml) builds and publishes the site on every push to `master`.

1. On GitHub, go to **Settings → Pages → Build and deployment**.
2. Set **Source** to **GitHub Actions**.
3. Push any change to `master` (or run the workflow manually from the **Actions** tab → *Deploy MkDocs site to GitHub Pages* → **Run workflow**).
4. After the run finishes (green check), your site is live at **https://nespotts.github.io/auction-mkdocs/**.

That's it — from then on, editing `docs/index.md` and pushing to `master` redeploys automatically.

### Option B — Manual deploy from your machine

MkDocs can build and push the site to a `gh-pages` branch in one command:

```powershell
mkdocs gh-deploy
```

Then set **Settings → Pages → Source** to **Deploy from a branch**, branch **`gh-pages`** / folder **`/ (root)`**.

> Use **either** Option A **or** Option B, not both — they manage Pages differently. Option A (Actions) is preferred; `gh-deploy` is handy for a quick one-off publish.

## Updating the analysis

1. Edit the relevant page under `docs/` (the summary table is in `docs/index.md`; per-lot detail is in the `details-*.md` pages).
2. Preview locally with `mkdocs serve`.
3. Commit and push:
   ```powershell
   git add docs/
   git commit -m "Update auction figures"
   git push
   ```
4. With Option A configured, the live site updates within a minute or two.

## Notes

- All figures in the analysis are **estimates for personal bidding research**, not guaranteed valuations. See the *Methodology & caveats* section at the bottom of the page.
- To upgrade the theme later: bump the versions in `requirements.txt`, run `pip install -r requirements.txt --upgrade`, re-test `mkdocs serve`, and commit.
