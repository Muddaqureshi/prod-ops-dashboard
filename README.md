# Prod Ops Insights — Predictive Modeling Dashboard

**Live Dashboard:** https://muddaqureshi.github.io/prod-ops-dashboard/

GitHub platform growth forecasts through December 2026, built on canonical data and a 5-model Darts ensemble.

---

## 📊 Dashboard Purpose

Predictive modeling for 6 GitHub platform metrics:

| Metric | Description |
|--------|-------------|
| **Repos Created** | Monthly new repository creation |
| **PR Merges** | Monthly pull request merges |
| **Issues Created** | Monthly issues opened |
| **Commits** | Monthly commits pushed (deduplicated) |
| **Actions Runs** | Monthly workflow runs (Jun 2025+) |
| **Actions Minutes** | Monthly compute time (Jun 2025+) |

Used by Tala Karadsheh (Sr. Dir, Insights & Product Ops) and the Prod Ops Insights team for Q1 FY27 strategic planning ("The Forward Commit").

---

## 🏗️ Data Sources

All data queried from **Trino** (`delta.canonical.*` tables) — spam/staff-filtered canonical source of truth.

| Metric | Trino Table | Date Range |
|--------|-------------|------------|
| Repos | `delta.canonical.repositories_all` | May 2025 – May 2026 |
| PRs | `delta.canonical.pull_requests_all` | May 2025 – May 2026 |
| Issues | `delta.canonical.issues_all` | May 2025 – May 2026 |
| Commits | `delta.canonical.commits_all` | May 2025 – May 2026 |
| Actions Runs | `delta.canonical.github_actions_workflow_runs_all` | May 2025 – May 2026 |
| Actions Minutes | `delta.canonical.github_actions_workflow_runs_all` | May 2025 – May 2026 |

**Training window:** 13 months of actuals (May 2025 – May 2026)  
**Forecast horizon:** Jun–Dec 2026 (7 months)

---

## 🤖 Model Methodology

**5-Model Darts Ensemble** with inverse-MAPE weighted averaging:

| Model | Type | Notes |
|-------|------|-------|
| **Prophet** | Additive (Meta/Darts) | Automatic changepoints, yearly seasonality |
| **LinearRegression** | ML with lagged features (Darts) | 12-month lag window |
| **ExponentialSmoothing** | Triple exponential (Darts) | Seasonal decomposition |
| **AutoTheta** | Optimized Theta (Darts) | Auto-selects decomposition type |
| **Monte Carlo** | Custom 10K simulation | Trailing 12-month growth distribution |

**Validation:** 12-month holdout backtest per metric. Models exceeding 60% MAPE excluded.  
**Per-metric selection:** Monte Carlo excluded from Actions metrics (620% MAPE on short series).  
**Ensemble weighting:** Each model weighted by `1/MAPE` from holdout validation, normalized to sum to 1.

See [`github/prod-ops-predictive-modeling`](https://github.com/github/prod-ops-predictive-modeling) for training scripts, backtest results, and full methodology docs.

---

## 🔧 Recent Fixes (June 18, 2026)

Three fixes applied to improve visual accuracy of the live dashboard:

| Commit | Fix | Details |
|--------|-----|---------|
| `25e9f37` | YoY chart stepped lines | Changed Chart.js `tension: 0.3 → 0` for honest flat monthly bars |
| `929b9a2` | X-axis scale | Fixed weekly → monthly axis ticks (was showing misleading scale jumps) |
| `e4ff9a7` | May data source | Used complete monthly totals instead of incomplete weekly rollup for May 2026 |

---

## 🚀 Update Instructions

The dashboard is a single self-contained `index.html` with hardcoded forecast values. To update with new actuals or re-run the model:

1. **Re-train models** in [`github/prod-ops-predictive-modeling`](https://github.com/github/prod-ops-predictive-modeling):
   ```bash
   cd prod-ops-predictive-modeling
   python3 scripts/refresh_predictive.py
   ```
2. **Copy updated HTML** → replace `index.html` in this repo
3. **Push to main** → GitHub Pages auto-deploys within ~1 min

---

## 👥 Collaboration

- **Dashboard design & modeling:** @Muddaqureshi
- **Predictive modeling collaboration:** @Deepthi (Deepthi Coppisetty, Principal TPM, Engineering) — h/t for original dashboard design and data validation
- **Dashboard requirements & sponsor:** Tala Karadsheh (Sr. Dir, Insights & Product Ops)
- **Data query validation:** @leehjoshua (Josh Lee)

---

## 📁 Repo Structure

```
index.html      — Live dashboard (single-file, self-contained)
README.md       — This file
ARCHIVE.md      — Repo status and archiving context
```

This repo exists solely to serve GitHub Pages. All source work lives in [`github/prod-ops-predictive-modeling`](https://github.com/github/prod-ops-predictive-modeling).

---

*Internal GitHub use only.*
