# Data Collection

## Project Overview

This project uses the **NIH ChestX-ray14 Dataset** to develop and evaluate **APEX (AI-Powered Pulmonary EXtended Aging Framework)**, a framework designed to model pulmonary aging from chest radiographs and investigate its relationship to lung disease.

The initial phase of the project focuses on learning normal pulmonary aging patterns using healthy chest X-rays before expanding to disease-specific analyses.

---

## Dataset

### NIH ChestX-ray14

**Source:** National Institutes of Health (NIH)

**Dataset Size:**
- 112,120 frontal chest X-ray images
- 30,805 unique patients

**Metadata Includes:**
- Patient age
- Patient sex
- Disease labels
- Image filename

### Downloaded Components

To avoid excessive storage usage during development, only a subset of the dataset was downloaded initially.

Downloaded archives:

- images_001.tar.gz
- images_002.tar.gz
- images_003.tar.gz
- images_004.tar.gz

Metadata downloaded:

- Data_Entry_2017.csv

These files provide a representative subset of the full dataset while allowing rapid experimentation and model development.

---

## Directory Structure

```text
APEX/
├── data/
│   ├── raw_archives/
│   │   ├── images_001.tar.gz
│   │   ├── images_002.tar.gz
│   │   ├── images_003.tar.gz
│   │   └── images_004.tar.gz
│   │
│   ├── extracted/
│   │
│   ├── metadata/
│   │   └── Data_Entry_2017.csv
│   │
│   └── subsets/
│
├── notebooks/
├── src/
└── models/
```

---

## Data Collection Objectives

The data collection phase aims to:

1. Obtain chest radiographs with associated patient age information.
2. Identify healthy ("No Finding") images for pulmonary age modeling.
3. Create manageable subsets for efficient experimentation.
4. Establish a foundation for later disease-specific analysis.

---

## Planned Dataset Filtering

### Healthy Aging Cohort

For training the initial pulmonary aging model, images will be filtered using:

```text
Finding Labels == "No Finding"
```

This cohort will be used to learn normal pulmonary aging trajectories.

### Future Disease Cohorts

Later experiments will investigate conditions including:

- Pneumonia
- Fibrosis
- Emphysema
- Infiltration
- Atelectasis

These cohorts will be used to evaluate:

- Pulmonary Age Gap (PAG)
- Disease-Specific Aging Signatures (DAS)
- Regional Pulmonary Aging Maps (RPAM)

---

## Data Storage Strategy

Because the complete NIH dataset exceeds 45 GB, a staged collection strategy is used:

### Phase 1
- Download a limited number of archives.
- Extract only necessary files.
- Build a healthy aging subset.

### Phase 2
- Expand dataset size if needed.
- Incorporate disease-specific cases.

### Phase 3
- Perform cross-dataset validation using external datasets such as CheXpert.

This approach reduces storage requirements while enabling iterative development.

---

## Current Status

### Completed

- [x] Created project directory structure
- [x] Downloaded NIH metadata
- [x] Downloaded image archives 001–004

### In Progress

- [ ] Extract image archives
- [ ] Analyze metadata distribution
- [ ] Create healthy aging subset
- [ ] Build training dataset

### Upcoming

- [ ] Train healthy pulmonary aging model
- [ ] Compute Pulmonary Age Gap (PAG)
- [ ] Generate Disease-Specific Aging Signatures (DAS)
- [ ] Develop Regional Pulmonary Aging Maps (RPAM)

---

## Notes

The goal of this phase is not to use the entire NIH ChestX-ray14 dataset immediately, but rather to establish a reproducible and scalable data pipeline that supports future experimentation and model development for the APEX framework.
