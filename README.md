# HabitRPG — Static (GitHub Pages) Build

This is a self-contained, no-backend version of HabitRPG. It's a single
`index.html` file — no Flask, no database, no server of any kind.

## What changed from the Flask version

| Flask version | Static version |
|---|---|
| SQLAlchemy + SQLite (`database.py`) | Browser `localStorage` |
| Login/register (`flask_login`) | None — single local profile per browser |
| `model.py`, `licensing.py` on the server | Ported directly into JavaScript, running client-side |
| Pro paywall / licence keys (`licensing.py`) | Removed — a static page can't validate a payment, so every feature (tags, dashboard, stats, all frequencies, pet) is unlocked by default |
| "Store" for buying freeze tokens | Replaced with a "Data" panel: **Export** (download JSON backup), **Import** (restore from JSON), and a free "+3 freeze tokens" top-up, since there's no payment processor |
| Multi-device sync via server DB | None — data lives only in the browser that created it. **Export regularly if this matters to you.** |

All the streak math, XP/leveling, frequency handling (daily / weekly-days /
weekly-×-per-week / monthly), freeze tokens, tags, heatmap, dashboard, and
theme system are ported line-for-line from `model.py`, `themes.py`, and the
original `index.html`'s JS — so behavior should match the desktop/Flask app.

## Deploying to GitHub Pages

1. Create a new GitHub repo (or use an existing one), e.g. `habitrpg-web`.
2. Add this `index.html` to the repo root (or to a `/docs` folder — your choice).
3. Push it:
   ```bash
   git init
   git add index.html
   git commit -m "Static HabitRPG"
   git branch -M main
   git remote add origin https://github.com/<you>/habitrpg-web.git
   git push -u origin main
   ```
4. In the repo on GitHub: **Settings → Pages**.
   - Source: **Deploy from a branch**
   - Branch: `main`, folder `/ (root)` (or `/docs` if you put it there)
   - Save.
5. GitHub will give you a URL like `https://<you>.github.io/habitrpg-web/`.
   It can take a minute or two to go live the first time.

That's it — no build step, no dependencies, nothing else to configure.

## Notes / limitations to be aware of

- **One browser = one set of data.** Opening the Pages URL on your phone and
  your laptop gives you two separate, independent habit lists (unless you
  export from one and import into the other).
- **Clearing browser data wipes your habits.** Use the Export button
  periodically as a backup, especially before clearing cookies/site data or
  switching browsers.
- **Debug tools** (advance a day, for testing streak logic) are hidden by
  default — add `?debug=1` to the URL to reveal them in the Data panel.
- If you ever want real accounts + cross-device sync + real payments back,
  that requires a server again (the original Flask app, hosted on
  Railway/Render/Fly.io, etc.) — GitHub Pages can't do any of that on its own.
