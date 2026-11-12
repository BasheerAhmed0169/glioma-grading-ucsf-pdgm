# Final report: MRI, clinical and multimodal glioma grade classification (UCSF-PDGM)

## What was done

- Data: 495 patients, one tumour slice per patient, 4 MRI sequences (T1, T1c, T2, FLAIR) and age + sex.
- MRI features: frozen ResNet50 (ImageNet) numbers, reduced with PCA.
- MRI models: Logistic Regression, Decision Tree, Random Forest, SVM, Hist Gradient Boosting. Clinical models: Logistic Regression, Decision Tree, Random Forest, SVM, Hist Gradient Boosting. Multimodal models: Logistic Regression, Decision Tree, Random Forest, SVM, Hist Gradient Boosting. Training with missing values: False.
- Evaluation: 5-fold patient-level cross-validation repeated with seeds [42, 43, 44].
- Order of work: binary task first (grade 4 vs grade 2/3), then three classes (grade 2 / 3 / 4). Each task has four parts: clinical only, MRI only, MRI + clinical, MRI + clinical with missing clinical data.

## Binary classification: grade 2/3 vs grade 4

Patients: 495 (grade 2/3: 99, grade 4: 396). Main metric: MCC. Every number is mean ± standard deviation over 3 seeds x 5 patient-level folds.

### Part 1. Clinical only (age, sex)
Models used: Decision Tree, Hist Gradient Boosting, Logistic Regression, Random Forest, SVM.
Best by MCC: **Hist Gradient Boosting** (0.439).

| model | mcc | balanced_acc | macro_f1 | auc | accuracy | recall grade 2/3 | recall grade 4 |
|---|---|---|---|---|---|---|---|
| Decision Tree | 0.435 ± 0.062 | 0.744 ± 0.035 | 0.708 ± 0.032 | 0.757 ± 0.053 | 0.785 ± 0.027 | 0.677 ± 0.076 | 0.812 ± 0.036 |
| Hist Gradient Boosting | 0.439 ± 0.061 | 0.749 ± 0.033 | 0.709 ± 0.033 | 0.755 ± 0.054 | 0.783 ± 0.029 | 0.693 ± 0.058 | 0.806 ± 0.034 |
| Logistic Regression | 0.425 ± 0.059 | 0.749 ± 0.037 | 0.693 ± 0.028 | 0.795 ± 0.052 | 0.761 ± 0.023 | 0.730 ± 0.081 | 0.769 ± 0.032 |
| Random Forest | 0.389 ± 0.083 | 0.715 ± 0.043 | 0.687 ± 0.045 | 0.714 ± 0.054 | 0.774 ± 0.041 | 0.616 ± 0.074 | 0.813 ± 0.049 |
| SVM | 0.260 ± 0.089 | 0.603 ± 0.047 | 0.611 ± 0.054 | 0.780 ± 0.053 | 0.796 ± 0.023 | 0.282 ± 0.119 | 0.924 ± 0.040 |

### Part 2. MRI only
Models used: Decision Tree, Hist Gradient Boosting, Logistic Regression, Random Forest, SVM.
Best by MCC: **Logistic Regression** (0.392).

| model | mcc | balanced_acc | macro_f1 | auc | accuracy | recall grade 2/3 | recall grade 4 |
|---|---|---|---|---|---|---|---|
| Decision Tree | 0.193 ± 0.094 | 0.612 ± 0.057 | 0.575 ± 0.045 | 0.633 ± 0.058 | 0.671 ± 0.059 | 0.512 ± 0.168 | 0.711 ± 0.100 |
| Hist Gradient Boosting | 0.241 ± 0.092 | 0.627 ± 0.056 | 0.617 ± 0.043 | 0.722 ± 0.034 | 0.746 ± 0.023 | 0.428 ± 0.134 | 0.826 ± 0.038 |
| Logistic Regression | 0.392 ± 0.056 | 0.727 ± 0.030 | 0.680 ± 0.032 | 0.796 ± 0.032 | 0.754 ± 0.036 | 0.680 ± 0.066 | 0.773 ± 0.051 |
| Random Forest | 0.275 ± 0.118 | 0.582 ± 0.041 | 0.592 ± 0.056 | 0.762 ± 0.029 | 0.815 ± 0.021 | 0.195 ± 0.085 | 0.970 ± 0.023 |
| SVM | 0.324 ± 0.132 | 0.609 ± 0.052 | 0.626 ± 0.066 | 0.778 ± 0.045 | 0.821 ± 0.029 | 0.255 ± 0.106 | 0.962 ± 0.031 |

### Part 3. MRI + clinical (multimodal)
Models used: Decision Tree, Hist Gradient Boosting, Logistic Regression, Random Forest, SVM.
Best by MCC: **Logistic Regression** (0.447).

| model | mcc | balanced_acc | macro_f1 | auc | accuracy | recall grade 2/3 | recall grade 4 |
|---|---|---|---|---|---|---|---|
| Decision Tree | 0.425 ± 0.089 | 0.745 ± 0.051 | 0.697 ± 0.046 | 0.751 ± 0.077 | 0.768 ± 0.041 | 0.707 ± 0.096 | 0.784 ± 0.049 |
| Hist Gradient Boosting | 0.386 ± 0.101 | 0.698 ± 0.055 | 0.692 ± 0.050 | 0.823 ± 0.031 | 0.798 ± 0.032 | 0.532 ± 0.103 | 0.865 ± 0.029 |
| Logistic Regression | 0.447 ± 0.073 | 0.752 ± 0.039 | 0.714 ± 0.038 | 0.855 ± 0.038 | 0.789 ± 0.033 | 0.690 ± 0.068 | 0.814 ± 0.037 |
| Random Forest | 0.394 ± 0.115 | 0.660 ± 0.059 | 0.682 ± 0.062 | 0.836 ± 0.028 | 0.831 ± 0.028 | 0.376 ± 0.116 | 0.944 ± 0.022 |
| SVM | 0.437 ± 0.100 | 0.686 ± 0.047 | 0.709 ± 0.049 | 0.840 ± 0.037 | 0.838 ± 0.027 | 0.434 ± 0.089 | 0.939 ± 0.025 |

### Part 4. MRI + clinical with missing clinical data
Table: MCC for each model and each amount of missing clinical data.

| model | 0% missing | 25% missing | 50% missing | 75% missing | 100% missing | only age missing | only sex missing |
|---|---|---|---|---|---|---|---|
| Clinical - Decision Tree | 0.435 ± 0.062 | 0.398 ± 0.086 | 0.300 ± 0.109 | 0.181 ± 0.096 | 0.000 ± 0.000 | 0.000 ± 0.000 | 0.432 ± 0.064 |
| Clinical - Hist Gradient Boosting | 0.439 ± 0.061 | 0.401 ± 0.088 | 0.304 ± 0.092 | 0.176 ± 0.102 | 0.000 ± 0.000 | 0.000 ± 0.000 | 0.449 ± 0.064 |
| Clinical - Logistic Regression | 0.425 ± 0.059 | 0.380 ± 0.094 | 0.297 ± 0.100 | 0.165 ± 0.095 | 0.000 ± 0.000 | 0.000 ± 0.000 | 0.426 ± 0.054 |
| Clinical - Random Forest | 0.389 ± 0.083 | 0.360 ± 0.105 | 0.288 ± 0.074 | 0.159 ± 0.088 | 0.000 ± 0.000 | 0.000 ± 0.000 | 0.360 ± 0.086 |
| Clinical - SVM | 0.260 ± 0.089 | 0.233 ± 0.097 | 0.178 ± 0.130 | 0.100 ± 0.130 | 0.000 ± 0.000 | 0.000 ± 0.000 | 0.000 ± 0.000 |
| MRI - Decision Tree | 0.193 ± 0.094 | 0.193 ± 0.094 | 0.193 ± 0.094 | 0.193 ± 0.094 | 0.193 ± 0.094 | 0.193 ± 0.094 | 0.193 ± 0.094 |
| MRI - Hist Gradient Boosting | 0.241 ± 0.092 | 0.241 ± 0.092 | 0.241 ± 0.092 | 0.241 ± 0.092 | 0.241 ± 0.092 | 0.241 ± 0.092 | 0.241 ± 0.092 |
| MRI - Logistic Regression | 0.392 ± 0.056 | 0.392 ± 0.056 | 0.392 ± 0.056 | 0.392 ± 0.056 | 0.392 ± 0.056 | 0.392 ± 0.056 | 0.392 ± 0.056 |
| MRI - Random Forest | 0.275 ± 0.118 | 0.275 ± 0.118 | 0.275 ± 0.118 | 0.275 ± 0.118 | 0.275 ± 0.118 | 0.275 ± 0.118 | 0.275 ± 0.118 |
| MRI - SVM | 0.324 ± 0.132 | 0.324 ± 0.132 | 0.324 ± 0.132 | 0.324 ± 0.132 | 0.324 ± 0.132 | 0.324 ± 0.132 | 0.324 ± 0.132 |
| MRI+Clinical - Decision Tree | 0.425 ± 0.089 | 0.409 ± 0.110 | 0.341 ± 0.097 | 0.262 ± 0.103 | 0.206 ± 0.098 | 0.206 ± 0.098 | 0.425 ± 0.089 |
| MRI+Clinical - Hist Gradient Boosting | 0.386 ± 0.101 | 0.373 ± 0.126 | 0.331 ± 0.101 | 0.279 ± 0.107 | 0.264 ± 0.137 | 0.264 ± 0.137 | 0.385 ± 0.100 |
| MRI+Clinical - Logistic Regression | 0.447 ± 0.073 | 0.421 ± 0.061 | 0.420 ± 0.059 | 0.384 ± 0.058 | 0.361 ± 0.080 | 0.373 ± 0.071 | 0.458 ± 0.071 |
| MRI+Clinical - Random Forest | 0.394 ± 0.115 | 0.362 ± 0.142 | 0.274 ± 0.118 | 0.212 ± 0.130 | 0.098 ± 0.127 | 0.095 ± 0.131 | 0.387 ± 0.116 |
| MRI+Clinical - SVM | 0.437 ± 0.100 | 0.399 ± 0.117 | 0.300 ± 0.092 | 0.248 ± 0.100 | 0.108 ± 0.133 | 0.122 ± 0.125 | 0.441 ± 0.100 |

Effect of missing clinical data on each multimodal model:

- MRI+Clinical - Decision Tree: MCC 0.425 with full clinical data and 0.206 with all clinical data missing (change -0.219, larger than the fold-to-fold spread). Only age missing: 0.206; only sex missing: 0.425.
- MRI+Clinical - Hist Gradient Boosting: MCC 0.386 with full clinical data and 0.264 with all clinical data missing (change -0.123, larger than the fold-to-fold spread). Only age missing: 0.264; only sex missing: 0.385.
- MRI+Clinical - Logistic Regression: MCC 0.447 with full clinical data and 0.361 with all clinical data missing (change -0.086, larger than the fold-to-fold spread). Only age missing: 0.373; only sex missing: 0.458.
- MRI+Clinical - Random Forest: MCC 0.394 with full clinical data and 0.098 with all clinical data missing (change -0.296, larger than the fold-to-fold spread). Only age missing: 0.095; only sex missing: 0.387.
- MRI+Clinical - SVM: MCC 0.437 with full clinical data and 0.108 with all clinical data missing (change -0.329, larger than the fold-to-fold spread). Only age missing: 0.122; only sex missing: 0.441.

Reference points with all clinical data missing: Clinical - Decision Tree 0.000; Clinical - Hist Gradient Boosting 0.000; Clinical - Logistic Regression 0.000; Clinical - Random Forest 0.000; Clinical - SVM 0.000; best MRI-only model (Logistic Regression) 0.392.

## Three-class classification: grade 2 / grade 3 / grade 4

Patients: 495 (grade 2: 56, grade 3: 43, grade 4: 396). Main metric: macro-F1. Every number is mean ± standard deviation over 3 seeds x 5 patient-level folds.

### Part 1. Clinical only (age, sex)
Models used: Decision Tree, Hist Gradient Boosting, Logistic Regression, Random Forest, SVM.
Best by macro-F1: **Logistic Regression** (0.476).

| model | mcc | balanced_acc | macro_f1 | auc | accuracy | recall grade 2 | recall grade 3 | recall grade 4 |
|---|---|---|---|---|---|---|---|---|
| Decision Tree | 0.219 ± 0.060 | 0.442 ± 0.055 | 0.399 ± 0.036 | 0.666 ± 0.037 | 0.615 ± 0.052 | 0.389 ± 0.188 | 0.250 ± 0.152 | 0.688 ± 0.072 |
| Hist Gradient Boosting | 0.187 ± 0.047 | 0.432 ± 0.045 | 0.384 ± 0.029 | 0.686 ± 0.033 | 0.560 ± 0.045 | 0.429 ± 0.146 | 0.255 ± 0.151 | 0.612 ± 0.060 |
| Logistic Regression | 0.311 ± 0.061 | 0.532 ± 0.068 | 0.476 ± 0.039 | 0.766 ± 0.034 | 0.677 ± 0.024 | 0.609 ± 0.133 | 0.255 ± 0.156 | 0.732 ± 0.032 |
| Random Forest | 0.198 ± 0.039 | 0.410 ± 0.040 | 0.392 ± 0.030 | 0.666 ± 0.028 | 0.647 ± 0.029 | 0.399 ± 0.104 | 0.087 ± 0.084 | 0.743 ± 0.038 |
| SVM | 0.005 ± 0.019 | 0.335 ± 0.005 | 0.299 ± 0.011 | 0.766 ± 0.039 | 0.799 ± 0.005 | 0.006 ± 0.022 | 0.000 ± 0.000 | 0.998 ± 0.007 |

### Part 2. MRI only
Models used: Decision Tree, Hist Gradient Boosting, Logistic Regression, Random Forest, SVM.
Best by macro-F1: **Hist Gradient Boosting** (0.454).

| model | mcc | balanced_acc | macro_f1 | auc | accuracy | recall grade 2 | recall grade 3 | recall grade 4 |
|---|---|---|---|---|---|---|---|---|
| Decision Tree | 0.134 ± 0.084 | 0.432 ± 0.086 | 0.363 ± 0.046 | 0.594 ± 0.068 | 0.496 ± 0.070 | 0.477 ± 0.212 | 0.296 ± 0.186 | 0.521 ± 0.105 |
| Hist Gradient Boosting | 0.274 ± 0.098 | 0.462 ± 0.063 | 0.454 ± 0.056 | 0.691 ± 0.059 | 0.776 ± 0.025 | 0.430 ± 0.188 | 0.053 ± 0.083 | 0.904 ± 0.030 |
| Logistic Regression | 0.265 ± 0.053 | 0.496 ± 0.054 | 0.449 ± 0.036 | 0.721 ± 0.052 | 0.656 ± 0.032 | 0.562 ± 0.172 | 0.209 ± 0.140 | 0.718 ± 0.046 |
| Random Forest | 0.139 ± 0.111 | 0.364 ± 0.027 | 0.351 ± 0.043 | 0.708 ± 0.061 | 0.804 ± 0.012 | 0.102 ± 0.082 | 0.000 ± 0.000 | 0.991 ± 0.012 |
| SVM | 0.242 ± 0.129 | 0.401 ± 0.046 | 0.397 ± 0.056 | 0.738 ± 0.051 | 0.811 ± 0.019 | 0.220 ± 0.140 | 0.000 ± 0.000 | 0.982 ± 0.013 |

### Part 3. MRI + clinical (multimodal)
Models used: Decision Tree, Hist Gradient Boosting, Logistic Regression, Random Forest, SVM.
Best by macro-F1: **Logistic Regression** (0.496).

| model | mcc | balanced_acc | macro_f1 | auc | accuracy | recall grade 2 | recall grade 3 | recall grade 4 |
|---|---|---|---|---|---|---|---|---|
| Decision Tree | 0.322 ± 0.056 | 0.551 ± 0.047 | 0.476 ± 0.036 | 0.686 ± 0.058 | 0.663 ± 0.054 | 0.636 ± 0.174 | 0.315 ± 0.166 | 0.704 ± 0.073 |
| Hist Gradient Boosting | 0.340 ± 0.090 | 0.504 ± 0.072 | 0.492 ± 0.054 | 0.769 ± 0.048 | 0.785 ± 0.027 | 0.532 ± 0.209 | 0.082 ± 0.079 | 0.897 ± 0.039 |
| Logistic Regression | 0.336 ± 0.074 | 0.538 ± 0.066 | 0.496 ± 0.052 | 0.786 ± 0.048 | 0.712 ± 0.040 | 0.555 ± 0.145 | 0.277 ± 0.163 | 0.781 ± 0.054 |
| Random Forest | 0.317 ± 0.116 | 0.438 ± 0.057 | 0.435 ± 0.060 | 0.801 ± 0.046 | 0.822 ± 0.016 | 0.334 ± 0.179 | 0.000 ± 0.000 | 0.980 ± 0.012 |
| SVM | 0.378 ± 0.131 | 0.473 ± 0.058 | 0.469 ± 0.061 | 0.800 ± 0.036 | 0.829 ± 0.025 | 0.447 ± 0.172 | 0.000 ± 0.000 | 0.973 ± 0.017 |

### Part 4. MRI + clinical with missing clinical data
Table: macro-F1 for each model and each amount of missing clinical data.

| model | 0% missing | 25% missing | 50% missing | 75% missing | 100% missing | only age missing | only sex missing |
|---|---|---|---|---|---|---|---|
| Clinical - Decision Tree | 0.399 ± 0.036 | 0.388 ± 0.041 | 0.365 ± 0.065 | 0.331 ± 0.061 | 0.266 ± 0.079 | 0.277 ± 0.059 | 0.404 ± 0.041 |
| Clinical - Hist Gradient Boosting | 0.384 ± 0.029 | 0.376 ± 0.040 | 0.340 ± 0.074 | 0.316 ± 0.085 | 0.235 ± 0.104 | 0.261 ± 0.061 | 0.405 ± 0.037 |
| Clinical - Logistic Regression | 0.476 ± 0.039 | 0.455 ± 0.037 | 0.443 ± 0.087 | 0.379 ± 0.058 | 0.296 ± 0.001 | 0.296 ± 0.001 | 0.487 ± 0.036 |
| Clinical - Random Forest | 0.392 ± 0.030 | 0.357 ± 0.043 | 0.323 ± 0.070 | 0.286 ± 0.090 | 0.205 ± 0.116 | 0.261 ± 0.053 | 0.418 ± 0.045 |
| Clinical - SVM | 0.299 ± 0.011 | 0.296 ± 0.001 | 0.296 ± 0.001 | 0.300 ± 0.014 | 0.296 ± 0.001 | 0.296 ± 0.001 | 0.296 ± 0.001 |
| MRI - Decision Tree | 0.363 ± 0.046 | 0.363 ± 0.046 | 0.363 ± 0.046 | 0.363 ± 0.046 | 0.363 ± 0.046 | 0.363 ± 0.046 | 0.363 ± 0.046 |
| MRI - Hist Gradient Boosting | 0.454 ± 0.056 | 0.454 ± 0.056 | 0.454 ± 0.056 | 0.454 ± 0.056 | 0.454 ± 0.056 | 0.454 ± 0.056 | 0.454 ± 0.056 |
| MRI - Logistic Regression | 0.449 ± 0.036 | 0.449 ± 0.036 | 0.449 ± 0.036 | 0.449 ± 0.036 | 0.449 ± 0.036 | 0.449 ± 0.036 | 0.449 ± 0.036 |
| MRI - Random Forest | 0.351 ± 0.043 | 0.351 ± 0.043 | 0.351 ± 0.043 | 0.351 ± 0.043 | 0.351 ± 0.043 | 0.351 ± 0.043 | 0.351 ± 0.043 |
| MRI - SVM | 0.397 ± 0.056 | 0.397 ± 0.056 | 0.397 ± 0.056 | 0.397 ± 0.056 | 0.397 ± 0.056 | 0.397 ± 0.056 | 0.397 ± 0.056 |
| MRI+Clinical - Decision Tree | 0.476 ± 0.036 | 0.467 ± 0.050 | 0.442 ± 0.045 | 0.400 ± 0.042 | 0.375 ± 0.043 | 0.375 ± 0.043 | 0.476 ± 0.036 |
| MRI+Clinical - Hist Gradient Boosting | 0.492 ± 0.054 | 0.467 ± 0.048 | 0.457 ± 0.067 | 0.434 ± 0.077 | 0.389 ± 0.081 | 0.389 ± 0.081 | 0.492 ± 0.054 |
| MRI+Clinical - Logistic Regression | 0.496 ± 0.052 | 0.481 ± 0.058 | 0.477 ± 0.055 | 0.475 ± 0.054 | 0.455 ± 0.050 | 0.450 ± 0.050 | 0.505 ± 0.043 |
| MRI+Clinical - Random Forest | 0.435 ± 0.060 | 0.420 ± 0.052 | 0.376 ± 0.041 | 0.350 ± 0.047 | 0.300 ± 0.015 | 0.300 ± 0.015 | 0.433 ± 0.059 |
| MRI+Clinical - SVM | 0.469 ± 0.061 | 0.447 ± 0.058 | 0.403 ± 0.059 | 0.363 ± 0.054 | 0.307 ± 0.023 | 0.307 ± 0.023 | 0.458 ± 0.059 |

Effect of missing clinical data on each multimodal model:

- MRI+Clinical - Decision Tree: macro-F1 0.476 with full clinical data and 0.375 with all clinical data missing (change -0.101, larger than the fold-to-fold spread). Only age missing: 0.375; only sex missing: 0.476.
- MRI+Clinical - Hist Gradient Boosting: macro-F1 0.492 with full clinical data and 0.389 with all clinical data missing (change -0.103, larger than the fold-to-fold spread). Only age missing: 0.389; only sex missing: 0.492.
- MRI+Clinical - Logistic Regression: macro-F1 0.496 with full clinical data and 0.455 with all clinical data missing (change -0.041, inside the fold-to-fold spread). Only age missing: 0.450; only sex missing: 0.505.
- MRI+Clinical - Random Forest: macro-F1 0.435 with full clinical data and 0.300 with all clinical data missing (change -0.135, larger than the fold-to-fold spread). Only age missing: 0.300; only sex missing: 0.433.
- MRI+Clinical - SVM: macro-F1 0.469 with full clinical data and 0.307 with all clinical data missing (change -0.161, larger than the fold-to-fold spread). Only age missing: 0.307; only sex missing: 0.458.

Reference points with all clinical data missing: Clinical - Decision Tree 0.266; Clinical - Hist Gradient Boosting 0.235; Clinical - Logistic Regression 0.296; Clinical - Random Forest 0.205; Clinical - SVM 0.296; best MRI-only model (Hist Gradient Boosting) 0.454.

## Previous work and earlier runs

- Kheirbakhsh et al. (2026, medRxiv preprint, not peer reviewed): UCSF-PDGM, 495 patients, MRI + age + sex, grade 2&3 vs 4, slice-level evaluation with fine-tuned DenseNet121. Clinical-only: accuracy 0.80, MCC 0.44. Best single MRI sequence (T1c): accuracy 0.83, MCC 0.48. Best fusion: accuracy 0.86, MCC 0.54. Check the numbers in the PDF before citing. A missing-clinical-data test was not seen in that paper.
- Earlier run in this project (notebook 04, binary task, frozen ResNet50 features, mean over 3 seeds x 5 folds): MRI-only MCC 0.392; clinical-only MCC 0.451; concatenation fusion MCC 0.447; with all clinical data removed fusion MCC 0.361, MRI-only 0.392, clinical-only 0.000.
- Earlier scratch-CNN baseline (tumor.ipynb, 3 classes, MRI only, one split of 75 test patients): accuracy 0.8133, balanced accuracy 0.4019, macro-F1 0.3978; almost every patient was predicted as grade 4.

These earlier numbers use other models and settings, so compare only the direction of the effects.

## How to read these results

- No significance tests are included here. Differences smaller than the standard deviation should not be treated as real.
- One tumour slice per patient, MRI described by frozen ResNet50 features (no fine-tuning), only age and sex as clinical data.
- Missing clinical data is simulated by hiding values; it is not real hospital missingness.
- Grade 2 and grade 3 have few patients, so three-class results (especially their per-class recall) are unstable.
- Dataset: UCSF-PDGM (Calabrese et al., 2022), Hugging Face copy MedOtter/UCSF-PDGM (CC BY 4.0). Cite the original dataset.
