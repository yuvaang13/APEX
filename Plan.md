# APEX Project Plan
## AI-Powered Pulmonary EXtended Aging Framework for Lung Disease Characterization

---

## 1. Project Overview

APEX is a machine learning research project that reframes chest X-ray analysis as a **biological aging problem** instead of a classification problem. The goal is to learn how lungs age normally and then measure how disease distorts that aging process.

Rather than predicting disease labels directly, APEX introduces:
- **Pulmonary Age Gap (PAG)** → overall lung health deviation
- **Disease-Specific Aging Signatures (DAS)** → disease pattern differences
- **Regional Pulmonary Aging Maps (RPAM)** → spatial disease localization

---

## 2. Main Objective

> Can we model normal lung aging from chest X-rays and use deviations from that model to detect and characterize disease earlier and more interpretably than standard classification systems?

---

## 3. Project Phases

# Phase 1 — Data Engineering (COMPLETED / IN PROGRESS)
### Goal: Build clean, usable dataset

### Tasks:
- [x] Download NIH ChestX-ray14 metadata
- [x] Extract partial image archives
- [x] Build image index (metadata ↔ files)
- [x] Verify dataset integrity (24,999 images matched)

### Outputs:
- `available_images.csv`
- `healthy.csv`
- Verified image directory structure

---

# Phase 2 — Dataset Construction
### Goal: Create modeling-ready dataset

### Tasks:
- Filter healthy (“No Finding”) images
- Remove missing or corrupted samples
- Extract patient age labels
- Create train/validation/test splits (70/15/15)
- Analyze age distribution bias

### Outputs:
- `healthy_train.csv`
- `healthy_val.csv`
- `healthy_test.csv`
- Age distribution plots

---

# Phase 3 — Baseline Model (Lung Age Prediction)
### Goal: Learn normal pulmonary aging

### Model:
- CNN (ResNet18 or EfficientNet)
- Input: Chest X-ray image
- Output: Predicted lung age

### Training:
- Loss: MAE (Mean Absolute Error)
- Optimizer: Adam
- Pretrained ImageNet backbone

### Outputs:
- Trained lung age model
- Predicted vs actual age plot
- Performance metrics (MAE, RMSE, R²)

---

# Phase 4 — Pulmonary Age Gap (PAG)
### Goal: Quantify disease-related aging

### Formula:
PAG = Predicted Lung Age − Chronological Age

### Tasks:
- Compute PAG for all patients
- Compare distributions across:
  - Healthy
  - Pneumonia
  - Fibrosis
  - COPD-related cases
- Statistical significance testing

### Outputs:
- PAG distribution plots
- Group comparison charts
- Statistical test results

---

# Phase 5 — Disease-Specific Aging Signatures (DAS)
### Goal: Identify disease patterns in aging space

### Tasks:
- Compute mean and variance of PAG per disease
- Cluster disease groups based on aging behavior
- Measure similarity between diseases

### Outputs:
- Disease clustering visualization
- Signature profiles per disease
- Distance matrix between diseases

---

# Phase 6 — Regional Pulmonary Aging Maps (RPAM)
### Goal: Localize disease effects in lungs

### Tasks:
- Divide X-ray into 6 regions:
  - Upper L/R
  - Middle L/R
  - Lower L/R
- Estimate regional age contributions
- Generate heatmaps per image

### Outputs:
- RPAM heatmaps
- Region-wise aging analysis
- Disease localization patterns

---

# Phase 7 — Cross-Dataset Validation (Advanced)
### Goal: Test generalization

### Tasks:
- Train on NIH dataset
- Test on external dataset (CheXpert or similar)
- Compare performance degradation

### Outputs:
- Cross-dataset performance report
- Robustness analysis

---

# Phase 8 — Paper + Science Fair Submission
### Goal: Final presentation

### Tasks:
- Write full research paper
- Create figures:
  - Model architecture diagram
  - PAG plots
  - RPAM heatmaps
  - Disease signature clustering
- Prepare science fair poster

### Outputs:
- Final paper (IEEE/ISEF style)
- GitHub repository
- Presentation slides/poster

---

## 4. Key Deliverables

### Core Scientific Contributions:
1. **Pulmonary Age Gap (PAG)** — continuous lung health metric  
2. **Disease-Specific Aging Signatures (DAS)** — disease pattern representation  
3. **Regional Pulmonary Aging Maps (RPAM)** — spatial disease localization  

---

## 5. Success Criteria

APEX is successful if it demonstrates:

- Lung age can be predicted from chest X-rays with reasonable error
- Disease increases Pulmonary Age Gap significantly
- Different diseases show distinct aging signatures
- Spatial patterns correspond to known lung pathology locations

---

## 6. Tools & Stack

- Python
- PyTorch / TensorFlow (recommended PyTorch)
- Pandas / NumPy
- Matplotlib / Seaborn
- Google Colab or local GPU
- NIH ChestX-ray14 dataset

---

## 7. Timeline (Suggested)

### Week 1–2:
- Dataset finalization
- Healthy cohort construction

### Week 3:
- Train lung age model

### Week 4:
- PAG + initial analysis

### Week 5:
- DAS + RPAM prototypes

### Week 6:
- Paper + science fair prep

---

## 8. Notes

APEX is not just a classification system. It is a framework for interpreting lung disease through **biological aging dynamics**, enabling more explainable and structured medical AI analysis.
