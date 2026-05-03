# Formal Shocks, Informal Buffers

**Identifying the Conditional Transmission of Monetary Policy Through Panel Local Projections, 2000–2019**

Nikola Meshkovski · BSc Applied Economics · Corvinus University of Budapest · 2026
Instructor: Ahmet Benlialper

📄 **Paper:** [`Meshkovski_Nikola_Formal_Shocks_Informal_Buffers_2026.pdf`](Meshkovski_Nikola_Formal_Shocks_Informal_Buffers_2026.pdf)
📦 **Replication package:** [`replication/`](replication/)

---

## What this paper does

Most cross-country evidence on monetary-policy transmission ignores the informal economy — yet the global shadow-economy share averages roughly 32% of GDP, with many emerging markets above 38%. Informal firms rarely borrow from regulated banks, informal workers are paid in cash, and informal prices track local conditions rather than monetary aggregates. Theory therefore predicts that a large informal sector should **attenuate** monetary transmission on two margins: dampening the inflation response (because informal prices are disconnected from the interest-rate channel) and dampening the output response (because informality absorbs workers displaced from the formal sector).

This paper tests that prediction using identified monetary-policy shocks for 122 countries over 2000–2019, in a panel local-projection framework that estimates impulse responses out to 12 horizons.

### Research question

Does the size of a country's informal economy condition the response of inflation and real output to identified monetary-policy shocks, and does that effect differ between developed economies and EMDEs?

### Theoretical mechanism

Two dual-sector channels (Castillo & Montoro 2012; Alberola & Urrutia 2020; Colombo, Menna & Tirelli 2019):

1. **Inflation channel.** Informal prices, set outside the formal interest-rate transmission, do not adjust to a contractionary shock — pulling the economy-wide inflation response toward zero.
2. **Output channel.** Workers displaced from the formal sector are absorbed by informal employment at lower wages, attenuating marginal-cost pressure and providing an output buffer.

Both channels predict weaker transmission where informality is larger.

### Empirical strategy

- **Shock series.** Identified monetary-policy shocks of Choi, Willems & Yoo (2024) — country-year residuals purged of macroeconomic forecasts and global factors. Using identified shocks rather than Taylor-rule residuals or raw policy-rate changes is one of the paper's three contributions.
- **Moderator.** MIMIC-based informal-economy share of GDP from Medina & Schneider (2018), entered with a lag. Within-country movement is slow, so MIMIC functions as a structural state variable.
- **Estimator.** Panel local projection (Jordà 2005), one regression per horizon $h = 0, \dots, 12$, of the form

  $$y_{i,t+h} - y_{i,t-1} = \alpha_i + \delta_t + \beta_h \cdot \text{MPS}_{i,t} + \gamma_h \cdot \text{MIMIC}_{i,t-1} + \theta_h \cdot (\text{MPS}_{i,t} \times \text{MIMIC}_{i,t-1}) + \mathbf{X}_{i,t}'\boldsymbol{\Phi}_h + \varepsilon_{i,t,h}$$

  with country and year fixed effects and lagged controls. The interaction coefficient $\theta_h$ is the object of interest.
- **Inference.** Driscoll–Kraay-style standard errors clustered on country.
- **Outcomes (three).** Inflation, log real GDP per capita, real GDP growth.
- **Sub-samples (three).** Full panel, developed, emerging — split at the World Bank / IMF $14{,}000 USD threshold on average 2000–2019 real GDP per capita.
- **Robustness.** Alternative governance specifications; outlier winsorization; minimum within-country panel length.

### Headline findings

1. **Inflation responses** are largely consistent with the dampening prediction in the emerging sub-sample; the developed sub-sample shows muted heterogeneity, as expected when MIMIC variation is small.
2. **Real-output responses** differ from the inflation margin and exhibit asymmetric behaviour at short vs. medium horizons, consistent with the labour-buffer mechanism in EMDEs.
3. The moderating role of informality is **concentrated in emerging economies**, consistent with a threshold interpretation: informality only matters where it is structurally large enough to bind.

See §5 of the paper for full impulse-response figures, sub-sample tables, and inference.

### Three contributions

1. Large-panel test using **identified shocks** rather than residual-based proxies, addressing long-standing measurement concerns in EMDE samples.
2. **Separate treatment** of the inflation and real-output margins — they respond differently and conflating them obscures the mechanism.
3. Evidence that informality's moderating role is **concentrated in emerging economies**, supporting a threshold view of the structural moderators of monetary transmission.

---

## Repository structure

```
.
├── README.md
├── Meshkovski_Nikola_Formal_Shocks_Informal_Buffers_2026.pdf   ← paper
├── .gitignore
│
└── replication/                                                 ← reproduction bundle
    ├── Project3_Nikola.qmd        ← Quarto source (R + prose, ~3,400 lines)
    ├── Project3_Nikola.pdf        ← rendered output of the .qmd
    ├── renv.lock                  ← exact R package versions
    ├── renv/                      ← renv environment
    │   ├── activate.R
    │   └── settings.json
    │
    ├── data/                      ← raw input data, read-only
    │   ├── data_raw_theglobaleconomy.xlsx
    │   ├── data_raw_monetary-transmission.xlsx
    │   ├── Real_GDP_pc_WorldBank.xls
    │   └── metadata_theglobaleconomy_19-03-26.txt
    │
    ├── figs/                      ← all figures (PDF), regenerated on each render
    └── tables/                    ← all tables (xlsx), regenerated on each render
        └── descriptive_tables/
```

The two PDFs at different levels are intentional: the paper at the root is the polished document for casual readers, and the one inside `replication/` is rebuilt every time the `.qmd` is rendered.

---

## Data

| File                                           | Source                                          | Contents                                                                |
| ---------------------------------------------- | ----------------------------------------------- | ----------------------------------------------------------------------- |
| `data_raw_theglobaleconomy.xlsx`               | TheGlobalEconomy.com (compiled WB / IMF / FH)   | Inflation, growth, governance, financial-depth, MIMIC informality, etc. |
| `data_raw_monetary-transmission.xlsx`          | Choi, Willems & Yoo (2024)                      | Identified country-year monetary-policy shocks (`MPS`)                  |
| `Real_GDP_pc_WorldBank.xls`                    | World Bank, World Development Indicators        | Real GDP per capita (constant 2015 US$); used for the development split |
| `metadata_theglobaleconomy_19-03-26.txt`       | TheGlobalEconomy.com                            | Variable definitions and source citations for the main panel            |

All raw data files are committed as-is so the analysis is bit-for-bit reproducible. Refer to the original providers' terms before redistributing them outside this repository.

---

## Reproducing the results

### Software requirements

| Tool       | Version    | Notes                                                                   |
| ---------- | ---------- | ----------------------------------------------------------------------- |
| **R**      | ≥ 4.3      | Tested on the version recorded in `renv.lock`                           |
| **Quarto** | ≥ 1.4      | Check with `quarto --version`                                           |
| **LaTeX**  | xelatex    | TinyTeX is the easiest path: `quarto install tinytex`                   |
| **renv**   | latest     | Will be installed automatically by the bootstrap step below             |

The `.qmd` uses `xelatex` because the paper is set in Times New Roman; if you do not have a LaTeX engine that supports it, the PDF render will fail at the final step (HTML output will still work).

### Step-by-step

```bash
# 1. Clone
git clone https://github.com/nmeskovski/informality-monetary-transmission.git
cd informality-monetary-transmission/replication

# 2. (One-time) Install LaTeX if you don't have it
quarto install tinytex

# 3. Bootstrap the R package environment from renv.lock
#    This downloads and installs every package at the exact version used.
Rscript -e 'if (!requireNamespace("renv", quietly = TRUE)) install.packages("renv"); renv::restore()'

# 4. Render the Quarto document
quarto render Project3_Nikola.qmd
```

### What to expect

- **First render takes 5–15 minutes** depending on machine speed. The panel local projections estimate 12 horizons × 3 dependent variables × 3 sub-samples × multiple specifications. Chunk caching is on by default (`cache: true` in the YAML), so subsequent renders only re-run chunks that changed.
- **If you change the data or specifications**, the safest reset is to delete the cache:
  ```bash
  rm -rf Project3_Nikola_cache _freeze
  ```
  then render again.
- **If `renv::restore()` fails** on a single package, the most common cause is a missing system library (e.g., `libxml2-dev`, `libssl-dev`, `libgit2-dev` on Debian/Ubuntu; `pkg-config` on macOS). Install the system dependency and re-run `renv::restore()`.
- **If the LaTeX step fails** with a missing package, run `quarto install tinytex --update` and let TinyTeX auto-install the missing TeX packages on the next render.

### Outputs

After a successful render, you will find:

- `replication/Project3_Nikola.pdf` — the rebuilt analysis document, identical (modulo timestamp) to the one already in the repo
- `replication/figs/*.pdf` — every figure used in the paper, named after its chunk label
- `replication/tables/descriptive_tables/*.xlsx` — descriptive statistics, missingness comparisons across development groups

These directories are emptied and regenerated on each render — do not store anything you want to keep inside them.

---

## Workflow at a glance

The `.qmd` is organised in roughly the following phases (see chunk labels for the exact sequence):

1. **Setup & data loading** — read three raw Excel files, merge by country–year, restrict to 2000–2019.
2. **Sample construction** — classify countries as Developed / Emerging by average real GDP per capita; build missingness and descriptive tables by group.
3. **Diagnostics** — panel unit-root tests, AR(1) persistence on MIMIC, within–between variation decomposition, multicollinearity (VIF).
4. **Variable construction** — lagged regressors, forward-looking dependent variables for each horizon, interaction terms, governance PCA composite, outlier visualisation and winsorisation.
5. **Geographical coverage** — maps of the full panel vs. the estimation sample, and by development status.
6. **LP estimation** — three dependent variables (inflation, log GDP p.c., GDP growth) × three samples (full, developed, emerging) × baseline + governance-robustness specifications, all 12 horizons.
7. **IRF plots & summary tables** — impulse-response figures for the interaction coefficient $\theta_h$ and the direct effects $\beta_h$ and $\gamma_h$, plus full coefficient summary tables for the appendix.

---

## How to cite

> Meshkovski, N. (2026). *Formal Shocks, Informal Buffers: Identifying the Conditional Transmission of Monetary Policy Through Panel Local Projections, 2000–2019.* BSc Project, Corvinus University of Budapest, Institute of Economics.

A BibTeX entry will be added once the paper has a DOI or permanent archive.

---

## Use of generative AI

In line with Provision 6/2025 of the Corvinus Vice-Rector for Academic Programmes, the use of generative AI is disclosed in the front matter of the paper. AI assistance was limited to wording refinement, LaTeX equation formatting, literature discovery, and phrasing of technical definitions. The empirical workflow — data cleaning, merging, estimation, diagnostics, robustness checks — was conducted independently in R and Quarto, and all numerical results were verified by the author.

---

## License

To be added. Code is intended for academic use; the raw data files retain the licenses of their original providers and are included here strictly for replication. Please consult the source organisations (World Bank, IMF, Freedom House, TheGlobalEconomy.com, Choi–Willems–Yoo) before redistributing them.

---

## Contact

Nikola Meshkovski · Corvinus University of Budapest · BSc Applied Economics
For questions about reproducing the results, please open an issue on this repository.
