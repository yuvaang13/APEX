# APEX v2.5 — AI-Powered Estimation of Pulmonary Extended Aging for Early Detection of Lung Disease

# Abstract
APEX v2.5 is a deep learning framework designed to estimate pulmonary biological age from chest radiographs using a DenseNet121-based regression architecture trained on the NIH ChestX-ray14 dataset. Instead of performing direct disease classification, the system models pulmonary health as a continuous biological aging process. The key output is the Pulmonary Age Gap (PAG), defined as the difference between predicted pulmonary age and chronological age. APEX v2.5 additionally learns a high-dimensional 1024-feature embedding space that encodes latent pulmonary structure for downstream representation learning, clustering, and disease separation analysis. The framework is designed to support both quantitative biomarker extraction and unsupervised discovery of disease structure in latent space.

<img width="581" height="375" alt="Screenshot 2026-06-30 at 10 52 40 PM" src="https://github.com/user-attachments/assets/acd31120-7730-4671-908b-28461e0eb317" />


---

# 1. Introduction
Pulmonary diseases such as pneumonia, fibrosis, emphysema, pleural effusion, consolidation, and nodular abnormalities evolve gradually over time and often exhibit subtle radiographic changes that are not easily captured by traditional classification-based machine learning systems. These models typically rely on discrete labels and fail to represent the continuous nature of disease progression. APEX v2.5 addresses this limitation by reframing pulmonary disease analysis as a continuous aging estimation problem.

The central hypothesis is that healthy lungs follow a smooth, low-dimensional biological aging manifold within radiographic feature space. Disease states introduce structured deviations from this manifold, manifesting as accelerated or distorted pulmonary aging patterns. By learning this manifold through regression on chronological age, APEX enables disease characterization through deviation analysis rather than classification.

This formulation provides three key advantages: (1) continuous biomarkers instead of binary outputs, (2) emergent disease structure in embedding space without explicit supervision, and (3) improved interpretability through a biologically meaningful scalar metric (PAG).

---

# 2. Dataset Construction

The dataset used in APEX v2.5 is derived from the NIH ChestX-ray14 dataset, which contains approximately 75,000 frontal chest radiographs collected from multiple clinical institutions. Each sample includes a chest X-ray image, patient identifier, age annotation, and multi-label disease tags.

## 2.1 Data Filtering
The dataset undergoes strict preprocessing to ensure consistency and reliability. Samples are removed if:
- the image file is missing or corrupted
- the age metadata is absent or invalid
- the file path cannot be resolved across image folders (images_001 to images_012)

After filtering, the dataset consists of a cleaned subset suitable for supervised regression learning.

## 2.2 Cohort Definition
Two cohorts are defined:

- Healthy cohort: images labeled “No Finding”
- Diseased cohort: all remaining diagnostic labels

The healthy cohort is exclusively used for learning baseline pulmonary aging behavior, while diseased samples are reserved for evaluation and downstream analysis.

## 2.3 Dataset Split Strategy
Splitting is performed at the patient level to prevent data leakage:

- Training set: 70%
- Validation set: 15%
- Test set: 15%

This ensures that no patient appears in multiple splits, preserving generalization validity.

---

# 3. Preprocessing Pipeline

Each chest X-ray is processed using a standardized pipeline:

1. Image loading from PNG format
2. Conversion to RGB channels (grayscale replicated to 3 channels if needed)
3. Resizing to 224 × 224 pixels
4. Normalization using ImageNet statistics:
   - mean = (0.485, 0.456, 0.406)
   - std = (0.229, 0.224, 0.225)
5. Conversion to PyTorch tensor

## 3.1 Data Augmentation (Training Only)
To improve robustness and generalization, augmentation is applied during training:
- Random horizontal flip (p = 0.5)
- Random rotation (±5 degrees)
- Brightness jitter (±10%)
- Contrast jitter (±10%)

These augmentations simulate variation across imaging devices, hospital environments, and acquisition protocols while preserving anatomical structure.

---

# 4. Model Architecture (APEX v2.5)

APEX v2.5 uses DenseNet121 pretrained on ImageNet as the backbone feature extractor. DenseNet is selected due to dense connectivity, improved gradient flow, and strong performance in medical imaging tasks.

## 4.1 Feature Extraction
The backbone produces a 1024-dimensional embedding through global average pooling over convolutional feature maps:

Input → DenseNet121 → Feature Maps → Global Average Pooling → 1024-D Embedding

This embedding represents latent pulmonary structure including texture, vascular patterns, anatomical variation, and disease-related distortions learned implicitly through age regression.

## 4.2 Regression Head
The embedding is passed through a fully connected regression head:

1024 → 512 → ReLU → Dropout(0.3) → 1

This design balances model capacity and regularization:
- 512 hidden units provide nonlinear transformation capacity
- ReLU introduces non-linearity
- Dropout prevents overfitting
- Final layer outputs scalar pulmonary age

## 4.3 Loss Function
The model is trained using Mean Absolute Error (MAE):

L = |y_true − y_pred|

MAE is chosen for robustness to noisy age labels and stability during optimization.

## 4.4 Optimization
Optimization uses AdamW with:
- learning rate = 1e-4
- weight decay = 1e-4
- ReduceLROnPlateau scheduling based on validation loss

---

# 5. Pulmonary Age Gap (PAG)

PAG is defined as:

PAG = predicted pulmonary age − chronological age

Interpretation:
- PAG > 0 → accelerated pulmonary aging
- PAG ≈ 0 → normal pulmonary aging
- PAG < 0 → younger-than-expected pulmonary structure

PAG acts as a continuous biomarker of pulmonary health and is hypothesized to correlate with disease burden.

---

# 6. Training Procedure

Training proceeds as follows:

1. Load batch of chest X-rays
2. Forward pass through DenseNet121 backbone
3. Extract 1024-dimensional embedding
4. Pass embedding through regression head
5. Compute MAE loss against chronological age
6. Backpropagation using AdamW
7. Update model parameters
8. Validate on held-out validation set

Best model selection is based on lowest validation MAE.

---

# 7. Outputs

APEX v2.5 produces three outputs:
- Predicted pulmonary age (scalar)
- Pulmonary Age Gap (PAG)
- 1024-dimensional embedding vector

---

# 8. Results

## 8.1 Training Dynamics
During training, the model is expected to show rapid initial convergence followed by gradual stabilization of validation loss, indicating successful learning of pulmonary aging structure.

<img width="588" height="440" alt="Screenshot 2026-06-30 at 10 51 57 PM" src="https://github.com/user-attachments/assets/9009a63d-451e-4e2a-aa94-da2fc9187247" />


## 8.2 Pulmonary Age Gap Distribution
The PAG distribution is expected to approximate a centered distribution in healthy cohorts with increased variance in diseased cohorts.

---

# 9. Model Checkpointing
The best-performing model is saved as:

APEXv2.5_best.pth

This checkpoint includes:
- model weights
- optimizer state
- epoch number
- validation loss

---

# 10. Phase 7 Embedding Utility

The 1024-dimensional embeddings form the basis for Phase 7 analysis and are used for:
- PCA dimensionality reduction
- UMAP manifold visualization
- clustering of disease phenotypes
- cosine similarity analysis
- centroid-based disease separation

These embeddings enable unsupervised discovery of disease structure in pulmonary aging space.

---

# 11. Discussion

APEX v2.5 reframes pulmonary disease analysis as continuous representation learning over a biological aging manifold. Instead of discrete classification, it enables emergent structure discovery in latent space. The dual-output design (PAG + embeddings) provides both interpretable scalar biomarkers and high-dimensional structural representations.

---

# 12. Limitations

Current limitations include:
- dataset bias across imaging institutions
- noisy and discretized age labels
- absence of longitudinal patient tracking
- single imaging modality (X-ray only)
- no explicit disease supervision during training
- limited external validation at training stage

---

# 13. Conclusion

APEX v2.5 provides a unified framework for pulmonary aging estimation and latent representation learning. It produces a clinically meaningful biomarker (PAG), a structured embedding space, and a scalable foundation for Phase 7 disease clustering and manifold analysis.

---

# 14. Future Work

Future directions include Phase 7 embedding clustering, external validation on CheXpert and MIMIC-CXR datasets, cross-institution calibration of PAG distributions, self-supervised pretraining to improve representation robustness, and temporal modeling of pulmonary aging trajectories over time.

# 15. Notes
Best Val Loss: 4.3557022738894196               
Mean PAG: 0.08015361                 
Std PAG: 5.7669187                  
