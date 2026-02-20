## Seminar Syllabus & Session Plan
# Science, Frankenstein, and Causal Inference: Analyzing Neighborhood Crime with Synthetic Control Methods

**ICQCM Summit 2026**
**Instructor:** Yung Chun, PhD | Research Assistant Professor, Brown School, Washington University in St. Louis
**Date:** February 27, 2026 | 10:15 am – 12:30 pm
**Location:** Sheraton New Orleans Hotel
**Materials:** https://github.com/YungChun0303/ICQCM

---

## Seminar Overview

Science is the pursuit of causality. This seminar introduces the "Frankenstein" of causal inference: the Synthetic Control Method (SCM). We explore how SCM constructs artificial counterfactuals to overcome the limitations of traditional quasi-experiments in comparative case studies. After a theoretical grounding in SCM fundamentals and its econometric advantages, participants engage in a hands-on data walk using R — applying SCM to real-world neighborhood crime and criminal justice data to model the impacts of a place-based federal policy intervention.

---

## Learning Objectives

By the end of this seminar, participants will be able to:

1. **Explain** the logic of causal inference and situate SCM within the broader landscape of causal identification strategies (RCT, natural experiment, quasi-experiment)
2. **Articulate** when SCM is preferred over Difference-in-Differences, and what assumptions it requires
3. **Describe** how a synthetic control is constructed — the role of donor weights, predictor balance, and pre-treatment fit
4. **Interpret** SCM output: trend plots, balance tables, donor weight tables, treatment effect estimates, and placebo test results
5. **Run** a complete SCM pipeline in R using the `tidysynth` package
6. **Critically evaluate** the assumptions, limitations, and potential misuses of the method

---

## Required Preparation

Participants should complete the following **before arriving**:

| Task | Instructions |
|---|---|
| Install R | https://cran.r-project.org (version 4.1 or higher) |
| Install RStudio | https://posit.co/download/rstudio-desktop |
| Install R packages | Run in console: `install.packages(c("tidyverse", "tidysynth", "zoo", "knitr", "kableExtra", "here"))` |
| Download materials | https://github.com/YungChun0303/ICQCM → green "Code" button → "Download ZIP" |
| Open the script | Open `1000_synth.Rmd` in RStudio and confirm it loads without errors |

No prior knowledge of the Synthetic Control Method is expected. Basic familiarity with R is helpful but not required.

---

## Materials

| File | Description |
|---|---|
| `icqcm_scm_chun_v1.pdf` | Lecture slides |
| `denver_crime_qtr.csv` | Quarterly crime panel, Denver census tracts, 2011–2021 |
| `denver_stop_qtr.csv` | Quarterly police stop panel, Denver census tracts, 2011–2021 |
| `1000_synth.Rmd` | Main analysis script — worked through live during the seminar |
| `0100_dataprep.Rmd` | Data preparation script — for reference only |

---

## Session Plan

**Total time:** 2 hours 15 minutes (including one 10-minute break)

---

### Block 1 — Why Causality? Why SCM? *(~35 minutes)*

| Topic | Slides |
|---|---|
| Welcome, housekeeping, confirm materials downloaded | 1 |
| Science and causal inference — Pearl's ladder of causation; association vs. intervention vs. counterfactual | 2–4 |
| Causal identification strategies — RCT, natural experiment, quasi-experiment | 5 |
| Why SCM? — The Doppelgänger vs. Frankenstein intuition; when DiD fails; the unique unit problem | 6–9 |

**Key question to pose to participants:**
*"How many of you would call what you do 'science'? What would it take to actually claim causality in your research?"*

---

### Block 2 — SCM Theory & Identification *(~25 minutes)*

| Topic | Slides |
|---|---|
| The identification strategy — V weights (covariates), W weights (donor units), constructing the counterfactual | 10–13 |
| Statistical inference — in-time placebo, in-space placebo, RMSPE ratio, Fisher p-value | 14 |
| The Denver CNI case — program overview, research questions, raw trend preview | 15–19 |

**Key concept to land:**
The synthetic control is not found — it is *optimized*. The algorithm simultaneously solves for which variables to match on (V) and which donor units to weight (W), producing a weighted composite that mirrors the treated unit's pre-treatment trajectory as closely as possible.

---

### ☕ Break *(~10 minutes)*

*Confirm everyone has RStudio open and `1000_synth.Rmd` loaded. Help troubleshoot setup issues.*

---

### Block 3 — Live Coding: Data Walk in R *(~55 minutes)*

| Activity | Rmd Section | Key Concept |
|---|---|---|
| Setup & data overview — load packages, inspect the balanced panel structure, understand the group variable | §1–2 | Balanced panel requirement; adjacent tract exclusion for spillover control |
| Descriptive trends — plot CNI vs. donor average for crime and stops; assess parallel trends visually | §3 | Visual pre-treatment parallel trends check; divergence after 2016 as preliminary evidence |
| Build the SCM step by step — walk through all four `tidysynth` functions; check balance table and donor weights | §4 (Steps 1–5) | The four-function pipeline; predictor balance as pre-flight check; sparse donor weights as expected outcome |
| Results — run all 8 outcomes; compare observed vs. synthetic; interpret treatment effect table | §5–8 | Reading the gap (negative = reduction); percent change for practical magnitude; researcher judgment on credibility |
| Placebo tests — in-space placebos; MSPE ratio table; Fisher p-values | §9 | Uniqueness-based inference; interpreting placebo plots visually; what a significant result looks like |

**The four `tidysynth` functions:**

```r
data %>%
  synthetic_control(...)    # Step 1: Declare panel structure
  generate_predictor(...)   # Step 2: Specify what to match on
  generate_weights(...)     # Step 3: Solve for donor weights
  generate_control()        # Step 4: Build the counterfactual
```

**Key outputs participants will see:**
- `grab_balance_table()` — predictor fit check
- `grab_unit_weights()` — which donors contribute to the synthetic
- `plot_trends()` — observed vs. synthetic trajectory
- Treatment effect summary table — average gap and % change for 8 outcomes
- `plot_placebos()` — visual placebo overlay
- `grab_significance()` — MSPE ratios and Fisher p-values

---

### Block 4 — Wrap-Up & Discussion *(~10 minutes)*

| Topic |
|---|
| Limitations and critical reflection — data demands, donor pool sensitivity, no mechanism identification, risk of overfitting |
| Open discussion — where does this fit in your research? What does the causal framework assume? What does it render invisible? |

**Discussion prompt:**
*"SCM is powerful, but it operates within a positivist causal framework. What does that framework assume? What kinds of questions can't it answer? How does this sit alongside the critical quantitative orientation ICQCM emphasizes?"*

---

## Data Overview

### Study Context
The Denver Community Navigation Initiative (CNI) was a place-based policing reform implemented in census tract 08031000800 (Sun Valley neighborhood). The program replaced traditional police responses with community health navigators for certain call types and was supported by a $1 million+ Byrne Criminal Justice Innovation (BCJI) federal grant.

| Parameter | Value |
|---|---|
| Treated unit | FIPS 8031000800 (Sun Valley, Denver CO) |
| Treatment start | 2016 Q3 (de facto) |
| Study window | 2011 Q1 – 2021 Q4 (44 quarters) |
| Panel type | Balanced (all tracts present for all 44 quarters) |
| Adjacent tracts | Excluded from donor pool (spillover control) |

### Outcome Variables

**Crime panel (`denver_crime_qtr.csv`)**

| Variable | Description |
|---|---|
| `Allcrime` | Total quarterly crime incidents |
| `Property` | Property crimes (quarterly count) |
| `Violence` | Violent crimes (quarterly count) |
| `Drug` | Drug crimes (quarterly count) |

**Stop panel (`denver_stop_qtr.csv`)**

| Variable | Description |
|---|---|
| `allstop` | Total quarterly police stops |
| `no_action` | Stops with no enforcement action |
| `n_citation` | Stops resulting in a citation |
| `arrest` | Stops resulting in an arrest |

### Covariates (ACS 2016, 5-year estimates)

| Variable | Description |
|---|---|
| `med_inc` | Median household income |
| `pct_color` | Share of non-white residents |
| `pop` | Total tract population |
| `pop_density` | Population per square mile |

---

## Key Methodological Concepts

**Synthetic Control Method (SCM)**
A causal inference method for comparative case studies with a single (or few) treated units. SCM constructs a weighted combination of untreated "donor" units whose pre-treatment trajectory best matches the treated unit. The post-treatment gap between the observed and synthetic trajectories is the estimated treatment effect.

*Original reference: Abadie, Diamond & Hainmueller (2010). Synthetic control methods for comparative case studies. Journal of the American Statistical Association, 105(490), 493–505.*

**Pre-treatment fit**
The degree to which the synthetic control tracks the treated unit before the intervention. Good pre-treatment fit (low MSPE) is necessary — though not sufficient — for a credible counterfactual.

**RMSPE ratio**
The ratio of post-treatment root mean squared prediction error to pre-treatment RMSPE. Used to construct Fisher-style p-values by ranking the treated unit's ratio against all placebo units.

**In-space placebo test**
Running the SCM for every donor unit as if it were the treated unit. Since donors were not treated, their post-treatment gaps should be small. A treated unit whose gap substantially exceeds the placebo distribution provides evidence of a genuine treatment effect.

**Donor pool**
The set of untreated units available as building blocks for the synthetic control. Adjacent tracts are excluded to prevent spillover contamination; only units with complete pre-treatment data are eligible.

---

## Suggested Further Reading

1. Abadie, A., Diamond, A., & Hainmueller, J. (2010). Synthetic control methods for comparative case studies: Estimating the effect of California's tobacco control program. *Journal of the American Statistical Association*, 105(490), 493–505.

2. Abadie, A. (2021). Using synthetic controls: Feasibility, data requirements, and methodological aspects. *Journal of Economic Literature*, 59(2), 391–425.

3. Pearl, J., & Mackenzie, D. (2018). *The Book of Why: The New Science of Cause and Effect*. Basic Books.

4. Currie, J., & MacLeod, W. B. (2020). Understanding doctor decision making: The case of depression treatment. *Econometrica* — (for illustration of causal reasoning in social contexts)

5. Xu, Y. (2017). Generalized synthetic control method: Causal inference with interactive fixed effects models. *Political Analysis*, 25(1), 57–76. *(For extensions to multiple treated units)*

---

## `tidysynth` Package Resources

**Primary documentation**

| Resource | Description | URL |
|---|---|---|
| CRAN page | Official package page with PDF reference manual | https://cran.r-project.org/package=tidysynth |
| GitHub repository | Source code and README with full worked example | https://github.com/edunford/tidysynth |
| PDF reference manual | Complete documentation for all 21 functions | https://cran.r-project.org/web/packages/tidysynth/tidysynth.pdf |

**Package author:** Eric Dunford (Georgetown University). The package is available through both CRAN and GitHub.

---

**The built-in example dataset — `smoking`**

The package ships with a classic dataset from Abadie et al. (2010) examining the effect of California's Proposition 99 (1988 tobacco control law) on cigarette consumption. This is the standard teaching example for SCM and is immediately runnable after installing the package:

```r
library(tidysynth)
data("smoking")   # 1,209 rows: 39 US states × 31 years (1970–2000)
```

This dataset is ideal for self-study after the seminar because the expected results are well-documented in the original Abadie et al. (2010) paper, making it easy to verify your output against a published benchmark.

---

**Key functions quick reference**

| Function | Purpose |
|---|---|
| `synthetic_control()` | Declare panel structure, treated unit, and treatment time |
| `generate_predictor()` | Specify variables to match on (callable multiple times for different time windows) |
| `generate_weights()` | Solve constrained optimization for donor weights |
| `generate_control()` | Build the counterfactual using estimated weights |
| `grab_balance_table()` | Check predictor fit between treated and synthetic |
| `grab_unit_weights()` | See which donors contribute and how much |
| `grab_synthetic_control()` | Extract full time series of observed vs. synthetic |
| `grab_significance()` | MSPE ratios and Fisher p-values (requires `generate_placebos = TRUE`) |
| `plot_trends()` | Line plot of observed vs. synthetic trajectory |
| `plot_placebos()` | Overlay of placebo runs for visual inference |
| `plot_mspe_ratio()` | Bar chart of MSPE ratios across all units |

---

## Limitations of SCM

Participants should leave the seminar with an honest understanding of when SCM is and is not appropriate:

**Data requirements are demanding.** SCM requires a long pre-treatment period (ideally 10+ time periods), a balanced panel, and a sufficiently large and diverse donor pool. With fewer than ~10 donors or a short pre-treatment window, results are not trustworthy.

**Donor pool selection is a researcher choice.** Results can be sensitive to who is included or excluded from the donor pool. This choice should be theoretically motivated and reported transparently.

**SCM does not identify mechanisms.** The method estimates *that* a gap exists post-treatment, but cannot explain *why*. Mechanism identification requires additional qualitative evidence or mediation analysis.

**Extrapolation is limited.** SCM results are specific to the treated unit and context. They do not generalize in the way experimental results do.

**The framework is positivist.** Like all quantitative causal methods, SCM assumes that a stable causal effect exists and can be measured. This may not hold in complex social systems, and the method renders invisible questions about power, context, and the politics of measurement.
