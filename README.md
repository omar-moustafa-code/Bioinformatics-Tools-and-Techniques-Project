# Bioinformatics-Tools-and-Techniques-Project

## Biological Question
Does the gut bacterial community of ALS patients differ from that of healthy spouse controls in within-sample diversity (alpha) and between-sample composition (beta)?

## Project Overview
This project reanalyzes publicly available 16S ribosomal RNA sequencing data from the Hertzberg et al. (2021) cohort to investigate whether the gut bacterial communities of ALS (amyotrophic lateral sclerosis) patients differ from those of their cohabiting healthy spouse controls. The study leverages a spouse-paired design — a methodological strength that controls for shared diet, household environment, and lifestyle factors that typically confound microbiome studies. The upstream DADA2 processing was pre-provided; all downstream diversity analysis and visualization was conducted in Python within this repository.

## Methods
All downstream analysis was performed in Python 3.12 in a single reproducible Jupyter Notebook:

- Load data: ASV count table, SILVA taxonomy, and sample metadata (pandas)
- Prevalence filter: remove ASVs present in <5% of samples (min. 2 samples)
- Relative abundance: convert raw counts to per-sample proportions
- Alpha diversity: Shannon index per sample; paired Wilcoxon signed-rank test within household pairs (scikit-bio, scipy)
- Beta diversity: Bray-Curtis dissimilarity matrix (scikit-bio)
- Ordination: Principal Coordinates Analysis (PCoA) visualization (scikit-bio, matplotlib, seaborn)
- PERMANOVA: test group separation with 999 permutations (scikit-bio)
- Differential abundance: per-genus Mann-Whitney U tests with Benjamini-Hochberg FDR correction (scipy, statsmodels)


