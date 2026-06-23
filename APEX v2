# APEX v2: A Deep Learning Pipeline for Pulmonary Age Estimation and Dataset-Driven Biological Age Gap Modeling

## Abstract

APEX v2 is a deep learning pipeline designed to estimate pulmonary age from chest X-ray images using a supervised regression model trained on the NIH ChestX-ray14 dataset. The system is built as a reproducible end-to-end workflow that includes dataset construction, preprocessing, training, and evaluation. A key component of APEX v2 is the computation of the Pulmonary Age Gap (PAG), defined as the difference between predicted age and chronological age. This document describes the full system architecture, dataset pipeline, and training methodology used in APEX v2.

---

## 1. Introduction

APEX v2 is a machine learning system designed to learn age-related structure in chest X-ray images. The system is built as a modular pipeline consisting of dataset construction, image indexing, supervised regression training, and evaluation.

The goal of APEX v2 is not classification or diagnosis, but continuous age estimation from imaging data, followed by analysis of prediction deviation.

---

## 2. System Overview

APEX v2 is composed of four main components:

1. Dataset Construction Pipeline  
2. Image Indexing and Path Mapping System  
3. Deep Learning Regression Model  
4. Evaluation and Pulmonary Age Gap Computation

Each component is designed to be reproducible across local (Mac) and cloud (Google Colab) environments.

---

## 3. Dataset Construction

### 3.1 Source Data

APEX v2 uses the NIH ChestX-ray14 dataset, consisting of multiple image archives (001–005) and a metadata CSV file containing image labels and patient ages.

### 3.2 Preprocessing Steps

The dataset construction pipeline performs the following operations:

- Extraction of image archives into structured folders
- Mapping of image filenames to absolute file paths
- Joining image paths with metadata entries
- Removal of missing or unmatched records
- Filtering of “No Finding” (healthy) cases
- Age filtering to retain values between 18 and 90

### 3.3 Final Dataset Size

After preprocessing, the dataset is split into:

- Training set
- Validation set
- Test set

All splits are derived exclusively from the filtered healthy cohort.

---

## 4. Data Splitting Strategy

The dataset is divided using a randomized stratification strategy:

- 70% Training
- 15% Validation
- 15% Testing

Splits are performed on the cleaned dataset after filtering and validation.

Each sample in the dataset includes:

- Image Index
- Patient Age
- Full image file path

---

## 5. Model Architecture

APEX v2 uses DenseNet121 as the base convolutional neural network.

### 5.1 Modifications

The original classification head is replaced with a regression layer:

- Input: 1024 features (DenseNet embedding)
- Output: 1 scalar (predicted age)

### 5.2 Loss Function

The model is trained using Mean Absolute Error (MAE):

MAE = |y_pred − y_true|

---

## 6. Training Procedure

The model is trained using the following configuration:

- Optimizer: AdamW
- Learning Rate: 1e-4
- Batch Size: 32
- Input Resolution: 224 × 224
- Epochs: configurable (typically 20–40)

Training is performed on GPU when available.

---

## 7. Pulmonary Age Gap (PAG)

APEX v2 introduces a derived metric:

PAG = Predicted Age − Actual Age

PAG is computed for each sample in the test set.

### 7.1 Interpretation

- PAG > 0 → model predicts older pulmonary appearance
- PAG < 0 → model predicts younger pulmonary appearance
- PAG ≈ 0 → alignment with expected age pattern

---

## 8. Evaluation

Model performance is evaluated using:

- Mean Absolute Error (MAE)
- Prediction vs Actual correlation
- Distribution of PAG values

Evaluation is performed on a held-out test set that was not used during training or validation.

---

## 9. Implementation Details

APEX v2 is implemented using:

- Python
- PyTorch
- Pandas
- Scikit-learn
- Google Colab (training environment)
- Local preprocessing environment (MacOS)

The system is designed for reproducibility across local and cloud environments using standardized CSV-based dataset indexing.

---

## 10. Limitations

- Dataset limited to NIH ChestX-ray14
- Age labels may contain noise
- Model does not incorporate clinical metadata beyond age
- Single architecture (DenseNet121) used for baseline results

---

## 11. Conclusion

APEX v2 is a structured machine learning pipeline for pulmonary age estimation using chest X-ray imaging. The system integrates dataset construction, deep learning regression, and a derived metric (PAG) into a unified workflow. It is designed as a reproducible framework for future extensions in medical imaging-based age modeling.

---

## 12. Future Improvements

Planned extensions to APEX v2 include:

- Multi-dataset training support
- Architecture comparison (ResNet, EfficientNet)
- Uncertainty estimation in age predictions
- Improved stratified sampling for age distribution balancing
