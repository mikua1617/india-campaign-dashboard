# India campaign dashboard — setup checklist

This reuses everything from the US dashboard setup — same Instantly account,
same Google service account, same Gmail sender. If you still have those
values handy, this is a 10-minute setup, not a from-scratch build.

## 1. Create the repo
1. github.com → New repository → name it `india-campaign-dashboard`.
2. Public (same reasoning as before — the published data is aggregate
   counts only, no PII, so a public repo for free GitHub Pages is fine).
3. Don't initialize with a README.

## 2. Upload the files
Same structure as the US repo:
```
.github/workflows/daily-update.yml
docs/index.html
docs/data.json
fetch_data.py
email_report.py
leads_sheet_sync.py
README.md
```

## 3. Add secrets — reuse the same three values from the US repo
Repo → Settings → Secrets and variables → Actions → New repository secret,
for each of:
- `INSTANTLY_API_KEY` — same value as the US repo
- `GMAIL_ADDRESS` — same value as the US repo
- `GMAIL_APP_PASSWORD` — same value as the US repo
- `GOOGLE_SERVICE_ACCOUNT_JSON` — same value as the US repo (same service
  account works across any number of repos — nothing India-specific to set
  up here)

No new Google Cloud project, no new service account, no new app password —
all of that was one-time setup you already did.

## 4. Enable GitHub Pages
Settings → Pages → Source: "Deploy from a branch" → Branch: `main`,
folder: `/docs` → Save. Your link:
`https://mikua1617.github.io/india-campaign-dashboard/`

## 5. Test run
Actions tab → "Daily Instantly dashboard update" → "Run workflow".

## What's different from the US version
- Filters campaigns by `India_` prefix instead of `US_`
- Report sends to Tarika, Shubham, Saurabh, and Aishwerya (set in
  `RECIPIENTS` in `email_report.py`). The same 4 people also get view
  access to the per-campaign lead Sheets and master pivot (set in
  `SHARE_WITH` in `leads_sheet_sync.py`) -- both lists are kept in sync
  manually, edit both if the recipient list changes.
- Leads sheets are created inside a dedicated "India" subfolder within the
  same shared drive used for the US sheets — no separate sharing needed
  since you already have manager access to the whole shared drive
- Cron schedule is the same weekday-only 8am IST slot as the US repo — both
  will run independently, no conflict

## Everything else
Identical to the US dashboard: daily upsert logic, rolling-24h sent/replies,
lifetime bounce/opens/clicks tracking, master engagement pivot across all
active India campaigns, and on-request Lusha phone enrichment (just ask,
same as before — no automation, confirmed cost before any credits spend).
