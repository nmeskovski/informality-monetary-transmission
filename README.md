# Formal Shocks, Informal Buffers

**Identifying the Conditional Transmission of Monetary Policy Through Panel Local Projections, 2000–2019**

Nikola Meshkovski · BSc Applied Economics · Corvinus University of Budapest · 2026
Instructor: Ahmet Benlialper

---

## Overview

This repository contains the paper and the full replication package for *Formal Shocks, Informal Buffers*. The project tests whether the size of the informal economy moderates monetary-policy transmission to inflation and real output. It combines the identified monetary-policy shocks of Choi, Willems, and Yoo (2024) with the MIMIC-based informality estimates of Medina and Schneider (2018) and applies a panel local-projection (LP) framework to 122 countries over 2000–2019, with separate estimation for developed economies and EMDEs.

Headline contributions:

1. Large-panel tests using identified shocks rather than Taylor-rule residuals or policy-rate changes.
2. Separate treatment of the inflation and real-output margins, which respond differently.
3. Evidence that informality's moderating role is concentrated in emerging economies, consistent with a threshold interpretation.

The paper itself is `Project3_Nikola.pdf` at the top of this repository. Everything needed to reproduce the empirical results lives in `replication/`.

---

## Repository structure

```
.
├── README.md                                  ← this file
├── Project3_Nikola.pdf                        ← the paper (final, formatted)
├── .gitignore
│
└── replication/                               ← self-contained reproduction bundle
    ├── Project3_Nikola.qmd                    ← Quarto source (R + prose)
    ├── Project3_Nikola.pdf                    ← rendered output of the .qmd
    ├── renv.lock                              ← exact package versions
    ├── renv/                                  ← renv project environment
    │   ├── activate.R
    │   └── settings.json
    │
    ├── data/                                  ← raw input data (read-only)
    │   ├── data_raw_theglobaleconomy.xlsx     ← macro & governance panel
    │   ├── data_raw_monetary-transmission.xlsx← Choi–Willems–Yoo shock series
    │   ├── Real_GDP_pc_WorldBank.xls          ← World Bank real GDP per capita
    │   └── metadata_theglobaleconomy_19-03-26.txt ← variable definitions
    │
    ├── figs/                                  ← all figures (PDF), generated
    └── tables/                                ← all tables (xlsx), generated
        └── descriptive_tables/
```

The two top-level `.pdf` files are intentional. The one at the root is the polished paper for casual readers; the one inside `replication/` is rebuilt every time the `.qmd` is rendered and shows whatever the current code produces.

---

## Data sources

| File                                          | Source                                           | What it provides                                                          |
| --------------------------------------------- | ------------------------------------------------ | ------------------------------------------------------------------------- |
| `data_raw_theglobaleconomy.xlsx`              | TheGlobalEconomy.com (compiled WB / IMF / FH)    | Inflation, growth, governance, financial-depth, MIMIC informality, etc.   |
| `data_raw_monetary-transmission.xlsx`         | Choi, Willems, & Yoo (2024)                      | Identified country-year monetary-policy shocks (`MPS`)                    |
| `Real_GDP_pc_WorldBank.xls`                   | World Bank, World Development Indicators         | Real GDP per capita (constant 2015 US$), used for development split       |
| `metadata_theglobaleconomy_19-03-26.txt`      | TheGlobalEconomy.com                             | Variable definitions and source citations for the main panel              |

All raw data files are committed as-is so the analysis is bit-for-bit reproducible. Refer to the original providers' terms before redistributing them outside this repository.

---

## Reproducing the results

### Requirements

- **R** ≥ 4.3
- **Quarto** ≥ 1.4 (`quarto --version`)
- **TinyTeX** or another LaTeX distribution with `xelatex` (the paper uses Times New Roman; `xelatex` is set in the YAML header)
- **renv** (will be installed automatically by `renv::restore()` if missing)

### Steps

```bash
# 1. Clone
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>/replication

# 2. Restore the exact R package environment
Rscript -e 'install.packages("renv"); renv::restore()'

# 3. Render the Quarto document
quarto render Project3_Nikola.qmd
```

The first render takes several minutes — the panel local projections estimate 12 horizons across three dependent variables and multiple sub-samples, and chunk caching is on by default. Subsequent renders are much faster. If you change the data or specifications, set `cache: false` in the YAML header (or delete the `_freeze/` directories) before rendering.

### What gets produced

- `Project3_Nikola.pdf` — the rebuilt analysis document
- `figs/*.pdf` — every figure referenced in the paper, named after its chunk label
- `tables/descriptive_tables/*.xlsx` — descriptive and missingness tables

These directories are emptied and regenerated on each render; do not store anything you want to keep inside them.

---

## Methodology, in brief

The estimation framework is the panel local-projection regression of Jordà (2005), with a contractionary monetary shock interacted with the (lagged) MIMIC informality share. The baseline specification includes country and year fixed effects, lags of the dependent variable and shock, and standard macroeconomic controls. Inference uses Driscoll–Kraay-style standard errors clustered on country.

Pre-estimation diagnostics include panel unit-root tests, AR(1) persistence checks on the MIMIC moderator, within–between variation decomposition, multicollinearity (VIF), and outlier visualization with winsorization. A governance principal-component composite is constructed from four World Bank governance indicators. See §4 of the paper for the full identification strategy and §5 for results.

---

## How to cite

If you use the code or build on the analysis, please cite the paper:

> Meshkovski, N. (2026). *Formal Shocks, Informal Buffers: Identifying the Conditional Transmission of Monetary Policy Through Panel Local Projections, 2000–2019.* BSc Project, Corvinus University of Budapest.

A BibTeX entry will be added once the paper has a DOI or permanent archive.

---

## Use of generative AI

In line with Provision 6/2025 of the Corvinus Vice-Rector for Academic Programmes, the use of generative AI is disclosed in the front matter of the paper. AI assistance was limited to wording refinement, LaTeX equation formatting, literature discovery, and phrasing of technical definitions. The empirical workflow — data cleaning, merging, estimation, diagnostics, robustness — was conducted independently in R and Quarto, and all results were verified by the author.

---

## License

Code in this repository is released under the MIT License (see `LICENSE`, if present). The raw data files retain the licenses of their original providers and are included here for replication only — please consult the source organizations before redistributing them.

---

## Contact

Nikola Meshkovski · Corvinus University of Budapest · BSc Applied Economics
For questions about reproducing the results, please open a GitHub issue.
