# Antifp: In Silico Prediction of Antifungal Peptides

Welcome to the official repository for Antifp, a computational method and web server for predicting and designing antifungal peptides (AFPs) from amino acid sequences. This resource is designed to support researchers in antifungal therapeutics, antimicrobial peptide research, and computational drug discovery.

**Web Server:** http://webs.iiitd.edu.in/raghava/antifp
**Standalone Software:** Available via the Download menu on the web server
**Mobile App (Android):** Available via the Download menu on the web server

---

## Citation

Agrawal, P., Bhalla, S., Chaudhary, K., Kumar, R., Sharma, M., & Raghava, G. P. S. (2018).
In Silico Approach for Prediction of Antifungal Peptides.
*Frontiers in Microbiology*, 9:323.
https://doi.org/10.3389/fmicb.2018.00323

---

## About the Tool

Antifp is a sequence-based computational method developed to predict and design antifungal peptides using Support Vector Machine (SVM) and other machine learning classifiers. It is built on the largest AFP dataset to date (1459 unique AFPs) and employs a wide range of peptide features including amino acid composition, dipeptide composition, binary profiles, and terminus-based patterns.

The tool integrates data from:

* DRAMP — Data Repository of Antimicrobial Peptides (1585 AFPs extracted, 1459 unique after filtering)
* SwissProt (for randomly generated negative peptides in DS2 and Main datasets)
* AMPs with no antifungal activity (for negative dataset in DS1)

---

## Key Features

**Three Curated Datasets**

* Antifp_Main — 1459 AFPs vs. 1459 mixed negatives (AMPs + random peptides)
* Antifp_DS1 — 1459 AFPs vs. 1459 non-antifungal AMPs
* Antifp_DS2 — 1459 AFPs vs. 1459 randomly generated SwissProt peptides
* 80/20 split for training and validation across all datasets

**Comprehensive Feature Extraction**

* Amino Acid Composition (AAC) — 20-dimensional vector
* Dipeptide Composition (DPC) — 400-dimensional vector
* Terminus Composition — N5, N10, N15, C5, C10, C15, and combined (N5C5, N10C10, N15C15)
* Binary Profile — captures residue order from terminal segments
* AAC + Mass, Charge, pI — extended 23-dimensional physicochemical feature vector

**Multiple Machine Learning Classifiers**

* Support Vector Machine (SVM) — SVMlight v6.02, RBF kernel
* Random Forest (RF)
* J48 Decision Tree
* SMO (Sequential Minimal Optimization)
* Naïve Bayes
* All classifiers implemented via WEKA suite (except SVM)

**Motif Analysis**

* MERCI (Motif-EmeRging and with Classes-Identification) used for exclusive motif extraction
* Exclusive AFP motifs identified: `CFCT`, `RCFC`, `NCAS`, `CASV`
* Exclusive non-AFP motifs identified: `CGNTK`, `GNTK`, `NTKH`
* Motifs extracted from all three datasets (Antifp_Main, DS1, DS2)

**Hard Benchmarking Dataset**

* Antifp_hard — 291 AFPs vs. 291 compositionally similar non-AFPs
* Negative sequences selected by minimum Euclidean distance from AFP composition
* Used to test model ability to discriminate similar-composition but different-activity peptides

**Web Server Modules**

* Predict — AFP or non-AFP classification with SVM score and physicochemical properties
* Mutational Series — generate and rank all single-mutation analogs for AFP design
* Sliding Window Prediction — scan overlapping windows of a full protein sequence
* Download — datasets in FASTA format

**Standalone & Mobile App**

* Standalone: Python (v2.7.11) + wxPython (v3.0.0) for Linux, Mac, Windows 64-bit
* Mobile App: Python (v2.7.11) + Kivy (v1.9.2) for Android (.apk format)
* Both implement the best model; minimum sequence length of 15 residues required

---

## Overview

Antifp provides SVM-based prediction along with:

* Binary classification of AFPs vs. non-AFPs across three dataset settings
* Residue composition and positional preference analysis
* Exclusive motif identification in AFPs via MERCI
* Novel AFP design via single-mutation scanning
* Protein-level sliding window scanning for putative antifungal regions
* Hard benchmarking against compositionally similar peptides with different activity
* Standalone and mobile deployment options

---

### Residue Composition Insights

Analysis of AFPs in the Antifp_Main dataset revealed:

* **Enriched residues in AFPs:** C, G, H, K, R, S — positively charged and cationic
* **Enriched residues in non-AFPs:** A, D, E, I, L, V, W
* **N-terminus preference:** R at position 1, V at position 2, K at position 3
* **C-terminus preference:** C highly preferred at positions 1 and 3; H at position 2
* Cationic residues (K, R) enable electrostatic interaction with negatively charged fungal membranes

---

### Best Model Performance — Antifp_Main Dataset

**Amino Acid Composition (AAC) — SVM**

| Dataset | Sen (%) | Spc (%) | Acc (%) | MCC | ROC |
|---------|---------|---------|---------|-----|-----|
| Training | 88.61 | 87.93 | 88.27 | 0.77 | 0.94 |
| Validation | 86.60 | 85.91 | 86.25 | 0.73 | 0.94 |

**Dipeptide Composition (DPC) — SVM**

| Dataset | Sen (%) | Spc (%) | Acc (%) | MCC | ROC |
|---------|---------|---------|---------|-----|-----|
| Training | 88.53 | 85.02 | 86.77 | 0.74 | 0.94 |
| Validation | 89.69 | 87.63 | 88.66 | 0.77 | 0.95 |

**Binary Profile (N15C15) — SVM**

| Dataset | Sen (%) | Spc (%) | Acc (%) | MCC | ROC |
|---------|---------|---------|---------|-----|-----|
| Training | 85.55 | 84.23 | 84.88 | 0.70 | 0.92 |
| Validation | 85.39 | 83.90 | 84.64 | 0.69 | 0.92 |

**AAC + Mass, Charge, pI — SVM (Best Composition Model)**

| Dataset | Acc (%) | MCC |
|---------|---------|-----|
| Training | 88.78 | 0.78 |
| Validation | 83.33 | 0.67 |

---

### Best Model Performance — Antifp_DS2 Dataset

| Feature | Acc (%) | MCC | ROC |
|---------|---------|-----|-----|
| AAC (Training) | 92.81 | 0.86 | 0.97 |
| AAC (Validation) | 90.38 | 0.81 | 0.96 |
| DPC (Training) | 91.87 | 0.84 | 0.96 |
| DPC (Validation) | 92.10 | 0.84 | 0.96 |
| Binary N15C15 (Training) | 92.32 | 0.85 | 0.97 |
| Binary N15C15 (Validation) | 92.70 | 0.85 | 0.97 |

---

### Benchmarking on Hard Dataset (Antifp_hard)

Performance of Antifp models and existing methods on compositionally similar AFP vs. non-AFP sequences:

| Method | Algorithm | Sen (%) | Spc (%) | Acc (%) |
|--------|-----------|---------|---------|---------|
| **Antifp Binary Profile** | **SVM** | **81.95** | **63.45** | **75.43** |
| Antifp Composition | SVM | 61.51 | 62.89 | 62.20 |
| ClassAMP | SVM | 37.11 | 59.79 | 48.45 |
| ClassAMP | Random Forest | 14.43 | 75.94 | 45.18 |
| iAMP-2L | FKNN | 20.96 | 22.34 | 21.56 |

> Binary profile-based model significantly outperforms all existing methods on compositionally similar peptides, confirming its strength in discriminating sequences with similar composition but different activity.

---

### Three Web Server Models

| Model | Dataset | Feature | Best Use Case |
|-------|---------|---------|--------------|
| Model 1 (Antifp_DS1_binary) | Antifp_DS1 | N15C15 Binary | Check if peptide is exclusively antifungal (no other AMP activity) |
| Model 2 (Antifp_DS2_binary) | Antifp_DS2 | N15C15 Binary | Check antifungal activity with no prior knowledge of peptide |
| Model 3 (Antifp_Main_binary) | Antifp_Main | N15C15 Binary | Check exclusively antifungal property with no prior knowledge |

---

### Improvements Over Existing Methods

* First dedicated web server exclusively for AFP prediction and design
* Largest AFP dataset used to date (1459 unique AFPs from DRAMP)
* Three dataset settings for different experimental needs
* Binary profile models outperform composition-based models on hard benchmarking
* Outperforms ClassAMP (SVM/RF) and iAMP-2L on compositionally similar datasets
* Includes mobile app and standalone software for offline use
* Physicochemical property calculation (mass, charge, pI) integrated into prediction

---

### Limitations

* Post-translational modifications not considered during model development
* Cannot predict broad-spectrum activity of designed peptides
* Minimum peptide length of 15 residues required for binary profile models
* Structural properties (secondary structure, surface accessibility) not incorporated
* Randomly generated negative peptides (DS2) may not fully represent biological non-AFPs

---

## Applications

* Antifungal peptide discovery and design
* Screening peptide libraries for antifungal activity
* Scanning protein sequences for putative antifungal regions
* Design of AFP analogs with enhanced potency via mutational series
* Machine learning benchmarking for AMP/AFP research
* Combat against drug-resistant fungal pathogens (*Candida*, *Aspergillus*, *Cryptococcus*)

---

## Contact & Authors

**Prof. Gajendra P. S. Raghava**
raghava@iiitd.ac.in | raghava@imtech.res.in
Center for Computational Biology, Indraprastha Institute of Information Technology, New Delhi, India
Council of Scientific and Industrial Research, Institute of Microbial Technology, Chandigarh, India
http://webs.iiitd.edu.in/raghava/

**Piyush Agrawal** — CSIR-IMTECH, Chandigarh (equal contribution)
**Sherry Bhalla** — CSIR-IMTECH, Chandigarh (equal contribution)
**Kumardeep Chaudhary** — CSIR-IMTECH, Chandigarh
**Rajesh Kumar** — CSIR-IMTECH, Chandigarh
**Meenu Sharma** — CSIR-IMTECH, Chandigarh

Developed at **CSIR-Institute of Microbial Technology (IMTECH), Chandigarh** and **IIIT Delhi, India**

---

## License

This tool is distributed under the terms of the
**Creative Commons Attribution License (CC BY 4.0)**
© 2018 Agrawal, Bhalla, Chaudhary, Kumar, Sharma and Raghava.
Use, distribution, or reproduction in other forums is permitted provided the original authors and publication are credited.

---

## Acknowledgements

Supported by:

* J. C. Bose National Fellowship, Department of Science and Technology (DST)
* DST-INSPIRE Fellowship
* Indian Council of Medical Research (ICMR)
* Council of Scientific and Industrial Research (CSIR) — project Open GENESIS BSC0121
* Department of Biotechnology (DBT) — project BTISNET

We acknowledge all researchers whose published work on antifungal and antimicrobial peptides contributed to this dataset.
