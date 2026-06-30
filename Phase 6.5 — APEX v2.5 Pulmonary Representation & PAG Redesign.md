# APEX v2.5 — AI-Powered Estimation of Pulmonary Extended Aging for Early Detection of Lung Disease

---

# Abstract

APEX v2.5 is a deep learning framework designed to model pulmonary aging as a continuous biological signal extracted from chest radiographs. Instead of directly classifying disease presence, the system learns a regression mapping from chest X-rays to an estimated pulmonary biological age. This enables the derivation of a clinically interpretable biomarker: the Pulmonary Age Gap (PAG), defined as the difference between predicted pulmonary age and chronological age.

Unlike conventional classification-based radiology models, APEX v2.5 focuses on representation learning and aging dynamics. The model produces both (1) a scalar age prediction used to compute PAG and (2) a high-dimensional embedding (1024-D) representing latent pulmonary structure. This dual-output design enables downstream analysis including disease clustering, manifold learning, and visualization of pulmonary aging trajectories.

---

# 1. Introduction

Pulmonary diseases such as fibrosis, emphysema, pneumonia, and chronic obstructive conditions often develop gradually over long time horizons. In many cases, early pathological changes are subtle and not easily separable using binary classification models trained on radiographs.

APEX v2.5 reframes this problem by introducing the hypothesis that pulmonary disease manifests as a deviation from normal biological aging patterns observable in chest X-rays. Instead of learning discrete disease labels, the model learns a continuous aging function over pulmonary structure.

Formally, we assume:

- Healthy lungs follow a smooth aging manifold in imaging space
- Disease introduces systematic deviations from this manifold
- These deviations can be quantified via a learned age predictor

Thus, pulmonary disease detection becomes an **outlier detection problem in biological aging space**, rather than a direct classification task.

---

# 2. Dataset Construction

## 2.1 Data Source

The dataset used in APEX v2.5 is derived from the NIH ChestX-ray14 dataset, which contains chest radiographs collected from hospital systems with corresponding metadata.

Key properties:
- ~75,000 frontal chest X-rays
- 12 image archives (images_001 through images_012)
- Multi-label disease annotations
- Patient-level identifiers
- Age metadata (critical for regression task)

---

## 2.2 Data Filtering

To ensure consistency in biological age modeling, the dataset is filtered as follows:

### Inclusion criteria:
- Valid frontal chest X-ray image
- Available age metadata
- Successfully matched image file path

### Exclusion criteria:
- Missing or corrupted image files
- Missing age labels
- Unresolvable metadata entries

This yields a cleaned dataset suitable for supervised regression.

---

## 2.3 Cohort Definition

The dataset is implicitly divided into:

### Healthy cohort
- Label: “No Finding”
- Used to learn baseline pulmonary aging structure

### Diseased cohort
- All remaining diagnostic labels
- Used only in downstream evaluation (not training)

This ensures that the model learns **natural aging patterns only**, avoiding direct disease supervision bias.

---

## 2.4 Train / Validation / Test Split

Patient-level splitting is enforced to prevent data leakage.

| Split | Percentage | Purpose |
|------|------------|--------|
| Train | 70% | Model optimization |
| Validation | 15% | Hyperparameter tuning |
| Test | 15% | Final evaluation |

---

# 3. Preprocessing Pipeline

Each chest X-ray undergoes a standardized preprocessing pipeline:

1. Image loading (PNG format)
2. RGB conversion (even if grayscale source)
3. Resizing to 224 × 224
4. Intensity normalization using ImageNet statistics:
   - mean = [0.485, 0.456, 0.406]
   - std = [0.229, 0.224, 0.225]
5. Tensor conversion

### Data augmentation (training only):
- Random horizontal flip (p = 0.5)
- Small rotation (±5 degrees)
- Brightness/contrast jitter

These augmentations simulate inter-scanner variability while preserving anatomical structure.

---

# 4. Model Architecture (APEX v2.5)

## 4.1 Backbone Network

APEX v2.5 uses DenseNet121 pretrained on ImageNet as the feature extractor.

Rationale:
- Dense connectivity improves gradient flow
- Proven performance in medical imaging tasks
- Efficient feature reuse for radiographic structures

The backbone outputs a 1024-dimensional feature tensor.

---

## 4.2 Feature Embedding Layer

After DenseNet feature extraction:
