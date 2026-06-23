# APEX v1 — Lung Age Prediction Baseline Model

## Overview

APEX v1 is the first working implementation of the APEX (AI-powered Pulmonary EXtended Aging) framework. This version focuses on building a baseline deep learning model that predicts biological lung age from chest X-ray images using a convolutional neural network (ResNet18).

The goal of v1 is to establish a measurable baseline relationship between chest X-ray features and chronological age, which will later be extended into pulmonary aging metrics (PAG, DAS, RPAM).

## Objective

Train a deep learning model to predict patient age from chest X-ray images and evaluate how accurately pulmonary imaging features correlate with chronological age.

This serves as the foundation for:

- Pulmonary Age Gap (PAG)
- Disease-Specific Aging Signatures (DAS)
- Regional Pulmonary Aging Maps (RPAM)

## Dataset

### Source
NIH ChestX-ray14 dataset (sample subset)

### Size
- ~3,000 chest X-ray images
- Paired with metadata:
  - Patient Age
  - Image Index
  - Finding Labels (not used in v1 training)

### Structure

sample_images/
    00000001_000.png
    00000002_001.png
    ...

subsets/
    lung_age_sample.csv

### Preprocessing
- Images resized to 224×224
- Normalized to [0, 1]
- Train/test split: 80/20
- Random sampling used for subset creation

## Model Architecture

### Backbone
- ResNet18 (pretrained on ImageNet)

### Modification
- Final fully connected layer replaced with:
  nn.Linear(in_features, 1)

### Output
- Single scalar: predicted age (years)

## Training Configuration

- Loss Function: Mean Absolute Error (L1 Loss)
- Optimizer: Adam
- Learning Rate: 1e-4
- Epochs: 3
- Batch Size: 16
- Hardware: Google Colab GPU

## Evaluation Metric

Mean Absolute Error (MAE):

MAE = (1/n) * Σ |predicted_age − actual_age|

This measures the average prediction error in years.

## Results (APEX v1)

Test MAE: 20.624169057210285

### Interpretation

The model’s predictions are on average off by ~20.6 years. This indicates:

- Chest X-rays contain measurable age-related structure
- The model is learning signal, but with high noise
- Performance is limited by:
  - Small dataset subset
  - Mixed disease states
  - Lack of lung-specific filtering
  - No regional feature modeling

## Model Output Example

Input: Chest X-ray image  
Output: Predicted age (years)

Example:
Input → Chest X-ray  
Output → 54.3 years

## Saved Artifacts

apex_model_v1.pth

Stored in:
- Google Drive
- Local download (optional backup)

## Limitations

- Small dataset (~3,000 images)
- No disease separation
- No spatial localization
- High MAE (~20.6 years)
- Mixed pathology reduces signal quality
- Not yet a true “lung age” model

## Scientific Significance

APEX v1 demonstrates that:

- Chest X-rays contain quantifiable age-related features
- CNNs can extract biological aging signals from imaging
- A baseline for pulmonary aging prediction is feasible

## Next Steps (APEX v2)

### 1. Pulmonary Age Gap (PAG)
PAG = Predicted Lung Age − Chronological Age

### 2. Disease-Specific Aging Signatures (DAS)
- Pneumonia
- COPD
- Fibrosis
- Healthy comparison

### 3. Regional Pulmonary Aging Maps (RPAM)
- Lung segmentation
- Regional aging heatmaps

### 4. Full Dataset Scaling
- Expand from 3,000 → 100,000+ images

## Conclusion

APEX v1 establishes a baseline deep learning model for pulmonary age prediction using chest X-rays. While accuracy is limited (MAE ~20.6 years), it provides the foundational step toward modeling biological lung aging and disease-specific pulmonary deterioration.
