# APEX Methodology

---

# 1. Overview

APEX models pulmonary disease as a deviation from normal biological lung aging learned from chest radiographs. The pipeline is split into three stages: (1) lung age prediction, (2) pulmonary age deviation analysis, and (3) disease pattern discovery in aging space.

---

# 2. Dataset Construction

## 2.1 Data Source
- NIH ChestX-ray14 dataset
- Frontal chest radiographs with metadata (age, sex, disease labels)

## 2.2 Cleaning & Filtering
- Match metadata entries to valid image files
- Remove missing/corrupted samples
- Retain only samples with valid age labels

Result: 24,999 verified images

## 2.3 Cohort Definition
- Healthy cohort: “No Finding”
- Disease cohorts: remaining labeled conditions

## 2.4 Splitting Strategy
- Train/Validation/Test = 70/15/15
- Patient-level separation to prevent leakage

---

# 3. Preprocessing

- Resize images to fixed resolution
- Normalize pixel intensities
- Convert to tensor format
- Apply augmentation only on training set:
  - small rotations
  - flips
  - brightness/contrast shifts

---

# 4. Lung Age Prediction Model (APEXv2)

## 4.1 Task Formulation
Regression problem:
- Input: chest X-ray
- Output: predicted biological lung age

## 4.2 Architecture
- CNN-based backbone (EfficientNet/ResNet variant)
- Pretrained on ImageNet
- Regression head for age output

## 4.3 Training Setup
- Loss: Mean Absolute Error (MAE)
- Optimizer: Adam
- Learning rate scheduling
- Early stopping based on validation MAE

## 4.4 Model Selection
- Best checkpoint selected using lowest validation MAE

## 4.5 Performance
- Test MAE: ~4 years

---

# 5. Pulmonary Age Gap (PAG)

## 5.1 Definition
PAG = Predicted Lung Age − Chronological Age

## 5.2 Computation Pipeline
For each sample:
1. Run inference with APEXv2
2. Subtract ground-truth age
3. Store result per patient and diagnosis group

---

# 6. Statistical Analysis of PAG

## 6.1 Group Comparisons
- Healthy vs disease groups
- Disease-to-disease comparisons

## 6.2 Metrics
- Mean / median PAG
- Standard deviation
- Effect size (Cohen’s d)

## 6.3 Testing
- t-test or Mann–Whitney U (based on distribution)
- ANOVA for multi-class comparison

---

# 7. Disease Representation in Aging Space (DAS)

## 7.1 Feature Extraction
- Extract embeddings from intermediate CNN layers of APEXv2

## 7.2 Projection
- PCA for variance structure
- UMAP/t-SNE for visualization

## 7.3 Analysis
- Cluster diseases in embedding space
- Compute inter-disease distances
- Identify separation patterns

---

# 8. Baseline Comparison

## 8.1 Models
- Standard CNN disease classifier
- Logistic regression baseline

## 8.2 Evaluation
- Accuracy
- Precision / Recall
- ROC-AUC

## 8.3 Comparison Goal
Assess whether aging-based representation (PAG + embeddings) encodes disease information comparably to direct classification.

---

# 9. Reliability Analysis

- Error vs age
- Error vs sex
- Error vs disease group
- Residual distribution analysis

---

# 10. RPAM (Optional Extension)

## 10.1 Method
- Partition lung region into anatomical zones
- Attribute contribution to age prediction using saliency/attention methods

## 10.2 Output
- Regional heatmaps of aging intensity

---

# 11. Cross-Dataset Evaluation

- Train on NIH ChestXray14
- Test on external datasets (CheXpert / MIMIC-CXR)
- Measure MAE degradation and distribution shift
