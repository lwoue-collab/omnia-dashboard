# Red Points — AI Visibility Dashboard

Weekly LLM visibility monitoring for Red Points, built on GitHub Pages.

## What it tracks

- **BOFU category-aware mention rate** — unprompted share of voice on neutral queries (no vendor named)
- **MOFU and TOFU mention rates** — new prompts added April 2026, data building
- **Visibility by content theme** — fake-products, brand-impersonation, unauthorized-sellers, manual-enforcement, global-enforcement, category-aware
- **Per-prompt breakdown** — who LLMs mention for each of the 18 category-aware prompts
- **Content priorities** — derived from visibility gaps
- **Typeform pipeline** — new prompts pending review

## Setup (one time, ~30 minutes)

### 1. Create the GitHub repo

```bash
git init omnia-dashboard
cd omnia-dashboard
# copy all files here
git add .
git commit -m "initial dashboard"
git remote add origin https://github.com/YOUR-ORG/omnia-dashboard.git
git push -u origin main
```

### 2. Enable GitHub Pages

- Go to repo Settings → Pages
- Source: **Deploy from a branch**
- Branch: `main` / root `/`
- Save — your dashboard will be live at `https://YOUR-ORG.github.io/omnia-dashboard/`

### 3. Add your Omnia API key

- Go to repo Settings → Secrets and variables → Actions
- Click **New repository secret**
- Name: `OMNIA_API_KEY`
- Value: your Omnia API key (find in Omnia account settings)

### 4. Test the data refresh manually

```bash
export OMNIA_API_KEY=your_key_here
python fetch_data.py
```

This updates `data.json` with live Omnia data. Commit and push to see it on the dashboard.

### 5. Automatic weekly refresh

The GitHub Action runs every Monday at 08:00 UTC automatically. You can also trigger it manually:
- Go to repo → Actions → "Weekly Omnia data refresh" → Run workflow

## Files

```
index.html          — the dashboard (single page, reads data.json)
data.json           — all visibility data (updated weekly by the Action)
fetch_data.py       — Python script that pulls from Omnia API
.github/workflows/
  weekly-refresh.yml  — GitHub Actions workflow
```

## Adding new prompts

When you add new category-aware prompts to Omnia:
1. Add the prompt UUID and text to `CATEGORY_AWARE_PROMPTS` in `fetch_data.py`
2. Add the prompt to `category_aware_prompts` array in `data.json` with `"pending": true`
3. The next weekly refresh will populate the mention data automatically

## Updating tagging or themes

The theme tags pulled from Omnia are defined in `THEME_TAGS` in `fetch_data.py`. If you rename a tag in Omnia, update it here too.

## Who maintains this during mat leave

Waleska or Antonella can trigger the data refresh manually via GitHub Actions (no code needed) if the automatic Monday run fails. The dashboard itself requires no maintenance — it reads from `data.json` which the Action keeps updated.

If you need to add a new prompt to Omnia and want it tracked in the dashboard, update `fetch_data.py` (lines 30–50) and `data.json` (the `category_aware_prompts` array).
