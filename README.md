# Workout Check Log — Ram & Jay

A simple mobile-first webapp for logging a workout as Done or Not Done, for Ram and Jay, over a 45-day plan.

**Live link:** https://jkakarlapudi.github.io/workout-check-log/

## The plan

45 days, Mon 9 Nov 2026 – Wed 23 Dec 2026.

- Day 1 – Chest & Back 1
- Day 2 – Arms 1
- Day 3 – Legs & Shoulders
- Day 4 – Rest
- Day 5 – Chest & Back 2
- Day 6 – Arms 2 & Delts
- Day 7 – Legs 2 & Posterior Chain

…and that 7-day cycle repeats through Day 45 (Wed 23 Dec 2026, Legs & Shoulders).

Today, Sat 21 Nov 2026, is Day 13 – Arms 2 & Delts.

## What lives where

- `index.html` — the whole app. Vanilla HTML/CSS/JS, no build step, no framework to install. It fetches `data/workout-log.csv` on load (cache-busting) and also embeds the same 45 rows as a fallback if the fetch fails.
- `data/workout-log.csv` — the source of truth. One row per date: `Date,Day,Weekday,Planned session,Ram,Jay,Notes`. Ram and Jay start blank. Edit this file and the app shows it.
- `.nojekyll` — tells GitHub Pages to serve the site as-is.
- `.github/workflows/pages.yml` — deploys this repo to GitHub Pages on every push to `main`.

## How saving works

There is **no token in this code and no token in any file in this repo**.

To save a tap back to the repo:

1. Make a fine-grained GitHub personal access token:
   GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token.
   Give it access to **only** `jkakarlapudi/workout-check-log`, with **Contents: Read and write** only. Set a short expiry.
2. Open the live link on your phone, scroll to **Settings**, paste the token, and tap **Save token on this device**.
3. The token is stored **only** in that browser's `localStorage`. It is sent **only** to `api.github.com` when saving. Ram and Jay each paste the token on their own phone. Never share a screenshot of it, and revoke it on GitHub if it leaks.

When a token is set, tapping Done / Not Done:
- updates the CSV in the app,
- calls the GitHub Contents API — `GET` the file to get its SHA, then `PUT` the updated CSV with a message like `Log Day 13: Ram Done, Jay Not Done`,
- shows Saving / Saved / error, then refreshes from the repo.

When no token is set, the app is read-only for GitHub: you can still tap to preview changes locally and use **Download updated CSV** to get the updated file. Nothing is written to the repo.

## Streaks

- Total Done counts every `Done` in that person's column.
- Current streak counts consecutive Done days ending today or yesterday.
- Rest days (Days 4, 11, 18, 25, 32, 39) can still be logged, but they never break a streak.
- A blank or Not Done day on a training day breaks the streak.

## Deploy

Push to `main` and GitHub Pages deploys automatically via `.github/workflows/pages.yml`. The site is public, so anyone with the link can read the log — only someone with a token can write to it.
