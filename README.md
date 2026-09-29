# Homology-Based Variant-Effect Predictors Break Down on Cytochrome P450 Pharmacogenes

This repository accompanies the paper **"Homology-Based Variant-Effect Predictors Break Down on Cytochrome P450 Pharmacogenes"** (PSB 2027). It contains the analysis code, a partial copy of the input data, and supplementary materials for the paper.

## Contents

```
├── README.md
├── LICENSE
├── requirements.txt      # conda environment spec
├── code/                 # analysis notebooks
│   ├── cyp2c9_activity_prediction.ipynb
│   ├── alphamissense_ambiguous.ipynb
│   └── knn_hyperparameter_tuning.ipynb
└── data/                 # input data (incomplete, see below)
```

- **`code/`** — Jupyter notebooks that reproduce the analyses and figures in the paper.
  - `cyp2c9_activity_prediction.ipynb` — combines AlphaMissense pathogenicity scores and ESM-2 evolutionary
    scores into a calibrated ensemble for predicting CYP2C9 variant activity, with validation against
    held-out CLICK-seq, ClinVar, and PharmVar data. Produces Fig. 2, Fig. 3, Fig. S4, Table 1, and
    Table S1, and exports `am_calibrated_error.csv` for use by `alphamissense_ambiguous.ipynb`.
  - `alphamissense_ambiguous.ipynb` — shows that CYP pharmacogenes have a higher rate of "ambiguous"
    AlphaMissense predictions than the genome-wide average, and tests whether structural/biochemical
    priors (substitution chemistry, secondary structure, substrate-recognition-site distance) explain
    AM's prediction error. Also computes the ClinPGx substrate-discordance statistic cited in Section 3.5.
    Produces Fig. 1, Fig. S5, and the discordance-rate numbers reported in-text.
  - `knn_hyperparameter_tuning.ipynb` — nested cross-validated hyperparameter search (pooling, neighbor
    weighting, distance metric, K) for the KNN-over-ESM-2-embeddings component of the ensemble. Produces
    Fig. S2 and the optimal hyperparameters (mean pooling, distance-weighted, Euclidean, K=30) used
    throughout `cyp2c9_activity_prediction.ipynb`.
- **`data/`** — the input files the notebooks read from. **This is an incomplete subset of the full data used in the paper.** See the "Required input files" cell near the top of each notebook for the exact files it expects.
- **Supplementary materials** — supplementary figures, tables, and text referenced in the paper.

## Requirements

- Linux, Python 3.12
- [conda](https://docs.conda.io/) (or [miniconda](https://docs.conda.io/en/latest/miniconda.html)/[mamba](https://mamba.readthedocs.io/))

All package dependencies are pinned in [`requirements.txt`](requirements.txt), which is a conda environment export.

## Installation

Create and activate the conda environment:

```bash
conda create --name my_env --file requirements.txt
conda activate my_env
```

## Running the notebooks

1. Launch Jupyter from the `code/` directory so the notebooks' relative data paths (`../data/`) resolve correctly:

   ```bash
   cd code
   jupyter lab
   ```

2. Open any notebook and run cells top to bottom. Each notebook begins with a "Required input files" cell documenting exactly which files it reads from `../data/`. Run `knn_hyperparameter_tuning.ipynb` before `cyp2c9_activity_prediction.ipynb` if you want to re-derive the KNN hyperparameters from scratch; otherwise `cyp2c9_activity_prediction.ipynb` uses the already-selected configuration directly. `alphamissense_ambiguous.ipynb` reads `am_calibrated_error.csv`, which `cyp2c9_activity_prediction.ipynb` exports, so run the latter at least once first.

<!-- ## Citation

If you use this code or data, please cite:

> Xu, H., Samori, I., Nayar, G., & Altman, R. B. (2027). Homology-Based Variant-Effect Predictors Break Down on Cytochrome P450 Pharmacogenes. *Pacific Symposium on Biocomputing (PSB)*.

```bibtex
@inproceedings{xu2027homology,
  title     = {Homology-Based Variant-Effect Predictors Break Down on Cytochrome P450 Pharmacogenes},
  author    = {Xu, Helen and Samori, Issah and Nayar, Gowri and Altman, Russ B.},
  booktitle = {Pacific Symposium on Biocomputing (PSB)},
  year      = {2027}
}
```

**Authors:** Helen Xu¹, Issah Samori², Gowri Nayar³, Russ B. Altman¹ ² ³ ⁴

¹ School of Medicine, Stanford University, Stanford, CA, USA
² Department of Bioengineering, Stanford University, Stanford, CA, USA
³ Department of Biomedical Data Science, Stanford University, Stanford, CA, USA
⁴ Department of Genetics, Stanford University, Stanford, CA, USA

Correspondence: russ.altman@stanford.edu -->

## License

This project is licensed under the MIT License — see [`LICENSE`](LICENSE) for details.
