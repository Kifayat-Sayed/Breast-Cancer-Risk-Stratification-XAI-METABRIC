# 🩺 Explainable Artificial Intelligence for Clinical Risk Stratification in Breast Cancer Patients Using the METABRIC Dataset

> **An Explainable AI (XAI) framework for multiclass breast cancer risk stratification using clinicopathological and genomic data from the METABRIC dataset.**

![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)
![Machine Learning](https://img.shields.io/badge/Machine%20Learning-Supervised-green)
![XAI](https://img.shields.io/badge/Explainable-AI-orange)
![SHAP](https://img.shields.io/badge/SHAP-Explainability-red)
![License](https://img.shields.io/badge/License-MIT-brightgreen)

---

# 📖 Project Overview

Breast cancer remains one of the leading causes of cancer-related mortality worldwide. Early identification of patients at different levels of clinical risk is essential for selecting appropriate treatment strategies and improving patient outcomes.

This project presents an **Explainable Artificial Intelligence (XAI)** framework for predicting **breast cancer risk categories** using the **METABRIC (Molecular Taxonomy of Breast Cancer International Consortium)** dataset.

Instead of providing only predictions, the framework explains **why** a patient is classified into a particular risk group, increasing transparency and supporting clinical decision-making.

The developed model predicts patients into one of three clinically meaningful risk categories:

* 🟢 Low Risk
* 🟡 Intermediate Risk
* 🔴 High Risk

---

# 🎯 Project Objectives

The primary objectives of this research are to:

* Develop an accurate multiclass breast cancer risk prediction model.
* Compare the performance of multiple supervised machine learning algorithms.
* Identify the most influential clinicopathological and genomic biomarkers.
* Apply Explainable AI techniques to improve model transparency.
* Interpret individual and global model predictions using SHAP.
* Support clinicians with trustworthy and interpretable AI-driven risk assessment.

---

# 📊 Dataset

## METABRIC Dataset

**METABRIC (Molecular Taxonomy of Breast Cancer International Consortium)** is one of the largest publicly available breast cancer datasets, containing comprehensive clinical and genomic information.

**Dataset Source**

https://www.cbioportal.org/study/summary?id=brca_metabric

### Dataset Statistics

| Description                  | Value                     |
| ---------------------------- | ------------------------- |
| Original Patients            | **2509**                  |
| Patients after Preprocessing | **1981**                  |
| Dataset Type                 | Clinical + Genomic        |
| Prediction Task              | Multiclass Classification |

---

# 🧬 Features Used

The model utilizes a combination of:

* Clinical variables
* Demographic information
* Tumor characteristics
* Gene expression features
* Molecular subtype information
* Clinicopathological biomarkers

---

# 🤖 Machine Learning Models

The following supervised learning algorithms were evaluated and compared:

| Model               | Purpose                     |
| ------------------- | --------------------------- |
| Logistic Regression | Baseline linear classifier  |
| Random Forest       | Ensemble learning           |
| XGBoost             | Gradient Boosting           |
| LightGBM            | Efficient Gradient Boosting |
| CatBoost            | Categorical Boosting        |

Each model was trained, validated, and compared using identical evaluation metrics.

---

# 🔍 Explainable Artificial Intelligence (XAI)

To improve interpretability, several Explainable AI techniques were applied.

### Feature Importance

* Random Forest Feature Importance
* XGBoost Feature Importance

### SHAP (SHapley Additive exPlanations)

The project includes:

* SHAP Summary Plot
* SHAP Waterfall Plot
* SHAP Force Plot
* Global Feature Importance
* Local Prediction Explanations

These visualizations explain how individual features contribute to each prediction, making the model more transparent for healthcare applications.

---

# 📈 Model Evaluation

Performance was evaluated using standard multiclass classification metrics:

* Accuracy
* Precision
* Recall
* F1 Score
* Confusion Matrix

These metrics provide a comprehensive assessment of predictive performance across all risk categories.

---

# 📂 Repository Structure

```text
├── data/
│   ├── raw/
│   └── processed/
│
├── notebooks/
│   ├── Data_Preprocessing.ipynb
│   ├── Model_Training.ipynb
│   ├── Model_Evaluation.ipynb
│   └── SHAP_Analysis.ipynb
│
├── results/
│   ├── Figures/
│   ├── Confusion_Matrices/
│   ├── Feature_Importance/
│   └── SHAP_Plots/
│
├── images/
│   ├── Workflow.png
│   └── Model_Architecture.png
│
├── reports/
│   ├── Synopsis.pdf
│   └── Dissertation.pdf
│
├── references/
│   └── Research_Papers/
│
├── README.md
└── requirements.txt
```

---

# ⚙️ Project Workflow

```text
METABRIC Dataset
        │
        ▼
Data Cleaning & Preprocessing
        │
        ▼
Feature Engineering
        │
        ▼
Train-Test Split
        │
        ▼
Machine Learning Models
(Logistic Regression, RF, XGBoost,
LightGBM, CatBoost)
        │
        ▼
Performance Evaluation
        │
        ▼
Explainable AI (SHAP)
        │
        ▼
Clinical Risk Interpretation
```

---

# 🚀 Technologies Used

* Python
* Pandas
* NumPy
* Scikit-learn
* XGBoost
* LightGBM
* CatBoost
* SHAP
* Matplotlib
* Seaborn
* Jupyter Notebook

---

# 📊 Expected Outputs

The project generates:

* Trained machine learning models
* Classification performance reports
* Confusion matrices
* Feature importance rankings
* SHAP explanation plots
* Risk prediction visualizations

---

# 🎓 Research Significance

This work demonstrates how Explainable AI can bridge the gap between predictive performance and clinical interpretability.

By combining high-performing machine learning models with SHAP explanations, the framework enables clinicians to understand **which clinical and genomic features drive each prediction**, increasing trust in AI-assisted decision-making.

Potential applications include:

* Clinical decision support
* Personalized medicine
* Risk stratification
* Treatment planning
* Breast cancer prognosis research

---

# 👨‍💻 Author

**Kifayat Sayed**

**MSc Artificial Intelligence and Machine Learning**

**Walsh College**

2026

---

# ⭐ Acknowledgements

* Molecular Taxonomy of Breast Cancer International Consortium (METABRIC)
* cBioPortal for Cancer Genomics
* SHAP Development Team
* Scikit-learn
* XGBoost
* LightGBM
* CatBoost

---

## ⭐ Support

If you found this project helpful, please consider **starring ⭐ the repository** and sharing it with others interested in **Artificial Intelligence**, **Machine Learning**, **Healthcare Analytics**, and **Explainable AI**.
