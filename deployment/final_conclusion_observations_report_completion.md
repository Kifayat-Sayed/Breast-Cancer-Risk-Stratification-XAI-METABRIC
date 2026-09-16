# Final Conclusion, Observations and Report Completion Guide

## Project

**Explainable Artificial Intelligence for Breast Cancer Risk Stratification Using Clinicopathological and Genomic Features: A METABRIC-Based Study**

---

# 1. Final Conclusion

This study developed an explainable machine-learning framework for breast cancer mortality-risk stratification using clinicopathological and genomic characteristics from the METABRIC dataset. The work was designed not only to evaluate predictive performance, but also to examine clinically relevant associations and provide interpretable explanations of model behaviour.

The original METABRIC cohort contained **2,509 patients**. An exploratory survival-based three-group analysis classified patients as High risk (≤60 months), Intermediate risk (61–180 months), and Low risk (>180 months). After removal of records without the survival information required for this analysis, **1,981 patients** remained: 491 High-risk, 963 Intermediate-risk, and 527 Low-risk patients.

For the primary machine-learning task, a more rigorous binary endpoint was defined as **death within 60 months**. Patients who died within 60 months were classified as events. Patients who died after 60 months or remained alive beyond 60 months were classified as non-events. Patients whose follow-up ended before or at 60 months without an observed death were excluded because their 60-month outcome could not be determined reliably. This produced **1,917 eligible patients**, consisting of **427 events and 1,490 non-events**, with an event prevalence of **22.27%**.

Twenty-three clinicopathological and genomic predictors were used. Data preprocessing was incorporated into the modelling pipelines to reduce information leakage. Numerical variables were median-imputed and standardised, while categorical variables were imputed with an explicit Unknown category and one-hot encoded. These transformations were fitted within the relevant training folds during cross-validation.

Five machine-learning approaches were evaluated using stratified nested cross-validation: **Logistic Regression, Random Forest, XGBoost, LightGBM and CatBoost**. The primary optimisation metric was the **F2-score**, reflecting the project's emphasis on identifying mortality events and therefore assigning greater importance to recall.

The models demonstrated different strengths. CatBoost achieved the highest mean F2-score (**0.6507**) and recall (**0.8900**), while Logistic Regression achieved the highest mean ROC-AUC (**0.7692**) and PR-AUC (**0.5122**). Random Forest achieved a mean F2-score of **0.6399**, ROC-AUC of **0.7600**, PR-AUC of **0.4846**, Brier score of **0.1873**, recall of **0.8550**, and precision of **0.3197**. Random Forest was therefore retained as the report-aligned final model for detailed evaluation and XAI analysis. This is a study-design choice and should not be presented as a claim that Random Forest was the numerical winner across all metrics.

Using pooled out-of-fold Random Forest predictions, an analytical operating threshold of **0.330** was selected using the F2-score. At this threshold, the model achieved an F2-score of **0.6490**, recall of **0.8548**, precision of **0.3306**, ROC-AUC of **0.7575**, PR-AUC of **0.4666**, and Brier score of **0.1873**. The resulting confusion matrix contained **365 true positives, 751 true negatives, 739 false positives and 62 false negatives**. The threshold therefore prioritised sensitivity to mortality events but also produced a substantial number of false-positive classifications.

The global SHAP analysis identified **Nottingham Prognostic Index, Tumor Size, Lymph Nodes Examined Positive, Age at Diagnosis, ER Status, PAM50/Claudin-low Subtype, PR Status, Integrative Cluster, 3-Gene Classifier Subtype and Chemotherapy** among the most influential original predictors. Patient-level SHAP analysis showed that individual predictions resulted from combinations of positive and negative feature contributions rather than from one variable alone.

The final deployment workflow further established technical reproducibility. The complete preprocessing and Random Forest pipeline was saved as a Joblib artifact, reloaded successfully, and reproduced identical probability predictions, with a maximum probability difference of **0.000000000000** during the integrity test. Deployment metadata, feature schema, threshold information, model configuration and performance summaries were also preserved.

Overall, the project demonstrates the feasibility of integrating statistical analysis, leakage-controlled machine learning, nested cross-validation, out-of-fold threshold analysis and explainable AI for breast cancer mortality-risk research. The final outcome should be described as a **reproducible research prototype**, not as a clinically validated diagnostic or treatment system.

---

# 2. Final Observations

## 2.1 Dataset and outcome

- Original cohort: **2,509**
- Eligible binary-classification cohort: **1,917**
- Excluded: **592**
- Events: **427**
- Non-events: **1,490**
- Event prevalence: **22.27%**
- Predictors used: **23**
- Transformed features: **80**

The outcome distribution demonstrates a moderately imbalanced classification problem, supporting the use of recall-sensitive metrics and F2 optimisation.

## 2.2 Statistical analysis

The exploratory statistical analysis identified multiple significant associations after Benjamini-Hochberg FDR correction. Particularly strong associations were observed for NPI, lymph-node involvement, tumour size, tumour stage, ER status, Integrative Cluster, PAM50/Claudin-low subtype and histologic grade.

Important results included:

| Predictor | Adjusted q-value | Effect/Association |
|---|---:|---:|
| Nottingham Prognostic Index | 4.05 × 10⁻⁴² | Rank-biserial = −0.438 |
| Lymph Nodes Examined Positive | 9.62 × 10⁻³¹ | Rank-biserial = −0.350 |
| Tumor Size | 1.54 × 10⁻²⁴ | Rank-biserial = −0.332 |
| Tumor Stage | 3.72 × 10⁻²⁴ | Rank-biserial = −0.340 |
| ER Status | 9.43 × 10⁻¹⁹ | Cramer's V = 0.206 |
| Integrative Cluster | 2.70 × 10⁻¹⁸ | Cramer's V = 0.242 |
| PAM50/Claudin-low Subtype | 7.42 × 10⁻¹⁷ | Cramer's V = 0.221 |
| Histologic Grade | 3.57 × 10⁻¹⁶ | Cramer's V = −0.239 |

These are statistical associations, not causal effects and not ML feature-selection criteria.

## 2.3 Five-model comparison

| Model | Mean F2 | ROC-AUC | PR-AUC | Brier | Recall | Precision |
|---|---:|---:|---:|---:|---:|---:|
| Logistic Regression | 0.6421 | 0.7692 | 0.5122 | 0.1952 | 0.8408 | 0.3307 |
| Random Forest | 0.6399 | 0.7600 | 0.4846 | 0.1873 | 0.8550 | 0.3197 |
| XGBoost | 0.6312 | 0.7436 | 0.4715 | 0.1569 | 0.8785 | 0.3003 |
| LightGBM | 0.6161 | 0.7289 | 0.4547 | 0.1729 | 0.7941 | 0.3277 |
| CatBoost | 0.6507 | 0.7545 | 0.4916 | 0.1504 | 0.8900 | 0.3143 |

The comparison shows that model performance depends on the metric. CatBoost had the highest mean F2 and recall; Logistic Regression had the highest ROC-AUC and PR-AUC; CatBoost had the lowest Brier score. Random Forest provided a competitive multi-metric profile and was retained for the detailed XAI stage.

## 2.4 Random Forest outer-fold stability

| Fold | F2 |
|---|---:|
| 1 | 0.6284 |
| 2 | 0.6294 |
| 3 | 0.6522 |
| 4 | 0.6079 |
| 5 | 0.6818 |
| **Mean** | **0.6399** |
| **Sample SD** | **0.0282** |

## 2.5 Pooled OOF operating point

At the analytical threshold of **0.330**:

- TP = **365**
- TN = **751**
- FP = **739**
- FN = **62**
- Recall = **0.8548**
- Precision = **0.3306**
- F2 = **0.6490**
- ROC-AUC = **0.7575**
- PR-AUC = **0.4666**
- Brier = **0.1873**

The threshold is **not clinically validated** and must not be described as a clinical decision threshold.

## 2.6 SHAP observations

The leading global predictors by mean absolute SHAP value were:

1. Nottingham Prognostic Index — 0.112239
2. Tumor Size — 0.034604
3. Lymph Nodes Examined Positive — 0.031376
4. Age at Diagnosis — 0.030659
5. ER Status — 0.024398
6. PAM50 + Claudin-low Subtype — 0.019350
7. PR Status — 0.017426
8. Integrative Cluster — 0.009921
9. 3-Gene Classifier Subtype — 0.009392
10. Chemotherapy — 0.007811

SHAP values explain the fitted model's behaviour; they do not establish causation.

---

# 3. What the Final Report Should Include

## Front Matter

1. Title page
2. GitHub repository link
3. Declaration/academic-integrity statement, if required
4. Acknowledgements
5. Abstract
6. Keywords
7. Table of Contents
8. List of Tables
9. List of Figures
10. List of Abbreviations

## Chapter 1 — Introduction

Include:

- Breast cancer background
- Clinical risk stratification problem
- Motivation for AI/ML
- Importance of explainability
- Problem statement
- Research gap
- Aim
- Objectives
- Research questions
- Scope
- Significance
- Report structure

### Suggested Research Questions

**RQ1:** Which clinicopathological and genomic characteristics are statistically associated with the survival-based risk outcome in the METABRIC cohort?

**RQ2:** How effectively can machine-learning models identify patients at risk of death within 60 months using clinicopathological and genomic predictors?

**RQ3:** Which predictors contribute most strongly to the Random Forest mortality-risk predictions, and how do these contributions vary at the individual-patient level?

## Chapter 2 — Literature Review

Cover:

- Breast cancer prognosis
- METABRIC and molecular subtyping
- Clinicopathological prognostic factors
- ML in breast cancer
- Class imbalance
- Cross-validation and leakage prevention
- Explainable AI
- SHAP
- Clinical deployment limitations
- Research gap

The review should synthesise and compare studies rather than simply list papers.

## Chapter 3 — Dataset and Methodology

Include:

- Dataset description
- Original cohort
- Variable types
- Missing data
- Exploratory three-group survival stratification
- Primary 60-month binary endpoint
- Inclusion/exclusion logic
- Leakage-prone variables removed
- 23 predictors
- Preprocessing
- Numerical and categorical transformations
- Statistical tests
- FDR correction
- Five ML algorithms
- Nested 5-fold outer / 3-fold inner CV
- F2 optimisation
- ROC-AUC, PR-AUC, recall, precision, Brier and other metrics
- SHAP methodology
- Patient-level explanations
- Deployment/reproducibility workflow

## Chapter 4 — Exploratory and Statistical Results

Include:

- Cohort flow
- Risk-group distribution
- Missing-data observations
- Statistical association table
- FDR-adjusted results
- Effect sizes
- Interpretation of major associations

## Chapter 5 — Machine-Learning Results

Include:

- Five-model comparison
- Nested-CV results
- Random Forest detailed results
- ROC curve
- PR curve
- Confusion matrix
- Calibration curve
- Outer-fold F2 stability
- OOF threshold
- Error analysis
- Discussion of false positives and false negatives

## Chapter 6 — Explainable AI Results

Include:

- Global SHAP importance
- Original-predictor aggregation
- SHAP directionality
- Important categorical levels
- Patient-level TP/TN/FN explanations
- Interpretation caveats

## Chapter 7 — Deployment and Reproducibility

Include:

- Saved model
- Preprocessing + RF pipeline
- Feature schema
- Deployment metadata
- Threshold file
- Manifest
- Reload integrity test
- Environment/version information
- Example predictions
- Research-prototype warning

## Chapter 8 — Discussion

Explicitly answer RQ1, RQ2 and RQ3 and connect results with the literature.

## Chapter 9 — Conclusion

Use the final conclusion from Section 1 or the polished conclusion in Section 13 below.

---

# 4. Recommended Figures

| Figure | Content |
|---|---|
| Figure 6.1 | Final Random Forest ROC curve |
| Figure 6.2 | Final Random Forest Precision-Recall curve |
| Figure 6.3 | Confusion matrix at OOF threshold 0.330 |
| Figure 6.4 | Calibration curve |
| Figure 6.5 | Outer-fold F2 stability |
| Figure 6.6 | Global SHAP importance |
| Figure 6.7 | Patient-level SHAP explanations |

### Figure 6.1 Caption

> **Figure 6.1. Receiver operating characteristic curve for the final Random Forest model.** The curve represents pooled out-of-fold predictions across the 1,917 eligible patients, with an ROC-AUC of 0.7575.

### Figure 6.2 Caption

> **Figure 6.2. Precision-recall curve for the final Random Forest model.** The curve represents pooled out-of-fold predictions, with a PR-AUC of 0.4666.

### Figure 6.3 Caption

> **Figure 6.3. Confusion matrix for the final Random Forest model at the pooled out-of-fold operating threshold of 0.330.** The model produced 365 true positives, 751 true negatives, 739 false positives and 62 false negatives.

### Figure 6.4 Caption

> **Figure 6.4. Calibration curve for the final Random Forest model.** The figure compares predicted mortality probabilities with observed outcome frequencies using pooled out-of-fold predictions.

### Figure 6.5 Caption

> **Figure 6.5. Outer-fold F2-score stability for the Random Forest model.** F2-scores across the five outer folds were 0.6284, 0.6294, 0.6522, 0.6079 and 0.6818, with a mean of 0.6399 and sample standard deviation of 0.0282.

### Figure 6.6 Caption

> **Figure 6.6. Global SHAP importance for the final Random Forest model.** Mean absolute SHAP values are aggregated to the original 23 predictors to show their overall contribution to model predictions.

### Figure 6.7 Caption

> **Figure 6.7. Patient-level SHAP explanations for representative true-positive, true-negative and false-negative cases.** The figure demonstrates how individual feature contributions combine to influence model output for different patients.

---

# 5. Recommended Tables

| Table | Content |
|---|---|
| Table 1 | Dataset and variable summary |
| Table 2 | Exploratory survival-risk group distribution |
| Table 3 | Statistical association results with FDR correction |
| Table 4 | Five-model nested-CV performance |
| Table 5 | Random Forest outer-fold performance |
| Table 6 | Pooled OOF operating-point metrics |
| Table 7 | Final Random Forest configuration |
| Table 8 | Global SHAP importance |
| Table 9 | Patient-level SHAP contributions |
| Table 10 | Deployment artefacts and reproducibility metadata |

Every table should be introduced in the text before it appears.

---

# 6. Limitations

The report should explicitly discuss:

1. **Single-dataset limitation:** The model was developed using METABRIC and may not generalise to other populations.
2. **No external validation:** Performance has not been independently validated on another cohort.
3. **Censoring and endpoint construction:** Patients with insufficient follow-up were excluded from the binary endpoint.
4. **Class imbalance:** The event prevalence was 22.27%.
5. **Threshold validation:** The 0.330 threshold is an analytical OOF operating point and is not clinically validated.
6. **Treatment variables:** Chemotherapy, hormone therapy, surgery and radiotherapy are included, so this is not strictly a pretreatment prognostic model.
7. **Association versus causation:** Statistical tests and SHAP do not establish causal effects.
8. **Retrospective dataset:** Historical treatment and data-collection practices may limit contemporary applicability.
9. **Deployment limitations:** The saved model is a research artifact and requires a compatible software environment.

---

# 7. Ethical and Clinical Considerations

Include:

- Patient privacy and de-identification
- Responsible use of clinical data
- Potential algorithmic bias
- False-positive and false-negative consequences
- Explainability requirements
- Human oversight
- No autonomous diagnosis
- No autonomous treatment recommendation
- Requirement for external and prospective validation

### Recommended statement

> The developed system is intended as a research and decision-support prototype for investigating mortality-risk modelling and model interpretability. It is not a clinically validated diagnostic or treatment system, and its predictions should not be used independently of qualified clinical judgement.

---

# 8. Future Work

Recommended future work:

1. External validation using an independent cohort.
2. Prospective validation in a clinical workflow.
3. Contemporary population validation.
4. External calibration assessment.
5. Decision-curve and clinical-utility analysis.
6. Survival-specific modelling that directly accounts for censoring.
7. Comparison with Cox models, Random Survival Forests and survival boosting.
8. Temporal validation.
9. Fairness and subgroup performance analysis.
10. Uncertainty quantification.
11. SHAP stability analysis across resamples.
12. Additional genomic integration.
13. Secure API deployment for research environments.
14. Human-in-the-loop clinical evaluation.

---

# 9. Final Project Contributions

The completed project provides:

- A METABRIC-based breast cancer mortality-risk framework.
- A clearly defined 60-month binary mortality endpoint.
- Leakage-controlled preprocessing.
- Statistical association analysis with FDR correction.
- Five-model machine-learning comparison.
- Nested cross-validation.
- F2-based optimisation.
- Out-of-fold threshold selection.
- Global SHAP interpretation.
- Patient-level SHAP explanations.
- Original-predictor SHAP aggregation.
- A saved reproducible model pipeline.
- Deployment metadata and feature schema.
- Model reload integrity verification.
- A research-oriented deployment workflow with explicit limitations.

---

# 10. Recommended Appendices

## Appendix A — Data Dictionary

Original variable name, description, type, analytical role, transformation and missing-value treatment.

## Appendix B — Full Statistical Results

All tested predictors, p-values, adjusted q-values and effect sizes.

## Appendix C — Full Model Comparison

Complete five-model performance table including mean and standard deviation.

## Appendix D — Final Model Configuration

Include:

- 800 trees
- Maximum depth = 4
- Minimum split = 2
- Minimum leaf = 5
- Maximum features = 0.5
- Class weight = balanced
- Random state = 42

## Appendix E — Complete SHAP Results

All 23 original-predictor SHAP values.

## Appendix F — Patient-Level SHAP

Complete top-10 contributions for the selected TP, TN and FN cases.

## Appendix G — Deployment Artefacts

- `metabric_60_month_mortality_random_forest.joblib`
- `deployment_metadata.json`
- `deployment_feature_schema.csv`
- `deployment_threshold.csv`
- `deployment_manifest.json`

## Appendix H — Final Audit and Reproducibility

Include the final audit output and environment versions.

---

# 11. Final Submission Checklist

- [ ] Original cohort = 2,509
- [ ] Eligible cohort = 1,917
- [ ] Excluded = 592
- [ ] Events = 427
- [ ] Non-events = 1,490
- [ ] Prevalence = 22.27%
- [ ] Predictors = 23
- [ ] Transformed features = 80
- [ ] Five models reported
- [ ] Nested 5×3 CV explained
- [ ] F2 rationale explained
- [ ] Five-model comparison included
- [ ] Random Forest described as report-aligned, not universally best
- [ ] RF F2 = 0.6399
- [ ] RF ROC-AUC = 0.7600
- [ ] RF PR-AUC = 0.4846
- [ ] RF Brier = 0.1873
- [ ] RF recall = 0.8550
- [ ] RF precision = 0.3197
- [ ] OOF threshold = 0.330
- [ ] OOF F2 = 0.6490
- [ ] OOF ROC-AUC = 0.7575
- [ ] OOF PR-AUC = 0.4666
- [ ] OOF recall = 0.8548
- [ ] OOF precision = 0.3306
- [ ] Confusion matrix values verified
- [ ] Global SHAP included
- [ ] Patient-level SHAP included
- [ ] SHAP not interpreted causally
- [ ] Limitations included
- [ ] Ethical considerations included
- [ ] Future work included
- [ ] Deployment labelled research prototype
- [ ] Threshold labelled not clinically validated
- [ ] External-validation limitation included
- [ ] Treatment-variable limitation included
- [ ] All figures captioned
- [ ] All figures referenced in text
- [ ] All tables captioned
- [ ] All tables referenced in text
- [ ] APA 7 checked
- [ ] References checked
- [ ] Page numbers checked
- [ ] Table of Contents updated
- [ ] List of Figures updated
- [ ] List of Tables updated
- [ ] Abbreviations included
- [ ] GitHub link included after title page
- [ ] Appendices referenced
- [ ] Final audit retained
- [ ] Saved model reload tested
- [ ] Environment versions recorded

---

# 12. Recommended Final Report Structure

1. Title Page
2. GitHub Repository Link
3. Declaration / Academic Integrity Statement
4. Acknowledgements
5. Abstract
6. Keywords
7. Table of Contents
8. List of Tables
9. List of Figures
10. List of Abbreviations
11. Chapter 1 — Introduction
12. Chapter 2 — Literature Review
13. Chapter 3 — Dataset and Methodology
14. Chapter 4 — Exploratory and Statistical Results
15. Chapter 5 — Machine-Learning Results
16. Chapter 6 — Explainable AI Results
17. Chapter 7 — Deployment and Reproducibility
18. Chapter 8 — Discussion
19. Chapter 9 — Conclusion
20. References
21. Appendices

---

# 13. Polished Final Conclusion for Direct Use in the Report

> This study developed an explainable machine-learning framework for breast cancer mortality-risk stratification using clinicopathological and genomic characteristics from the METABRIC dataset. The analysis combined statistical association testing, leakage-controlled preprocessing, nested cross-validation, multi-model comparison, out-of-fold threshold optimisation and SHAP-based explainability to provide both predictive and interpretive evidence.
>
> From the original cohort of 2,509 patients, 1,917 patients met the eligibility criteria for the primary 60-month mortality endpoint, comprising 427 events and 1,490 non-events. Five machine-learning algorithms were evaluated using nested stratified cross-validation. The results demonstrated that model performance depended on the evaluation metric: CatBoost achieved the highest mean F2-score and recall, whereas Logistic Regression achieved the highest mean ROC-AUC and PR-AUC. Random Forest produced a competitive multi-metric profile and was retained as the report-aligned final model for detailed evaluation and explainability.
>
> The final Random Forest achieved a mean outer-fold F2-score of 0.6399, ROC-AUC of 0.7600, PR-AUC of 0.4846, Brier score of 0.1873, recall of 0.8550 and precision of 0.3197. Using pooled out-of-fold predictions, an analytical operating threshold of 0.330 produced an F2-score of 0.6490, recall of 0.8548 and precision of 0.3306. These results indicate that the selected operating point prioritised identification of mortality events while also producing a substantial number of false-positive classifications.
>
> The explainability analysis showed that Nottingham Prognostic Index, tumour size, positive lymph nodes, age at diagnosis, ER status and molecular subtype information were among the most influential predictors of model behaviour. Patient-level SHAP analysis further demonstrated that individual predictions resulted from combinations of multiple feature contributions. These findings support the value of combining predictive modelling with explainability when investigating clinical risk-stratification problems.
>
> The completed deployment workflow established a reproducible research pipeline by saving the complete preprocessing and Random Forest model, associated metadata, feature schema and analytical threshold. Reloading the saved model reproduced the original probability predictions exactly in the integrity test. This demonstrates technical reproducibility of the final modelling workflow.
>
> Nevertheless, the results must be interpreted within the limitations of the study. The model was developed using a retrospective METABRIC cohort and was not externally or prospectively validated. The 60-month endpoint required exclusion of patients with insufficient follow-up, and the selected threshold has not undergone clinical validation. Treatment-related predictors also mean that the model should not be interpreted as a purely pretreatment prognostic model. Finally, SHAP explanations describe model behaviour and do not establish causal relationships.
>
> Therefore, the principal outcome of this project is not a clinically deployable diagnostic tool, but a reproducible and interpretable research framework demonstrating how clinicopathological and genomic data can be combined with machine learning and explainable AI for breast cancer mortality-risk analysis. Future work should focus on independent external validation, survival-specific modelling, calibration and clinical-utility assessment, subgroup fairness analysis and prospective evaluation before any consideration of clinical implementation.

---

# 14. Final Overall Observation

The completed project should be presented as an **end-to-end analytical and explainability study**, rather than simply as a classification model.

The strongest narrative is:

**Clinical problem → METABRIC data → exploratory survival stratification → rigorous 60-month endpoint → leakage-controlled preprocessing → statistical association analysis → nested machine-learning comparison → Random Forest detailed evaluation → OOF threshold optimisation → SHAP global interpretation → patient-level explanation → reproducible deployment artifact → limitations → future clinical validation.**

This sequence gives the final report a coherent research story and connects the statistical, machine-learning and explainability components into one complete methodology.

