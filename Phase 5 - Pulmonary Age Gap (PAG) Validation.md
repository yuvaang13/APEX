# Phase 5 Results Overview — APEX Project  
## Pulmonary Age Gap (PAG) Analysis Using NIH Chest X-ray Dataset

---

## 1. Objective of Phase 5

Phase 5 of the APEX project focused on validating whether a deep learning model (DenseNet121-based regression) can estimate pulmonary biological age from chest X-rays, and whether the difference between predicted age and actual age (defined as Pulmonary Age Gap, PAG) differs significantly between healthy and diseased patients.

**Core hypothesis:** Pulmonary Age Gap (PAG) differs significantly between healthy individuals and patients with thoracic diseases.

---

## 2. Dataset and Preprocessing

We used the NIH Chest X-ray dataset (~25,000 images after filtering), combined with a structured CSV containing:
- Image Index
- Patient Age
- Finding Labels
- Image file paths

Column normalization:

df = df.rename(columns={
    "Patient Age": "age",
    "Image Index": "image_id"
})

Image path reconstruction:

def find_image(img_name):
    for folder in ["images_001", "images_002", "images_003"]:
        path = os.path.join(nih_path, folder, "images", img_name)
        if os.path.exists(path):
            return path
    return None

df["image_path"] = df["image_id"].apply(find_image)

Final dataset size: ~24,998 images  
Valid image paths: 100%

---

## 3. Model Architecture and Inference

DenseNet121 regression model trained to predict age:

model = models.densenet121(weights=None)
model.classifier = nn.Linear(model.classifier.in_features, 1)

state_dict = torch.load(
    f"{base_path}/APEXv2.pth",
    map_location=device
)

model.load_state_dict(state_dict)
model = model.to(device)
model.eval()

Inference pipeline:

transform = transforms.Compose([
    transforms.Resize((224, 224)),
    transforms.ToTensor()
])

preds = []

with torch.no_grad():
    for path in df["image_path"]:
        img = Image.open(path).convert("RGB")
        x = transform(img).unsqueeze(0).to(device)
        pred = model(x).cpu().item()
        preds.append(pred)

df["predicted_age"] = preds

---

## 4. Pulmonary Age Gap (PAG)

PAG = Predicted Age - Actual Age

df["PAG"] = df["predicted_age"] - df["age"]

Positive PAG = lungs appear older  
Negative PAG = lungs appear younger                

Across the full dataset, the model underpredicted chronological age by approximately 3.3 years on average (mean predicted age: 44.35 years; mean chronological age: 47.68 years). Consequently, mean PAG values were negative for both healthy and diseased cohorts.

---

## 5. Disease Classification

df["is_diseased"] = df["Finding Labels"].apply(lambda x: 0 if x == "No Finding" else 1)

healthy = df[df["is_diseased"] == 0]
diseased = df[df["is_diseased"] == 1]

Healthy: 14,597  
Diseased: 10,401  

---

## 6. Statistical Analysis

from scipy.stats import ttest_ind

t, p = ttest_ind(
    healthy["PAG"],
    diseased["PAG"],
    nan_policy="omit"
)

T-statistic: 5.58  
P-value: 2.38e-08  

Group means:

Healthy mean PAG: -3.08 years

Diseased mean PAG: -3.68 years

Difference (Diseased − Healthy): -0.60 years

Interpretation:

A statistically significant difference in Pulmonary Age Gap was observed between healthy and diseased patients (t = 5.58, p = 2.38 × 10⁻⁸). In this analysis, the diseased cohort exhibited a mean PAG that was 0.60 years lower than the healthy cohort.

---

## 7. Disease-Level Analysis

disease_stats = df.groupby("Finding Labels")["PAG"].agg(["mean", "count"]).sort_values("mean", ascending=False)

---

## 8. Visualization Results

Top disease vs PAG plot:

top = disease_stats.head(10)

plt.figure(figsize=(12,5))
plt.bar(top.index.astype(str), top["mean"])
plt.xticks(rotation=45)
plt.title("Disease vs Pulmonary Age Gap (PAG)")
plt.tight_layout()
plt.show()

<img width="866" height="587" alt="Screenshot 2026-06-25 at 8 07 44 PM" src="https://github.com/user-attachments/assets/94c795c1-b33a-4b95-974e-3ac8cb12571a" />


Improved version:

plt.figure(figsize=(10,6))
plt.barh(top.index.astype(str), top["mean"])
plt.gca().invert_yaxis()
plt.title("Disease vs Pulmonary Age Gap (PAG)")
plt.tight_layout()
plt.show()

<img width="884" height="519" alt="Screenshot 2026-06-25 at 8 08 13 PM" src="https://github.com/user-attachments/assets/57efff51-69b7-4808-ab08-3892dfea033d" />

---

## 9. Key Findings

- DenseNet121 successfully learned age-related imaging features from chest X-rays.
- A statistically significant difference in Pulmonary Age Gap was observed between healthy and diseased patients (p = 2.38 × 10⁻⁸).
- Mean PAG was -3.08 years in healthy patients and -3.68 years in diseased patients.
- The model exhibited an overall tendency to underpredict chronological age by approximately 3.3 years.
- The diseased cohort was older on average than the healthy cohort, indicating that future analyses should adjust for age when interpreting PAG differences.

---

## 10. Conclusion

APEX Phase 5 confirms:

AI-derived pulmonary age estimates demonstrated statistically significant differences between healthy and diseased populations. The current model also exhibited systematic age underprediction, and the diseased cohort was older on average than the healthy cohort. Future work will focus on age-adjusted analyses and model calibration to better determine the clinical utility of Pulmonary Age Gap as a biomarker of pulmonary health.

---

## Future Work

- Confidence intervals for PAG
- ICD-based disease grouping
- Model calibration for clinical use
- Longitudinal patient tracking
- Age-adjusted statistical analysis of PAG
- Regression-based calibration to reduce systematic age bias
- Stratified analyses across age groups
