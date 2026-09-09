# ADHD Budget Planner — free live demo

The hosted, capped demo of [FinNestStudio's ADHD Budget Planner & Safe to Spend
Tracker](https://www.etsy.com/shop/FinNestStudio). One file, no build step, no
dependencies — GitHub Pages serves `index.html` and that is the whole site.

## What the demo is

The real planner, with example data already in it, limited to **40 logged
spends**. Everything else works: bills, goals, the month view, milestones, the
money check, CSV export, backup export.

- **Nothing leaves the browser.** No server, no account, no analytics, no
  network calls of any kind. The one outbound link is the Etsy shop button.
- **Sandboxed storage.** The demo saves under `finnest_adhd_budget_demo`, so it
  can never touch a buyer's real file at `finnest_adhd_budget_v1` — even if
  someone opens both from the same browser.
- **The work is portable.** At the cap the demo offers a backup download. That
  file is a standard FinNest backup and the paid planner opens it with every
  spend, bill and goal intact. This is proved by a test, not assumed — see
  `_build/adhd-v23/test/demo-cap.test.mjs` in the main project.

## Publishing it

The repo has no build step: whatever `index.html` contains is what visitors get.

```bash
git init -b main
git add -A
git commit -m "ADHD Budget Planner free demo"
git remote add origin https://github.com/<your-username>/<repo-name>.git
git push -u origin main
```

Then in the repo on github.com: **Settings → Pages → Source: Deploy from a
branch → Branch: `main` / `/ (root)` → Save.**

The site appears at `https://<your-username>.github.io/<repo-name>/` within a
minute or two. Put that URL in the Etsy listing description and in the shop
announcement.

`.nojekyll` is present so GitHub serves the files as-is rather than running them
through Jekyll.

## Updating it after a product release

From the main project, one command rebuilds the demo from the current source and
drops it here:

```bash
node _build/adhd-v23/build.mjs --demo --deploy
```

Then commit and push this repo again. The build refuses to ship if the demo is
not sandboxed, if the cap is missing, or if the cap has landed outside the app
closure where it cannot work.

## The two settings worth changing

Both live at the top of `_build/adhd-v23/src/90-demo-cap.js` in the main
project — change them there and rebuild, not in `index.html`, which is
generated.

| Setting | Now | Note |
|---|---|---|
| `DEMO_CAP` | `40` | Matches what the competing hosted demos allow. |
| `DEMO_BUY_URL` | shop front, `?ref=demo` | Swap in the direct listing URL once it is live — it converts better, and the `ref` lets Etsy stats show how much traffic the demo sends. |

## What is deliberately not here

The buyer file. `index.html` is built from the same source but is a different
build: the paid download has no cap, no Etsy link, and no demo code in it at
all. The build asserts this on every run.
