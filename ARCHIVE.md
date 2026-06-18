# Archive Notice — prod-ops-dashboard

## Status: Active (GitHub Pages deployment)

This repository serves a single purpose: hosting the **Prod Ops Insights Predictive Modeling Dashboard** on GitHub Pages.

**Live URL:** https://muddaqureshi.github.io/prod-ops-dashboard/

---

## What This Repo Contains

| File | Purpose |
|------|---------|
| `index.html` | The live dashboard — single-file, self-contained HTML/JS/CSS |
| `README.md` | Documentation: purpose, data sources, model methodology, update instructions |
| `ARCHIVE.md` | This file |

---

## What Was Accomplished

The dashboard delivers GitHub platform growth forecasts through December 2026 across 6 metrics (repos, PRs, commits, issues, Actions runs, Actions minutes). Built with a 5-model Darts ensemble (Prophet, LinearRegression, ExpSmoothing, AutoTheta, Monte Carlo) weighted by inverse-MAPE from 12-month holdout validation.

**Latest deployment:** commit `e4ff9a7` (June 18, 2026) — final accuracy fixes applied.

**Three fixes in the final release:**
1. `25e9f37` — YoY chart: removed false smoothing (tension 0.3 → 0)
2. `929b9a2` — Main chart x-axis: weekly scale → monthly scale
3. `e4ff9a7` — May 2026 data: complete monthly totals instead of partial weekly rollup

---

## Source of Truth

All modeling work, training scripts, backtest results, and documentation live in the working repo:

**[github/prod-ops-predictive-modeling](https://github.com/github/prod-ops-predictive-modeling)**

---

## If This Repo Is Archived

If this repository is archived in the future:
- The last deployed dashboard will remain accessible at the GitHub Pages URL above as long as Pages remains enabled
- All source work and documentation is preserved in `github/prod-ops-predictive-modeling`
- The canonical data queries are documented in that repo's `CANONICAL_QUERIES_AND_DATA.md`

---

*Last updated: June 2026 · Internal GitHub use only*
