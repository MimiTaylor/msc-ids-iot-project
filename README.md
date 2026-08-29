[![Open in Codespaces](https://classroom.github.com/assets/launch-codespace-2972f46106e565e64193e422d61a12cf1da4916b45550586e14ef0a7c637dd04.svg)](https://classroom.github.com/open-in-codespaces?assignment_repo_id=24199164)

# Lightweight Intrusion Detection System Using Machine Learning for IoT Devices

**Author:** Mimi Nguyet Mi Taylor  
**Programme:** MSc Computer Science, Birkbeck, University of London  
**Supervisor:** Professor Paul Yoo

---

## Project Overview

This project evaluates five traditional machine learning classifiers alongside a minimal
MLP baseline for deployment as a lightweight Intrusion Detection System (IDS) on
resource-constrained IoT devices. Classifiers are evaluated on both detection accuracy
(precision, recall and F1-score across seven attack categories) and 
resource efficiency (training time, inference timing, memory use and model size).

The dataset used is CICIoT2023 (Neto et al., 2023), a realistic IoT network traffic
dataset containing 33 attack types across seven categories, plus Benign traffic (34
classes total), generated using 105 heterogeneous IoT devices.

---

## Repository Structure

| Notebook | Description |
|---|---|
| 01_data_preprocessing.ipynb | Loads all 63 CICIoT2023 CSV files, maps 33 attack labels to 7 categories plus Benign, takes a stratified sample of 1,000,000 records |
| 02_data_cleaning.ipynb | Data cleaning, Random Forest feature selection, 70/30 train/test split, SMOTE-ENN resampling, and feature scaling for the scale-sensitive classifiers |
| 03_model_training.ipynb | Trains five traditional classifiers and an MLP baseline. Applies feature scaling to the scale-sensitive classifiers (Logistic Regression, KNN, MLP) and leaves the tree-based classifiers and Naive Bayes unscaled. Measures resource efficiency, runs cross-validation on the two top models to check ranking stability, and generates results and charts |
| 04_ids_prototype.ipynb | Lightweight IDS prototype using the strongest candidate classifier (Decision Tree) identified in Notebook 03 and section 6.2 of the report |
| 05_data_leakage_check.ipynb | Verification notebook, not part of the pipeline. Refits the feature selection step on the training partition only and compares the result against the feature set used in this project. Writes no files. |

---

## Output Files

| File | Description |
|---|---|
| results_resources.csv | Training time, inference timing, memory use and model size for all classifiers |
| results_accuracy.csv | Precision, recall and F1-score per classifier per attack category |
| results_cross_validation.csv | Cross-validation mean and standard deviation macro F1 for Decision Tree and Random Forest |
| results_cv_fold_scores.csv | Raw per-fold macro F1 scores from the repeated cross-validation |
| prototype_predictions.csv | Per-record predictions from the IDS prototype |
| prototype_detection_summary.csv | Per-category detection rates from the IDS prototype |
| chart_resource_efficiency.png | Resource efficiency comparison charts |
| chart_macro_f1.png | Macro F1-score comparison bar chart |
| chart_f1_by_category.png | F1-score by attack category and classifier |
| chart_confusion_matrix.png | Confusion matrix for the IDS prototype |
| smote_enn_balance.png | Class distribution before and after SMOTE-ENN |
| feature_importances.png | Random Forest feature importance scores |
| class_distribution.png | Class distribution in the stratified sample (1,000,000 records) |

---

## Setup and Installation

### Requirements
- Python 3.13
- Virtual environment recommended

### Install dependencies

```bash
pip install pandas numpy matplotlib seaborn scikit-learn imbalanced-learn joblib jupyter
```

### Activate virtual environment (if using one)

```bash
source ~/ids_project/bin/activate
```

### Launch Jupyter

```bash
jupyter notebook
```

Run the notebooks in order: 01, then 02, then 03, then 04. Each notebook saves files
that the next notebook depends on.

Notebook 05 is a verification notebook and is not part of the pipeline. It requires only the stratified sample produced by Notebook 01, writes no files, and does not affect the outputs of the other four.

### Expected Runtime

Notebook 01 takes several minutes to load all 63 CSV files (45,019,243 rows).
Notebook 03 takes roughly 45 to 60 minutes to run in full. Most of this is the MLP
training to convergence and the repeated cross-validation of Decision Tree and Random
Forest, which applies SMOTE-ENN inside every fold. K-Nearest Neighbor's inference cost
on the resampled training set (2,273,555 records) adds further time.


## Dataset

This project uses the **CICIoT2023** dataset (Neto et al., 2023).

- **Source:** https://www.unb.ca/cic/datasets/iotdataset-2023.html
- **Size:** 45,019,243 network flow records across 63 CSV files, as verified by loading all files used in this project
- **Features:** 39 features plus 1 label column
- **Attack types:** 33 attack types across 7 categories, plus Benign traffic (34 classes total)

The dataset is not included in this repository due to its size. It can be downloaded from the
source above and the CSV files placed in a folder called `MERGED_CSV` inside your
`IDS_PROJECT_REPORT` directory before running Notebook 01.

---

## Classifiers Evaluated

1. Decision Tree
2. Random Forest
3. Naive Bayes
4. Logistic Regression
5. K-Nearest Neighbor (KNN)
6. Multilayer Perceptron (MLP) minimal baseline

---

## References

Neto, E.C.P., Dadkhah, S., Ferreira, R., Zohourian, A., Lu, R. and Ghorbani, A.A. (2023) 'CICIoT2023: a real-time dataset and benchmark for large-scale attacks in IoT environment', Sensors, 23(13), p.5941. doi: 10.3390/s23135941.
