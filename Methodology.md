# Methodology

## Overview

This project introduces **APEX (AI-Powered Pulmonary EXtended Aging Framework)**, a computational framework designed to model biological lung aging from chest radiographs and analyze how lung disease alters normal aging patterns.

Unlike traditional approaches that directly classify diseases, APEX reframes the problem as a continuous aging and deviation modeling task, enabling more interpretable and structured analysis of pulmonary health.

The system is built on four core components:

1. Healthy Pulmonary Aging Model  
2. Pulmonary Age Gap (PAG)  
3. Disease-Specific Aging Signatures (DAS)  
4. Regional Pulmonary Aging Maps (RPAM)

---

## 1. Dataset and Preprocessing

### Dataset Source
The primary dataset used is the NIH ChestX-ray14 dataset, which contains:

- 112,120 frontal chest X-rays  
- 30,805 patients  
- Multi-label disease annotations  
- Patient age metadata  

### Data Subset Strategy

Due to dataset size constraints, a subset is used:

- ~24,999 images extracted from selected archives  
- Metadata from `Data_Entry_2017.csv`  
- Filtering based on available image files on disk  

### Healthy Cohort Selection

For learning normal pulmonary aging patterns:

Only images satisfying this condition are used in the initial aging model.

### Image Matching

Since metadata includes more records than available images, only entries with matching image files on disk are retained.

---

## 2. Image Processing

All chest X-ray images are preprocessed before model training:

- Converted to grayscale (if required)  
- Resized to 224 × 224 pixels  
- Normalized pixel intensity values  
- Standard tensor formatting for CNN input  

No manual feature engineering is applied, as feature extraction is learned directly by the neural network.

---

## 3. Healthy Pulmonary Aging Model

### Objective

To learn a mapping from chest X-ray images to biological lung age using only healthy patients.

### Model Input

### Model Output

### Model Type

A convolutional neural network (CNN), initially based on architectures such as:

- ResNet18 (pretrained on ImageNet)  
- or similar lightweight CNN backbone  

### Training Objective

The model minimizes prediction error between:

- Predicted lung age  
- Chronological patient age  

Loss function:

---

## 4. Pulmonary Age Gap (PAG)

### Definition

The Pulmonary Age Gap quantifies deviation from expected lung aging:

### Interpretation

- PAG ≈ 0 → normal aging pattern  
- PAG > 0 → accelerated aging (possible pathology)  
- PAG < 0 → slower-than-expected aging  

### Purpose

PAG serves as a continuous biomarker of lung health rather than a categorical disease label.

---

## 5. Disease-Specific Aging Signatures (DAS)

### Concept

Different lung diseases may produce distinct patterns of accelerated or localized aging.

Instead of treating diseases as labels, APEX models them as distributional shifts in aging behavior.

### Method

For each disease group:

1. Compute distribution of Pulmonary Age Gaps  
2. Compare across diseases  
3. Identify characteristic aging patterns  

This produces a disease-level “signature” in aging space.

---

## 6. Regional Pulmonary Aging Maps (RPAM)

### Objective

To localize aging effects within different anatomical regions of the lung.

### Method

Each chest X-ray is divided into regions:

- Upper Left  
- Upper Right  
- Middle Left  
- Middle Right  
- Lower Left  
- Lower Right  

For each region:

A spatial heatmap is constructed to visualize localized aging.

### Output

A 2D heatmap representing spatial distribution of accelerated aging and disease localization patterns.

---

## 7. Experimental Design

### Experiment 1: Healthy Aging Learning

- Train model only on healthy images  
- Evaluate prediction error against chronological age  

Metrics:
- MAE  
- R² score  

---

### Experiment 2: Pulmonary Age Gap Analysis

- Compare PAG across:
  - Healthy  
  - Pneumonia  
  - Fibrosis  
  - COPD-related cases  

Hypothesis:
> Diseased lungs exhibit significantly higher PAG values.

---

### Experiment 3: Disease Signature Analysis

- Compute PAG distributions per disease class  
- Perform clustering analysis  
- Measure similarity between disease aging profiles  

Goal:
> Identify whether diseases form distinct aging signatures.

---

### Experiment 4: Regional Aging Analysis

- Generate RPAM heatmaps  
- Compare spatial aging distribution across diseases  

Goal:
> Identify localized vs diffuse aging patterns.

---

## 8. Evaluation Metrics

The framework is evaluated using:

- Mean Absolute Error (MAE)  
- Root Mean Squared Error (RMSE)  
- Correlation between predicted and actual age  
- Statistical significance of PAG differences  
- Clustering separability for DAS  
- Spatial consistency of RPAM outputs  

---

## 9. Summary

APEX reframes chest X-ray analysis from a classification problem into a biological aging modeling framework. By learning normal pulmonary aging and measuring deviations from it, the system enables:

- Continuous lung health measurement (PAG)  
- Disease pattern characterization (DAS)  
- Spatial localization of pathology (RPAM)  

This approach provides a more interpretable and clinically meaningful alternative to traditional diagnostic AI systems.
