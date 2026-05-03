# Formal Shocks, Informal Buffers

> **BSc Applied Economics** · Corvinus University of Budapest · 2026
> **Course:** Project 3 · **Instructor:** Benlialper Ahmet

 **Paper:** [`Meshkovski_Nikola_Formal_Shocks_Informal_Buffers_2026.pdf`](Meshkovski_Nikola_Formal_Shocks_Informal_Buffers_2026.pdf)
 **Replication package:** [`replication/`](replication/)

---

## Abstract

This paper tests whether the size of a country's informal economy moderates the transmission of identified monetary policy shocks to inflation and real output. Using panel local projections across **122 countries over 2000–2019**, and interacting the Choi–Willems–Yoo (2024) shock series with the Medina–Schneider (2018) MIMIC informality measure, the study finds that a larger informal sector significantly dampens both the inflation response and the anomalous positive output response to a contractionary shock — effects concentrated in emerging economies and statistically undetectable in advanced ones.

---

## Research Hypothesis

> *The size of a country's informal economy systematically moderates the transmission of identified monetary policy shocks to inflation and real output, with this moderation concentrated in emerging and developing economies where informality is structurally large enough to bind.*

Two testable predictions follow from the dual-sector buffer mechanisms of Castillo & Montoro (2012) and Alberola & Urrutia (2020):

- **H1 (Inflation channel):** A larger informal economy dampens the disinflationary response to a contractionary shock (δₕ > 0 against a textbook-negative βₕ).
- **H2 (Output channel):** A larger informal economy attenuates the real-output response — dampening an anomalous puzzle-positive direct response toward zero (δₕ < 0 against a puzzle-positive βₕ in EMDE samples).

---

## Key Findings

| Margin | Horizon | δₕ (Full) | δₕ (Emerging) | δₕ (Developed) | Significant? |
|---|---|---|---|---|---|
| Inflation (H1) | h = 0 | 0.035 | 0.044 | ≈ 0 | ✅ Yes (emerging) |
| Inflation (H1) | h = 1 | 0.061 | 0.062 | ≈ 0 | ✅ Yes (emerging) |
| Log GDP p.c. (H2) | h = 0 | −0.0001 | −0.0003 | ≈ 0 | ✅ Yes (emerging) |
| Log GDP p.c. (H2) | h = 1 | — | −0.0003 | ≈ 0 | ✅ Yes (emerging) |
| Both margins | h ≥ 2 | — | — | — | ❌ No |

**Marginal effects at the developed–emerging MIMIC contrast (≈20% vs. ≈38% of GDP):**
- At h = 1, the inflation interaction implies a **≈1.1 pp difference** in the disinflationary response — large enough to substantially erode, and in the most informal economies to reverse, the conventional tightening effect.
- At h = 1 in the emerging subsample, informality reduces the anomalous positive output response roughly **six-fold** relative to a hypothetical zero-informality baseline.
- The moderating role is **not detectable in advanced economies**, consistent with compressed MIMIC variation (IQR ≈ 14–24% of GDP) and a potential non-linearity threshold.

---

## Data

| File | Source | Contents |
|---|---|---|
| `data_raw_theglobaleconomy.xlsx` | TheGlobalEconomy.com (compiled WB / IMF / FH) | Inflation, growth, governance, financial depth, MIMIC informality, etc. |
| `data_raw_monetary-transmission.xlsx` | Choi, Willems & Yoo (2024) | Identified country-year monetary-policy shocks (`MPS`) |
| `Real_GDP_pc_WorldBank.xls` | World Bank, World Development Indicators | Real GDP per capita (constant 2015 US$); used for the development split |
| `metadata_theglobaleconomy_19-03-26.txt` | TheGlobalEconomy.com | Variable definitions and source citations for the main panel |

- **Panel:** 122 countries (35 developed, 87 emerging), 2000–2019, avg. ≈15 usable years per country
- **Development split:** World Bank / IMF upper-middle-income threshold of $14,000 avg. real GDP per capita (constant 2015 US$), held fixed across years
- **Note:** All raw data files are committed as-is for bit-for-bit reproducibility. Consult the original providers' terms before redistribution.

### Key Variables

| Variable | Description | Unit | Source | Role |
|---|---|---|---|---|
| `mps` | Monetary policy shock | pp | Choi, Willems & Yoo (2024) | Key regressor |
| `mimic` | Informal economy (MIMIC method) | % of GDP | Medina & Schneider (2018) | Key moderator |
| `infl` | CPI inflation | annual % | World Bank (via TGE) | Dependent variable |
| `gdp_pc_real` | Real GDP per capita | constant 2015 US$ | World Bank | Dependent variable (log) |
| `credit_gdp` | Domestic credit to private sector | % of GDP | World Bank (via TGE) | Control |
| `gov_pca` | Governance PCA composite (4 WGI indicators) | index | WGI (via TGE) | Governance control |

> **Note:** `mps`, `infl`, and `gdp_growth` are winsorised at the 1st and 99th percentiles. The four WGI governance indicators (regulatory quality, control of corruption, political stability, voice & accountability) are collapsed into a single PCA composite explaining 84.3% of variance.

---

## Methodology

1. **Panel Local Projections** (Jordà, 2005) — one regression per horizon h = 0, …, 5, robust to short-run autoregressive misspecification and transparent in the inclusion of two-way fixed effects.
2. **Interaction design** — MPS × MIMIC interaction recovers the moderating coefficient δₕ; identification exploits the asymmetry between time-varying MPS (99.6% within-country) and structurally slow MIMIC (99.6% between-country), so the interaction term is well-identified even after country fixed effects absorb MIMIC levels.
3. **Inference** — country-clustered standard errors (Cameron, Gelbach & Miller, 2011), accommodating heteroskedasticity and the mechanical serial correlation of LP cumulative outcomes.
4. **Robustness** — alternative governance specifications (each of the four raw WGI indicators in turn); re-inclusion of Turkey (the most-affected winsorised country); results are visually and quantitatively unchanged.

**Main Specification:**

$$Y_{i,t+h} - Y_{i,t-1} = \alpha_{i,h} + \lambda_{t,h} + \beta_h MPS_{i,t}^w + \gamma_h MIMIC_{i,t-1} + \delta_h (MPS_{i,t}^w \times MIMIC_{i,t-1}) + \theta_h X_{i,t-1} + \varepsilon_{i,t+h}$$

where δₕ is the coefficient of economic interest, country and year fixed effects are estimated separately at each horizon, and controls vary by dependent variable (lagged GDP growth or lagged inflation, credit-to-GDP, gov_pca).

---

## Three Contributions

1. **Identified shocks** — large-panel test using the Choi–Willems–Yoo (2024) shock series rather than Taylor-rule residuals or raw policy-rate changes, addressing longstanding EMDE measurement concerns. 
2. **Separate margins** — inflation and real-output responses are treated and interpreted distinctly; they respond through different mechanisms, and conflating them obscures the buffer channel. 
3. **Threshold evidence** — informality's moderating role is concentrated in emerging economies, supporting a non-linear threshold interpretation and challenging advanced-economy-grounded inference in cross-country transmission work.

---

## Repository Structure

```
.
├── README.md
├── CITATION.cff
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

## How to Reproduce

### Software Requirements

| Tool | Version | Notes |
|---|---|---|
| **R** | ≥ 4.3 | Tested on the version recorded in `renv.lock` |
| **Quarto** | ≥ 1.4 | Check with `quarto --version` |
| **LaTeX** | xelatex | TinyTeX is the easiest path: `quarto install tinytex` |
| **renv** | latest | Installed automatically by the bootstrap step below |

The `.qmd` uses `xelatex` because the paper is set in Times New Roman; if you do not have a LaTeX engine that supports it, the PDF render will fail at the final step (HTML output will still work).

### Step-by-Step

Each step below is tagged with the shell it should be run in: 🖥️ **Terminal** (bash / zsh / PowerShell) or 📊 **R / RStudio console**.

#### 1. 🖥️ Terminal — Clone the repository

```bash
git clone https://github.com/nmeskovski/informality-monetary-transmission.git
cd informality-monetary-transmission/replication
```

All remaining steps assume the working directory is `replication/`.

#### 2. 🖥️ Terminal — Install LaTeX (one-time setup, skip if you already have it)

```bash
quarto install tinytex
```

#### 3. Restore the R package environment

This downloads and installs every R package at the exact version pinned in `renv.lock`. Pick **one** of the two options below — they do the same thing.

**Option A — 📊 R / RStudio console** *(recommended if you use RStudio)*

Open `replication/` in RStudio (or launch R from inside the `replication/` directory), then run:

```r
if (!requireNamespace("renv", quietly = TRUE)) install.packages("renv")
renv::restore()
```

When `renv::restore()` lists the packages it will install, type `y` to confirm.

**Option B — 🖥️ Terminal** *(no need to open R)*

```bash
Rscript -e 'if (!requireNamespace("renv", quietly = TRUE)) install.packages("renv"); renv::restore(prompt = FALSE)'
```

#### 4. Render the Quarto document

**Option A — 🖥️ Terminal**

```bash
quarto render Project3_Nikola.qmd
```

**Option B — 📊 RStudio**

Open `Project3_Nikola.qmd` in RStudio and click the **Render** button in the editor toolbar (or press `Ctrl/Cmd + Shift + K`).

### What to Expect

- **First render takes 5–15 minutes** depending on machine speed. The panel local projections estimate 6 horizons × 2 dependent variables × 3 sub-samples × multiple specifications. Chunk caching is on by default (`cache: true` in the YAML), so subsequent renders only re-run chunks that changed.
- **To reset the cache** after changing data or specifications, run in 🖥️ Terminal from inside `replication/`:
  ```bash
  rm -rf Project3_Nikola_cache _freeze
  ```
  (Two paths separated by a space — the chunk cache directory and the Quarto freeze directory.)
- **If `renv::restore()` fails** on a single package, the most common cause is a missing system library (e.g., `libxml2-dev`, `libssl-dev`, `libgit2-dev` on Debian/Ubuntu; `pkg-config` on macOS). Install the system dependency and re-run `renv::restore()`.
- **If the LaTeX step fails** with a missing package, run `quarto install tinytex --update` in 🖥️ Terminal and let TinyTeX auto-install missing packages on the next render.

### Outputs

After a successful render:

- `replication/Project3_Nikola.pdf` — the rebuilt analysis document, identical (modulo timestamp) to the one in the repo
- `replication/figs/*.pdf` — every figure used in the paper, named after its chunk label
- `replication/tables/descriptive_tables/*.xlsx` — descriptive statistics and missingness comparisons across development groups

These directories are emptied and regenerated on each render — do not store anything you want to keep inside them.

---

## Workflow at a Glance

The `.qmd` is organised in roughly the following phases (see chunk labels for the exact sequence):

1. **Setup & data loading** — read three raw Excel files, merge by country–year, restrict to 2000–2019
2. **Sample construction** — classify countries as Developed / Emerging by average real GDP per capita; build missingness and descriptive tables by group
3. **Diagnostics** — panel unit-root tests, AR(1) persistence on MIMIC, within–between variation decomposition, multicollinearity (VIF)
4. **Variable construction** — lagged regressors, forward-looking dependent variables for each horizon, interaction terms, governance PCA composite, outlier visualisation and winsorisation
5. **Geographical coverage** — maps of the full panel vs. the estimation sample, and by development status
6. **LP estimation** — two dependent variables (inflation, log GDP p.c.) × three samples (full, developed, emerging) × baseline + governance-robustness specifications, all 6 horizons
7. **IRF plots & summary tables** — impulse-response figures for the interaction coefficient δₕ and the direct effects βₕ and γₕ, plus full coefficient summary tables for the appendix

---

## How to Cite

> Meshkovski, N. (2026). *Formal Shocks, Informal Buffers: Identifying the Conditional Transmission of Monetary Policy Through Panel Local Projections, 2000–2019.* BSc Project, Corvinus University of Budapest, Institute of Economics.

A BibTeX entry will be added once the paper has a DOI or permanent archive.

---

## Use of Generative AI

In line with Provision 6/2025 of the Corvinus Vice-Rector for Academic Programmes, the use of generative AI is disclosed in the front matter of the paper. AI assistance was limited to wording refinement, LaTeX equation formatting, literature discovery, and phrasing of technical definitions. The empirical workflow — data cleaning, merging, estimation, diagnostics, robustness checks — was conducted independently in R and Quarto, and all numerical results were verified by the author.

---

## References

- Alberola, E., & Urrutia, C. (2020). Does informality facilitate inflation stability? *Journal of Development Economics*, 146, Article 102505.
- Castillo, P., & Montoro, C. (2012). Inflation dynamics in the presence of informal labour markets (BIS Working Paper No. 372).
- Choi, S., Willems, T., & Yoo, S. Y. (2024). Revisiting the monetary transmission mechanism through an industry-level differential approach. *Journal of Monetary Economics*, 145, Article 103556.
- Colombo, E., Menna, L., & Tirelli, P. (2019). Informality and the labor market effects of financial crises. *World Development*, 119, 1–22.
- Ha, J., Kim, D., Kose, M. A., & Prasad, E. S. (2025). Resolving puzzles of monetary policy transmission in emerging markets. *European Economic Review*, 173, Article 104957.
- Jordà, Ò. (2005). Estimation and inference of impulse responses by local projections. *American Economic Review*, 95(1), 161–182.
- Medina, L., & Schneider, F. (2018). Shadow economies around the world: What did we learn over the last 20 years? (IMF Working Paper No. 18/17).
- Restrepo-Echavarría, P. (2014). Macroeconomic volatility: The role of the informal economy. *European Economic Review*, 70, 454–469.
- Rusnák, M., Havranek, T., & Horváth, R. (2013). How to solve the price puzzle? A meta-analysis. *Journal of Money, Credit and Banking*, 45(1), 37–70.
  
...

The full reference list is available in §7 of the paper.

---

## License

BSc Project 3 Portfolio Thesis in Corvinus University of Budapest. Code is intended for academic use; the raw data files retain the licenses of their original providers and are included here strictly for replication. Please consult the source organisations (World Bank, IMF, Freedom House, TheGlobalEconomy.com, Choi–Willems–Yoo) before redistributing them officially.

---

## Contact

For questions about reproducing the results.
Nikola Meshkovski · nmeskovski@gmail.com
