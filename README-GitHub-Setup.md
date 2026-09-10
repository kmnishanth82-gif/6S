# 6S App — GitHub Pages Setup

This gives your team a permanent web link (e.g. `https://yourname.github.io/6s-app/`)
that anyone can open, on any phone, anytime — no downloading a file each time.

## ⚠️ Fixing "the app is not opening" — start completely clean

Your repo has been through several incremental updates (icons moved, service
worker added, concurrency fixes, etc.), and it's easy for a repo to end up
with a mismatched mix of old and new files. The safest fix is to wipe the
repo and re-upload everything fresh, all at once, from this batch of files —
don't mix in anything from earlier uploads.

1. In your GitHub repo, delete every existing file (select all → Delete files → Commit)
2. Upload the fresh set below, all in one go (Step 3)
3. Redeploy the Apps Script as a genuinely **new version** (see Step 1) —
   this matters even if the URL hasn't changed, otherwise the newer app
   features (like conflict-safe saving) will silently fail
4. On your phone, fully close the app and clear the site's data/cache before
   reopening the link, so nothing old is still cached locally

## What's new in this version
- **New EHS module** — a dedicated tab (bottom nav) for logging Incidents/
  Injuries, Near Misses, Hazards, and Unsafe Acts, independent of the
  9-department 6S tracking. Each entry records type, location, severity
  (low/medium/high), description, reporter, and an optional photo; open
  items can be closed with a corrective action and closer's name. EHS reps
  (Selvam, Ashik) are shown on the tab. Open high-severity items surface in
  the alerts bell, on the Home screen snapshot, in the Excel export
  ("EHS Log" sheet), and — once you wire up the daily digest — in the daily
  email summary too.
- **Icon replaced** with your 6S color wheel graphic (Sort/Set in Order/
  Shine/Standardize/Sustain/Safety) — replaces the previous shield badge
- **Clearer error handling** — if something ever fails to load again, you'll
  see an actual error message and a Retry button instead of an endless
  spinner, which makes it much easier to diagnose next time
- Runs entirely off **your Google Sheet + Google Drive** (no dependency on Claude at all)
- Photos upload straight to a "6S App Photos" folder in your Google Drive
- 9 departments, 3-phase timeline, reviewer-gated scoring (Team Leader + 2 HO
  Reps only, with names recorded), meetings, verification, alerts bell,
  photo timeline, and Excel export — all conflict-safe for multiple users

## Updating an existing deployment for the EHS module
The EHS module needs the updated `Google-Sheets-Sync-Script.gs` (it fixes
how "close an item" updates work for a plain list like the EHS log — the
old script only handled that pattern for lists nested inside an object).
If you already have a Sheet + Apps Script deployment:
1. Open your Sheet → Extensions → Apps Script
2. Select all the existing code and replace it with the new
   `Google-Sheets-Sync-Script.gs` in full
3. Deploy → Manage deployments → Edit (pencil icon) → Version: **New
   version** → Deploy
4. No changes needed to the Sheet itself — an "EHS Incidents" tab will be
   created automatically the first time someone logs an entry

## Step 1 — Set up the Google Sheet backend (5 min)
Follow the instructions inside `Google-Sheets-Sync-Script.gs` — paste it into
Apps Script on a Google Sheet, deploy as a Web App, and copy the `/exec` URL
it gives you. You'll paste that URL into the app on first launch.

*(If you already have a deployment, go to Deploy → Manage deployments → Edit
→ New version → Deploy, so it definitely picks up the latest script.)*

## Step 2 — Create a GitHub repository
1. Go to [github.com](https://github.com) and sign in (or create a free account)
2. Click the **+** icon (top right) → **New repository**
3. Name it something like `6s-app`
4. Set it to **Public** (required for free GitHub Pages)
5. Click **Create repository**

## Step 3 — Upload the app files
1. In your new repository, click **Add file** → **Upload files**
2. Upload these, keeping the exact names and folder structure:
   - `index.html`
   - `manifest.json`
   - `sw.js`
   - the whole `icons` folder (5 PNG files inside it)
3. Click **Commit changes**

`sw.js` is a small service worker — it's what makes Chrome/Android offer a
real **"Install app"** option instead of just a bookmark shortcut. On
iPhone, "Add to Home Screen" is Apple's install mechanism either way, so
that behaves correctly without it.

This also gives the app a proper icon — when someone installs it, it shows
the 6S badge instead of a generic browser icon.

## Step 4 — Turn on GitHub Pages
1. In the repository, go to **Settings** → **Pages** (left sidebar)
2. Under "Build and deployment" → **Source**, select **Deploy from a branch**
3. Branch: `main`, Folder: `/ (root)` → **Save**
4. Wait 1–2 minutes. GitHub will show your live URL, something like:
   `https://yourusername.github.io/6s-app/`

## Step 5 — First launch
Open that URL on your phone or laptop. It will ask you to paste your Google
Sheet Web App URL from Step 1 — do that once, and it's remembered on that
device from then on. Anyone else opening the link will be asked to connect
too (paste the same Web App URL) — after that, everyone reads and writes the
same shared Sheet.

## Updating the app later
If you want changes made to the app in future, upload a new `index.html` to
replace the old one in the same repository (Step 3) — GitHub Pages updates
automatically within a minute or two.
