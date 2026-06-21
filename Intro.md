# APEX: An AI-Powered Pulmonary EXtended Aging Framework for Disease Characterization Using Chest Radiographs

## Introduction

Lung disease remains one of the leading causes of morbidity and mortality worldwide, with conditions such as pneumonia, chronic obstructive pulmonary disease (COPD), and pulmonary fibrosis often progressing silently before clinical detection. Traditional diagnostic methods for chest radiographs rely on classification models that predict the presence or absence of disease. While effective, these approaches provide limited insight into *how* disease alters lung structure and physiology.

Recent advances in deep learning for medical imaging have demonstrated that neural networks can estimate biological age from radiographs across multiple organ systems. These findings suggest that imaging data contains latent information about tissue aging and degeneration beyond what is captured by standard diagnostic labels. However, few approaches have explored whether such "biological aging signals" can be used to characterize disease progression in a clinically meaningful and interpretable way.

To address this gap, we propose **APEX (Pulmonary EXtended Aging Framework)**, a novel AI system that models lung aging patterns from chest X-rays and uses deviations from expected aging trajectories to detect and characterize disease. Instead of directly predicting disease labels, APEX learns a representation of *normal pulmonary aging* and quantifies how disease alters this trajectory.

## Core Idea

The central hypothesis of this work is that:

> Lung diseases manifest as measurable deviations from normal pulmonary aging patterns in chest radiographs.

Based on this hypothesis, APEX introduces three key concepts:

### 1. Pulmonary Age Gap (PAG)
We define the Pulmonary Age Gap as:

This metric quantifies whether a patient's lungs appear biologically older or younger than expected. Positive values indicate accelerated aging, which may correspond to underlying pathology.

### 2. Disease-Specific Aging Signatures (DAS)
Different diseases are hypothesized to affect lung tissue in distinct ways. Rather than treating disease as a categorical label, APEX models disease as a unique aging pattern distribution. These patterns, or "signatures," capture how each condition modifies pulmonary aging behavior.

### 3. Regional Pulmonary Aging Maps (RPAM)
To improve interpretability, APEX divides the lungs into anatomical regions and estimates regional aging patterns. This produces a spatial heatmap of aging acceleration across the lungs, enabling localization of abnormal regions associated with disease.

## Research Objectives

This study aims to answer the following questions:

1. Can a deep learning model learn meaningful representations of normal lung aging from chest X-rays?
2. Do lung diseases systematically alter predicted biological lung age?
3. Do different diseases produce distinct pulmonary aging signatures?
4. Can regional aging maps localize disease-relevant abnormalities?

## Significance

Unlike traditional diagnostic models that output a single disease label, APEX provides a continuous, interpretable measure of lung health. By framing disease as accelerated or altered biological aging, this approach introduces a new perspective for understanding pulmonary pathology.

If successful, this framework could contribute:
- A novel biomarker for lung health (Pulmonary Age Gap)
- A method for disease characterization beyond classification
- An interpretable spatial mapping of disease impact in chest radiographs

## Dataset (Initial Work)

Initial experiments will utilize the NIH ChestX-ray14 dataset. A subset of images with available demographic metadata will be used to train and evaluate the model, beginning with healthy (“No Finding”) cases to establish baseline pulmonary aging patterns.

## Conclusion

APEX reframes chest X-ray analysis from a classification problem into a biological aging modeling problem. By learning how lungs age under normal conditions and measuring deviations from this trajectory, we aim to develop a more interpretable and clinically meaningful framework for early disease detection and characterization.