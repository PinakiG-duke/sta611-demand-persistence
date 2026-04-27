# When Does Demand Have Memory?
### Empirical Characterization of AR(1) Persistence as Foundation for Adaptive Retail Allocation

**STA 611: Introduction to Mathematical Statistics — Final Project**  
Pinaki Ghosh | M.S. Interdisciplinary Data Science, Duke University | April 27, 2026

---

## Overview

This repository contains the code, figures, and analysis notebooks for a study examining whether weekly retail demand exhibits statistically meaningful persistence — and whether that persistence is stable across time and store locations.

The study is motivated by a proposed research framework called the **Economic Value of Adaptivity (EVA)**, which measures the maximum economic gain an adaptive AI-driven allocation policy delivers over a static one-shot benchmark. The EVA framework assumes demand follows a log-Normal AR(1) process, where the persistence parameter ρ governs how much this week's demand predicts next week's — and therefore how much value a forward-looking allocation policy can extract over a reactive one. This paper empirically tests whether that assumption holds in real retail data, and characterizes how ρ behaves across SKUs, product categories, and store locations.

### Research Questions

- **RQ1:** Is AR(1) demand persistence statistically significant and economically meaningful across the continuous retail SKU catalog, or is weekly demand effectively memoryless?
- **RQ2:** Is the persistence parameter ρ a SKU-level property, or is it predictable from product category taxonomy?
- **RQ3:** Does ρ remain stable over a multi-year horizon, or does it exhibit regime shifts consistent with the burst-and-decay (DE3) and progressive amplification (DE4) demand environments posited in the EVA typology?

### Key Findings

| Finding | Result |
|---|---|
| ρ significance (RQ1) | 91.1% of SKUs reject H₀: ρ = 0; mean ρ̂ = 0.550 |
| Category explains ρ (RQ2) | F = 0.979, p = 0.384, η² = 0.044 — category does not explain ρ |
| Temporal stability (RQ3a) | 78.3% of high-ρ SKUs show structural regime change (Chow test) |
| Cross-store stability (RQ3b) | SKU explains 70.5% of ρ variance; Store explains 3.1% (Two-Way ANOVA, RCBD) |
| Specification improvement | Two-period model reduces RSS by 10.0% for break-detected SKUs vs 3.2% baseline |

---

## Repository Structure

```
sta611-demand-persistence/
│
├── README.md                          # This file
├── requirements.txt                   # Python dependencies
│
├── notebooks/
│   ├── Block1_Data_Loading.ipynb
│   ├── Block2_SKU_Selection_Weekly_Aggregation.ipynb
│   ├── Block2b_Intermittent_SKU_Sales_Share.ipynb
│   ├── Block3_AR1_Estimation.ipynb
│   ├── Block4_ANOVA_Category.ipynb
│   ├── Block4b_SKU_Level_Rho_Classification.ipynb
│   ├── Block5_GOF_Tests.ipynb
│   ├── Block5b_SW_Power_Simulation.ipynb
│   ├── Block6_Structural_Break_Chow.ipynb
│   ├── Block7_Summary_Figure.ipynb
│   ├── Block8_Cross_Store_RCBD.ipynb
│   └── Block9_Model_Fit_Comparison.ipynb
│
├── figures/
│   ├── sku_rho_ranked.png             # Figure 1: SKU-level ρ ranked by tier
│   ├── anova_rho.png                  # Figure 2: One-way ANOVA ρ by category
│   ├── gof_qq_plots.png               # Figure 3: Q-Q plots by persistence tier
│   ├── structural_breaks.png          # Figure 4: ρ pre- vs post-break
│   └── cross_store_2way_anova.png     # Figure 5: Cross-store RCBD
│
├── paper/
│   └── STA611_FinalProject_Ghosh_Pinaki_27042026.pdf
│
└── data/
    └── README_data.md                 # Instructions for downloading M5 data
```

---

## Data

This project uses the **M5 Walmart Hierarchical Sales dataset** from the Kaggle M5 Forecasting Accuracy Competition.

**The data is not included in this repository** because Kaggle requires each user to accept the competition rules before downloading.

### Download Instructions

1. Create a Kaggle account at [kaggle.com](https://www.kaggle.com)
2. Accept the competition rules at: [kaggle.com/competitions/m5-forecasting-accuracy/data](https://www.kaggle.com/competitions/m5-forecasting-accuracy/data)
3. Install the Kaggle API: `pip install kaggle`
4. Download your API token from [kaggle.com/settings](https://www.kaggle.com/settings) → API → Create New Token
5. Place `kaggle.json` at `~/.kaggle/kaggle.json` and run `chmod 600 ~/.kaggle/kaggle.json`
6. Download the data:

```bash
kaggle competitions download -c m5-forecasting-accuracy -p data/
cd data && unzip m5-forecasting-accuracy.zip
```

### Files Required

| File | Size | Description |
|---|---|---|
| `sales_train_validation.csv` | ~115 MB | Daily unit sales for 30,490 SKU-store combinations |
| `calendar.csv` | ~100 KB | Maps day indices to dates; includes event and SNAP flags |
| `sell_prices.csv` | ~194 MB | Weekly prices per item per store |

### Data Path Configuration

All notebooks expect the data at:
```
D:\Ops\STA611\m5-forecasting-accuracy\
```

Update the `DATA_DIR` variable in Block 1 to match your local path before running.

---

## Running the Analysis

### Prerequisites

```bash
pip install -r requirements.txt
```

### Execution Order

Run notebooks in block order. Each block saves intermediate outputs that subsequent blocks depend on.

| Block | Notebook | Description |
|---|---|---|
| 1 | Block1_Data_Loading | Load and validate M5 files |
| 2 | Block2_SKU_Selection_Weekly_Aggregation | Aggregate to weekly; stratified sample |
| 2b | Block2b_Intermittent_SKU_Sales_Share | Economic weight of intermittent SKUs |
| 3 | Block3_AR1_Estimation | OLS AR(1) estimation; 95% CIs per SKU |
| 4 | Block4_ANOVA_Category | One-way ANOVA: does category predict ρ? |
| 4b | Block4b_SKU_Level_Rho_Classification | Persistence tier classification and validation |
| 5 | Block5_GOF_Tests | Shapiro-Wilk and KS GOF tests on AR(1) residuals |
| 5b | Block5b_SW_Power_Simulation | Power simulation demonstrating SW over-rejection at n≈273 |
| 6 | Block6_Structural_Break_Chow | Chow test for temporal stability of ρ |
| 7 | Block7_Summary_Figure | Four-panel summary figure |
| 8 | Block8_Cross_Store_RCBD | Two-way ANOVA (RCBD): cross-store generalization |
| 9 | Block9_Model_Fit_Comparison | Static vs two-period model fit comparison |

---

## Requirements

```
pandas>=1.5
numpy>=1.23
scipy>=1.9
matplotlib>=3.6
statsmodels>=0.13
kaggle
jupyter
```

See `requirements.txt` for the full list with pinned versions.

---

## Research Context

This paper is the first empirical layer of a larger research program on the Economic Value of Adaptivity (EVA). The EVA simulation study — which generates EVA surfaces across the four demand environments (DE1–DE4) and a grid of ρ and σ² values — builds directly on the empirical findings here. Specifically:

- SKU-level ρ estimation is established as a necessity (not a modeling choice) by the RQ2 null result
- The DE3 and DE4 regime structures are validated as empirically grounded, not merely theoretical
- The updated demand specification with time-varying ρ(s,t) and t(ν) errors provides the simulation's calibrated parameter space

The next phase of this research will extend the AR(1) estimation to the full 1,410 eligible CA_1 SKUs, develop real-time structural break detection, and build a discrete choice model of customer repurchase behavior as the structural foundation for why ρ varies across SKUs and stores.

---

## Citation

If you use this code or findings, please cite:

```
Ghosh, P. (2026). When Does Demand Have Memory? Empirical Characterization of 
AR(1) Persistence as Foundation for Adaptive Retail Allocation. 
STA 611 Final Project, Duke University.
```

---

## Contact

Pinaki Ghosh — Duke University, M.S. Interdisciplinary Data Science  
GitHub: github.com/PinakiG-duke/sta611-demand-persistence
