For the full methodology, model comparisons, and results, see **[Identification of Mutations in Cancer Gene.pdf](results/Identification of Mutations in Cancer Gene.pdf)**.

# Identification of Pathogenic Mutations in Cancer Genes

A machine learning–based framework for distinguishing **pathogenic (driver)** mutations from **benign (passenger)** variants using ClinVar genomic annotations, with a strong emphasis on **high recall** for cancer-associated mutations.

---

## 📌 Overview

Cancer is fundamentally driven by genetic mutations, but identifying *clinically relevant* mutations from sequencing data remains a major challenge. This project presents a **robust, interpretable, and reproducible ML pipeline** that classifies genetic variants as **benign or pathogenic**, prioritizing **diagnostic sensitivity (recall)** to minimize false negatives.

Rather than optimizing only for accuracy, this work focuses on identifying *as many true cancer-driving mutations as possible*, which is critical for precision medicine and downstream clinical decision-making.

*Here we are using ClinVar as a proxy dataset for cancer driver/passenger mutation prioritization. Cancer-specific cohorts (e.g., TCGA) are access-restricted. ClinVar is public and provides clinically curated labels. In this project, pathogenic variants are treated as the positive class and benign variants as the negative class.*

---

## 🎯 Objectives

- Develop a **supervised learning pipeline** for mutation pathogenicity classification
- Handle **severe class imbalance** inherent in clinical genomic datasets
- **Maximize recall** for pathogenic variants to reduce missed cancer drivers
- Analyze **feature importance** to identify key biological indicators
- Ensure **pipeline reproducibility** for future clinical or research use

---

## 🧬 Dataset

- **Source:** ClinVar (via Kaggle: `kevinarvai/clinvar-conflicting`)
- **Samples:** 65,188 genetic variants
- **Features:** 46 genomic annotations
- **Target Variable:**  
  - `0` → Benign (Passenger mutation)  
  - `1` → Pathogenic (Driver mutation)
- **Class Imbalance:** ~3:1 (Benign : Pathogenic)

---

## 🛠️ Methodology

### 1. Data Preprocessing
- Dropped features with **>99% missing values**
- Imputed remaining missing values using:
  - Linear interpolation (numerical features)
  - Forward-fill (categorical features)
  - Domain-specific imputation for LoFtool scores

### 2. Feature Engineering
- Applied **Box-Cox transformation** to allele frequencies:
  - `AF_ESP`, `AF_EXAC`, `AF_TGP`
- Encoded categorical variables using **Label Encoding**
- Selected biologically meaningful features using correlation analysis

### 3. Class Imbalance Handling
- Used **class-weight tuning** and **SMOTE**
- Adjusted decision thresholds using **Precision–Recall curves**

---

## 🤖 Models Implemented

| Model | Purpose |
|------|--------|
| Logistic Regression | Baseline linear classifier |
| Random Forest | Non-linear ensemble learning |
| XGBoost | Gradient boosting on structured genomic data |
| SVM | High-recall classifier |
| KNN | Distance-based baseline |
| Neural Network (PyTorch) | Deep learning comparison |

---

## 📊 Evaluation Metrics

Given the clinical importance of not missing cancer mutations, evaluation focused on:

- **Recall (Sensitivity)** ⭐ *Primary metric*
- F1-Score
- Confusion Matrix
- Precision–Recall Curve
- Accuracy (secondary metric)

---

## 📈 Key Results

- **Logistic Regression (Balanced):**
  - Recall (Pathogenic): **0.73**
- **Random Forest (Threshold-tuned):**
  - Recall improved from **0.20 → ~0.75**
- **SVM:**
  - Recall (Pathogenic): **0.77**
- **Neural Network:**
  - Recall (Pathogenic): **0.65**
- **XGBoost Regressor:**
  - R² = **0.945** (for cDNA position prediction)

👉 **Ensemble and margin-based models consistently outperformed baselines when recall was prioritized.**

---

## 🧠 Key Insights

- **Rare allele frequencies** are strong indicators of pathogenicity
- Functional impact scores (SIFT, PolyPhen, BLOSUM62) significantly improve predictions
- **Accuracy alone is misleading** in clinical datasets
- **Threshold tuning is critical** for real-world cancer detection systems

---

## 📁 Project Structure

```text
├── data/
│   └── clinvar.csv
├── models/
│   ├── machine_learning_models.py
│   └── notebook.ipynb
├── results/
│   └── identificatoin of Mutations in Cancer Gene.pdf
├── requirements.txt
└── README.md
```


## 🔬 Future Work

- Incorporate raw sequence embeddings (BERT / DNABERT)
- Explore multimodal learning with protein structure data
- Integrate clinical metadata for patient-level predictions
- Deploy as a clinical decision-support tool


## Authors

- Soham Tanaji Umbare
- Naumisharanya Tirth
- Shivam Yadav
- Anurag Kumar

Department of Computer Science & Engineering
Indian Institute of Information Technology Raichur

---

⭐ _If you find this work interesting, consider giving it a star on GitHub!_

---
🧑‍💻 Happy Experimenting! 🔬