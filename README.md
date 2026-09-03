# Large-Scale Network Traffic Anomaly Detection Using Unsupervised Machine Learning

## Project Overview

This project investigates the use of unsupervised machine learning for detecting anomalous network traffic. The study compares three anomaly-detection algorithms:

- Isolation Forest (IF)
- One-Class Support Vector Machine (OCSVM)
- Local Outlier Factor (LOF)

The models are trained using benign network traffic so that abnormal network behaviour can be identified without requiring labelled attack samples during model training.

The project evaluates the models based on their overall detection performance, attack-specific detection capability, false-positive behaviour, attack prevalence, and computational scalability.

---

## Dataset

The project uses the **CICIDS2017** network intrusion detection dataset.

The original data consists of eight CSV files containing approximately **2.83 million network flows** and **79 columns**. During preprocessing, the files were combined and cleaned, and **70 numerical features** were selected for anomaly detection.

For the main experiment:

- Training data: 50,000 benign network flows
- Attack flows used for training: 0
- Test data: 50,000 flows
- Benign test flows: 25,000
- Attack test flows: 25,000
- Attack categories evaluated: 14

The CICIDS2017 dataset is not included in this repository because of its size and should be obtained separately from the dataset provider. (https://www.unb.ca/cic/datasets/ids-2017.html)

---

## Methodology

The experimental workflow used in this project was:

1. Load and combine the CICIDS2017 CSV files.
2. Clean the dataset and handle invalid or missing values.
3. Separate benign and malicious network traffic.
4. Select 70 numerical network-flow features.
5. Standardise the selected features.
6. Train the anomaly-detection models using benign traffic only.
7. Evaluate the models using unseen benign and attack traffic.
8. Compare overall model performance.
9. Analyse detection performance across individual attack categories.
10. Evaluate attack prevalence and false-alert behaviour.
11. Evaluate model scalability using increasing training dataset sizes.

---

## Machine Learning Models

### Isolation Forest

Isolation Forest detects anomalies by isolating unusual observations through randomly generated decision trees. It was evaluated for both anomaly-detection effectiveness and computational scalability.

### One-Class SVM

One-Class SVM learns a decision boundary around normal network traffic and classifies observations outside this boundary as anomalies.

### Local Outlier Factor

Local Outlier Factor detects anomalies by comparing the local density of an observation with the densities of neighbouring observations. Novelty detection was used to allow the trained model to classify previously unseen network flows.

---

## Evaluation Metrics

The models were evaluated using:

- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC
- False Positive Rate
- Training time
- Prediction time
- Attack-specific detection rate

Additional experiments investigated model performance under different attack prevalence levels and different training dataset sizes.

---

## Main Results

| Model | Accuracy | Precision | Recall | F1-score | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Isolation Forest | 0.7488 | 0.8839 | 0.5728 | 0.6951 | 0.8063 |
| One-Class SVM | 0.7703 | **0.9203** | 0.5918 | 0.7204 | 0.8059 |
| Local Outlier Factor | **0.8554** | 0.8579 | **0.8520** | **0.8549** | **0.8970** |

Local Outlier Factor achieved the strongest overall detection performance, including the highest accuracy, recall, F1-score, and ROC-AUC.

One-Class SVM achieved the highest precision and the lowest false-positive rate, while Isolation Forest demonstrated the strongest computational scalability.

The results indicate that no single anomaly-detection algorithm was optimal across every evaluation criterion.

---

## Attack-Specific Analysis

Performance varied considerably across the 14 attack categories.

Examples of strong detection results included:

- DDoS - LOF: 95.95%
- DoS Hulk - LOF: 93.39%
- SSH-Patator - LOF: 99.19%
- DoS Slowhttptest - OCSVM: 91.55%
- Infiltration - Isolation Forest: 86.11%

Some attack categories were considerably more difficult to detect, demonstrating that strong aggregate model performance does not necessarily indicate equally strong detection across all forms of malicious traffic.

---

## Scalability

The models were also trained using increasing quantities of benign traffic to evaluate computational scalability.

At 100,000 training flows:

| Model | Training Time |
|---|---:|
| Isolation Forest | **0.24 s** |
| Local Outlier Factor | 10.79 s |
| One-Class SVM | 190.61 s |

Isolation Forest demonstrated substantially better training scalability than the other two algorithms.

---

## Repository Structure

```text
SIT326-HD-Task1/
│
├── HD_Task1.ipynb
├── README.md
│
├── figures/
│   ├── model_performance_comparison.png
│   ├── roc_curve_model_comparison.png
│   ├── detection_rate_by_attack_type.png
│   ├── precision_vs_attack_prevalence.png
│   ├── scalability_training_time.png
│   ├── isolation_forest_confusion_matrix.png
│   ├── one_class_svm_confusion_matrix.png
│   ├── lof_confusion_matrix.png
│   └── other experimental visualisations
│
└── results/
    ├── final_model_comparison.csv
    ├── detection_rate_by_attack_type.csv
    ├── attack_prevalence_analysis.csv
    ├── expected_alerts_1_percent_attacks.csv
    └── scalability_training_results.csv
