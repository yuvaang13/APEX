# APEX v2
## AI-Powered Estimation of Pulmonary Extended Aging for Early Detection of Lung Disease

---

## Overview

APEX v2 is a deep learning framework designed to estimate pulmonary biological age directly from chest radiographs. The model learns age-related pulmonary patterns from healthy chest X-rays and generates a predicted pulmonary age for each patient.

The core metric introduced by APEX is the **Pulmonary Age Gap (PAG)**:

\[
PAG = Predicted Pulmonary Age - Chronological Age
\]

A positive PAG may indicate accelerated pulmonary aging, while a negative PAG may indicate a younger-than-expected pulmonary profile.

---

## Motivation

Many lung diseases develop gradually over time and may not be easily identifiable during their earliest stages. Rather than directly classifying disease, APEX approaches the problem through biological aging.

The central hypothesis is:

> Patients with pulmonary abnormalities exhibit accelerated pulmonary aging that can be quantified using chest radiographs.

By first learning normal aging patterns from healthy individuals, APEX establishes a baseline from which abnormal aging signatures can be detected.

---

## Dataset

### Source

NIH ChestX-ray14 Dataset

### Training Population

Only healthy patients labeled:

```text
No Finding
```

were included in model development.

### Dataset Processing Pipeline

1. Load NIH metadata.
2. Index all available chest X-ray images.
3. Match metadata records to image files.
4. Filter for healthy patients only.
5. Filter ages between 18 and 90 years.
6. Create train, validation, and test splits.

### Final Dataset Statistics

| Split | Samples |
|---------|---------:|
| Train | 17,086 |
| Validation | 3,661 |
| Test | 3,662 |
| Total Healthy Samples | 24,409 |

### Available Images

NIH image archives used:

```text
images_001
images_002
images_003
images_004
images_005
```

Total indexed images:

```text
44,999
```

---

## Model Architecture

### Backbone

DenseNet121

### Initialization

ImageNet pretrained weights

### Input

```text
224 × 224 RGB chest radiograph
```

### Output

```text
Predicted Pulmonary Age (years)
```

### Loss Function

Mean Absolute Error (L1 Loss)

### Optimizer

AdamW

Learning Rate:

```text
1e-4
```

### Training Duration

```text
25 epochs
```

### Hardware

Kaggle GPU Environment

---

## APEX Framework

```text
Chest X-Ray
      ↓
DenseNet121
      ↓
Predicted Pulmonary Age
      ↓
Compare Against Actual Age
      ↓
Pulmonary Age Gap (PAG)
      ↓
Potential Indicator of Accelerated Pulmonary Aging
```

---

## Results

### Test Performance

| Metric | Value |
|---------|---------:|
| Test MAE | 4.42 Years |
| PAG Mean | +1.45 Years |
| PAG Standard Deviation | 5.52 Years |

### Interpretation

APEX v2 achieved a mean absolute error of approximately 4.42 years on a held-out test set of healthy chest radiographs.

The model demonstrated the ability to learn age-related pulmonary features directly from imaging data and generate biologically meaningful pulmonary age estimates.

---

## Predicted vs Actual Age

<img width="771" height="552" alt="Screenshot 2026-06-24 at 3 29 01 PM" src="https://github.com/user-attachments/assets/fa14a3ad-93e4-42a3-8180-20e9e0577a16" />


---

## Pulmonary Age Gap (PAG)

APEX introduces the Pulmonary Age Gap as a quantitative biomarker:

\[
PAG = PredictedAge - ActualAge
\]

Examples:

| Actual Age | Predicted Age | PAG |
|------------|--------------|-----|
| 50 | 51 | +1 |
| 50 | 62 | +12 |
| 50 | 45 | -5 |

Potential interpretation:

- Positive PAG → accelerated pulmonary aging
- Near-zero PAG → expected pulmonary aging
- Negative PAG → younger pulmonary profile

---

## Future Work

### APEX v2.1

Evaluate PAG across pulmonary disease cohorts:

- Fibrosis
- Emphysema
- Atelectasis
- Effusion
- Infiltration
- Mass
- Nodule
- Consolidation
- Pneumonia

### APEX v3

Develop disease-specific pulmonary aging signatures and risk prediction models.

### APEX Clinical Vision

Use pulmonary age estimation as an early screening biomarker for detecting subclinical lung disease before significant symptoms emerge.

---

## Folder Structure

```text
APEXv2/
├── splits/
│   ├── train.csv
│   ├── val.csv
│   └── test.csv
│
├── raw_image_archives/
│   ├── images_001/
│   ├── images_002/
│   ├── images_003/
│   ├── images_004/
│   └── images_005/
│
├── notebooks/
├── models/
└── results/
```

---

## Citation

```text
APEX v2: AI-Powered Estimation of Pulmonary Extended Aging
for Early Detection of Lung Disease.

Developed by Yuvaan G.
```
