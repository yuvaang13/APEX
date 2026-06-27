# APEX Phase 6 — Healthy Pulmonary Aging Validation (PAG)

## Title

APEX Phase 6: Validation of the Pulmonary Age Gap (PAG) Framework Using DenseNet121 on NIH Chest X-rays

---

# 1. Overview

Phase 6 evaluates whether APEXv2 learns a meaningful latent representation of pulmonary structure from chest radiographs.

Rather than performing disease classification, the objective is to determine whether a pretrained DenseNet121 model produces a stable pulmonary aging representation that can be normalized into a new metric called the **Pulmonary Age Gap (PAG)**.

The primary goals of this phase are:

- Validate that PAG is stable across healthy individuals.
- Verify that PAG is not random noise.
- Compare PAG distributions between healthy and diseased patients.
- Assess whether the current APEXv2 model has learned disease-associated pulmonary aging.

---

# 2. Dataset

## 2.1 Source

- NIH Chest X-ray Dataset
- Kaggle Version

---

## 2.2 Files Used

- lung_age_dataset.csv
- NIH Chest X-ray image folders
- APEXv2.pth

---

## 2.3 Healthy Cohort

Filtering criterion:

```
Finding Labels == "No Finding"
```

Healthy images available:

**14,597**

Healthy images evaluated:

**2,000**

---

## 2.4 Diseased Cohort

Filtering criterion:

```
Finding Labels != "No Finding"
```

Diseased images evaluated:

**2,000**

Diseased images include multiple pulmonary pathologies contained within the NIH dataset.

---

# 3. Model Architecture

## Backbone

DenseNet121

Final layer replaced with

```
Linear(1024 → 1)
```

Weights:

```
APEXv2.pth
```

---

# 4. Methodology

## Image Processing

Each image undergoes:

- Resize to 224 × 224
- RGB conversion
- Tensor conversion

---

## Forward Pass

Each chest X-ray is processed as

```
Chest X-ray
        ↓
DenseNet121
        ↓
Scalar Output Score
```

---

## Pulmonary Age Gap (PAG)

PAG is defined as

```
PAG = (score − μhealthy) / σhealthy
```

where

- μhealthy = mean healthy score
- σhealthy = standard deviation of healthy score

Using the healthy population as the normalization reference allows both healthy and diseased cohorts to be compared on the same pulmonary aging scale.

---

# 5. Implementation Pipeline

```
NIH Chest X-ray
        ↓
Image Preprocessing
        ↓
DenseNet121
        ↓
Scalar Output
        ↓
Healthy Normalization
        ↓
Pulmonary Age Gap (PAG)
        ↓
Statistical Analysis
```

---

# 6. Results

## 6.1 Healthy Cohort Statistics

Healthy images processed:

**2,000**

Missing images:

**0**

Healthy PAG Mean

```
0.0000
```

Healthy PAG Standard Deviation

```
1.0000
```

---

## 6.2 Healthy PAG Distribution

**Figure 1 — Healthy PAG Distribution**

<img width="618" height="444" alt="Screenshot 2026-06-27 at 11 25 41 AM" src="https://github.com/user-attachments/assets/d37d78dc-df63-42c4-b141-e2c004a1bdd4" />

---

## 6.3 PAG Stability


The first 300 healthy samples were visualized to inspect output consistency.

**Figure 2 — PAG Stability Across Healthy Samples**

<img width="665" height="454" alt="Screenshot 2026-06-27 at 11 26 16 AM" src="https://github.com/user-attachments/assets/a692268a-d327-4c2e-85fd-1fc8f34906c2" />


---

## 6.4 Rolling Variance

Rolling variance was computed over a moving window of 50 samples.

This verifies that PAG maintains relatively stable variance throughout the dataset without collapsing.

**Figure 3 — Rolling Variance of PAG**

<img width="631" height="447" alt="Screenshot 2026-06-27 at 11 26 33 AM" src="https://github.com/user-attachments/assets/90e51d4c-c72d-4327-b650-bf64cbfdd5bd" />


---

## 6.5 Random Noise Validation

Healthy PAG values were compared against randomly generated Gaussian noise.

Unlike random noise, PAG demonstrates structured behavior learned from chest radiographs.

**Figure 4 — PAG vs Random Noise**

<img width="607" height="549" alt="Screenshot 2026-06-27 at 11 27 01 AM" src="https://github.com/user-attachments/assets/2ab05571-96e7-4570-82a0-374c384e01e8" />


---

## 6.6 Diseased PAG Distribution

The same model was evaluated on a cohort of diseased chest radiographs.

PAG values were computed using the healthy population normalization.

**Figure 5 — Diseased PAG Distribution**

<img width="718" height="465" alt="Screenshot 2026-06-27 at 11 27 23 AM" src="https://github.com/user-attachments/assets/3b80001d-83ca-4e77-9ab6-b65a7c052791" />


---

## 6.7 Healthy vs Diseased PAG Distribution

Healthy and diseased PAG distributions were compared directly.

**Figure 6 — Healthy vs Diseased PAG Comparison**

<img width="768" height="470" alt="Screenshot 2026-06-27 at 11 27 47 AM" src="https://github.com/user-attachments/assets/dc66d587-87ea-4c69-8c3a-44557fc982bb" />


---

## 6.8 PAG Boxplot Comparison

A boxplot comparison was generated to visualize differences in the distributions.

**Figure 7 — Healthy vs Diseased PAG Boxplot**

<img width="690" height="528" alt="Screenshot 2026-06-27 at 11 28 41 AM" src="https://github.com/user-attachments/assets/338628ad-40dd-4b65-99ad-381e0d8b9787" />


---

# 7. Quantitative Results

Average Healthy PAG

```
0.0000
```

Average Diseased PAG

```
0.0023
```

Healthy Standard Deviation

```
1.0000
```

Diseased Standard Deviation

```
0.9553
```

Difference in Mean PAG

```
0.0023
```

---

# 8. Interpretation

Several important observations were made.

### Stable Healthy Representation

The healthy cohort produced a well-centered distribution with unit variance after normalization.

The rolling variance and stability analyses demonstrate that APEXv2 does not collapse to a constant output.

---

### Non-Random Signal

Comparison with Gaussian noise indicates that PAG is not random.

Instead, the model learns a structured latent representation from healthy chest radiographs.

---

### Limited Disease Separation

Although diseased patients were evaluated using the healthy normalization reference, the average PAG increased by only

```
0.0023
```

relative to healthy individuals.

This shift is extremely small and indicates that APEXv2 currently provides minimal separation between healthy and diseased pulmonary structure.

---

# 9. Discussion

The results suggest that the current version of APEX successfully learns stable image representations but has not yet learned pulmonary aging features that strongly distinguish disease.

Possible reasons include:

- insufficient training data
- limited training duration
- chronological age prediction may not directly encode pulmonary aging
- disease labels within the NIH dataset are highly heterogeneous
- averaging all diseases together may obscure disease-specific pulmonary aging effects

The stability analyses demonstrate that the framework itself is functioning correctly.

However, stronger pulmonary aging representations will require additional model development.

---

# 10. Limitations

Current limitations include:

- approximately 2,000 healthy images used during validation
- approximately 2,000 diseased images used for comparison
- no external validation dataset
- no longitudinal patient data
- heterogeneous disease categories
- possible scanner and hospital bias
- current model not explicitly optimized for pulmonary age estimation

---

# 11. Conclusion

Phase 6 successfully validates the computational framework underlying the Pulmonary Age Gap (PAG).

Major findings include:

- stable latent pulmonary representations
- reproducible healthy PAG distribution
- non-random learned signal
- successful healthy normalization framework
- limited separation between healthy and diseased cohorts

While these results demonstrate that the PAG framework is computationally valid, they also indicate that **APEXv2 requires additional training before meaningful pulmonary aging inference can be achieved.**

The negligible difference between healthy and diseased PAG suggests that the current model has not yet learned disease-associated pulmonary aging patterns.

Consequently, future work will focus on improving the quality of the learned representation rather than extending clinical analyses prematurely.

---

# 12. Future Work (APEXv3)

The next iteration of APEX will focus on improving pulmonary age representation through:

- training on substantially larger datasets
- longer training schedules
- improved age regression performance
- disease-specific PAG analysis
- external dataset validation
- Grad-CAM explainability
- latent feature visualization using t-SNE and UMAP
- calibration across multiple imaging institutions
- patient-level longitudinal pulmonary aging analysis

The primary objective of **APEXv3** will be to strengthen disease separation while preserving the stable PAG framework established in Phase 6.

---

# Pipeline Summary

```
NIH Chest X-ray
        ↓
Image Preprocessing
        ↓
DenseNet121
        ↓
Scalar Output
        ↓
Healthy Normalization
        ↓
Pulmonary Age Gap (PAG)
        ↓
Healthy Validation
        ↓
Disease Comparison
        ↓
Model Assessment
        ↓
APEXv3 Development
```
