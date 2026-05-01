# UBI — Machine Learning Authentication of Bohol Ubi 'Kinampay' via EDXRF Elemental Fingerprinting

[![License: MIT](https://img.shields.io/badge/Code%20License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Data License: CC BY 4.0](https://img.shields.io/badge/Data%20License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19949864.svg)](https://doi.org/10.5281/zenodo.19949864)

This repository accompanies the manuscript:

> **Bongalos, J. B., Bautista VII, A. T., Rallos, R. V., Jomoc, D. B., Backian, G. S., Masangcay, T. D., Agoo, E. M. G., & Magtaas, R. A. H.** (submitted). *Machine Learning Framework for Variety Authentication and Geographic Provenance of Bohol Ubi 'Kinampay' (Dioscorea alata L.) Using EDXRF Elemental Fingerprinting.* Scientific Reports.

It contains the source dataset, complete analytical pipeline, and figure-generation code required to reproduce every result reported in the paper.

---

## Overview

The paper presents a Random Forest classification framework with leave-one-out cross-validation (LOOCV) and SHAP (SHapley Additive exPlanations) explainability for authenticating Bohol Ubi 'Kinampay' purple yam (*Dioscorea alata* L.) via energy-dispersive X-ray fluorescence (EDXRF) elemental fingerprinting. The framework benchmarks ten classification algorithms across four validation schemes on a 52-sample dataset (36 authentic Bohol Kinampay; 16 non-authentic from Palawan and Benguet) and identifies seven elements (K, Mn, Cu, Zn, S, Cl, Sr) as discriminative features.

**Headline result.** Random Forest achieved 86.5% LOOCV accuracy (sensitivity = 94.4%, specificity = 68.8%, AUC = 0.888), outperforming all benchmarked models under LOOCV. SHAP analysis identified Mn (26.3%), Sr (17.2%), and Cu (16.3%) as the dominant predictors, with all seven elements showing consistent class-directional contributions.

---

## Repository Structure

```
UBI/
├── EDXRF_DATA.csv                                          # Source dataset (52 × 8)
├── Final_EDXRF_Enhanced_Multi_Classifier_multi-Models.ipynb  # Full analytical pipeline
├── UBI_Kinampay.py                                         # Auxiliary script
├── pca_scores.csv                                          # PCA scores (derived output)
├── y_data.csv                                              # Class labels (derived output)
├── requirements.txt                                        # Python package versions
├── LICENSE                                                 # MIT (code) + CC-BY 4.0 (data)
├── .gitignore                                              # Standard Python / Jupyter
├── .zenodo.json                                            # Zenodo archive metadata
└── README.md                                               # This file
```

### Dataset description (`EDXRF_DATA.csv`)

| Column | Type | Description |
|---|---|---|
| `Class` | int | 0 = non-authentic (Palawan + Benguet, n = 16); 1 = authentic Bohol Kinampay (n = 36) |
| `K` | float | Potassium concentration (ppm) |
| `Mn` | float | Manganese concentration (ppm) |
| `Cu` | float | Copper concentration (ppm) |
| `Zn` | float | Zinc concentration (ppm) |
| `S` | float | Sulfur concentration (ppm) |
| `Cl` | float | Chlorine concentration (ppm) |
| `Sr` | float | Strontium concentration (ppm) |

Measurements were obtained on a Bruker S2 PUMA ED-XRF spectrometer with Si(Li) semiconductor detector, Mo anode (50 kV, 45 µA), 200-second live time, and IPE 2017 / 2018 / 2019 Certified Reference Materials with 90–110% recovery acceptance. See manuscript Section 2.1 for full instrumental details.

---

## Reproduction

### Quick start

```bash
git clone https://github.com/Jbong17/UBI.git
cd UBI
pip install -r requirements.txt
jupyter notebook Final_EDXRF_Enhanced_Multi_Classifier_multi-Models.ipynb
```

Run all cells. With `random_state = 42` fixed throughout, every reported number, table, and figure should reproduce exactly.

### Key configurations reported in the paper

| Component | Value |
|---|---|
| Random Forest (final / SHAP model) | `n_estimators=500, max_depth=4, min_samples_leaf=3, min_samples_split=5, max_features='sqrt', class_weight='balanced', random_state=42` |
| Train / test split | 75 / 25 stratified, `random_state=42` |
| Validation schemes | training accuracy, 75/25 hold-out, 5-fold CV, LOOCV |
| Feature scaling | `StandardScaler` (zero mean, unit variance), fit per CV fold |
| SHAP backend | `TreeExplainer` (shap 0.50.0) |

### Requirements

Tested with Python 3.8+ and the following minimum versions (full pins in `requirements.txt`):

- `scikit-learn==1.0.2`
- `shap==0.50.0`
- `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`, `statsmodels`

---

## Citation

If you use this code or data, please cite the paper (citation will be updated upon acceptance):

```bibtex
@article{bongalos2026kinampay,
  author  = {Bongalos, Jerald B. and Bautista VII, Angel T. and Rallos, Roland V. and
             Jomoc, Dennis B. and Backian, Grace S. and Masangcay, Teresita D. and
             Agoo, Esperanza Maribel G. and Magtaas, Remjohn Aron H.},
  title   = {Machine Learning Framework for Variety Authentication and Geographic
             Provenance of Bohol Ubi 'Kinampay' ({Dioscorea} alata {L.}) Using
             {EDXRF} Elemental Fingerprinting},
  journal = {Scientific Reports},
  year    = {2026},
  note    = {Manuscript submitted}
}
```

A versioned snapshot of this repository is permanently archived on Zenodo: **DOI: 10.5281/zenodo.XXXXXXX** *(to be issued upon submission release).*

---

## Authors and Affiliations

- **Jerald B. Bongalos** *(corresponding)* — De La Salle University; Asian Institute of Management; DOST-Science Education Institute; DOST-Philippine Nuclear Research Institute
  📧 jerald_bongalos@dlsu.edu.ph | jbongalos.phdinds2028@aim.edu
- **Angel T. Bautista VII** — DOST-Philippine Nuclear Research Institute
- **Roland V. Rallos** — DOST-Philippine Nuclear Research Institute
- **Dennis B. Jomoc** — Bohol Island State University – Bilar Campus
- **Grace S. Backian** — Northern Philippines Root Crops Research and Training Center, Benguet State University
- **Teresita D. Masangcay** — Northern Philippines Root Crops Research and Training Center, Benguet State University
- **Esperanza Maribel G. Agoo** — De La Salle University
- **Remjohn Aron H. Magtaas** — DOST-Philippine Nuclear Research Institute

---

## License

- **Code** (notebook, scripts) — [MIT License](LICENSE)
- **Data** (`EDXRF_DATA.csv`, `pca_scores.csv`, `y_data.csv`) — [Creative Commons Attribution 4.0 International (CC-BY 4.0)](https://creativecommons.org/licenses/by/4.0/)

You are free to use, modify, and redistribute the contents under the terms above with appropriate attribution.

---

## Acknowledgements

This work was supported by the Department of Science and Technology – Science Education Institute (DOST-SEI) Human Resource Development Program (master's scholarship and thesis grant) and the Philippine Nuclear Science Foundation, Inc. (additional thesis grant). We thank the Provincial Agriculture Offices of Oriental Mindoro and Palawan for kind assistance during fieldwork, and the Institute of Plant Breeding – National Plant Genetic Resources Laboratory (IPB-NPGRL), University of the Philippines Los Baños, for valuable technical assistance. Special thanks to Mr. Fernando B. Aurigue, Retired Career Scientist, for his generosity and his technical and financial assistance throughout this project.

---

## Contact

For questions, issues, or collaboration inquiries, please open a [GitHub Issue](https://github.com/Jbong17/UBI/issues) or contact the corresponding author.
