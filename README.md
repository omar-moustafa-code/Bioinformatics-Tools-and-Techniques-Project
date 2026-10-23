# ALS Gut Microbiome Reanalysis (PRJNA566436)

Course project (BIOL 3601, *Bioinformatics Tools and Techniques*, Project 6): reanalysis of the 16S rRNA gut-microbiome data from Hertzberg et al. (2021), comparing 9 ALS patients with their 9 spouse controls.

## Summary of findings (exploratory, n = 9 pairs)
| Analysis | Result |
|---|---|
| Sequencing depth | Balanced between groups (paired Wilcoxon p = 0.65) |
| Shannon diversity | Higher in ALS, 9/9 pairs; paired Wilcoxon p = 0.0039 (the minimum possible for n = 9); holds after rarefaction |
| Richness / Pielou evenness | Both higher in ALS (p = 0.0039 / 0.012) |
| Bray-Curtis PERMANOVA | pseudo-F = 1.71; unrestricted p ≈ 0.02; **exact paired (512 swaps) p = 0.0078** |
| Dispersion (PERMDISP) | Bray-Curtis: n.s. (p = 0.36). Aitchison: differs (p = 0.011), so interpret that PERMANOVA cautiously |
| Genus level (51 genera) | **No genus reaches q < 0.05** (best: Blautia, [Ruminococcus] torques group, Oscillibacter, q = 0.066 on paired CLR tests) |
| *Prevotella* | Lower in ALS (15% vs 43% mean relative abundance) but not significant (paired q = 0.37) |

Limitations: tiny sample; unmodelled confounders (sex, diet, dysphagia, riluzole); ~19% of filtered reads lack a genus label; compositional data with pseudocount-sensitive genus tests.

## Repository structure
```
.
├── README.md
├── ALS_Microbiome_Reanalysis_v2.ipynb   # upgraded notebook (runs top to bottom)
├── CORRECTIONS.md                       # errata for the original report + revised results text
├── data/
│   ├── metadata.tsv      # sample, status (ALS/Control), pair
│   ├── asv_table.tsv     # 275 ASVs x 18 samples (DADA2 counts)
│   └── taxonomy.tsv      # SILVA 138 assignments
└── report/               # PDF report
```

## Data provenance
Raw reads: NCBI SRA BioProject PRJNA566436. ASVs were inferred with DADA2 (R) and annotated against SILVA 138. Record the exact R/dada2 versions and truncation parameters you used in `data/PROVENANCE.md`; do not state versions you did not run.

## Reproduce
1. `git clone <repo-url> && cd <repo>`
2. `python -m venv .venv && source .venv/bin/activate`
3. `pip install scikit-bio statsmodels scipy pandas numpy matplotlib seaborn jupyter` (pin the versions you used in `requirements.txt`)
4. `jupyter lab ALS_Microbiome_Reanalysis_v2.ipynb`, then run all cells. The notebook downloads `data/` if absent.

Colab: open the notebook and run all cells; no local setup needed.

## Note on sample size
The original study analysed 10 ALS-spouse pairs plus 10 healthy couples. The public BioProject (PRJNA566436) contains 18 samples, so this reanalysis has 9 pairs.

## Reference
Hertzberg VS, et al. (2021). Gut microbiome in amyotrophic lateral sclerosis, spousal-control design. BioProject PRJNA566436. Verify the full citation against the paper before publishing.
