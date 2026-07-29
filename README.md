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
(precision, recall and F1-score across seven attack categories) and resource efficiency
(training time, inference time, memory usage and model size).

The dataset used is CICIoT2023 (Neto et al., 2023), a realistic IoT network traffic
dataset containing 33 attack types across seven categories, plus Benign traffic (34
classes total), generated using 105 heterogeneous IoT devices.

---

## Repository Structure

| Notebook | Description |
|---|---|
| 01_data_preprocessing.ipynb | Loads all 63 CICIoT2023 CSV files, maps 33 attack labels to 7 categories plus Benign, takes a stratified sample of 1,000,000 records |
| 02_data_cleaning.ipynb | Data cleaning, Random Forest feature selection, 70/30 train/test split, SMOTE-ENN resampling |
| 03_model_training.ipynb | Trains five traditional classifiers and MLP baseline, measures resource efficiency, generates results and charts | Also includes a supplementary test retraining Logistic Regression with feature scaling, to test whether this resolves its convergence problem |
| 04_ids_prototype.ipynb | Lightweight IDS prototype using the most suitable classifier identified in Notebook 03 |

---

## Output Files

| File | Description |
|---|---|
| results_resources.csv | Training time, inference time, memory usage and model size for all classifiers |
| results_accuracy.csv | Precision, recall and F1-score per classifier per attack category |
| prototype_predictions.csv | Per-record predictions from the IDS prototype |
| prototype_detection_summary.csv | Per-category detection rates from the IDS prototype |
| chart_resource_efficiency.png | Resource efficiency comparison charts |
| chart_macro_f1.png | Macro F1-score comparison bar chart |
| chart_f1_by_category.png | F1-score by attack category and classifier |
| chart_confusion_matrix.png | Confusion matrix for the IDS prototype |
| smote_enn_balance.png | Class distribution before and after SMOTE-ENN |
| feature_importances.png | Random Forest feature importance scores |
| class_distribution.png	Class distribution in the stratified sample (1,000,000 records) |

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

### Expected Runtime

Notebook 01 takes several minutes to load all 63 CSV files (~45 million rows).
Notebook 03 takes approximately 10-15 minutes to train all six classifiers
sequentially, primarily due to Logistic Regression's slow convergence and
K-Nearest Neighbour's inference cost on the resampled training set (2,273,555 records). 


## Dataset

This project uses the **CICIoT2023** dataset (Neto et al., 2023).

- **Source:** https://www.unb.ca/cic/datasets/iotdataset-2023.html
- **Size:** approximately 45 million network flow records across 63 CSV files
- **Features:** 39 features plus 1 label column
- **Attack types:** 33 attack types across 7 categories, plus Benign traffic (34 classes total)

The dataset is not included in this repository due to its size. It can be Downloaded from the
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
