# Whalen Sawmill Auction — Resale Analysis (MkDocs site)

A single-page [MkDocs](https://www.mkdocs.org/) site (using the [Material](https://squidfunk.github.io/mkdocs-material/) theme) presenting Facebook Marketplace resale values, sales comps, new-cost references, and estimated auction hammer prices for the **Whalen Realty Sawmill Equipment Auction** (formerly Glen Garbers Sawmill) — Wauseon, OH, June 13, 2026.

The content lives in **[`docs/index.md`](docs/index.md)**. Everything else here is the site scaffolding.

## Project structure

```
AuctionInfo/
├─ docs/
│  └─ index.md                  ← the auction analysis (edit this)
├─ mkdocs.yml                   ← site config + Material theme
├─ requirements.txt             ← Python dependencies (pinned)
├─ .github/workflows/deploy.yml ← auto-deploy to GitHub Pages on push to main
├─ .gitignore                   ← ignores .venv/ and site/
├─ .venv/                       ← local virtual environment (git-ignored)
└─ README.md                    ← you are here
```

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

1. **Set your site URL.** Edit `mkdocs.yml` and change `site_url:` to your real Pages URL:
   ```yaml
   site_url: https://<your-github-username>.github.io/<your-repo-name>/
   ```
   (For a repo named `AuctionInfo` owned by user `jdoe`, that's `https://jdoe.github.io/AuctionInfo/`.)

2. **Put the project on GitHub** (this folder is not yet a git repo):
   ```powershell
   git init
   git add .
   git commit -m "Auction resale analysis MkDocs site"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<your-repo-name>.git
   git push -u origin main
   ```

### Option A — Automatic deploy via GitHub Actions (recommended)

The included workflow [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml) builds and publishes the site on every push to `main`.

1. On GitHub, go to **Settings → Pages → Build and deployment**.
2. Set **Source** to **GitHub Actions**.
3. Push any change to `main` (or run the workflow manually from the **Actions** tab → *Deploy MkDocs site to GitHub Pages* → **Run workflow**).
4. After the run finishes (green check), your site is live at the `site_url` above.

That's it — from then on, editing `docs/index.md` and pushing to `main` redeploys automatically.

### Option B — Manual deploy from your machine

MkDocs can build and push the site to a `gh-pages` branch in one command:

```powershell
mkdocs gh-deploy
```

Then set **Settings → Pages → Source** to **Deploy from a branch**, branch **`gh-pages`** / folder **`/ (root)`**.

> Use **either** Option A **or** Option B, not both — they manage Pages differently. Option A (Actions) is preferred; `gh-deploy` is handy for a quick one-off publish.

## Updating the analysis

1. Edit `docs/index.md`.
2. Preview locally with `mkdocs serve`.
3. Commit and push:
   ```powershell
   git add docs/index.md
   git commit -m "Update auction figures"
   git push
   ```
4. With Option A configured, the live site updates within a minute or two.

## Notes

- All figures in the analysis are **estimates for personal bidding research**, not guaranteed valuations. See the *Methodology & caveats* section at the bottom of the page.
- To upgrade the theme later: bump the versions in `requirements.txt`, run `pip install -r requirements.txt --upgrade`, re-test `mkdocs serve`, and commit.
