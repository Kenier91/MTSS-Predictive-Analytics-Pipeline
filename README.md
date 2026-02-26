# MTSS Predictive Analytics & Teacher Intervention Tool



## Executive Summary
This project is an automated, end-to-end Python data pipeline designed to transform school-wide Multi-Tiered System of Supports (MTSS) problem-solving. By bridging the gap between historical assessment data and predictive analytics, this tool empowers educational leadership to proactively target student interventions months before high-stakes state accountability testing. 

* **Transformed school-wide MTSS problem-solving** by developing an automated data tool that dynamically places students into Tiers 1, 2, and 3 based on real-time assessment growth and missing standard proficiencies.
* **Predicted state accountability outcomes** by deploying an AI-driven forecasting model that accurately projected end-of-year achievement levels across **ELA (FAST PM3), Algebra 1, Geometry, and Biology (EOCs)**, allowing administration to strategically allocate intervention resources to high-impact "bubble" students.
* **Facilitated highly targeted Professional Learning Communities (PLCs)** by automatically generating formatted, customized Excel workbooks for every teacher, isolating specific standards needing intervention for each student cluster.
* **Conducted rigorous statistical comparative analyses** across instructional teams to evaluate teacher efficacy and assessment trends, providing school leadership with transparent, data-backed insights.

---

## 🛠️ Tech Stack & Methodologies
* **Language:** Python 3
* **Data Processing:** `pandas`, `numpy`, `openpyxl`
* **Machine Learning:** `scikit-learn`, `xgboost`, `k-modes` (K-Prototypes)
* **Statistical Analysis:** `scipy`, `statsmodels` (ANOVA, Kruskal-Wallis, Tukey's HSD)
* **Reporting:** `fpdf` (Automated PDF generation)

---

## 🧠 Advanced Pipeline Architecture



### 1. Data Ingestion & Imputation
Merges fragmented Student Information System (SIS) rosters, attendance records, and historical assessment data. Handles missing Unit Assessment (USA) or Progress Monitoring (PM) data using localized standard-specific median imputation to preserve student growth trajectories across all four subject areas.

### 2. MTSS Tiering & K-Prototypes Clustering
Calculates sequential point-growth between assessments. Maps students to Tier 1, 2, or 3 based on benchmark proficiency and growth velocity. Because the data contains both numerical (test scores) and categorical (demographics, ESE/ELL status) variables, the pipeline utilizes the **K-Prototypes Machine Learning algorithm** to segment students within their Tiers into highly specific intervention clusters.

### 3. AI-Driven Forecasting (Walk-Forward Nested CV)
To predict the final State Accountability score, the pipeline tests an ensemble of regressors (Linear, Random Forest, Gradient Boosting, XGBoost). 
* To prevent data leakage and simulate chronological student testing, it utilizes a strict **Walk-Forward Time Series Split**.
* Hyperparameters are tuned via **Nested Cross-Validation** (GridSearchCV).
* The algorithm that achieves the lowest RMSE is automatically selected to generate the final school-wide predictions for each respective subject.

---

## 📊 Model Evaluation & Metrics
*Note: While predictive models were developed and deployed across ELA, Algebra 1, Geometry, and Biology, the following metrics reflect the performance of the winning algorithm (Linear Regression) specifically predicting the final 9th Grade ELA FAST PM3 scale score.*

| Metric | Score | Interpretation |
| :--- | :--- | :--- |
| **RMSE** | `5.055` | Root Mean Squared Error. The average point deviation from the actual PM3 scale score. Penalizes larger errors. |
| **MAE** | `4.035` | Mean Absolute Error. The model is, on average, off by just 4 scale score points. |
| **R²** | `0.961` | R-Squared. The pipeline successfully explains an outstanding 96.1% of the variance in final test scores. |

*(Please refer to the `documentation/ELA_Predictive_Model_Metrics_Explanation.pdf` for a deeper dive into these metrics).*

---

## 🔒 FERPA Compliance & Data Privacy
**Strict adherence to the Family Educational Rights and Privacy Act (FERPA) is maintained throughout this repository.** No actual student, teacher, or school identities are exposed. The code in the `/notebooks` directory is designed to be executed locally on secure district servers. All output files and statistical reports provided in the `/sample_outputs` and `/documentation` folders utilize **real-world distributions with fully masked/anonymized PII** (e.g., "Teacher A", "Student 101"). This ensures the statistical findings and model metrics reflect authentic educational data while strictly protecting the privacy of all individuals and institutions.

## 📂 Repository Structure
* `/notebooks`: Contains the core `.ipynb` Python pipeline scripts engineered to process data for **ELA, Algebra 1, Geometry, and Biology**. 
* `/presentations`: High-level slide decks for different subject areas explaining the MTSS framework, the predictive metrics, and how educators should interpret the data to drive interventions.
* `/documentation`: Auto-generated PDF reports demonstrating the pipeline's rigorous statistical hypothesis testing capabilities and predictive metrics. *(Populated with anonymized real-world data).*
* `/sample_outputs`: Anonymized Excel workbooks demonstrating the exact MTSS Tiering deliverables and EOC/PM3 predictions handed to instructional coaches and teachers.

---
*Developed by Kenier Ramirez, M.S.D.A | 2025*
