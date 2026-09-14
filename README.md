# Multimodal Fusion Framework for In-Hospital Mortality Prediction (MIMIC-III)
*Leveraging Multimodal Learning for Enhanced Disease Risk Prediction: A Multimodal Fusion Framework for In-Hospital Mortality Prediction Using MIMIC-III*.

## Summary

This project develops and evaluates three multimodal fusion strategies, Early Fusion, Late Fusion, and Cross-Attention Fusion, for predicting in-hospital mortality within the first 48 hours of ICU admission. Four structured modalities from the MIMIC-III Clinical Database (v1.4) are used:

- Vital signs
- Demographics
- Discrete clinical events (procedures and medications)
- Laboratory results

Test set performance (n = 6,409, shared cohort of 32,043 admissions):

| Model | AUROC | AUPRC |
|---|---|---|
| Best unimodal baseline (discrete events) | 0.848 | 0.396 |
| Early Fusion | 0.878 | 0.564 |
| Cross-Attention Fusion | 0.873 | 0.556 |
| Late Fusion | 0.901 | 0.567 |

Late Fusion outperformed four traditional ICU severity scores (SAPS II, APS III, SOFA, and qSOFA) on the same test set.


## Repository Structure

| File | Description |
|---|---|
| `baseline_extraction.ipynb` | Cohort definition, data extraction, preprocessing, feature engineering, and unimodal baseline models |
| `EDA.ipynb` | Exploratory data analysis of cohort composition, length of stay, and mortality patterns|
| `fusion_modelling.ipynb` | Early Fusion, Late Fusion, and Cross-Attention Fusion models, ablation studies, calibration, interpretability analysis, and severity-score benchmarking |
| `requirements.txt` | Python package dependencies |



## Data

This project uses the **MIMIC-III Clinical Database (v1.4)**, accessed through Google Cloud Platform BigQuery under credentialed access.

The dataset is **not included** in this repository.

Reproduction requires:

1. Completion of PhysioNet credentialing and the associated data-use agreement.
2. Access to MIMIC-III v1.4 via PhysioNet or a credentialed Google BigQuery project.

Dataset:

https://physionet.org/content/mimiciii/1.4/



## Environment

| Specification | Detail |
|---|---|
| Platform | Google Colaboratory |
| Runtime | Python 3.10 |
| Hardware | NVIDIA T4 GPU (16 GB) |
| Persistent Storage | Google Drive |

Install dependencies with:

```bash
pip install -r requirements.txt
```

## Running the Code

1. Open the notebooks in Google Colab or a local Jupyter environment with GPU support.
2. Configure credentialed access to MIMIC-III v1.4 through Google BigQuery.
3. Configure any required storage locations for intermediate datasets and model outputs.
4. Execute the notebooks in the following order:

   1. `baseline_extraction.ipynb`
   2. `EDA.ipynb`
   3. `fusion_modelling.ipynb`


## Checking a Reproduction

Reference performance on the held-out test set (n = 6,409):

| Model | AUROC | AUPRC |
|---|---|---|
| Best unimodal baseline (discrete events) | 0.848 | 0.396 |
| Early Fusion | 0.878 | 0.564 |
| Cross-Attention Fusion | 0.873 | 0.556 |
| Late Fusion | 0.901 | 0.567 |

Additional validation checks:

- Cross-Attention AUROC decreases from **0.873** to **0.846** when discrete events are removed.
- Late Fusion Brier score decreases from **0.150** to **0.061** after Platt scaling.
- Late Fusion outperforms SAPS II (**0.817**), APSIII (**0.805**), SOFA (**0.753**), and qSOFA (**0.590**) on AUROC.



## Key Hyperparameters

| Hyperparameter | Value |
|---|---|
| Learning rate | 1e-3 (Adam) |
| LSTM hidden size, unimodal baseline | 128, 2 layers |
| LSTM hidden size, fusion vitals encoder | 32, 1 layer |
| Attention heads | 4 |
| Dropout, Early Fusion head | 0.3 |
| Dropout, Cross-Attention head | 0.5 |
| Epochs | 30 |
| Batch size | 64 |
| Modality masking probability | 0.15 |
| Weight decay, Cross-Attention | 1e-4 |
| Attention robustness seeds | 0, 1, 2, 3, 4 |
| Main training seed | 42 |



## Data Availability

The MIMIC-III Clinical Database v1.4 is not redistributed as part of this repository. Access is governed by the PhysioNet Credentialed Health Data License 1.5.0.



## References

Johnson, A., Pollard, T., & Mark, R. (2016). *MIMIC-III Clinical Database (version 1.4).* PhysioNet. https://doi.org/10.13026/C2XW26

Johnson, A. E. W., Pollard, T. J., Shen, L., Lehman, L. H., Feng, M., Ghassemi, M., Moody, B., Szolovits, P., Celi, L. A., & Mark, R. G. (2016). *MIMIC-III, a freely accessible critical care database.* Scientific Data, 3, 160035.

