# Uplift modelling and experimentation framework

A worked analysis of randomised marketing experiments: how to check that a test can be trusted, estimate what it found, decide who a campaign should target, and say honestly what the data cannot tell you.

The project uses two public experiments. The Hillstrom email test (64,000 customers, interpretable features) is used first. The Criteo uplift dataset (about 14 million rows, anonymised features) follows, for effect heterogeneity at scale. A simulation layer, clearly labelled as simulated, covers topics the real data cannot, such as sequential testing.

## Status

| Notebook | Topic | Status |
|---|---|---|
| `00_data_acquisition` | Download Hillstrom data and verify it against published figures | Done |
| `01_experiment_diagnostics_and_power` | Randomisation checks, effect estimates with multiple-comparison control, covariate adjustment, power analysis checked by simulation | Done |
| `02_heterogeneous_effects_hillstrom` | Who responds: meta-learners, causal forest, Qini and uplift-by-decile evaluation | Planned |
| `03_criteo_uplift_at_scale` | Criteo v2 file checks, effect heterogeneity at scale, compliance-adjusted effects using the exposure field | Planned |
| `04_targeting_policy_and_budget` | Value of a targeting policy under stated economic assumptions, with uncertainty | Planned |
| `05_simulation_sequential_testing_and_cuped` | Sequential testing and variance reduction in simulated experiments | Planned |

## Results so far (notebook 01)

- **Randomisation looks sound.** Arm sizes match equal thirds (p = 0.90), the largest covariate imbalance is 0.014 standardised units against sampling noise of about 0.01, and covariates jointly carry no detectable information about assignment (p = 0.88).
- **Both emails raised visits, conversion and spend relative to no email**, and all six primary tests hold after Holm correction. Men's email: conversion from 0.57% to 1.25%, spend up about $0.77 per customer (95% CI about $0.49 to $1.06). Women's email: conversion up about 0.3 points, spend up about $0.42 per customer (95% CI about $0.17 to $0.68).
- **Covariate adjustment gave almost nothing** for this data: past-year spend is almost uncorrelated with two-week spend (r ≈ 0.015), so the best case was about 0.5% variance reduction.
- **The standard power formula is optimistic for spend.** Simulation puts power at the formula's minimum detectable effect at about 71% rather than 80%, because spend is heavy-tailed and its spread differs by arm (SD about 11.6 in control against 17.8 and 15.1 in the email arms).
- **Limits:** two-week window, customers who bought within the last year, gross revenue with no margin or email cost, and email arms that differ in content as well as audience. The notebook's closing section sets these out.

Stored outputs in notebook 01 were generated from a copy of the dataset that matches the published row count, arm sizes and summary rates. Running notebook 00 then 01 reproduces them from the original source.

## Repository layout

```
notebooks/            analysis notebooks, numbered in reading order
data/raw/             downloaded data (not committed; see data/README.md)
data/processed/       derived data (not committed)
reports/tables/       CSV tables written by the notebooks
requirements.txt      environment for notebooks 00-01
requirements-uplift.txt   additional libraries for notebook 02 onwards
```

## Running it

```bash
python -m venv .venv
source .venv/bin/activate            # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter lab
```

Open `notebooks/00_data_acquisition.ipynb` and run all cells, then `01_experiment_diagnostics_and_power.ipynb`. Both use fixed random seeds, and notebook 01 takes under a minute.

If the original data host is unreachable, set `HILLSTROM_SOURCE_URL` to a reachable copy before starting Jupyter. Notebook 00 verifies whichever file it gets against the published figures and refuses to continue if they do not match.

## Data and credit

Hillstrom data: Kevin Hillstrom, MineThatData E-Mail Analytics and Data Mining Challenge (2008). Criteo data, when added: Diemert, Betlei, Renaudin and Amini, "A Large Scale Benchmark for Uplift Modeling", AdKDD 2018. Details are in `data/README.md`.
