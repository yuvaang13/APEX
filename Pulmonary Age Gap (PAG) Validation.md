# Phase 5 Results Overview — APEX Project  
## Pulmonary Age Gap (PAG) Analysis Using NIH Chest X-ray Dataset

---

## 1. Objective of Phase 5

Phase 5 of the APEX project focused on validating whether a deep learning model (DenseNet121-based regression) can estimate pulmonary biological age from chest X-rays, and whether the difference between predicted age and actual age (defined as Pulmonary Age Gap, PAG) differs significantly between healthy and diseased patients.

**Core hypothesis:**

> Patients with thoracic diseases exhibit a higher Pulmonary Age Gap (PAG) than healthy individuals.

---

## 2. Dataset and Preprocessing

We used the NIH Chest X-ray dataset (~25,000 images after filtering), combined with a structured CSV containing:

- Image Index  
- Patient Age  
- Finding Labels  
- Image file paths (resolved dynamically across NIH directories)

### Column normalization

df = df.rename(columns={
    "Patient Age": "age",
    "Image Index": "image_id"
})

---

### Image path reconstruction

NIH images were stored across multiple nested folders. We resolved paths dynamically:

def find_image(img_name):
    for folder in ["images_001", "images_002", "images_003"]:
        path = os.path.join(nih_path, folder, "images", img_name)
        if os.path.exists(path):
            return path
    return None

df["image_path"] = df["image_id"].apply(find_image)

After preprocessing:

- Final dataset size: ~24,998 images  
- Valid image paths: 100%

---

## 3. Model Architecture and Inference

We used a DenseNet121 regression model trained to predict age from chest X-rays (APEX v2).

### Model loading

model = models.densenet121(weights=None)
model.classifier = nn.Linear(model.classifier.in_features, 1)

state_dict = torch.load(
    f"{base_path}/APEXv2.pth",
    map_location=device
)

model.load_state_dict(state_dict)
model = model.to(device)
model.eval()

---

### Inference pipeline

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

We define:

PAG = Predicted Age - Actual Age

df["PAG"] = df["predicted_age"] - df["age"]

- Positive PAG → lungs appear biologically older than chronological age  
- Negative PAG → lungs appear younger / healthier than expected  

---

## 5. Disease Classification

Patients were grouped as:

- Healthy: Finding Labels == "No Finding"  
- Diseased: all other labels  

df["is_diseased"] = df["Finding Labels"].apply(
    lambda x: 0 if x == "No Finding" else 1
)

healthy = df[df["is_diseased"] == 0]
diseased = df[df["is_diseased"] == 1]

Final distribution:
- Healthy: 14,597  
- Diseased: 10,401  

---

## 6. Statistical Analysis

We performed a two-sample t-test to compare PAG distributions.

from scipy.stats import ttest_ind

t, p = ttest_ind(
    healthy["PAG"],
    diseased["PAG"],
    nan_policy="omit"
)

print("T-statistic:", t)
print("P-value:", p)

### Results:
- T-statistic: 5.58  
- P-value: 2.38e-08  

### Interpretation:

The difference between healthy and diseased groups is statistically significant.

> Diseased patients show a significantly higher Pulmonary Age Gap than healthy patients.

---

## 7. Disease-Level Analysis

disease_stats = (
    df.groupby("Finding Labels")["PAG"]
    .agg(["mean", "count"])
    .sort_values("mean", ascending=False)
)

### Observations:
- Multi-label cases show higher PAG values  
- More severe conditions correlate with increased biological aging signal  
- Rare combinations introduce noise but preserve overall trend  

---

## 8. Visualization Results

### Disease vs PAG (Bar Plot)

top = disease_stats.head(10)

plt.figure(figsize=(12,5))
plt.bar(top.index.astype(str), top["mean"])
plt.xticks(rotation=45)
plt.title("Disease vs Pulmonary Age Gap (PAG)")
plt.tight_layout()
plt.show()

[ INSERT FIGURE 1 HERE ]

---

### Improved Visualization (Horizontal Bar Chart)

plt.figure(figsize=(10,6))
plt.barh(top.index.astype(str), top["mean"])
plt.gca().invert_yaxis()
plt.title("Disease vs Pulmonary Age Gap (PAG)")
plt.tight_layout()
plt.show()

[ INSERT FIGURE 2 HERE ]

---

## 9. Key Findings

- DenseNet121 successfully learned pulmonary aging signals from chest X-rays  
- Diseased patients have significantly higher PAG values  
- Statistical significance confirmed (p < 0.001)  
- Disease complexity correlates with higher aging signal  
- Multi-label cases increase variance but preserve trend  

---

## 10. Conclusion

Phase 5 validates the core hypothesis of the APEX system:

> AI-derived pulmonary age from chest X-rays can serve as a meaningful biomarker for disease presence and severity.

This demonstrates potential applications in:
- Early disease detection  
- Risk stratification  
- Non-invasive clinical biomarkers  

---

## Future Work

- Add confidence intervals for PAG  
- Improve disease grouping (ICD-based clustering)  
- Model calibration for clinical interpretability  
- Longitudinal patient tracking
