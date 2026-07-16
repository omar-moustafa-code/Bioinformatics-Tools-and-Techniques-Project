# Gut Microbial Diversity in Amyotrophic Lateral Sclerosis

**Reanalysis of 16S rRNA sequencing data from the Hertzberg et al. (2021) ALS cohort.**

This project investigates gut microbiome differences between ALS patients and their healthy spouse controls, leveraging a paired household design to control for environmental confounders. The analysis was implemented in a fully reproducible Python and R workflow.

---

## 📋 Table of Contents

- [Biological Question](#biological-question)
- [Hypotheses](#hypotheses)
- [Project Overview](#project-overview)
- [Key Findings](#key-findings)
- [Repository Structure](#repository-structure)
- [Methods](#methods)
- [Software and Libraries](#software-and-libraries)
- [How to Reproduce This Work](#how-to-reproduce-this-work)

---

## 🧬 Biological Question

**Does the gut bacterial community of ALS patients differ from that of healthy spouse controls in within-sample diversity (alpha) and between-sample composition (beta)?**

## 🔬 Hypotheses

1.  ALS patients will show **lower Shannon alpha diversity** than their matched controls.
2.  ALS patients will show **visible separation from controls** in a Bray-Curtis PCoA ordination.

## 📊 Project Overview

This project reanalyzes publicly available 16S ribosomal RNA sequencing data from the Hertzberg et al. (2021) cohort. The spouse-paired design is a key strength, as it controls for shared diet, household environment, and lifestyle factors—precisely the variables that confound typical case-control microbiome studies.

### Cohort Details

| Feature          | Description                                       |
| :--------------- | :------------------------------------------------ |
| **Samples**      | 18 stool samples (9 ALS patients + 9 spouses)     |
| **Design**       | 9 household pairs (spouse-paired)                 |
| **Target**       | V4 region of the 16S rRNA gene                    |
| **Primers**      | 515F / 806R                                       |
| **Platform**     | Illumina MiSeq                                    |
| **Accession**    | NCBI BioProject PRJNA566436                       |

---

## 🎯 Key Findings

| Finding                                                        | Result                                                                                            |
| :------------------------------------------------------------- | :------------------------------------------------------------------------------------------------ |
| **Alpha Diversity**                                            | ALS patients had **significantly higher** Shannon diversity than spouses (paired Wilcoxon, p = 0.004). *This was the opposite of the initial hypothesis.* |
| **Beta Diversity**                                             | Significant but modest separation in overall composition (PERMANOVA pseudo-F = 1.7, p ≈ 0.02).    |
| **Top ALS-Enriched Genera** (by q-value)                       | *Parabacteroides*, *Ruminococcus torques* group, *Blautia*, *Subdoligranulum*, *Lachnoclostridium*. |
| **Top Control-Enriched Genus**                                 | *Escherichia-Shigella*.                                                                           |
| **Key Negative Finding**                                       | *Akkermansia muciniphila*, a taxon of interest from mouse models, was not a top hit.              |

> **⚠️ Important Note:** These findings are preliminary and based on a small cohort (n=18). Replication in larger, independent cohorts is essential.

---

## 🛠️ Methods

### Upstream Processing (R/DADA2)
The raw reads were processed using the standard DADA2 workflow (Callahan et al., 2016) against the SILVA 138 reference database (Quast et al., 2013). This produced an Amplicon Sequence Variant (ASV) count table and taxonomy assignments, which served as the input for this analysis.

### Downstream Analysis (Python)
All downstream statistical analysis and visualization were performed in Python 3.12 within a dedicated Conda environment. The workflow is fully reproducible.

| Step | What It Does | Python Tools |
| :--- | :--- | :--- |
| 1 | Load ASV table, taxonomy, and sample metadata | `pandas` |
| 2 | Drop rare ASVs (<5% prevalence, min. 2 samples) | `pandas` |
| 3 | Convert counts to per-sample relative abundances | `pandas` |
| 4 | Compute Shannon alpha diversity; perform paired Wilcoxon test | `scikit-bio`, `scipy.stats` |
| 5 | Compute Bray-Curtis beta diversity matrix | `scikit-bio` |
| 6 | Project distances to 2D with PCoA and plot | `scikit-bio`, `matplotlib`, `seaborn` |
| 7 | Test group separation with PERMANOVA (999 permutations) | `scikit-bio` |
| 8 | Per-genus Mann-Whitney U tests + Benjamini-Hochberg FDR correction | `scipy.stats`, `statsmodels` |

## 💻 Software and Libraries

- **R (v4.4)**: `dada2` (v1.32) for upstream processing.
- **Python (v3.12)**: Core analysis environment.
    - **Core**: `pandas`, `numpy`, `scipy`, `statsmodels`
    - **Bioinformatics**: `scikit-bio`
    - **Visualization**: `matplotlib`, `seaborn`
- **Reproducibility**: All analyses are contained in a single Jupyter notebook with version-pinned dependencies (provided in the `environment.yml` file).

## 🔄 How to Reproduce This Work

1.  Clone this repository to your local machine.
2.  Navigate to the project directory and create the Conda environment:
    ```bash
    conda env create -f environment.yml
    conda activate als-microbiome
