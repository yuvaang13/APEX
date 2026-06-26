# APEX Phase 6 — Healthy Pulmonary Aging Validation (PAG)

## Title
APEX Phase 6: Validation of a Pulmonary Aging Proxy (PAG) Using DenseNet121 on NIH Chest X-rays

---

## 1. Overview

Phase 6 of the APEX framework evaluates whether a pretrained DenseNet121 model (APEXv2) learns a stable representation of pulmonary structure variation in healthy chest X-rays only.

Instead of performing disease classification, this phase tests whether the model produces a consistent latent signal defined as:

Pulmonary Age Gap (PAG): a normalized deviation score over healthy lung representations.

---

## 2. Dataset

### 2.1 Source
- NIH Chest X-ray dataset (Kaggle)
- Filtered subset: No Finding only

### 2.2 Files Used
- lung_age_dataset.csv
- NIH image dataset (images_*** folders)

### 2.3 Filtering Criteria
Only images with:
Finding Labels = No Finding

are included.

---

## 3. Model Architecture

### 3.1 Backbone
- DenseNet121
- Final layer replaced with:

Linear(1024 → 1)

### 3.2 Weights
- APEXv2.pth (state_dict)

---

## 4. Methodology

### 4.1 Image Processing
- Resize to 224×224
- Convert to tensor

### 4.2 Forward Pass
Each image:

X-ray → DenseNet121 → scalar score

### 4.3 PAG Definition
PAG is defined as:

PAG = (score - mean(score)) / std(score)

---

## 5. Implementation Pipeline

### 5.1 Image Indexing
All images are mapped:

filename → full_path

to enable fast lookup.

### 5.2 Inference
For each image:
- load image
- forward pass through model
- store output score

---

## 6. Results

### 6.1 Dataset Usage
- ~2000 healthy samples used
- minimal missing image mappings

---

### 6.2 PAG Distribution

FIGURE 1 — PAG Distribution (Healthy Cohort)

<img width="613" height="437" alt="Screenshot 2026-06-26 at 3 54 47 PM" src="https://github.com/user-attachments/assets/701dff17-9210-415e-881e-574171b712b8" />


---

### 6.3 Stability Analysis

FIGURE 2 — PAG Stability Across Samples

<img width="625" height="436" alt="Screenshot 2026-06-26 at 3 55 06 PM" src="https://github.com/user-attachments/assets/362ac56a-3f91-4e6f-b078-d2c8bf26c394" />


---

### 6.4 Rolling Variance

FIGURE 3 — PAG Rolling Variance

<img width="648" height="435" alt="Screenshot 2026-06-26 at 3 55 28 PM" src="https://github.com/user-attachments/assets/9b5a8f11-37ed-424a-b14b-59257b093f31" />


---

### 6.5 Noise Comparison

FIGURE 4 — PAG vs Random Noise

<img width="600" height="543" alt="Screenshot 2026-06-26 at 3 57 01 PM" src="https://github.com/user-attachments/assets/5d9199cb-7c7c-4491-9f67-96e53342f4c0" />


---

## 7. Validation

### 7.1 Collapse Check
The model is evaluated for:
- constant output (collapse)
- random noise behavior
- stable distribution

---

### 7.2 Interpretation
PAG represents:

A normalized deviation from the learned healthy pulmonary manifold.

It is NOT a clinical age predictor.

---

## 8. Limitations

- No true age labels used
- No longitudinal patient tracking
- Possible dataset bias (scanner/hospital effects)
- NIH heterogeneity

---

## 9. Conclusion

Phase 6 demonstrates:

- DenseNet121 learns structured representations of healthy lungs
- A stable scalar signal (PAG) can be derived
- The signal is non-random and statistically meaningful

This establishes the foundation for Phase 7:
disease deviation mapping and clinical extension of PAG.

---

## 10. Future Work

- Disease vs healthy separation
- External dataset validation
- t-SNE embedding visualization
- Patient-level longitudinal analysis
- Bias correction across imaging sources

---

## Pipeline Summary

NIH X-ray → preprocessing → DenseNet121 → score → normalization → PAG → analysis

---<img width="643" height="436" alt="Screenshot 2026-06-26 at 3 54 27 PM" src="https://github.com/user-attachments/assets/31c096b5-424d-451f-91af-4fafadee30b8" />
