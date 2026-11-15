# ML2Real --- LOOP AIRWISE Challenge

**Institution:** Jawaharlal Nehru University (JNU), New Delhi\
**Team:** ML2Real\
**Team Leader:** Prakhar Sharma\
**Team Member:** Nikhil Sharma\
**Organised by:** LOOP

## Project Overview

This project predicts the **next calendar day's mean PM2.5
concentration** for each of eight Indian cities using the official LOOP
AIRWISE competition dataset.

The final notebook implements a 47-feature CatBoost regression workflow.
It includes data inspection, exploratory analysis, feature engineering,
chronological validation, error analysis, final model training and
submission-file generation.

### Cities covered

Agra, Delhi, Jaipur, Kanpur, Kolkata, Lucknow, Mumbai and Varanasi.

## Repositories

-   **EDA and Data Analysis:**
    [Loop-Airwise-Challenge-Data-Analysis](https://github.com/Prakhar141205/Loop-Airwise-Challenge-Data-Analysis)
-   **Final Project Repository:**
    [Loop-Airwise-Official](https://github.com/Prakhar141205/Loop-Airwise-Official)

## Project Structure

``` text
Loop-Airwise/
├── data/
│   ├── train.csv
│   └── test.csv
├── notebooks/
│   └── LOOP_AIRWISE_Final_47_Feature_Notebook.ipynb
├── outputs/
│   └── submission.csv
├── report/
│   └── ML2Real_LOOP_AIRWISE_Final_Report.docx
├── README.md
└── requirements.txt
```

## Dataset Summary

  Dataset       Rows   Columns Date range
  ---------- ------- --------- --------------------------
  Training     9,856        33 2022-08-05 to 2025-12-18
  Test         2,464        32 2025-12-19 to 2026-10-22

The target column is `target_pm25_tomorrow`. The `row_id` column is
retained for submission alignment and is not used as a predictive
feature.

Only the official competition-provided data are used in the modelling
workflow. The competition's environmental variables are
modelled/reanalysis-style estimates and should not automatically be
described as direct monitoring-station measurements.

## Model Experiments

Three trained model configurations were evaluated using the
chronological validation split. A median predictor was also used as a
simple baseline.

  -----------------------------------------------------------------------------
  Experiment      Number of Model         Validation    Validation   Validation
                 predictors                      MAE          RMSE           R²
  ------------ ------------ ----------- ------------ ------------- ------------
  Baseline              --- Median           24.7631       34.0288      -0.0256
                            predictor                              

  Compact                15 Random            8.8166       12.6381       0.8585
                            Forest                                 

  Selected               47 CatBoost          8.6332       12.4391       0.8630
                            Regressor                              

  Full feature           86 CatBoost      **8.5966**   **12.3711**   **0.8644**
  set                       Regressor                              
  -----------------------------------------------------------------------------

**Selected project configuration:** 47-feature CatBoost Regressor.

The 86-feature CatBoost achieved the lowest recorded validation MAE. The
47-feature configuration was retained as the final project model because
it provides very similar validation performance while using 39 fewer
predictors. It is a reduced-feature trade-off, not the best-scoring
experiment.

### Selected CatBoost parameters

-   `iterations=500`
-   `learning_rate=0.05`
-   `depth=6`
-   `l2_leaf_reg=3`
-   `loss_function="RMSE"`
-   `random_seed=42`

### Validation approach

The notebook uses a chronological 80:20 split based on unique dates. The
earliest 80% of dates are used for development and the latest 20% are
held out for validation. All city rows belonging to a date remain in the
same partition. This better represents forecasting future dates than
randomly splitting individual rows.

**Selected 47-feature model validation results:**

-   MAE: **8.6332 µg/m³**
-   RMSE: **12.4391 µg/m³**
-   R²: **0.8630**

These are validation results, not scores on the hidden competition test
labels.

## Setup Instructions

### 1. Requirements

-   Python 3.10 or later (Python 3.12 is a suitable choice)
-   Jupyter Notebook or JupyterLab
-   The project files arranged in the directory structure shown above

### 2. Create and activate a virtual environment

**Linux / macOS:**

``` bash
python3 -m venv .venv
source .venv/bin/activate
```

**Windows PowerShell:**

``` powershell
py -m venv .venv
.venv\Scripts\Activate.ps1
```

### 3. Install dependencies

Run this command from the project root directory:

``` bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

### 4. Launch Jupyter

``` bash
jupyter notebook
```

Open:

``` text
notebooks/LOOP_AIRWISE_Final_47_Feature_Notebook.ipynb
```

## Running the Notebook

1.  Keep `data/train.csv` and `data/test.csv` in the `data/` folder.
2.  Open the final notebook from the `notebooks/` folder.
3.  Run all cells from top to bottom.
4.  The notebook performs data checks, feature engineering,
    chronological validation and error analysis.
5.  It then trains the selected CatBoost model on all labelled training
    data.
6.  The generated submission is saved to `outputs/submission.csv`.

The notebook uses relative paths. Run it from its saved location inside
the `notebooks/` directory so that `../data/` and `../outputs/` resolve
correctly.

## Submission Format

The generated `outputs/submission.csv` contains exactly two columns:

-   `row_id`
-   `predicted_pm25`

It produces one prediction for each row in the test dataset and
preserves the original test-row order. The notebook also checks that
predictions are present and finite.

## Important Notes and Limitations

-   The validation scores must not be presented as hidden-test
    performance.
-   The test labels are not available to the team.
-   The model learns statistical relationships; it does not establish
    pollution causation.
-   The analysis does not claim that Diwali fireworks caused any
    observed pollution change.
-   The model is a competition project prototype, not an operational
    public-health alert system.
-   Before real-world deployment, the approach would require independent
    evaluation, uncertainty estimation, monitoring and domain-expert
    review.

## Team

**ML2Real --- Jawaharlal Nehru University (JNU), New Delhi**

-   **Team Leader:** Prakhar Sharma
-   **Team Member:** Nikhil Sharma
