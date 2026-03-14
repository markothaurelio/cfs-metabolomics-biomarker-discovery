
# Chronic Fatigue Syndrome Metabolomics Feature Selection

This project applies **regularised logistic regression with repeated nested cross-validation**
to identify **metabolites consistently associated with Chronic Fatigue Syndrome (CFS)**.

The primary goal is **robust feature selection** rather than pure prediction performance.
Regularisation and repeated resampling are used to identify metabolites that remain
stable across many independently trained models.

---

# Dataset

Dataset: MetaboLights MTBLS161  
Provider: EMBL-EBI  
License: Public research dataset  
Link: https://www.ebi.ac.uk/metabolights/MTBLS161

File used in this project:

```
m_MTBLS161_mecfs_metabolite_profiling.tsv
```

The dataset contains metabolite concentrations measured from serum and urine samples.

### Dataset Summary

| Dataset | Samples | Metabolites |
|--------|--------|-------------|
| Full metabolomics matrix | 117 | 41 |
| Serum samples | 59 | 41 |
| Urine samples | 58 | 41 |

This analysis focuses on **serum samples**.

### Class Distribution (Serum)

| Class | Samples |
|------|--------|
| Chronic Fatigue Syndrome | 34 |
| Control | 25 |

---

# Methodology

Feature selection is performed using **elastic net regularised logistic regression**
within a **scikit-learn pipeline**.

Regularisation forces many coefficients to **zero**, which performs automatic
feature selection.

### Preprocessing Pipeline

1. Missingness filtering (>40% missing removed)
2. Median imputation
3. Standard scaling
4. Logistic regression with elastic net penalty

---

# Validation Strategy

To avoid optimistic bias and evaluate feature stability,
the project uses **repeated nested cross-validation**.

### Configuration

```
REPEATS = 10
OUTER_SPLITS = 5
INNER_SPLITS = 5
Total outer models = 50
```

Each outer fold produces a separate model and selected feature set.

---

# Feature Selection Stability

Feature stability was evaluated using **Jaccard similarity** between feature sets.

Mean Jaccard similarity:

```
0.5866
```

Total pairwise comparisons:

```
1225
```

This indicates **moderate agreement in selected metabolites across models**.

---

# Most Consistently Selected Metabolites

| Metabolite | Selection Frequency |
|------------|--------------------|
| L-Phenylalanine | 100% |
| D-Glucose | 96% |
| L-Serine | 92% |
| Glycine | 92% |
| L-Aspartate | 92% |
| L-Threonine | 90% |
| Citrulline | 90% |
| Hypoxanthine | 88% |
| L-Glutamine | 86% |
| Betaine | 82% |

These metabolites appear most consistently across models and represent
**candidate metabolic biomarkers associated with CFS**.

---

# Technologies Used

- Python
- NumPy
- Pandas
- Scikit‑learn

---

# Reproducibility

Run the analysis with:

```
python CFS_analysis.py
```

The script performs:

- data loading
- preprocessing
- nested cross-validation
- feature stability analysis

