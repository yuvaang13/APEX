# APEX Project Plan
## AI-Powered Pulmonary EXtended Aging Framework for Early Lung Disease Characterization and Detection

---

# 1. Project Overview

APEX (AI-Powered Pulmonary EXtended Aging Framework) is a medical AI research project that reframes chest X-ray analysis as a biological aging problem rather than a traditional disease classification problem.

Instead of directly predicting disease labels, APEX learns normal pulmonary aging patterns from chest radiographs and measures how disease alters those patterns. The central hypothesis is that lung diseases accelerate or distort biological aging signatures that can be quantified and analyzed through deep learning.

The framework introduces three primary research contributions:

1. **Pulmonary Age Gap (PAG)** — a continuous biomarker representing the difference between predicted biological lung age and chronological age.
2. **Disease-Specific Aging Signatures (DAS)** — characteristic aging patterns associated with different pulmonary diseases.
3. **Regional Pulmonary Aging Maps (RPAM)** — interpretable spatial representations of localized pulmonary aging acceleration.

---

# 2. Research Question

> Can biological lung age estimated from chest radiographs be used to detect, characterize, and interpret pulmonary disease more effectively than traditional classification-based approaches?

---

# 3. Core Hypothesis

The APEX framework is based on the hypothesis that:

> Pulmonary diseases manifest as measurable deviations from normal biological lung aging patterns.

If this hypothesis is correct, diseased patients should exhibit significantly different Pulmonary Age Gaps compared to healthy individuals, and different diseases should produce distinct aging signatures.

---

# 4. Project Status

Current Development Stage:

✅ Dataset Construction Completed

✅ Baseline Model Completed

✅ APEXv1 Completed

✅ APEXv2 Completed

✅ Pulmonary Age Prediction Achieved

✅ Test MAE Reduced from 20.62 Years (APEXv1) to ~4 Years (APEXv2)

🔄 PAG Analysis In Progress

🔄 Disease Characterization In Progress

⬜ RPAM Development Not Started

⬜ Cross-Dataset Validation Not Started

⬜ Research Paper Not Started

---

# 5. Project Phases

## Phase 1 — Data Engineering
### Status: COMPLETED

### Objective
Construct a clean and verified NIH ChestX-ray14 dataset for modeling.

### Completed Tasks

- [x] Download NIH ChestX-ray14 metadata
- [x] Extract image archives
- [x] Build metadata-image mapping pipeline
- [x] Verify dataset integrity
- [x] Match 24,999 valid chest radiographs
- [x] Create image availability index

### Deliverables

- `available_images.csv`
- Metadata-image lookup system
- Verified image dataset

---

## Phase 2 — Dataset Construction
### Status: COMPLETED

### Objective
Create a modeling-ready healthy cohort for pulmonary aging prediction.

### Completed Tasks

- [x] Identify healthy ("No Finding") patients
- [x] Extract age labels
- [x] Remove invalid records
- [x] Create train/validation/test splits
- [x] Analyze age distributions

### Deliverables

- `healthy_train.csv`
- `healthy_val.csv`
- `healthy_test.csv`
- Dataset statistics

---

## Phase 3 — APEXv1 Baseline Lung Age Model
### Status: COMPLETED

### Objective
Establish the first pulmonary age prediction baseline.

### Architecture

- CNN-based age predictor
- Chest X-ray input
- Age regression output

### Results

- Test MAE: 20.62 Years

### Outcome

Successfully demonstrated feasibility of pulmonary age estimation but required significant optimization.

### Deliverables

- APEXv1 model
- Training pipeline
- Initial evaluation framework

---

## Phase 4 — APEXv2 Enhanced Pulmonary Aging Model
### Status: COMPLETED

### Objective
Improve biological lung age prediction performance.

### Improvements

- Enhanced preprocessing
- Improved training strategy
- Optimized architecture
- Improved data utilization
- Better generalization performance

### Results

- Test MAE ≈ 4 Years

### Significance

This result demonstrates that biological lung age can be estimated with substantially greater accuracy and provides a strong foundation for downstream disease characterization.

### Deliverables

- APEXv2 model
- Final age prediction pipeline
- Performance benchmarks
- Predicted vs. actual age analysis

---

## Phase 5 — Pulmonary Age Gap (PAG) Validation
### Status: COMPLETED

### Objective

Determine whether disease significantly alters biological lung age.

### Definition

PAG = Predicted Lung Age − Chronological Age

### Research Questions

- Does PAG differ significantly between healthy and diseased patients?
- Which diseases exhibit the largest PAG deviations?
- Is PAG a useful biomarker of pulmonary health after accounting for model calibration?

### Tasks

### Tasks

- [x] Compute PAG for all patients
- [x] Compute PAG for healthy cohort
- [x] Compute PAG for disease cohorts
- [x] Generate PAG distribution plots
- [x] Perform significance testing
- [x] Calculate effect sizes
- [x] Rank diseases by PAG

### Deliverables

- PAG dataset
- PAG boxplots
- Statistical analysis report
- Disease comparison tables

---                  

## Phase 6 — PAG Calibration and Validation
### Status: COMPLETED

### Objective

Evaluate and calibrate Pulmonary Age Gap to ensure that observed differences reflect pulmonary aging rather than systematic prediction bias.

### Motivation

Phase 5 demonstrated a statistically significant difference in PAG between healthy and diseased cohorts. However, the model underpredicted chronological age by approximately 3.3 years on average, and the diseased cohort was older than the healthy cohort. This phase aims to quantify and reduce these effects.

### Tasks

- [x] Quantify systematic prediction bias
- [x] Perform age-stratified PAG analysis
- [x] Evaluate calibration across age groups
- [x] Apply regression-based calibration if appropriate
- [x] Recompute PAG after calibration
- [x] Compare calibrated and uncalibrated results

### Deliverables

- Calibration curves
- Age-stratified PAG analysis
- Bias report
- Calibrated PAG dataset

## Phase 7 — Disease-Specific Aging Signatures (DAS)
### Status: PLANNED

### Objective

Discover unique aging signatures associated with different diseases.

### Methodology

Instead of using disease labels directly, APEX will analyze latent aging representations learned by the age prediction model.

### Tasks

- [ ] Extract learned feature embeddings
- [ ] Perform PCA analysis
- [ ] Perform UMAP visualization
- [ ] Cluster disease groups
- [ ] Compute disease similarity matrix
- [ ] Identify unique disease aging signatures

### Deliverables

- Embedding visualizations
- Disease clusters
- Similarity heatmaps
- Disease signature profiles

---

## Phase 8 — Baseline Disease Classification Comparison
### Status: PLANNED

### Objective

Compare the APEX framework against traditional disease classification systems.

### Tasks

- [ ] Train baseline disease classifier
- [ ] Train ResNet classifier
- [ ] Compare against PAG-based methods
- [ ] Evaluate interpretability differences

### Metrics

- Accuracy
- Precision
- Recall
- F1 Score
- ROC-AUC

### Deliverables

- Comparative performance report
- ROC curves
- Classification benchmarks

---

## Phase 9 — Model Reliability and Bias Analysis
### Status: PLANNED

### Objective

Evaluate robustness and fairness of pulmonary age estimation.

### Tasks

- [ ] Analyze error by age group
- [ ] Analyze error by sex
- [ ] Analyze error by disease type
- [ ] Identify potential biases
- [ ] Evaluate uncertainty patterns

### Deliverables

- Reliability report
- Bias analysis figures
- Error breakdown charts

---

## Phase 10 — Regional Pulmonary Aging Maps (RPAM)
### Status: PLANNED

### Objective

Localize biological aging acceleration within the lungs.

### Tasks

- [ ] Divide lungs into anatomical regions
- [ ] Estimate regional age contributions
- [ ] Generate aging heatmaps
- [ ] Analyze disease localization patterns

### Deliverables

- RPAM heatmaps
- Region-specific aging metrics
- Localization analysis

---

---

## Phase 11 — APEXv3 Unified Pulmonary Foundation Model
### Status: PLANNED

### Objective

Develop the next-generation APEX model by integrating all validated components of the framework into a single unified deep learning system.

Unlike previous versions, which focused on individual research objectives (lung age prediction, Pulmonary Age Gap, Disease-Specific Aging Signatures, and Regional Pulmonary Aging Maps), APEXv3 will jointly learn these representations through a shared backbone trained on the largest available collection of chest radiographs.

The goal is to create a comprehensive pulmonary foundation model capable of simultaneously estimating biological lung age, characterizing disease-specific aging behavior, localizing abnormal pulmonary aging, and producing interpretable biomarkers within a single inference pipeline.

### Planned Improvements

- Train using the maximum available NIH ChestX-ray14 dataset
- Incorporate healthy and diseased cohorts during training
- Improve biological lung age estimation accuracy
- Learn disease-aware pulmonary aging representations
- Integrate Pulmonary Age Gap (PAG) prediction
- Integrate Disease-Specific Aging Signatures (DAS)
- Integrate Regional Pulmonary Aging Maps (RPAM)
- Improve feature representation through larger-scale training
- Optimize model robustness and generalization
- Develop an end-to-end pulmonary aging framework

### Proposed Architecture

```
Chest X-ray
      ↓
Shared DenseNet Backbone
      ↓
Shared Latent Pulmonary Representation
      ├──────────────► Lung Age Prediction
      ├──────────────► Pulmonary Age Gap (PAG)
      ├──────────────► Disease-Specific Aging Signatures (DAS)
      ├──────────────► Regional Pulmonary Aging Maps (RPAM)
      └──────────────► Explainability & Visualization
```

### Research Goals

- Improve pulmonary age estimation beyond APEXv2
- Increase separation between healthy and diseased pulmonary aging
- Learn richer latent pulmonary representations
- Improve disease characterization without relying solely on classification
- Produce a unified pulmonary aging framework suitable for downstream research and clinical validation

### Deliverables

- APEXv3 unified model
- End-to-end inference pipeline
- Multi-task training framework
- Comprehensive performance evaluation
- Unified pulmonary aging benchmark
- Final GitHub release

## Phase 12 — Cross-Dataset Validation
### Status: PLANNED

### Objective

Evaluate model generalization across institutions and datasets.

### Tasks

- [ ] Train using NIH ChestX-ray14
- [ ] Evaluate on external datasets
- [ ] Compare performance degradation
- [ ] Measure robustness

### Candidate Datasets

- CheXpert
- MIMIC-CXR
- PadChest

### Deliverables

- Generalization report
- Cross-dataset benchmarks

---

## Phase 13 — Research Paper and Science Fair Submission
### Status: PLANNED

### Objective

Prepare final scientific publication and competition materials.

### Tasks

- [ ] Write research paper
- [ ] Create architecture diagrams
- [ ] Generate publication-quality figures
- [ ] Prepare poster
- [ ] Prepare presentation

### Deliverables

- Research paper
- Science fair poster
- Presentation slides
- GitHub repository

---

# 14. Novel Contributions

The APEX framework seeks to contribute:

### Contribution 1 — Pulmonary Age Gap (PAG)

A continuous biological lung health biomarker derived from chest radiographs.

### Contribution 2 — Disease-Specific Aging Signatures (DAS)

A representation of disease through biological aging behavior rather than categorical classification.

### Contribution 3 — Regional Pulmonary Aging Maps (RPAM)

Spatial visualization of disease-related aging acceleration.

### Contribution 4 — Aging-Based Disease Characterization Framework

A new paradigm for pulmonary disease analysis based on biological aging dynamics.

---

# 15. Success Criteria

## Primary Success Metrics

- [x] Pulmonary age prediction MAE ≤ 5 years
- [ ] Significant PAG difference between healthy and diseased cohorts
- [ ] Reproducible disease-specific aging patterns

## Secondary Success Metrics

- [ ] Distinct disease clustering in latent space
- [ ] Improved interpretability compared to traditional classifiers

## Exploratory Success Metrics

- [ ] Successful RPAM localization
- [ ] Cross-dataset generalization

---

# 16. Technology Stack

- Python
- PyTorch
- NumPy
- Pandas
- Matplotlib
- Scikit-Learn
- NIH ChestX-ray14
- Google Colab
- GitHub

---

# 17. Current Project Summary

APEX has successfully progressed from an initial proof-of-concept pulmonary age prediction model (APEXv1, MAE = 20.62 years) to a substantially improved biological lung age estimation system (APEXv2, MAE ≈ 4 years). The project has now entered the scientific validation stage, where the primary objective is to determine whether deviations in predicted lung age can serve as meaningful biomarkers of pulmonary disease.

The next major milestone is validating the Pulmonary Age Gap (PAG) hypothesis and establishing whether biological lung aging patterns can reveal disease-specific characteristics that are not captured by traditional classification systems.
