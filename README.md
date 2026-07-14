# Gut Microbial Diversity in Amyotrophic Lateral Sclerosis

Reanalysis of 16S rRNA sequencing data from the Hertzberg et al. (2021) ALS cohort, investigating gut microbiome differences between ALS patients and their healthy spouse controls.

---

## 📋 Table of Contents

- [Biological Question](#biological-question)
- [Hypotheses](#hypotheses)
- [Project Overview](#project-overview)
- [Key Findings](#key-findings)
- [Repository Structure](#repository-structure)
- [Methods](#methods)
- [Requirements](#requirements)
- [Usage](#usage)
- [Results](#results)
- [Limitations](#limitations)
- [References](#references)
- [License](#license)

---

## 🧬 Biological Question

Does the gut bacterial community of ALS patients differ from that of healthy spouse controls in within-sample diversity (alpha) and between-sample composition (beta)?

## 🔬 Hypotheses

1. ALS patients will show **lower Shannon alpha diversity** than their matched controls
2. ALS patients will show **visible separation from controls** in a Bray-Curtis PCoA ordination

## 📊 Project Overview

This project reanalyzes publicly available 16S ribosomal RNA sequencing data from the Hertzberg et al. (2021) cohort to investigate whether the gut bacterial communities of ALS (amyotrophic lateral sclerosis) patients differ from those of their cohabiting healthy spouse controls.

### Cohort Details

| Feature | Description |
|---------|-------------|
| **Samples** | 18 stool samples (9 ALS patients + 9 spouses) |
| **Design** | 9 household pairs (spouse-paired) |
| **Target** | V4 region of 16S rRNA gene |
| **Primers** | 515F / 806R |
| **Platform** | Illumina MiSeq |
| **Accession** | NCBI BioProject PRJNA566436 |

### Why This Cohort?

The spouse-paired design is a methodological strength that controls for shared diet, household environment, and lifestyle factors — precisely the variables that confound typical case-control microbiome studies. A difference between case and control is therefore more likely attributable to disease status rather than environmental variation.

---

## 🎯 Key Findings

| Finding | Result |
|---------|--------|
| **Alpha Diversity** | ALS patients had **higher** Shannon diversity than spouses (paired Wilcoxon p = 0.004) — *opposite of hypothesis* |
| **Beta Diversity** | Significant but modest separation (PERMANOVA pseudo-F = 1.7, p ≈ 0.02) |
| **Top ALS-Enriched Genera** | *Parabacteroides*, *Ruminococcus torques* group, *Blautia*, *Subdoligranulum*, *Lachnoclostridium* |

> ⚠️ **Note:** These findings are preliminary and require replication in independent cohorts.

---

## 🛠️ Methods

All downstream analysis was performed in Python 3.12 within a single reproducible Jupyter Notebook.

### Pipeline Steps

| Step | What It Does | Python Tools |
|------|--------------|--------------|
| 1 | Load ASV table, taxonomy, and metadata | pandas |
| 2 | Drop rare ASVs (<5% prevalence, min. 2 samples) | pandas |
| 3 | Convert counts to per-sample relative abundances | pandas |
| 4 | Compute Shannon alpha diversity; paired Wilcoxon test | scikit-bio, scipy.stats |
| 5 | Compute Bray-Curtis beta diversity matrix | scikit-bio |
| 6 | PCoA projection and visualization | scikit-bio, matplotlib, seaborn |
| 7 | PERMANOVA test (999 permutations) | scikit-bio |
| 8 | Per-genus Mann-Whitney U tests + Benjamini-Hochberg FDR | scipy.stats, statsmodels |

### Upstream Processing

The raw reads were processed through DADA2 (Callahan et al. 2016) against the SILVA 138 reference database (Quast et al. 2013) to produce the ASV count table and taxonomy assignments used as input for this analysis.

---
