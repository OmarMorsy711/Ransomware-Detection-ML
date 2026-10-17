# Ransomware Detection using Behavioral Analysis

## Overview

This project builds a binary classifier that separates ransomware from benign Windows executables. Each sample is described by static PE (Portable Executable) header metadata together with behavioural features recorded at runtime, such as registry activity, file and process activity, DLL and API call counts, and execution time. A Logistic Regression model trained on these features identifies ransomware on a held-out test set with 99.66% accuracy.

## Dataset

Hussain, A. (2024). *Ransomware Dataset 2024* [Data set]. Zenodo. https://doi.org/10.5281/zenodo.13890887

- DOI: [10.5281/zenodo.13890887](https://doi.org/10.5281/zenodo.13890887)
- Kaggle mirror: [mexwell/ransomware-dataset-2024](https://www.kaggle.com/datasets/mexwell/ransomware-dataset-2024)
- Licence: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)

The dataset is not included in this repository. Download `ransom.csv` from one of the links above and place it in the same folder as `Ransomware_Detection.ipynb` before running the notebook.

## Method

1. **Load:** 21,752 samples are read from `ransom.csv`.
2. **Balance:** the data is balanced to 9,762 samples (5,000 benign and 4,762 ransomware).
3. **Feature selection:** static PE header fields and behavioural features (registry, process, file, DLL and API call counts, runtime) are kept. File hashes and family identifiers are dropped before splitting so that the model cannot learn the label from sample identity (label leakage).
4. **Encoding and scaling:** categorical fields are one-hot encoded and numeric fields standardised, giving 25,834 features.
5. **Model:** Logistic Regression, tuned with `RandomizedSearchCV` over the regularisation strength, penalty and solver, using 5-fold cross-validation scored on balanced accuracy.
6. **Evaluation:** the best model is evaluated once on a held-out test set.

## Results

Test set performance of the tuned Logistic Regression model:

| Metric            | Score  |
|-------------------|--------|
| Accuracy          | 99.66% |
| Balanced accuracy | 99.66% |
| Precision         | 99.58% |
| Recall            | 99.72% |
| F1 score          | 0.997  |

Confusion matrix (rows are the true class, columns the predicted class):

|                     | Predicted benign | Predicted ransomware |
|---------------------|------------------|----------------------|
| **True benign**     | 1,494 (TN)       | 6 (FP)               |
| **True ransomware** | 4 (FN)           | 1,425 (TP)           |

## How to run

1. Install Python 3.11 or later.
2. Install the dependencies:

   ```bash
   pip install -r requirements.txt
   ```

3. Download `ransom.csv` (see [Dataset](#dataset)) and place it next to the notebook.
4. Open `Ransomware_Detection.ipynb` in Jupyter (for example `pip install notebook` and then `jupyter notebook`) and run the cells in order.
