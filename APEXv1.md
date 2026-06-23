# APEX v1 — Lung Age Prediction Baseline Model

## Overview

APEX v1 is the first working implementation of the APEX (AI-powered Pulmonary EXtended Aging) framework. This version focuses on building a baseline deep learning model that predicts **biological lung age** from chest X-ray images using a convolutional neural network.

The goal of v1 is not clinical diagnosis, but to establish a measurable foundation for pulmonary aging prediction.

---

## Objective

Train a deep learning model to predict a patient’s age from chest X-ray images and evaluate how accurately pulmonary visual features correlate with chronological age.

This serves as the foundation for future APEX modules:

- Pulmonary Age Gap (PAG)
- Disease-Specific Aging Signatures (DAS)
- Regional Pulmonary Aging Maps (RPAM)

---

## Dataset

### Source
NIH ChestX-ray14 dataset (subset used for v1)

### Size
- ~3,000 chest X-ray images (sample subset)
- Paired with metadata including:
  - Patient Age
  - Image Index
  - Finding Labels (not used in v1 training directly)

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
- Train/test split: 80/20 random split
- Age filtered to valid range (0–100)

---

## Model Architecture

### Backbone
- ResNet18 (pretrained on ImageNet)

### Modification
- Final fully connected layer replaced with:

```python
nn.Linear(in_features, 1)
