# Intelligent Network Intrusion Detection & Attack Classification

**SWYNEX Technologies — Data Science Internship**

## 1. Project Overview

This project explores how data science and machine learning can support network intrusion detection. Using the UNSW-NB15 dataset, the project examines network traffic characteristics and builds a baseline classifier to distinguish normal traffic from potentially malicious activity.

The project covers the complete data-science workflow: problem definition, data preparation, exploratory data analysis (EDA), model development, evaluation, and conclusions.

## 2. Problem Statement

Network traffic contains information about protocols, services, connection states, packet counts, byte counts, and traffic rates. Analyzing these characteristics can help identify patterns associated with normal activity and cyberattacks.

The objective of this case study is to develop and evaluate a baseline machine-learning model that classifies network records as:

- **Normal (0):** Normal network activity
- **Attack (1):** Network activity labeled as an attack

The project also investigates how the findings can inform future multiclass attack classification and model improvement.

## 3. Dataset

The project uses the **UNSW-NB15 Network Intrusion Detection Dataset**.

Dataset files used:
- `UNSW_NB15_training-set.csv`
- `UNSW_NB15_testing-set.csv`
- `UNSW-NB15_features.csv`

| Dataset | Records | Columns |
|---|---:|---:|
| Training | 175,341 | 45 |
| Testing | 82,332 | 45 |

**Official dataset source:**  
https://research.unsw.edu.au/projects/unsw-nb15-dataset

The CSV files are not included in this repository. Refer to `data/README.md` for dataset information.

## 4. Data Preparation

The dataset preparation stage included:

- Inspecting dataset dimensions, columns, and data types.
- Checking missing values, duplicate rows, infinite values, and negative numerical values.
- Investigating categorical features and target distributions.
- Comparing the training and testing schemas.
- Comparing dataset columns against the supplied feature documentation.
- Investigating the unusual values found in `is_ftp_login`.
- Identifying `id` as a record identifier rather than a predictive feature.

### Data Quality Results

- Missing values: 0 in both datasets.
- Duplicate rows: 0 in both datasets.
- Infinite numerical values: 0 in both datasets.
- Training and testing columns: Consistent.
- Training and testing data types: Consistent.

Column names were standardized by removing leading and trailing whitespace. No missing-value imputation or duplicate removal was necessary.

The observed `is_ftp_login` values were retained because the available documentation did not justify an alternative recoding rule.

## 5. Exploratory Data Analysis

EDA was used to understand the distribution and characteristics of network traffic.

The analysis included:
- Summary statistics for numerical features.
- Normal-versus-attack class distribution.
- Distribution of attack categories.
- Attack percentages across connection states.
- Protocol-level attack patterns.
- Numerical feature distributions and potential skewness.
- Correlations between numerical features.

### Key Findings

**1. Class distribution:** Approximately 68.1% of training records are attacks and 31.9% are normal records. Evaluation should therefore use more than accuracy alone.

**2. Attack-category imbalance:** Some categories, such as Worms, contain substantially fewer records than common categories. This may make multiclass classification more challenging.

**3. Connection-state patterns:** Attack percentages vary across connection states, suggesting that connection-state information may be useful for classification.

**4. Protocol patterns:** Attack percentages differ across protocols in this dataset. These associations may provide predictive information but should not be assumed to generalize to every network.

**5. Skewed numerical features:** Traffic measurements can have long-tailed distributions. Transformations and outlier handling should be evaluated carefully instead of automatically deleting extreme observations.

**6. Feature correlations:** Correlated numerical features may contain overlapping information and can be investigated during future feature selection.

These findings describe the dataset and do not establish that any individual feature causes an attack.

## 6. Model Development

A baseline **Random Forest Classifier** was trained for binary intrusion detection.

### Modeling approach

- Target variable: `label`
- Class 0: Normal
- Class 1: Attack
- Features: Available network traffic attributes
- Excluded columns: `id` and `attack_cat`
- Categorical preprocessing: Most-frequent imputation and one-hot encoding
- Numerical preprocessing: Median imputation
- Classifier: Random Forest with 100 trees
- Random state: 42
- Class weighting: Balanced

The supplied training split was used for fitting, and the supplied testing split was used for final evaluation.

The `attack_cat` column was excluded to prevent target leakage because it provides attack-category information closely related to the binary label. The identifier `id` was also excluded from model inputs.

## 7. Model Evaluation

The baseline model produced the following test results:

| Metric | Result |
|---|---:|
| Accuracy | 87.05% |
| ROC-AUC | 0.9791 |
| Attack precision | 81.67% |
| Attack recall | 98.61% |
| Attack F1-score | 89.34% |
| Normal recall | 72.88% |

### Confusion Matrix

| Actual class | Predicted Normal | Predicted Attack |
|---|---:|---:|
| Normal | 26,966 | 10,034 |
| Attack | 629 | 44,703 |

### Interpretation

The model correctly identified 44,703 attack records and missed 629 attacks. Its attack recall of 98.61% indicates that it detected most attacks in the test dataset.

However, 10,034 normal records were incorrectly classified as attacks. These false positives could generate excessive alerts in a practical security environment.

The results illustrate why model evaluation must consider both missed attacks and false alarms. Accuracy alone does not fully describe performance.

ROC-AUC measures discrimination across classification thresholds; it should not be interpreted as the percentage of correct predictions.

## 8. Limitations

- The evaluation uses one dataset and may not represent current real-world network traffic.
- False positives could increase the workload of security analysts.
- False negatives could allow malicious traffic to go undetected.
- The baseline model has not been compared with alternative algorithms.
- Further hyperparameter tuning and threshold analysis may improve the balance between detection and false alarms.
- Additional datasets are needed to assess generalization.
- Real-world deployment would require additional validation, monitoring, and security testing.

## 9. Conclusion

This case study demonstrates an end-to-end data-science workflow for binary network intrusion detection using UNSW-NB15.

The dataset was inspected and validated, exploratory analysis identified patterns relevant to modeling, and a Random Forest baseline was trained and evaluated.

The model achieved 87.05% accuracy, a ROC-AUC of 0.9791, and 98.61% attack recall. Although the model detected most attack records in the test dataset, the number of false positives shows that further improvements are needed.

Future work could include comparing alternative classifiers, tuning decision thresholds, investigating feature importance, improving false-positive performance, and evaluating the model on additional datasets.

The model is a baseline for experimentation, not a production-ready intrusion detection system.

## 10. Project Notebooks

The implementation and analysis are documented in these notebooks:

1. **Data Preparation:** [`01_data_preparation.ipynb`](notebook/01_data_preparation.ipynb)
2. **Exploratory Data Analysis:** [`02_exploratory_data_analysis.ipynb`](notebook/02_exploratory_data_analysis.ipynb)
3. **Baseline Model and Evaluation:** [`03_baseline_intrusion_detection.ipynb`](notebook/03_baseline_intrusion_detection.ipynb)

## 11. Tools and Technologies

- Python
- Pandas and NumPy
- Matplotlib and Seaborn
- Scikit-learn
- Google Colab
- GitHub

## 12. Internship

Completed as part of the **Data Science Internship at SWYNEX Technologies**.
