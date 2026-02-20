# ICQCM 2026 — Synthetic Control Methods Seminar

**Seminar:** Science, Frankenstein, and Causal Inference: Analyzing Neighborhood Crime with Synthetic Control Methods

**Instructor:** Yung Chun, PhD — Washington University in St. Louis

**Date:** February 27, 2026 | 10:15 am – 12:30 pm | Sheraton New Orleans


---

### Analyzing Neighborhood Crime with Synthetic Control Methods

This repository contains materials for the **ICQCM 2026** session on Synthetic Control Methods (SCM). The workshop demystifies SCM through the "Frankenstein" metaphor—stitching together a donor pool to create a synthetic counterfactual.



---

## 1. Repository Structure

* `/syllabus: Lecture syllabus
* `/slides`: PDF and PPTX versions of the ICQCM presentation.
* `/data`: Sample neighborhood-level crime and stop data.
* `/scripts`: R scripts for implementing `tidysynth`.



## 2. Getting Started: Setting Up Your Lab (R & RStudio)

Before we start stitching our "Frankenstein" counterfactual, you need to set up your computational environment. We will use **R** and **RStudio**.

### What are R and RStudio?

Think of it like a car:

* **R is the Engine:** It is the actual programming language that does the heavy lifting, statistical calculations, and data processing.
* **RStudio is the Dashboard:** It is an Integrated Development Environment (IDE). It provides a user-friendly interface to write code, view plots, manage files, and see your data objects all in one window.

> **Note:** You must install **R** first, then **RStudio**. RStudio cannot run without the R engine.

### How to Download and Install

#### Step 1: Install R (The Engine)

1. Go to the **[CRAN Website](https://cran.r-project.org/)**.
2. Select your operating system:
* **Windows:** Click "install R for the first time" → "Download R for Windows."
* **macOS:** Click "Download R for macOS" and select the `.pkg` file that matches your chip (Apple silicon M1/M2/M3 or Intel).


3. Run the installer and follow the default prompts.

#### Step 2: Install RStudio (The Dashboard)

1. Go to the **[Posit Downloads Page](https://posit.co/download/rstudio-desktop/)**.
2. Scroll down to "2: Install RStudio" and click the button to download the installer for your OS.
3. Install the application like any other software.

### Why use R for Social Science?

For policy researchers and geographers, R offers:

* **Reproducibility:** Unlike "point-and-click" software (like SPSS or Excel), your R scripts are a permanent record of your analysis.
* **Spatial Power:** R has world-class libraries for Geographic Information Systems (GIS), essential for neighborhood-level analysis.
* **Open Source:** It is free and supported by a massive community of academic researchers.

[How to install R and RStudio on Windows](https://www.youtube.com/watch?v=fJS6iYeSDTM)

[How to install R and Rstudion on Mac](https://www.youtube.com/watch?v=g36cC76BbRo)

## 3. Recommended Packages

Once RStudio is open, run the following code in your console to prepare for the workshop:

```r
install.packages(c("tidyverse", "tidysynth", "sf", "ggplot2"))

```

---
## Contact
yung.chun@wustl.edu
