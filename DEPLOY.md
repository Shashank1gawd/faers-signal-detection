# Upload to GitHub and publish

## Option A: browser only (easiest)

1. Sign in at github.com → **New repository**.
2. Repository name: `faers-signal-detection`. Choose **Public**. Do **not** tick "Add a README" (this folder already has one). Click **Create repository**.
3. On the new empty repo page click **uploading an existing file**.
4. Drag in **everything inside this folder**: `index.html`, `README.md`, `LICENSE`, `.gitignore` and the `docs` folder. (Drag the files, not a zip.)
5. Commit message: `Initial commit` → **Commit changes**.

## Option B: Git command line

```bash
cd faers-signal-detection
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/faers-signal-detection.git
git push -u origin main
```

## Publish the live app (GitHub Pages)

1. Repo → **Settings → Pages**.
2. **Source:** Deploy from a branch. **Branch:** `main`, folder `/ (root)` → **Save**.
3. Wait 1 to 2 minutes. The site appears at `https://YOUR-USERNAME.github.io/faers-signal-detection/`.
4. Paste that link into the README "Live demo" section and into the repo's **About** box (gear icon on the repo home page).

## Before you publish: edit placeholders

- `LICENSE`: replace `YOUR NAME`.
- `README.md`: replace `YOUR-USERNAME`, `YOUR NAME`, `YOUR-PROFILE`.

## Suggested repo settings

- **About → Topics:** `pharmacovigilance`, `drug-safety`, `healthcare`, `javascript`, `portfolio`, `faers`, `openfda`, `signal-detection`
